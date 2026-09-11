---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.4 Transformers Solve Tasks"
tags: [hf, transformers, bert, gpt, bart, vit]
checkpoint: "bert-base-cased"
transformers: ">=4.40.0"
---

Different Transformer architectures solve different tasks. BERT encodes bidirectional context for understanding, GPT generates text autoregressively, BART combines both for seq2seq tasks, and ViT extends Transformers to images.

**The Intuition:** Think of it like different tools in a toolbox. BERT is a magnifying glass — it reads the whole sentence at once to understand meaning (classification, NER). GPT is a pen — it writes one word at a time, always looking backward (text generation). BART is a translator — it reads the full source and writes the full target. ViT is a camera that cuts a photo into patches and processes them like words.

**The Math / Mechanism:**

**Two Pretraining Objectives:**
- **Masked LM (MLM)** — encoder (BERT). Mask tokens, predict originals. Bidirectional. `P([MASK] | context both sides)`
- **Causal LM (CLM)** — decoder (GPT). Predict next token left-only. Autoregressive. `P(w_t | w_<t)`

**BERT (Encoder-Only):**
1. Tokenize with WordPiece, add `[CLS]` at start, `[SEP]` between pairs
2. Add segment embeddings (`token_type_ids`)
3. Pass through encoder → final hidden states `[batch, seq, 768]`
4. `[CLS]` output → classification head → logits

| Task | Head Added |
|------|-----------|
| Text classification | `ForSequenceClassification` (linear on `[CLS]`) |
| Token classification (NER) | `ForTokenClassification` (linear per token) |
| Question answering | `ForQuestionAnswering` (span start/end) |

**GPT-2 (Decoder-Only):**
- Tokenize → embeddings + positional → decoder blocks (masked self-attention) → LM head → next token
- No fine-tuning needed for many tasks (zero-shot) — see [[02-Transformer-Pipelines]]

**BART (Encoder-Decoder):**
- Corrupt input → reconstruct with decoder. Text infilling works best.
- Summarization: encoder reads full text, decoder generates summary

**ViT (Vision Transformer):**
1. Split image into patches (e.g., 16×16)
2. Each patch → vector via convolution
3. Add `[CLS]` + positional embeddings
4. Transformer encoder → MLP head → classification

**Worked example — same base, different heads:**
```python
from transformers import AutoModel, AutoModelForSequenceClassification
# base = AutoModel.from_pretrained("bert-base-cased")  # hidden states
# classifier = AutoModelForSequenceClassification.from_pretrained("bert-base-cased")  # + head
```

**Common pitfalls:**
- Using `AutoModel` when you need a head → no logits, only `last_hidden_state`
- Forgetting ViT also uses `[CLS]` like BERT — shared pattern
- Mixing MLM vs CLM objectives when explaining pretraining

**Try it / Check yourself:**
- Which objective for BERT vs GPT-2? Which head for NER vs QA?
- Why BART for summarization not BERT?

**AI Context:** Explains how each architecture solves tasks via heads/pretraining objectives. Prereq for [[05-Transformer-Architectures]]; connects to [[09-Using-Transformers]] §2 (heads table).

---
Source: https://huggingface.co/learn/llm-course/chapter1/4 (verified 2026-09-01)
Key insight: Once pretrained, add a task-specific head. Same base, different tasks.
Up: [[HuggingFace-LLM-Course_Index]]
