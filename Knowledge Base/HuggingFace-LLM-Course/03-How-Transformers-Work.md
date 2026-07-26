Transformers are the architecture behind modern LLMs — they use attention mechanisms to process entire sequences in parallel, replacing the sequential processing of RNNs. The key insight is that meaning comes from context: a word's meaning depends on all the words around it.

**The Intuition:** Think of attention like reading a sentence with a highlighter. When you read "The cat sat on the mat because it was tired," you mentally highlight "it" and connect it back to "cat." That's what attention does — it lets the model focus on relevant words when processing each word. The original Transformer was built for translation: the encoder sees the full English sentence, and the decoder generates French one word at a time, attending to both its own output and the encoder's representation.

**The Math:**

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
- **GPT-like** — auto-regressive (decoder-only), predict next token
- **BERT-like** — auto-encoding (encoder-only), bidirectional context
- **T5-like** — sequence-to-sequence (encoder-decoder)

**Architecture:**

```
Input → [Encoder] → Representation → [Decoder] → Output
```

| Architecture | Use Case | Examples |
|-------------|----------|----------|
| Encoder-only | Understanding (classification, NER, QA) | BERT, DistilBERT |
| Decoder-only | Generation (text gen) | GPT, Llama |
| Encoder-decoder | Transduction (translation, summarization) | T5, BART |

**Transfer Learning:** Pretrain on massive data (weeks, huge compute), then fine-tune on task-specific data (less data, lower cost, better results). Why not train from scratch? Pretrained models already have language knowledge.

**AI Context:** This file covers the Transformer architecture, its history, the three model families (encoder-only, decoder-only, encoder-decoder), attention mechanisms, and transfer learning. Core reference for understanding how modern LLMs work.

---
**Key Paper:** ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762) — Vaswani et al., 2017. **Architecture vs Checkpoint:** Architecture = skeleton (layers, operations). Checkpoint = weights loaded into architecture.
