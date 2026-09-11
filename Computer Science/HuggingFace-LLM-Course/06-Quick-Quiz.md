---
date: 2026-09-01
type: quiz
course: HF-LLM-Course
chapter: "01"
section: "1.6 Quick Quiz — Chapter 1 Review"
tags: [hf, quiz, transformers]
transformers: ">=4.40.0"
---

This is an ungraded practice quiz for Chapter 1 of the Hugging Face LLM Course. It covers pipeline usage, model architectures, transfer learning, and bias — all core concepts from the first chapter.

**The Intuition:** Think of this quiz as a sanity check before moving on. If you can answer these without looking things up, you've got a solid grasp of the fundamentals. If not, it's a sign to revisit the earlier chapters.

**The Math / Mechanism:**

**Questions:**

1. What task does `roberta-large-mnli` perform?
2. What does `pipeline("ner", grouped_entities=True)` return for "My name is Sylvain and I work at Hugging Face in Brooklyn."?
3. What replaces `...` in `filler("...")` for BERT's fill-mask?
4. Why does `pipeline("zero-shot-classification")(...)` fail without `candidate_labels`?
5. What is transfer learning?
6. True or false: A language model usually does not need labels for pretraining?
7. Describe the relationship between "model", "architecture", and "weights".
8. Which architecture for completing prompts? (Decoder-only)
9. Which architecture for summarizing? (Encoder-decoder)
10. Which architecture for classifying text? (Encoder-only)
11. What sources can model bias come from?

**Worked example — answering:**
```python
# Q1: pipeline("text-classification", model="roberta-large-mnli") → NLI: entail/neutral/contradiction
# Q3: filler("This [MASK] has been waiting for you.")  # BERT WordPiece mask
# Q4: zero_shot(..., candidate_labels=["positive","negative"])  # required
```

**Answers:**
1. Text classification — NLI (contradiction, neutral, entailment)
2. Entities (PER, ORG, LOC) with positions; `grouped_entities=True` groups multi-word entities
3. `"This [MASK] has been waiting for you."` — BERT uses `[MASK]`
4. Missing `candidate_labels` — zero-shot requires label candidates
5. Training on large dataset (pretraining), then fine-tuning on smaller task-specific dataset
6. True — pretraining is self-supervised
7. Architecture = skeleton. Weights = parameters. Model = umbrella term.
8. Decoder-only (GPT-like) — see [[05-Transformer-Architectures]]
9. Encoder-decoder (T5, BART)
10. Encoder-only (BERT-like)
11. Training data from internet, societal biases, fine-tuning doesn't remove intrinsic bias (see [[08-Bias-and-Limitations]])

**Common pitfalls:**
- Confusing `AutoModel` vs checkpoint — architecture vs weights (Q7)
- Forgetting `candidate_labels` for zero-shot
- Thinking fine-tuning removes bias (it doesn't)

**Try it / Check yourself:**
- Score yourself: 9+/11 = ready for Ch2 [[09-Using-Transformers]]
- Redo missed Qs with Hub search: find `roberta-large-mnli` card

**AI Context:** Self-assessment for Ch1 — pipelines [[02-Transformer-Pipelines]], families [[05-Transformer-Architectures]], transfer learning [[03-How-Transformers-Work]], bias [[08-Bias-and-Limitations]]. Gate before Ch2.

---
Source: https://huggingface.co/learn/llm-course/chapter1/6 (verified 2026-09-01)
Up: [[HuggingFace-LLM-Course_Index]]
