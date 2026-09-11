---
date: 2026-09-01
type: concept
course: HF-LLM-Course
chapter: "01"
section: "1.8 Bias and Limitations"
tags: [hf, llms, bias, ethics]
transformers: ">=4.40.0"
---

LLMs inherit biases from their training data — the entire internet, including its worst parts. Fine-tuning on your data doesn't remove these intrinsic biases, making this a critical concern for any production deployment.

**The Intuition:** Think of an LLM like a mirror reflecting society. If society has gender stereotypes about occupations, the mirror shows them too. BERT, trained on Wikipedia and BookCorpus (not raw internet), still associates "man" with lawyer/doctor and "woman" with nurse/teacher. The bias isn't in the model's design — it's in the data it learned from.

**The Math / Mechanism:**

**Bias Example (BERT fill-mask):**
- "This man works as a [MASK]." → `['lawyer', 'carpenter', 'doctor', 'waiter', 'mechanic']`
- "This woman works as a [MASK]." → `['nurse', 'waitress', 'teacher', 'maid', 'prostitute']`

Only one gender-free answer (waiter/waitress). Data reflection → model replication.

**Key Takeaways:**
- Models can generate sexist, racist, homophobic content
- Fine-tuning on your data doesn't remove intrinsic bias
- Bias comes from training data reflecting societal patterns
- Always be aware when deploying in production

**Mitigation Strategies (not covered in depth):**
- Bias detection and evaluation (e.g., CrowS-Pairs, StereoSet)
- Careful prompt engineering
- Output filtering
- Diverse training data
- Ongoing monitoring in production

**Worked example — reproducing bias:**
```python
from transformers import pipeline
# fill = pipeline("fill-mask", model="bert-base-cased")
# fill("This man works as a [MASK].")
# fill("This woman works as a [MASK].")
```

**Common pitfalls:**
- Assuming fine-tuning on clean data fixes base bias — it doesn't
- Not testing occupation/pronoun swaps before deployment
- Treating bias as model bug vs data property

**Try it / Check yourself:**
- Reproduce the BERT example and swap other attributes (pronouns, nationalities)
- List 2 mitigation checkpoints you'd add to a prod pipeline

**AI Context:** Covers bias root cause, concrete BERT example, why fine-tuning doesn't fix it, high-level mitigations. Essential for responsible deployment; links to [[06-Quick-Quiz]] Q11 and all production chapters.

---
Source: https://huggingface.co/learn/llm-course/chapter1/8 (verified 2026-09-01)
Up: [[HuggingFace-LLM-Course_Index]]
