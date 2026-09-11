---
date: {{date:YYYY-MM-DD}}
type: concept
course: HF-LLM-Course
chapter: "0X"
section: "X.Y Title"
tags: [hf, transformers]
checkpoint: "distilbert-base-uncased-finetuned-sst-2-english"
transformers: ">=4.40.0"
---

<Topic> is <one-sentence conversational definition that answers "what problem does this solve and where in pipeline?">.

**The Intuition:** Think of it like <real-world analogy>. <Extend the analogy to cover the main mechanism — e.g., pipeline = prep station → oven → plating>.

**The Math / Mechanism:**

| Component | Code / Shape | Meaning |
|-----------|--------------|---------|
|           |              |         |

**Worked example — <name>:**
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

# minimal runnable snippet — keep expected output as comment
inputs = tokenizer(["Example sentence"], padding=True, truncation=True, return_tensors="pt")
outputs = model(**inputs)
# → logits shape: [1, 2], hidden shape: [1, seq_len, 768]
```

**Common pitfalls:**
- Forgetting `padding=True` with batches → shape mismatch
- Padding without `attention_mask` → silently wrong logits
- Single sequence without batch dim `tensor(ids)` → `IndexError: Dimension out of range` → use `tensor([ids])` or `return_tensors="pt"`
- Forgetting `truncation=True` → crash at 512/1024 limit

**Try it / Check yourself:**
- Replicate `pipeline()` manually and verify `padding` + `attention_mask` gives same logits as single-sequence inference
- Question for self-check: <e.g., What task does `roberta-large-mnli` perform?>

**AI Context:** This file covers <scope>. Prereq [[prev-note]] → Next [[next-note]]. Connects to [[03-How-Transformers-Work]] / [[Deep-Learning_Index]].

---
Source: https://huggingface.co/learn/llm-course/chapterX/Y (verified YYYY-MM-DD)
Key Paper: ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)
Hub: `huggingface-cli login` → `model.push_to_hub()` / `tokenizer.push_to_hub()` — `config.json` + `model.safetensors`
Up: [[HuggingFace-LLM-Course_Index]]
