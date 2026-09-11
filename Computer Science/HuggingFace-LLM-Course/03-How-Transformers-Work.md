---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.3 How Transformers Work"
tags: [hf, transformers, attention, architecture]
transformers: ">=4.40.0"
---

Transformers are the architecture behind modern LLMs — they use attention mechanisms to process entire sequences in parallel, replacing the sequential processing of RNNs. The key insight is that meaning comes from context: a word's meaning depends on all the words around it.

**The Intuition:** Think of attention like reading a sentence with a highlighter. When you read "The cat sat on the mat because it was tired," you mentally highlight "it" and connect it back to "cat." That's what attention does — it lets the model focus on relevant words when processing each word. The original Transformer was built for translation: the encoder sees the full English sentence, and the decoder generates French one word at a time, attending to both its own output and the encoder's representation.

**The Math / Mechanism:**

**Transformer Timeline:**

| Date | Model | Breakthrough |
|------|-------|-------------|
| Jun 2017 | **Transformer** | "Attention Is All You Need" |
| Jun 2018 | **GPT** | First pretrained Transformer |
| Oct 2018 | **BERT** | Bidirectional understanding |
| May 2020 | **GPT-3** | Zero-shot learning |
| Jan 2023 | **Llama** | Multilingual LLM |
| Mar 2023 | **Mistral** | 7B params, grouped-query attention |

**Three Model Families:**
- **GPT-like** — auto-regressive (decoder-only), predict next token `P(w_t | w_<t)`
- **BERT-like** — auto-encoding (encoder-only), bidirectional `P(w_t | w_≠t)`
- **T5-like** — sequence-to-sequence (encoder-decoder)

**Architecture:**

```
Input → [Encoder] → Representation → [Decoder] → Output
```

| Architecture | Use Case | Examples |
|-------------|----------|----------|
| Encoder-only | Understanding (classification, NER, QA) | BERT, DistilBERT |
| Decoder-only | Generation (text gen) | GPT, Llama, SmolLM |
| Encoder-decoder | Transduction (translation, summarization) | T5, BART, Marian |

Transfer learning: Pretrain on massive data (weeks, huge compute) → fine-tune on task-specific data (less data, lower cost, better results). Why not from scratch? Pretrained weights already encode language.

**Worked example — choosing a family:**
```python
# Understanding → encoder: pipeline("text-classification", model="bert-base-cased")
# Generation  → decoder: pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")
# Translation → encoder-decoder: pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")
```

**Common pitfalls:**
- Confusing architecture vs checkpoint — architecture = skeleton, checkpoint = weights in it
- Using decoder-only for classification (possible but wasteful) — pick encoder per [[05-Transformer-Architectures]]
- Forgetting transfer learning is why fine-tuning beats training from scratch

**Try it / Check yourself:**
- Which family for: (a) completing prompts (b) summarizing (c) classifying text? (see [[06-Quick-Quiz]] Q8-10)
- Explain why pretraining is self-supervised (no labels needed)

**AI Context:** Core reference for how modern LLMs work — attention, three families, transfer learning. Prereq for [[04-Transformers-Solve-Tasks]] and [[05-Transformer-Architectures]]; connects to [[10-Attention-and-Transformers]].

---
Source: https://huggingface.co/learn/llm-course/chapter1/3 (verified 2026-09-01)
Key Paper: ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762) — Vaswani et al., 2017. Architecture vs Checkpoint: skeleton vs weights.
Up: [[HuggingFace-LLM-Course_Index]]
