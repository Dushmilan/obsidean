---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.5 Transformer Architectures"
tags: [hf, transformers, architecture, encoder, decoder]
transformers: ">=4.40.0"
---

Transformer architectures come in three flavors — encoder-only for understanding, decoder-only for generation, and encoder-decoder for transformation. Choosing the right one depends on whether you need to read, write, or translate.

**The Intuition:** Think of it like communication styles. Encoder-only is like a critic — reads the full review and gives a sentiment score. Decoder-only is like a storyteller — writes one word at a time, building on what came before. Encoder-decoder is like a translator — reads the full source sentence, then writes the full target sentence.

**The Math / Mechanism:**

| Architecture | Best For | Models | Pretraining |
|-------------|----------|--------|-------------|
| Encoder-only | Understanding (classification, NER, extractive QA) | BERT, DistilBERT, ModernBERT | MLM bidirectional |
| Decoder-only | Text generation | GPT, Llama, SmolLM, Gemma, DeepSeek V3 | CLM autoregressive |
| Encoder-decoder | Transduction (translation, summarization) | T5, BART, mBART, Marian | Span corruption / seq2seq |

**Decision framework:**
1. Bidirectional or unidirectional understanding needed?
2. Generating new text or analyzing existing?
3. Transforming one sequence to another?

**Modern LLM Training (Decoder-only):**
1. Pretraining — next token prediction on massive text
2. Instruction tuning — fine-tuned to follow instructions (see [[07-Inference-with-LLMs]] sampling)

**Advanced Attention Mechanisms:**
- **LSH Attention (Reformer)** — only compute attention for similar query-key pairs via hashing
- **Local Attention (Longformer)** — small window per token, some tokens get global attention (see [[09-Using-Transformers]] §4)
- **Axial Positional Encodings** — factorize position matrix for long sequences

**Worked example — pick the right checkpoint:**
```python
# encoder = "bert-base-cased"        # classification
# decoder = "HuggingFaceTB/SmolLM2-360M"  # generation
# enc-dec = "facebook/bart-large-cnn"     # summarization
```

**Common pitfalls:**
- Using encoder for generation (can't autoregress efficiently)
- Using decoder for extractive QA (no bidirectional context)
- Assuming encoder/decoder share weights — they don't, each specializes

**Try it / Check yourself:**
- Classify: prompt completion / summarization / sentiment — which family?
- Why do reforms like LSH matter for long contexts?

**AI Context:** Detailed family decision guide + modern LLM training + long-sequence attentions. Directly used in [[02-Transformer-Pipelines]] task routing and [[09-Using-Transformers]] §4 length limits.

---
Source: https://huggingface.co/learn/llm-course/chapter1/5 (verified 2026-09-01)
Key takeaway: Encoder and decoder don't share weights → each can specialize. Encoder trained to understand, decoder to generate.
Up: [[HuggingFace-LLM-Course_Index]]
