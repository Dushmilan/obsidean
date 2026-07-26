Inference is using a trained LLM to generate text from a prompt — predicting one token at a time using learned probabilities. The process has two phases: prefill (process all input) and decode (generate one token at a time), each with different computational characteristics.

**The Intuition:** Think of inference like autocomplete on your phone. When you type "The capital of France is," the model has already processed all those words (prefill — heavy computation), then it generates "Paris" one token at a time (decode — memory intensive because it must track everything it's seen). Temperature is like a creativity dial: low means "always pick the most likely word," high means "surprise me."

**The Math:**

**Two-Phase Inference:**

| Phase | What Happens | Bottleneck |
|-------|-------------|------------|
| Prefill | Tokenize → embed → process all input at once | Computation (processes all input) |
| Decode | Attention → probability → select next token (autoregressive) | Memory (tracks all generated tokens) |

**Sampling Strategies:**

| Strategy | How | Trade-off |
|----------|-----|-----------|
| Temperature | Scale logits by $1/T$ | Low = focused, High = creative |
| Greedy | Always pick highest probability | Fast but repetitive |
| Top-K | Consider top K most likely tokens | Limits vocabulary |
| Top-P (Nucleus) | Smallest set with cumulative prob ≥ P | Dynamic vocabulary |
| Beam Search | Track multiple sequences | More coherent, more compute |

**Key Metrics:**

| Metric | What it Measures |
|--------|-----------------|
| TTFT | Latency for first response (prefill) |
| TPOT | Generation speed per token |
| Throughput | Concurrent requests handled |
| VRAM Usage | GPU memory required |

**Practical Challenges:**
- Memory grows **quadratically** with context length
- KV Cache stores intermediate calculations to reduce repeated computation
- Trade-off: more memory for faster generation

**AI Context:** This file covers LLM inference internals — the two-phase process (prefill/decode), sampling strategies (temperature, greedy, top-k, top-p, beam search), key performance metrics, and practical challenges like KV cache and context length limitations.

---
Context length challenge: longer context = more info but quadratic memory growth. KV cache optimization makes long-context generation practical by storing and reusing intermediate Key-Value pairs.
