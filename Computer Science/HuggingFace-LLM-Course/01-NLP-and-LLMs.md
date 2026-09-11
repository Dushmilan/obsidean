---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.1 NLP and LLMs"
tags: [hf, nlp, llms, transformers]
transformers: ">=4.40.0"
---

NLP (Natural Language Processing) is the broader field of enabling computers to understand, interpret, and generate human language. LLMs are the latest advancement within NLP — massive models trained on huge data that perform many tasks with minimal task-specific training.

**The Intuition:** Think of NLP as teaching a foreign language. You start with individual words (token classification), then learn sentences (sentiment analysis), then hold conversations (text generation), and finally translate between languages (seq2seq). LLMs are like a student who learned by reading the entire internet — they're surprisingly good at everything but sometimes confidently wrong (hallucinations).

**The Math / Mechanism:**

**Core NLP Tasks:**

| Task | Example | Pipeline |
|------|---------|----------|
| Sentence classification | Sentiment, spam, grammatical correctness | `text-classification` |
| Token classification | POS tagging, NER (PER/ORG/LOC) | `token-classification` / `ner` |
| Text generation | Auto-complete, fill-in-the-blank | `text-generation`, `fill-mask` |
| QA | Extract answer from context | `question-answering` |
| Seq2seq | Translation, summarization | `translation`, `summarization` |

**LLM Characteristics:**
- **Scale** — millions to hundreds of billions of parameters
- **General capabilities** — multiple tasks without task-specific training
- **In-context learning** — learn from examples in the prompt
- **Emergent abilities** — capabilities that appear at scale, not explicitly programmed

**LLM Limitations:** Hallucinations (confident incorrect info), no true understanding (purely statistical patterns), bias (reproduce training data biases), limited context windows (see [[07-Inference-with-LLMs]]), significant compute cost.

**Worked example — task mapping:**
```python
from transformers import pipeline
# classifier = pipeline("text-classification")  # sentence → label
# ner = pipeline("ner")  # tokens → entities
# generator = pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")
```

**Common pitfalls:**
- Assuming LLMs = understanding — they predict next token statistically
- Ignoring context-window limits → truncated input without `truncation=True`
- Expecting deterministic output without fixing temperature/seed

**Try it / Check yourself:**
- Name 3 tasks that are token-classification vs seq2seq
- Why do hallucinations happen even in large models?

**AI Context:** This file covers foundational NLP vs LLMs — defines tasks, LLM traits (scale/general/in-context/emergent) and limits. Prereq for [[02-Transformer-Pipelines]] and all of Ch1. Connects to [[Computer Science/Machine-Learning/Machine-Learning_Index|Machine-Learning_Index]] (classification) and [[Computer Science/Deep-Learning/Deep-Learning_Index|Deep-Learning_Index]].

---
Source: https://huggingface.co/learn/llm-course/chapter1/1 (verified 2026-09-01)
Key Paper: ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)
Up: [[HuggingFace-LLM-Course_Index]]
