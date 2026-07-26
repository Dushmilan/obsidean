Different Transformer architectures solve different tasks. BERT encodes bidirectional context for understanding, GPT generates text autoregressively, BART combines both for seq2seq tasks, and ViT extends Transformers to images.

**The Intuition:** Think of it like different tools in a toolbox. BERT is a magnifying glass — it reads the whole sentence at once to understand meaning (classification, NER). GPT is a pen — it writes one word at a time, always looking backward (text generation). BART is a translator — it reads the full source and writes the full target. ViT is a camera that cuts a photo into patches and processes them like words.

**The Math:**

**Two Pretraining Approaches:**
- **Masked LM (MLM)** — encoder models (BERT). Mask tokens, predict originals. Bidirectional context.
- **Causal LM (CLM)** — decoder models (GPT). Predict next token from left context only. Autoregressive.

**BERT (Encoder-Only):**
1. Tokenize with WordPiece, add `[CLS]` at start, `[SEP]` between pairs
2. Add segment embeddings
3. Pass through encoder → final hidden states
4. `[CLS]` output → classification head

| Task | Head Added |
|------|-----------|
| Text classification | Sequence classification (linear on `[CLS]`) |
| Token classification (NER) | Token classification (linear per token) |
| Question answering | Span classification (predict start/end) |

**GPT-2 (Decoder-Only):**
- Tokenize → embeddings + positional encodings → decoder blocks (masked self-attention) → language modeling head → next token prediction
- No fine-tuning needed for many tasks (zero-shot)

**BART (Encoder-Decoder):**
- Corrupt input → reconstruct with decoder. Text infilling works best.
- Summarization: encoder reads full text, decoder generates summary

**ViT (Vision Transformer):**
1. Split image into patches (e.g., 16×16)
2. Each patch → vector via convolution
3. Add `[CLS]` + positional embeddings
4. Transformer encoder → MLP head → classification

**AI Context:** This file explains how each Transformer architecture (BERT, GPT-2, BART, Whisper, ViT) solves specific tasks. Covers the pretraining objectives (MLM vs CLM), task-specific heads, and the key parallel between BERT and ViT (both use `[CLS]` tokens).

---
**Key insight:** Once pretrained, you just add a task-specific head. Same base model, different tasks.
