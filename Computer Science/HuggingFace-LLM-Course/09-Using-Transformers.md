---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "02"
section: "2.1 Using Transformers"
tags: [hf, transformers, tokenizer, model, batching, attention-mask]
checkpoint: "distilbert-base-uncased-finetuned-sst-2-english"
transformers: ">=4.40.0"
---

Using Transformers — Behind the Pipeline is Chapter 2 of the 🤗 LLM Course. `pipeline()` hides 3 steps: tokenizer (text → `input_ids` + `attention_mask`) → model (IDs → logits/hidden states) → postprocessing (`softmax` → labels). Understanding them lets you use any checkpoint, control padding/truncation, and build batches manually.

**The Intuition:** Think of `pipeline()` as a cake factory assembly line. The tokenizer is the prep station (chops raw text into measured ingredients/tokens + maps to IDs, adds `[CLS]`/`[SEP]`), the model is the oven (transforms ingredients into baked hidden states `[batch, seq_len, hidden]` then a task head into logits `[batch, num_labels]`), and postprocessing is plating (`softmax` + `config.id2label` → human labels). `AutoTokenizer`/`AutoModel` are universal adapters — give them a checkpoint name and they fetch the right recipe (vocab + `config.json` + `model.safetensors`) from the Hub. Batching is baking multiple cakes at once — needs a rectangular tray, so short cakes get padding and a mask says "ignore the empty slots".

**The Math:**

### 1. Replicating `pipeline()` — The 3 Steps

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

raw = ["I've been waiting for a HuggingFace course my whole life.", "I hate this so much!"]
inputs = tokenizer(raw, padding=True, truncation=True, return_tensors="pt")
outputs = model(**inputs)  # logits shape: [2, 2]
probs = torch.nn.functional.softmax(outputs.logits, dim=-1)
model.config.id2label  # {0: 'NEGATIVE', 1: 'POSITIVE'}
# → [[0.0402, 0.9598], [0.9995, 0.0005]]
```

| Step | Code | Output |
|------|------|--------|
| Preprocess | `tokenizer(text, padding=True, truncation=True, return_tensors="pt")` | `input_ids` + `attention_mask` (+ `token_type_ids` for BERT) |
| Inference | `model(**inputs).logits` | Raw unnormalized scores, `[batch, num_labels]` |
| Postprocess | `softmax(logits, dim=-1)` + `id2label` | Probabilities + string labels |

Hidden states (base model) shape: `[batch_size, seq_len, hidden_size]` e.g. `[2, 16, 768]` (`outputs.last_hidden_state` for `AutoModel`). Logits shape after head: `[batch, num_labels]` e.g. `[2, 2]`. `model(**inputs)` returns a dict-like object — access via `outputs.logits`, `outputs["logits"]`, or `outputs[0]`.

Why logits need `softmax`: training fuses `SoftMax` + `CrossEntropy` loss, so models output unnormalized scores. `softmax(logits)` → probabilities.

Transformer library goals (from official intro): **Ease of use** (2 lines to SOTA), **Flexibility** (models are `nn.Module`), **Simplicity** (one-file forward pass, no cross-model sharing).

### 2. Models — `AutoModel`, Config + Weights, Heads

```python
from transformers import AutoModel, BertModel

# Auto wrapper — infers architecture from checkpoint name
model = AutoModel.from_pretrained("bert-base-cased")  # 12 layers, 768 hidden, 12 heads, cased
# Explicit class — same result when you know the type
model = BertModel.from_pretrained("bert-base-cased")
```

| Concept | File / API | Notes |
|---------|-----------|-------|
| `AutoModel` | Wrapper over `BertModel`, `DistilBertModel`, etc. | Guesses architecture from `config.json` |
| `config.json` | `model.save_pretrained("dir")` → `config.json` + `model.safetensors` | Architecture + metadata (origin, transformers version) |
| `model.safetensors` | State dict (was `pytorch_model.bin`) | All weights; loaded with `from_pretrained("dir")` |
| Sharing | `model.push_to_hub("my-awesome-model")` after `huggingface-cli login` or `notebook_login()` | Creates `username/my-awesome-model` on Hub |

**Encoding text — what the tokenizer returns:**
```python
enc = tokenizer("Hello, I'm a single sentence!")
# {'input_ids': [101, 8667, 117, ... 102], 'token_type_ids': [0,...], 'attention_mask': [1,...]}
tokenizer.decode(enc["input_ids"])  # "[CLS] Hello, I'm a single sentence! [SEP]"
# Multi-sentence: tokenizer("How are you?", "I'm fine, thank you!") → two lists per key
```
- `input_ids`: token → ID via vocab (must match pretraining vocab)
- `token_type_ids`: 0 = sentence A, 1 = sentence B (for pair tasks; `0` everywhere if single sentence)
- `attention_mask`: 1 = attend, 0 = ignore (padding) — see §4
- Special tokens (`[CLS]` 101, `[SEP]` 102 for BERT) are auto-added because pretraining used them. Not all models do — tokenizer knows.

**Model heads** — same base hidden states, different projection:

| Head | Class | Task |
|------|-------|------|
| Base | `*Model` | Hidden states `[batch, seq, hidden]` |
| Classification | `*ForSequenceClassification` | Linear on `[CLS]` → `[batch, num_labels]` |
| Token | `*ForTokenClassification` | Linear per token → NER/POS |
| QA | `*ForQuestionAnswering` | Span start/end |
| LM | `*ForMaskedLM` / `*ForCausalLM` | Vocab logits |
| Multiple choice | `*ForMultipleChoice` | Choice logits |

Input must be batched: even single sentence needs `tensor([ids])` not `tensor(ids)` — models expect `[batch, seq_len]` or they raise `IndexError: Dimension out of range`.

### 3. Tokenizers — Word → Character → Subword

| Type | How it splits | Vocab | Problem |
|------|---------------|-------|---------|
| Word-based | `split()` on space/punct → `["Jim","Henson","was","a","puppeteer"]` | 500k+ | `dog` ≠ `dogs`, many `[UNK]` |
| Character-based | Per char | Tiny (e.g. 256) | 1 word → 10+ tokens, low meaning per char |
| Subword | Frequent words kept, rare split: `annoyingly` → `annoying` + `ly`, `tokenization` → `token` + `ization` | ~30k, ~no UNK | Best balance (handles agglutinative languages like Turkish) |

Key algorithms: **Byte-level BPE** (GPT-2), **WordPiece** (BERT), **SentencePiece/Unigram** (multilingual, e.g. mBART/T5).

**Tokenizer API (2-step encoding = tokenize + convert):**
```python
tokenizer.tokenize("Using a Transformer network is simple")
# ['Using', 'a', 'transform', '##er', 'network', 'is', 'simple']  # subword split
ids = tokenizer.convert_tokens_to_ids(tokens) # [7993, 170, 11303, 1200, 2443, 1110, 3014]
tokenizer.decode(ids) # 'Using a Transformer network is simple'  # groups ## pieces
tokenizer("Hello!") # {'input_ids': [101, 8667,...,102], 'attention_mask': [1,...], 'token_type_ids': [0,...]}
# Loading/Saving mirrors models
tokenizer.save_pretrained("dir")  # vocab + tokenizer.json + config
AutoTokenizer.from_pretrained(checkpoint)  # auto-picks BertTokenizer etc.
```

Practical tip: try `tokenize` + `convert_tokens_to_ids` on `["I've been waiting...","I hate this so much!"]` and verify IDs match `tokenizer(raw, ... )["input_ids"]`.

### 4. Handling Multiple Sequences — Batching, Padding, Masks, Length

**Models expect a batch** — single-sequence trap:
```python
ids = tokenizer.convert_tokens_to_ids(tokenizer.tokenize("I've been waiting..."))
torch.tensor(ids)        # shape [14] → model fails: IndexError
torch.tensor([ids])      # shape [1, 14] → ok
tokenizer("...", return_tensors="pt")["input_ids"]  # shape [1, 14] auto-batched
```

**Padding — rectangular tensors required:**
```python
tok = tokenizer(["How are you?", "I'm fine, thank you!"], padding=True, return_tensors="pt")
# input_ids:      [[101,1731,1132,1128, 136, 102,  0,  0,  0,  0],
#                  [101,1045,1005,1049,2503, 117,5763,1128,136,102]]
# attention_mask: [[1,1,1,1,1,1,0,0,0,0],
#                  [1,1,1,1,1,1,1,1,1,1]]
# pad_token_id == 0 (tokenizer.pad_token_id) fills short rows
# Standalone: [[200,200,200],[200,200, pad_id]] — 200 is dummy ID
```

**Attention mask — don't attend to padding:**
```python
# Without mask, padding contextualizes → wrong logits:
# batched [0.5803,-0.4125] becomes [1.3373,-1.2163] — bug!
# With mask → matches single-sentence logits:
model(torch.tensor(batched_ids), attention_mask=torch.tensor([[1,1,1],[1,1,0]]))
# mask: 1 = real token, 0 = padding → ignored in attention layers
```

**Longer sequences — model limits:**
| Problem | Limit | Solution |
|---------|-------|----------|
| BERT/DistilBERT max 512, GPT-like 1024/2048 | `seq > max_position_embeddings` → crash | `truncation=True, max_length=512` |
| Very long docs | Standard attention O(n²) | Use Longformer / LED (sparse/local/global attention) |
| Need exact size | | `padding="max_length"`, `padding="longest"` (default), `max_length=8` |

```python
# Combine padding + truncation + max_length
tokenizer(["How are you?", "I'm fine, thank you!"],
          padding=True, truncation=True, max_length=5, return_tensors="pt")
# → input_ids [[101,1731,1132,1128,102],[101,1045,1005,1049,102]]  (truncated to 5)
```

### 5. Putting It All Together — The High-Level `tokenizer()` Call

`tokenizer()` does tokenize + convert + add specials + pad + truncate + tensorize:

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

# Single, batch, pad, truncate, tensor type — one API
tokenizer("I've been waiting for a HuggingFace course my whole life.")
tokenizer(["I've been waiting...","So have I!"])
tokenizer(sequences, padding="longest")              # to longest in batch
tokenizer(sequences, padding="max_length")           # to model max (512)
tokenizer(sequences, padding="max_length", max_length=8)
tokenizer(sequences, truncation=True)                # cut > model max
tokenizer(sequences, max_length=8, truncation=True)
tokenizer(sequences, padding=True, return_tensors="pt")  # torch
tokenizer(sequences, padding=True, return_tensors="np")  # numpy
tokenizer(sequences, padding=True, return_tensors="tf")  # tensorflow

# Special tokens are auto-added — compare:
ids_no_special = tokenizer.convert_tokens_to_ids(tokenizer.tokenize(seq))  # [1045,1005,...]
ids_with       = tokenizer(seq)["input_ids"]  # [101, 1045,1005,...,102] → [CLS]...[SEP]

# End-to-end
tokens = tokenizer(sequences, padding=True, truncation=True, return_tensors="pt")
output = model(**tokens)  # ready for softmax / argmax
```

**Common pitfalls:**
- Forgetting `padding=True` with batches of different lengths → shape mismatch
- Forgetting `attention_mask` when manually padding → silently wrong logits
- Forgetting `truncation=True` for long texts → `IndexError` / truncated silently by model
- Single sequence without batch dim → `IndexError: Dimension out of range` → wrap in list or use `return_tensors="pt"`
- Assuming `pytorch_model.bin` — current saves are `model.safetensors`
- `token_type_ids` missing on RoBERTa/DistilBERT is normal — only BERT-like needs it

**Try it:** Replicate `classifier(["I've been waiting...","I hate this so much!"])` manually (§1) and verify batched `padding=True` + `attention_mask` gives same logits as two single calls. Then try your own sentences, vary `temperature`-free (this chapter is non-generative), and decode with `tokenizer.decode`.

**AI Context:** This file covers HF LLM Course Ch.2 — `pipeline()` internals, `AutoModel`/`AutoTokenizer` + `config.json`/`safetensors` + `push_to_hub`, subword tokenization (BPE/WordPiece/SentencePiece), batching essentials (padding ID, attention_mask 1/0, truncation, 512-token limit, Longformer/LED), and the unified `tokenizer()` API (padding longest/max_length, truncation, return_tensors pt/np/tf, special tokens). Direct prerequisite for Ch.3 Fine-tuning (needs correct batching) and Ch.4 Sharing. Connects to [[03-How-Transformers-Work]] (architecture) and [[10-Attention-and-Transformers]].

---
**Source:** [HF LLM Course Ch.2 — Using Transformers](https://huggingface.co/learn/llm-course/chapter2/1) (sections 1–7: Introduction → Behind the pipeline → Models → Tokenizers → Handling multiple sequences → Putting it all together). Verified 2026-09-01.
**Hub:** `huggingface-cli login` → `push_to_hub()` ; `model.save_pretrained` / `tokenizer.save_pretrained`.
**Key Paper:** ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)
**Up:** [[HuggingFace-LLM-Course_Index]]

(End of file)
