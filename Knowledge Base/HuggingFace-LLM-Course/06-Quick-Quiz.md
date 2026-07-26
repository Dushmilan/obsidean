This is an ungraded practice quiz for Chapter 1 of the Hugging Face LLM Course. It covers pipeline usage, model architectures, transfer learning, and bias — all core concepts from the first chapter.

**The Intuition:** Think of this quiz as a sanity check before moving on. If you can answer these without looking things up, you've got a solid grasp of the fundamentals. If not, it's a sign to revisit the earlier chapters.

**The Math:**

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

**Answers:**
1. Text classification — NLI (contradiction, neutral, entailment)
2. Entities (PER, ORG, LOC) with positions; `grouped_entities=True` groups multi-word entities
3. `"This [MASK] has been waiting for you."` — BERT uses `[MASK]`
4. Missing `candidate_labels` — zero-shot requires label candidates
5. Training on large dataset (pretraining), then fine-tuning on smaller task-specific dataset
6. True — pretraining is self-supervised
7. Architecture = skeleton. Weights = parameters. Model = umbrella term.
8. Decoder-only (GPT-like)
9. Encoder-decoder (T5, BART)
10. Encoder-only (BERT-like)
11. Training data from internet, societal biases, fine-tuning doesn't remove intrinsic bias

**AI Context:** This file is a self-assessment quiz covering all Chapter 1 concepts — pipelines, model architectures, transfer learning, and bias. Use it to verify understanding before proceeding to Chapter 2.

---
