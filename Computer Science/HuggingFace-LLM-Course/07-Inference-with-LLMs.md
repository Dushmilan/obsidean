---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.7 Inference with LLMs"
tags: [hf, llms, inference, sampling, kv-cache]
transformers: ">=4.40.0"
---

Inference is using a trained LLM to generate text from a prompt — predicting one token at a time using learned probabilities. The process has two phases: prefill (process all input) and decode (generate one token at a time), each with different computational characteristics.

**The Intuition:** Think of inference like autocomplete on your phone. When you type "The capital of France is," the model has already processed all those words (prefill — heavy computation), then it generates "Paris" one token at a time (decode — memory intensive because it must track everything it's seen). Temperature is like a creativity dial: low means "always pick the most likely word," high means "surprise me."

**The Math / Mechanism:**

**Two-Phase Inference:**

| Phase | What Happens | Bottleneck |
|-------|-------------|------------|
| Prefill | Tokenize → embed → process all input at once | Computation (processes all input) |
| Decode | Attention → probability → select next token (autoregressive) | Memory (tracks all generated tokens, KV cache) |

**Sampling Strategies:**

| Strategy | How | Trade-off |
|----------|-----|-----------|
| Temperature | Scale logits by $1/T$ → `softmax(logits/T)` | Low = focused, High = creative |
| Greedy | Always pick `argmax` | Fast but repetitive |
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

Practical challenges: Memory grows **quadratically** with context length. KV Cache stores intermediate Key-Value pairs to avoid recomputation — trade-off: more VRAM for faster generation.

**Worked example — decoding knobs:**
```python
from transformers import pipeline
# gen = pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")
# gen("The capital of France is", do_sample=True, temperature=0.7, top_p=0.9, top_k=50, max_new_tokens=20)
# gen(..., num_beams=4)  # beam search
```

**Common pitfalls:**
- High temperature + top_p → incoherent text
- Ignoring KV cache VRAM → OOM on long context
- Confusing TTFT (prefill) vs TPOT (decode) when benchmarking

**Try it / Check yourself:**
- Compare greedy vs `temperature=0.9, top_p=0.95` on same prompt — which repeats?
- Why does throughput drop as context length doubles? (quadratic attention)

**AI Context:** Covers inference internals — prefill/decode, sampling, metrics, KV cache. Connects to [[09-Using-Transformers]] §4 (context limits/Longformer) and [[05-Transformer-Architectures]] (decoder-only generation).

---
Source: https://huggingface.co/learn/llm-course/chapter1/7 (verified 2026-09-01)
Key note: Longer context = more info but quadratic memory growth. KV cache reuses Key-Value pairs.
Up: [[HuggingFace-LLM-Course_Index]]
