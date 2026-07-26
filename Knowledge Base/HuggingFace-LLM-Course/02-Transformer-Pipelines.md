The `pipeline()` function is the highest-level API in the Transformers library — it connects a model with preprocessing and postprocessing so you can run NLP tasks in one line of code. It's the easiest way to get started with any Transformer model.

**The Intuition:** Think of `pipeline()` like a restaurant ordering system. You say "I want sentiment analysis" (the task), and the pipeline handles everything behind the scenes — preprocessing your text into tokens, feeding it through the model, and returning a human-readable result. You don't need to know how the kitchen works.

**The Math:**

**3-Step Flow:**
1. **Preprocess** — text → tokens (model-readable format)
2. **Model inference** — tokens → model predictions
3. **Postprocess** — predictions → human-readable output

**Available Pipelines:**

| Category | Pipelines |
|----------|-----------|
| Text | `text-generation`, `text-classification`, `summarization`, `translation`, `zero-shot-classification`, `fill-mask`, `ner`, `question-answering`, `feature-extraction` |
| Image | `image-classification`, `object-detection`, `image-to-text` |
| Audio | `automatic-speech-recognition`, `audio-classification`, `text-to-speech` |
| Multimodal | `image-text-to-text` |

**Key Examples:**
- **Zero-shot classification:** No fine-tuning needed — specify your own labels
- **Text generation:** Generate text from a prompt
- **Fill mask:** Predict `[MASK]` tokens
- **NER:** Extract named entities (PER, ORG, LOC)
- **Question answering:** Extract answer from context
- **Translation:** Translate between languages

**AI Context:** This file covers the Transformers `pipeline()` API — the simplest way to use pre-trained models. It explains the preprocessing → inference → postprocessing flow, lists all available pipeline types, and provides code examples for each major task.

---
Use any model from the Hub by passing its name: `pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")`. Inference Providers allow testing all models in-browser.
