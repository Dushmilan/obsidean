LLMs inherit biases from their training data — the entire internet, including its worst parts. Fine-tuning on your data doesn't remove these intrinsic biases, making this a critical concern for any production deployment.

**The Intuition:** Think of an LLM like a mirror reflecting society. If society has gender stereotypes about occupations, the mirror shows them too. BERT, trained on Wikipedia and BookCorpus (not raw internet), still associates "man" with lawyer/doctor and "woman" with nurse/teacher. The bias isn't in the model's design — it's in the data it learned from.

**The Math:**

**Bias Example (BERT fill-mask):**
- "This man works as a [MASK]." → `['lawyer', 'carpenter', 'doctor', 'waiter', 'mechanic']`
- "This woman works as a [MASK]." → `['nurse', 'waitress', 'teacher', 'maid', 'prostitute']`

Only one gender-free answer (waiter/waitress).

**Key Takeaways:**
- Models can generate sexist, racist, homophobic content
- Fine-tuning on your data doesn't remove intrinsic bias
- Bias comes from training data reflecting societal patterns
- Always be aware of this when deploying in production

**Mitigation Strategies (not covered in depth):**
- Bias detection and evaluation
- Careful prompt engineering
- Output filtering
- Diverse training data
- Ongoing monitoring in production

**AI Context:** This file covers bias and limitations in LLMs — the root cause (training data), a concrete example with BERT, why fine-tuning doesn't fix it, and high-level mitigation strategies. Essential reading for responsible AI deployment.

---
