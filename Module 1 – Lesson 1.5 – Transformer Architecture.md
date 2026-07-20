# Module 1 – Large Language Models (LLMs)

# Lesson 1.5 – Transformer Architecture

> **Estimated Study Time:** 6–8 Hours
> **Difficulty:** Intermediate → Advanced
> **Learning Outcome:** Understand the Transformer architecture in detail, including Encoder, Decoder, Self-Attention, Multi-Head Attention, Query-Key-Value (QKV), Residual Connections, Layer Normalization, and why Transformers became the foundation of all modern LLMs.

---

# Table of Contents

1. Introduction
2. Why Transformers Were Needed
3. What is a Transformer?
4. History of Transformers
5. Encoder-Decoder Architecture
6. Core Components
7. Query, Key & Value (QKV)
8. Self-Attention
9. Multi-Head Attention
10. Feed Forward Network
11. Residual Connections
12. Layer Normalization
13. Positional Encoding
14. Training vs Inference
15. Why GPT Uses Only the Decoder
16. Why BERT Uses Only the Encoder
17. Advantages of Transformers
18. Limitations
19. Summary
20. Interview Questions
21. Assignment

---

# 1. Introduction

The **Transformer** is the most influential architecture in modern Artificial Intelligence.

Nearly every state-of-the-art Large Language Model (LLM) is based on it, including:

* GPT
* ChatGPT
* Claude
* Gemini
* Llama
* DeepSeek
* Mistral
* Qwen

Before Transformers, Natural Language Processing (NLP) relied on **RNNs** and **LSTMs**, which processed words one after another. This made training slow and limited their ability to capture long-range relationships.

The Transformer solved these problems by introducing **Self-Attention**, allowing the model to process all tokens simultaneously.

---

# 2. Why Transformers Were Needed

## Problems with RNNs and LSTMs

Consider the sentence:

> **The student who studied throughout the night finally passed the examination because he worked hard.**

To understand **"he"**, the model must remember information from many words earlier.

RNNs process tokens sequentially:

```text
Word 1 → Word 2 → Word 3 → Word 4 → ...
```

This creates several problems:

* Slow training
* Difficulty remembering distant words
* Limited parallel processing
* Vanishing gradients in long sequences

As datasets and models grew larger, these limitations became major bottlenecks.

---

# 3. What is a Transformer?

## Formal Definition

A **Transformer** is a deep learning architecture that processes sequences using **self-attention mechanisms** instead of recurrence, enabling efficient parallel computation and modeling of long-range dependencies.

---

## Simple Definition

A Transformer allows **every word to look at every other word** in a sentence before making predictions.

---

## Engineering Definition

A Transformer is a stack of layers containing:

* Multi-Head Self-Attention
* Feed Forward Networks
* Residual Connections
* Layer Normalization
* Positional Encoding

Together, these components enable highly scalable sequence modeling.

---

# 4. History of Transformers

In 2017, researchers at Google published the paper:

> **Attention Is All You Need**

This paper introduced the Transformer architecture and demonstrated that attention alone could outperform recurrent models on language tasks.

This breakthrough led to:

| Year  | Model                                 |
| ----- | ------------------------------------- |
| 2018  | BERT                                  |
| 2018  | GPT-1                                 |
| 2019  | GPT-2                                 |
| 2020  | GPT-3                                 |
| 2022  | ChatGPT                               |
| 2023+ | GPT-4, Claude, Gemini, Llama, Mistral |

The Transformer rapidly became the standard architecture for NLP and later expanded into vision, audio, robotics, and multimodal AI.

---

# 5. Encoder-Decoder Architecture

The original Transformer consists of two major parts:

```text
Input Text
      │
      ▼
 ┌──────────┐
 │ Encoder  │
 └──────────┘
      │
 Context Representation
      │
      ▼
 ┌──────────┐
 │ Decoder  │
 └──────────┘
      │
      ▼
Output Text
```

### Encoder

The encoder reads and understands the input.

### Decoder

The decoder generates the output one token at a time.

---

# 6. Core Components of a Transformer

Each Transformer layer contains:

```text
Input
  │
  ▼
Positional Encoding
  │
  ▼
Multi-Head Attention
  │
  ▼
Residual Connection
  │
  ▼
Layer Normalization
  │
  ▼
Feed Forward Network
  │
  ▼
Residual Connection
  │
  ▼
Layer Normalization
  │
  ▼
Output
```

These operations are repeated many times.

Modern LLMs may contain dozens or even hundreds of such layers.

---

# 7. Query, Key, and Value (QKV)

Every token is transformed into three vectors:

* **Query (Q):** What information am I looking for?
* **Key (K):** What information do I contain?
* **Value (V):** What information should I contribute?

### Library Analogy

Imagine a library.

* **Query:** The book title you're searching for.
* **Key:** Labels on all books.
* **Value:** The content inside each book.

The model compares Queries with Keys to decide which Values are important.

---

# 8. Self-Attention

Self-Attention is the heart of the Transformer.

Instead of processing words independently, each token examines every other token.

Example:

> **The trophy didn't fit in the suitcase because it was too large.**

What does **"it"** refer to?

Possible references:

* Trophy
* Suitcase

Self-Attention analyzes the entire sentence and determines that **"it"** most likely refers to **"trophy"**.

---

## Simplified Process

```text
Every Token
      │
Looks at
Every Other Token
      │
Assign Importance
      │
Create Better Representation
```

---

# 9. Multi-Head Attention

One attention mechanism may focus on only one type of relationship.

Instead, Transformers use **multiple attention heads**.

Example:

Sentence:

> **The intelligent student solved the difficult mathematics problem quickly.**

Different heads may focus on:

| Head   | Learns                     |
| ------ | -------------------------- |
| Head 1 | Grammar                    |
| Head 2 | Subject-Verb relationships |
| Head 3 | Meaning                    |
| Head 4 | Context                    |
| Head 5 | Long-distance dependencies |

The outputs from all heads are combined.

This allows the model to capture many linguistic relationships simultaneously.

---

# 10. Feed Forward Network (FFN)

After attention, every token passes through a small neural network.

Purpose:

* Learn more complex features
* Increase model capacity
* Refine token representations

The FFN is applied independently to every token.

---

# 11. Residual Connections

As neural networks become deeper, information may degrade.

Residual Connections solve this problem.

Instead of passing only the processed output forward, the original input is added back.

```text
Input
 │
 ▼
Layer
 │
 ▼
Output + Original Input
```

Benefits:

* Easier optimization
* Better gradient flow
* Improved training stability

---

# 12. Layer Normalization

Layer Normalization stabilizes the numerical values flowing through the network.

Benefits:

* Faster convergence
* Stable training
* Reduced numerical instability
* Better generalization

Without normalization, training very deep models becomes difficult.

---

# 13. Positional Encoding

Because Transformers process all tokens in parallel, they need a way to understand word order.

Example:

* Dog bites man
* Man bites dog

Same words.

Different meanings.

Positional Encoding adds information about each token's position.

```text
Position 1 → Dog
Position 2 → bites
Position 3 → man
```

Now the model understands both the token and its location.

---

# 14. Training vs Inference

## Training

During training:

* Large datasets are used.
* The model predicts missing or next tokens.
* Errors are measured.
* Parameters are updated using gradient descent.

Goal:

Learn language patterns.

---

## Inference

Inference is what happens when users interact with the model.

Steps:

* Receive prompt
* Tokenize
* Run through Transformer
* Predict next token
* Generate response

No parameter updates occur during inference.

---

# 15. Why GPT Uses Only the Decoder

GPT (Generative Pre-trained Transformer) is designed for **text generation**.

It predicts the next token repeatedly.

Because generation is its primary task, GPT uses **only the Decoder portion** of the original Transformer.

This design is called a **Decoder-Only Transformer**.

Applications:

* ChatGPT
* Code generation
* Story writing
* Summarization

---

# 16. Why BERT Uses Only the Encoder

BERT focuses on **understanding** text rather than generating it.

It analyzes the entire input simultaneously.

Therefore, BERT uses **only the Encoder portion**.

Applications:

* Search
* Classification
* Sentiment Analysis
* Named Entity Recognition
* Question Answering

---

# 17. Advantages of Transformers

* Parallel processing
* Excellent long-range dependency handling
* Highly scalable
* Superior language understanding
* Foundation of modern LLMs
* Supports multimodal learning
* Efficient training on large datasets

---

# 18. Limitations

Despite their strengths, Transformers have challenges:

### High Computational Cost

Attention calculations become expensive as sequence length increases.

### Large Memory Requirements

Training and inference require significant GPU memory.

### Energy Consumption

Large-scale training consumes substantial electricity.

### Context Window Limits

Although much larger than earlier models, context windows remain finite.

### Hallucinations

Transformers predict likely tokens—they do not guarantee factual correctness.

---

# 19. Summary

The Transformer architecture revolutionized AI by replacing sequential processing with **Self-Attention**, enabling efficient parallel computation and superior handling of long-range dependencies.

Its main components include:

* Positional Encoding
* Query, Key, Value (QKV)
* Self-Attention
* Multi-Head Attention
* Feed Forward Networks
* Residual Connections
* Layer Normalization

These components work together to build increasingly rich representations of language, making Transformers the foundation of today's Large Language Models.

---

# 20. Interview Questions

## Beginner

1. What is a Transformer?
2. Why were Transformers introduced?
3. What is Self-Attention?
4. What is the role of Positional Encoding?

## Intermediate

5. Explain Query, Key, and Value (QKV).
6. What is Multi-Head Attention, and why is it useful?
7. Compare the Encoder and Decoder.
8. What are Residual Connections?

## Advanced

9. Why does GPT use only the Decoder while BERT uses only the Encoder?
10. Explain the computational trade-offs of Self-Attention.
11. Why do Transformers scale better than RNNs?

---

# 21. Assignment

## Theory

1. Explain the complete Transformer architecture in your own words.
2. Compare RNNs/LSTMs with Transformers.
3. Describe the role of Self-Attention, Multi-Head Attention, and QKV.
4. Explain why Residual Connections and Layer Normalization are essential.

## Practical Thinking Exercise

Suppose you are designing an AI-powered document summarizer.

For each Transformer component below, explain its role:

* Tokenization
* Positional Encoding
* Self-Attention
* Multi-Head Attention
* Feed Forward Network
* Residual Connection
* Layer Normalization
* Decoder

Finally, discuss why a Transformer-based model would be more suitable than an RNN-based model for summarizing long technical documents.

---

# Key Takeaways

* The **Transformer** is the core architecture behind modern LLMs.
* **Self-Attention** enables every token to consider every other token, capturing rich contextual relationships.
* **QKV (Query, Key, Value)** forms the mathematical basis of attention.
* **Multi-Head Attention** allows the model to learn different types of linguistic relationships simultaneously.
* **Residual Connections** and **Layer Normalization** make deep networks train efficiently and remain stable.
* **GPT** uses a **Decoder-only** architecture for generation, while **BERT** uses an **Encoder-only** architecture for understanding tasks.
* The next lesson, **Lesson 1.6 – Tokenization**, will explore how text is converted into tokens, compare algorithms such as BPE, WordPiece, and SentencePiece, and explain why tokenization is fundamental to LLM performance, cost, and context management.
