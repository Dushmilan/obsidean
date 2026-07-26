Transformer architectures come in three flavors — encoder-only for understanding, decoder-only for generation, and encoder-decoder for transformation. Choosing the right one depends on whether you need to read, write, or translate.

**The Intuition:** Think of it like communication styles. Encoder-only is like a critic — reads the full review and gives a sentiment score. Decoder-only is like a storyteller — writes one word at a time, building on what came before. Encoder-decoder is like a translator — reads the full source sentence, then writes the full target sentence.

**The Math:**

| Architecture | Best For | Models |
|-------------|----------|--------|
| Encoder-only | Understanding (classification, NER, extractive QA) | BERT, DistilBERT, ModernBERT |
| Decoder-only | Text generation | GPT, Llama, SmolLM, Gemma, DeepSeek V3 |
| Encoder-decoder | Transduction (translation, summarization) | T5, BART, mBART, Marian |

**Decision framework:**
1. Bidirectional or unidirectional understanding needed?
2. Generating new text or analyzing existing?
3. Transforming one sequence to another?

**Modern LLM Training (Decoder-only):**
1. Pretraining — next token prediction on massive text
2. Instruction tuning — fine-tuned to follow instructions

**Advanced Attention Mechanisms:**
- **LSH Attention (Reformer)** — only compute attention for similar query-key pairs via hashing
- **Local Attention (Longformer)** — small window per token, some tokens get global attention
- **Axial Positional Encodings** — factorize position matrix for long sequences

**AI Context:** This file covers all three Transformer architecture families in detail, provides a decision framework for choosing architectures, explains modern LLM training pipelines, and covers advanced attention mechanisms (LSH, local, axial). Essential reference for architecture selection.

---
**Key takeaway:** Encoder and decoder don't share weights → each can specialize. Encoder trained to understand, decoder trained to generate.
