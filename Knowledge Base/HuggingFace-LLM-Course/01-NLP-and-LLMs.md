NLP (Natural Language Processing) is the broader field of enabling computers to understand, interpret, and generate human language. LLMs are the latest advancement within NLP — massive models trained on huge data that perform many tasks with minimal task-specific training.

**The Intuition:** Think of NLP as teaching a foreign language. You start with individual words (token classification), then learn sentences (sentiment analysis), then hold conversations (text generation), and finally translate between languages (seq2seq). LLMs are like a student who learned by reading the entire internet — they're surprisingly good at everything but sometimes confidently wrong (hallucinations).

**The Math:**

**Core NLP Tasks:**

| Task | Example |
|------|---------|
| Sentence classification | Sentiment analysis, spam detection, grammatical correctness |
| Token classification | POS tagging, named entity recognition |
| Text generation | Auto-complete, fill-in-the-blank |
| QA | Extract answer from context |
| Seq2seq | Translation, summarization |

**LLM Characteristics:**
- **Scale** — millions to hundreds of billions of parameters
- **General capabilities** — multiple tasks without task-specific training
- **In-context learning** — learn from examples in the prompt
- **Emergent abilities** — capabilities that appear at scale, not explicitly programmed

**LLM Limitations:** Hallucinations (confident incorrect info), no true understanding (purely statistical patterns), bias (reproduce training data biases), limited context windows, significant compute cost.

**AI Context:** This file covers the foundational concepts of NLP and LLMs. It defines the field, lists core tasks, explains what makes LLMs different, and covers their key limitations. Essential reading before diving into Transformers.

---
Language processing is hard because computers don't process info like humans — ambiguity, cultural context, sarcasm, and humor all pose challenges that LLMs help with but don't fully solve.
