The Hugging Face LLM Course is a structured learning path covering NLP and large language models — from the basics of the Transformers library to advanced fine-tuning and reasoning models. It assumes solid Python and recommends an intro deep learning course as a prerequisite.

**The Intuition:** Think of this course like learning to drive. You start with the controls (Transformers pipelines), then learn how the engine works (transformer architecture), then practice on real roads (datasets and tokenizers), and finally tackle advanced maneuvers (fine-tuning, reasoning models). Each chapter builds on the last.

**The Math:**

| Part | Chapters | Focus |
|------|----------|-------|
| 1 | Ch 1–4 | Transformers library basics: how models work, use Hub models, fine-tune, share |
| 2 | Ch 5–8 | Datasets, Tokenizers, classic NLP tasks, LLM techniques |
| 3 | Ch 9 | Building demos (Gradio) |
| 4 | Ch 10–12 | Advanced: fine-tuning, dataset curation, reasoning models |

**Chapter 1 Roadmap:** [[01-NLP-and-LLMs]] → [[02-Transformer-Pipelines]] → [[03-How-Transformers-Work]] → [[04-Transformers-Solve-Tasks]] → [[05-Transformer-Architectures]] → [[06-Quick-Quiz]] → [[07-Inference-with-LLMs]] → [[08-Bias-and-Limitations]]

**Chapter 2 Roadmap:** [[09-Using-Transformers]] — Using Transformers (updated 2026-09-01): Ch2.1 Intro → Ch2.2 Behind the Pipeline (tokenizer → model → softmax) → Ch2.3 Models (`AutoModel`, `config.json` + `model.safetensors`, `push_to_hub`, `token_type_ids`) → Ch2.4 Tokenizers (Word/Char/Subword, BPE/WordPiece/SentencePiece, encode/decode) → Ch2.5 Handling Multiple Sequences (batch dim, `pad_token_id`, `attention_mask`, 512-limit/ Longformer/LED, truncation) → Ch2.6 Putting It All Together (`padding="longest"/"max_length"`, `truncation`, `return_tensors="pt"/"np"`, special tokens `[CLS]/[SEP]`) → Ch2.7 Recap

**Prerequisites:** Good Python knowledge, intro deep learning course recommended (fast.ai or DeepLearning.AI), PyTorch/TensorFlow familiarity helpful but not required.

**AI Context:** This file serves as the entry point for the HuggingFace LLM Course notes. It outlines the course structure, prerequisites, and links to all chapter notes. Use it to navigate the course material or understand the learning progression from NLP basics to advanced LLM techniques.

---
**Key Paper:** ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762) — Vaswani et al., 2017.
**Tools:** [Code Carbon](https://codecarbon.io/) for tracking training CO2, [ML CO2 Impact](https://mlco2.github.io/impact/) for carbon footprint calculation.

---

## 🌐 Main Vault Network

Every subject hub links every other — jump anywhere from here.

| Subject | Hub |
|----------|------|
| 📐 Mathematics | [[Maths]] |
| ⚛️ Physics | [[Physics_Index]] |
| 📊 Statistics | [[Stats_Index]] |
| 💻 Computer Science | [[Computer-Science_Index]] |
| 🤖 CS 188 — AI | [[CS-188_Index]] |
| 🧠 Machine Learning | [[Computer Science/Machine-Learning/Machine-Learning_Index|Machine-Learning_Index]] |
| 🔢 Deep Learning | [[Computer Science/Deep-Learning/Deep-Learning_Index|Deep-Learning_Index]] |
| 🎓 University — Semester 1 | [[Physics/Cross/GP1-Syllabus-Map|GP1 Syllabus Map]] |
| 🛠️ Projects & Ops (OpenCode) | [[OpenCode/INDEX|INDEX]] |

**Master index:** [[Vault-Index]]

---

**Up:** [[Vault-Index]]
