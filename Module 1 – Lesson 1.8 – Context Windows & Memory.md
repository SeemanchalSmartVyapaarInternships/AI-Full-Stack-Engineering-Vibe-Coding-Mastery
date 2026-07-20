# Module 1 – Large Language Models (LLMs)

# Lesson 1.8 – Context Windows & Memory

> **Estimated Study Time:** 7–8 Hours
> **Difficulty:** Intermediate → Advanced
> **Learning Outcome:** Understand how LLMs remember information during a conversation, what a context window is, why models forget information, how KV Cache works, and how production AI systems achieve long-term memory using RAG, vector databases, and memory architectures.

---

# Table of Contents

1. Introduction
2. Human Memory vs AI Memory
3. What is a Context Window?
4. Why is Context Important?
5. How Context Windows Work
6. Token-Based Memory
7. Context Window Sizes
8. Why LLMs Forget
9. Context Overflow
10. KV Cache
11. Prompt Caching
12. Sliding Window
13. Context Compression
14. Long-Term Memory
15. Retrieval-Augmented Memory (RAG)
16. AI Agent Memory Architecture
17. Best Practices
18. Limitations
19. Summary
20. Interview Questions
21. Assignment

---

# 1. Introduction

One of the biggest misconceptions about Large Language Models is:

> **"ChatGPT remembers everything."**

This is **false**.

An LLM **does not permanently remember your conversation** in the way humans remember experiences.

Instead, it remembers only the information that is present inside its **Context Window**.

Everything outside that window is invisible unless it is reintroduced.

Understanding context windows is one of the most important concepts in AI Engineering because it directly affects:

* Prompt Engineering
* AI Agents
* RAG Systems
* Long conversations
* Code generation
* Document analysis
* Multi-step reasoning

---

# 2. Human Memory vs AI Memory

Let's compare human memory with AI memory.

| Human Brain                       | LLM                                      |
| --------------------------------- | ---------------------------------------- |
| Long-term memory                  | No permanent memory during inference     |
| Short-term memory                 | Context Window                           |
| Experience                        | Training Data                            |
| Learning after every conversation | No (unless retrained or fine-tuned)      |
| Can remember childhood            | Cannot remember anything outside context |

---

### Human Example

Suppose someone tells you:

> "My name is Rahul."

Two hours later you still remember.

---

### AI Example

If the sentence

> "My name is Rahul."

is no longer inside the context window,

the model behaves as if it never saw it.

---

# 3. What is a Context Window?

## Formal Definition

A **Context Window** is the maximum number of **tokens** an LLM can process simultaneously during inference.

---

## Simple Definition

The Context Window is the model's **working memory**.

---

## Engineering Definition

The context window defines the maximum token sequence available to the Transformer for attention computation.

Only tokens inside this window participate in Self-Attention.

---

# 4. Why is Context Important?

Consider this conversation.

```
User:
My name is Amit.

Assistant:
Nice to meet you.

User:
What's my name?
```

If "My name is Amit" is still inside the context window,

the model answers:

> Amit

If that earlier message has fallen outside the context window,

the model no longer has access to it.

---

# 5. How Context Windows Work

Suppose a model has an **8-token** context window.

Conversation:

```text
I
love
Artificial
Intelligence
and
Machine
Learning
today
```

The window is now full.

If another token arrives:

```text
very
```

The oldest token is removed.

```text
love
Artificial
Intelligence
and
Machine
Learning
today
very
```

The word **I** is no longer available.

---

# 6. Token-Based Memory

Important:

LLMs measure memory using **tokens**, not words.

Example:

```
Hello world
```

may become

```
3 Tokens
```

A 100-page document might contain:

* 40,000 words
* 55,000 tokens

The model's memory usage depends entirely on token count.

---

# 7. Context Window Sizes

Different models support different context lengths.

| Model Category      |                                             Typical Context Window |
| ------------------- | -----------------------------------------------------------------: |
| Early GPT models    |                                                Few thousand tokens |
| Modern LLMs         |                                        Tens of thousands of tokens |
| Long-context models | Hundreds of thousands to over one million tokens (model-dependent) |

Larger context windows allow:

* Longer conversations
* Larger documents
* More code
* Better reasoning across extended inputs

However, larger context windows also increase computational cost.

---

# 8. Why LLMs Forget

An LLM forgets for several reasons.

---

## Context Overflow

Too many tokens.

Old information gets removed.

---

## Prompt Design

Important information appears too early and later falls outside the window.

---

## Attention Dilution

Even before reaching the limit, very long contexts can make it harder for the model to consistently focus on the most relevant information.

---

## No Persistent Learning

The model does not update its parameters during inference.

Every conversation starts fresh unless external memory is provided.

---

# 9. Context Overflow

Imagine:

```
Book

↓

1000 pages

↓

Context Window

↓

200 pages
```

The model cannot see all 1000 pages simultaneously.

Only the pages currently inside the window are available.

Everything else must be:

* Retrieved later
* Summarized
* Chunked

---

# 10. KV Cache (Key-Value Cache)

One of the most important inference optimizations.

---

## The Problem

Suppose the model generated:

```
Hello
```

Next token:

```
everyone
```

Without optimization, the model would recompute attention over:

```
Hello
```

again.

This repeats for every generated token.

---

## KV Cache Solution

Instead of recomputing previous attention information, the model stores previously computed **Keys** and **Values**.

Workflow:

```text
Prompt

↓

Compute Keys & Values

↓

Store in KV Cache

↓

Reuse

↓

Generate Faster
```

---

## Benefits

* Lower latency
* Faster generation
* Reduced computation
* Better efficiency

KV Cache is one reason chat applications feel responsive during generation.

---

# 11. Prompt Caching

Prompt caching is different from KV Cache.

Suppose many users send:

```
Company Policy Document

+

Different Questions
```

Instead of processing the policy repeatedly,

the system may cache reusable computations for the shared prompt (where supported by the serving infrastructure).

Benefits:

* Lower cost
* Faster inference
* Reduced latency

---

# 12. Sliding Window

Instead of storing everything,

some systems use a **Sliding Window**.

Example:

```
Conversation

↓

Last 100 messages

↓

Keep

↓

Older Messages

↓

Discard
```

Advantages:

* Constant memory usage
* Efficient processing
* Suitable for long-running chats

Trade-off:

Older context is lost unless summarized or retrieved.

---

# 13. Context Compression

Instead of keeping every message,

AI systems can summarize older content.

Example:

Original:

```
50 pages
```

↓

Summary:

```
2 pages
```

The summary replaces the original context, preserving important information while reducing token usage.

Benefits:

* Longer effective conversations
* Lower cost
* Better context utilization

---

# 14. Long-Term Memory

LLMs themselves do **not** have persistent long-term memory during inference.

Production AI systems add it externally.

Typical architecture:

```text
Conversation

↓

Embedding

↓

Vector Database

↓

Retrieve Later

↓

Insert Into Prompt
```

The model appears to "remember," but the memory actually comes from an external system.

---

# 15. Retrieval-Augmented Memory (RAG)

This is how most production AI assistants remember information.

Workflow:

```text
User Question

↓

Embedding

↓

Vector Search

↓

Relevant Documents

↓

Context Window

↓

LLM

↓

Answer
```

The retrieved information is inserted into the prompt before inference.

Benefits:

* Up-to-date information
* Domain-specific knowledge
* Scalable memory
* Reduced hallucinations

---

# 16. AI Agent Memory Architecture

Modern AI agents typically combine multiple memory types.

```text
                  AI Agent
                     │
      ┌──────────────┼──────────────┐
      │              │              │
      ▼              ▼              ▼
 Short-Term      Long-Term      External Tools
(Context Window) (Vector DB)    (APIs, Files, DBs)
      │              │              │
      └──────────────┼──────────────┘
                     ▼
              Final Prompt
                     │
                     ▼
                    LLM
```

This layered approach allows agents to behave as though they have much richer memory than the underlying model alone.

---

# 17. Best Practices

* Keep prompts focused and relevant.
* Place critical instructions near the beginning or restate them when necessary.
* Use document chunking for large inputs.
* Summarize older conversation history.
* Use RAG for persistent knowledge.
* Monitor token usage in production systems.
* Choose a model with an appropriate context window for your application.

---

# 18. Limitations

### Finite Context

Every model has a maximum context size.

---

### Cost

Longer contexts increase inference cost.

---

### Latency

Processing more tokens generally increases response time.

---

### Memory Is Not Learning

Adding information to the context window does not permanently teach the model.

---

### Retrieval Quality Matters

Poor retrieval or irrelevant documents can reduce answer quality even if the model itself is strong.

---

# 19. Summary

The **Context Window** is the LLM's temporary working memory. It determines how many tokens the model can consider at once during inference.

When the context window fills up, older information may be removed or summarized. Techniques such as **KV Cache**, **prompt caching**, **sliding windows**, and **context compression** improve efficiency, while **Retrieval-Augmented Generation (RAG)** and **vector databases** provide persistent external memory.

Understanding these concepts is essential for designing scalable chatbots, AI agents, coding assistants, and enterprise AI systems.

---

# 20. Interview Questions

## Beginner

1. What is a context window?
2. Why do LLMs forget earlier information?
3. What is the difference between tokens and words?
4. Why is context measured in tokens?

---

## Intermediate

5. Explain KV Cache.
6. Compare prompt caching and KV Cache.
7. What is a sliding window?
8. Why is context compression useful?

---

## Advanced

9. Design a memory architecture for an enterprise AI assistant.
10. Explain how RAG provides long-term memory without changing model parameters.
11. Discuss the trade-offs between increasing context window size and using retrieval.

---

# 21. Assignment

## Theory

1. Define a context window and explain its importance.
2. Compare human memory with LLM memory.
3. Explain how KV Cache improves inference efficiency.
4. Describe how RAG enables long-term memory.

---

## Practical Thinking Exercise

Imagine you are building an AI assistant for a software company.

Design a memory architecture that includes:

* Short-term conversation memory
* Long-term project knowledge
* Document retrieval
* Codebase search
* User preferences
* Conversation summaries

Explain how each component contributes to producing accurate, context-aware responses.

---

# Key Takeaways

* A **Context Window** is the temporary working memory of an LLM, measured in **tokens**.
* LLMs do not permanently remember conversations; they only process the tokens currently in context.
* **KV Cache** speeds up token generation by reusing previously computed attention information.
* **Prompt caching** reuses computation for repeated prompt prefixes in supported serving systems.
* **Sliding windows** and **context compression** help manage long conversations efficiently.
* **RAG** and **vector databases** provide practical long-term memory for production AI applications.
* Modern AI agents combine **short-term context**, **external memory**, and **tools** to deliver robust, scalable intelligence.

---

## 📖 Next Lesson (1.9)

**Reasoning in Large Language Models**

In the next lesson, we'll explore one of the most misunderstood topics in AI:

* What is reasoning?
* Do LLMs actually "think"?
* Pattern matching vs. reasoning
* Chain of Thought (CoT)
* Self-consistency
* Tree of Thoughts (ToT)
* ReAct (Reason + Act)
* Planning and tool use
* Reasoning models vs. standard chat models
* Practical engineering strategies for building reliable reasoning systems

This lesson will connect the internals of LLMs to the advanced prompting and agent techniques used in modern AI engineering.
