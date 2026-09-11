---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.2 Transformer Pipelines"
tags: [hf, transformers, pipeline]
checkpoint: "distilbert-base-uncased-finetuned-sst-2-english"
transformers: ">=4.40.0"
---

The `pipeline()` function is the highest-level API in the Transformers library — it connects a model with preprocessing and postprocessing so you can run NLP tasks in one line of code. It's the easiest way to get started with any Transformer model.

**The Intuition:** Think of `pipeline()` like a restaurant ordering system. You say "I want sentiment analysis" (the task), and the pipeline handles everything behind the scenes — preprocessing your text into tokens, feeding it through the model, and returning a human-readable result. You don't need to know how the kitchen works.

**The Math / Mechanism:**

**3-Step Flow (see [[09-Using-Transformers]] for manual replication):**
1. **Preprocess** — text → tokens (`input_ids` + `attention_mask` via `AutoTokenizer`)
2. **Model inference** — tokens → logits (`AutoModelFor*` → `outputs.logits`)
3. **Postprocess** — logits → human labels (`softmax` + `id2label`)

**Available Pipelines:**

| Category | Pipelines |
|----------|-----------|
| Text | `text-generation`, `text-classification`, `summarization`, `translation`, `zero-shot-classification`, `fill-mask`, `ner`, `question-answering`, `feature-extraction` |
| Image | `image-classification`, `object-detection`, `image-to-text` |
| Audio | `automatic-speech-recognition`, `audio-classification`, `text-to-speech` |
| Multimodal | `image-text-to-text` |

**Worked example — all tasks in one API:**
```python
from transformers import pipeline

# classifier = pipeline("text-classification")  # → [{'label':'POSITIVE','score':0.96}]
# ner = pipeline("ner", grouped_entities=True)
# ner("My name is Sylvain and I work at Hugging Face in Brooklyn.")
# fill = pipeline("fill-mask", model="bert-base-cased")  # needs [MASK]
# qa = pipeline("question-answering")
# qa(question="Where do I work?", context="My name is Sylvain...")
# translator = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")
# generator = pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")
```

**Common pitfalls:**
- `pipeline("zero-shot-classification")` without `candidate_labels` → error (see [[06-Quick-Quiz]])
- `fill-mask` with wrong mask token (`<mask>` vs `[MASK]` per checkpoint)
- Assuming default checkpoint is best — override with `model="..."` from Hub

**Try it / Check yourself:**
- Run `pipeline("sentiment-analysis")` on 2 sentences, then replicate manually per [[09-Using-Transformers]] §1
- Search Hub for POS tagger and test `ner` vs `token-classification`

**AI Context:** This file covers the `pipeline()` API — the entry point that groups preprocessing→inference→postprocessing. Prereq for [[03-How-Transformers-Work]] and demystified in [[09-Using-Transformers]].

---
Source: https://huggingface.co/learn/llm-course/chapter1/2 (verified 2026-09-01)
Hub: `pipeline(task, model="...")` — Inference Providers let you test any Hub model in-browser
Up: [[HuggingFace-LLM-Course_Index]]
