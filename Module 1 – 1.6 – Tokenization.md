# Module 1 – Large Language Models (LLMs)

# Lesson 1.6 – Tokenization

> **Estimated Study Time:** 5–6 Hours
> **Difficulty:** Intermediate
> **Learning Outcome:** Understand what tokenization is, why it is essential for Large Language Models (LLMs), how different tokenization algorithms work, and how tokenization affects model performance, cost, context window, and prompting.

---

# Table of Contents

1. Introduction
2. What is Tokenization?
3. Why Do LLMs Need Tokenization?
4. Characters vs Words vs Tokens
5. Tokenization Workflow
6. Types of Tokenization
7. Tokenization Algorithms
8. Special Tokens
9. Vocabulary
10. Token IDs
11. Context Window
12. Tokenization Examples
13. Why Tokenization Matters
14. Best Practices
15. Limitations
16. Summary
17. Interview Questions
18. Assignment

---

# 1. Introduction

When humans read a sentence, we naturally understand words, grammar, and meaning.

Example:

> Artificial Intelligence is transforming the world.

Humans immediately recognize:

* Artificial
* Intelligence
* is
* transforming
* the
* world

A computer cannot.

For a computer, text is initially just a sequence of characters.

Before an AI model can understand language, it must convert text into a machine-readable format.

This process is called **Tokenization**.

Without tokenization, modern LLMs like ChatGPT, Claude, Gemini, and Llama cannot process text.

---

# 2. What is Tokenization?

## Formal Definition

**Tokenization** is the process of dividing text into smaller units called **tokens**, which can then be converted into numerical representations for processing by a machine learning model.

---

## Simple Definition

Tokenization means **breaking text into small pieces** that an AI model can understand.

---

## Engineering Definition

Tokenization transforms raw text into a sequence of standardized tokens that are mapped to unique numerical IDs, enabling efficient processing by Transformer-based language models.

---

# 3. Why Do LLMs Need Tokenization?

Computers do not understand:

* Letters
* Words
* Sentences

They understand only numbers.

Therefore, text must pass through three stages:

```text
Text
   ↓
Tokens
   ↓
Token IDs
   ↓
Embeddings
```

Without tokens:

* No embeddings
* No attention
* No Transformer processing
* No text generation

Tokenization is the first computational step in every LLM.

---

# 4. Characters vs Words vs Tokens

Consider the sentence:

```text
Artificial Intelligence is amazing.
```

### Character Level

```text
A r t i f i c i a l
```

Every character is treated separately.

---

### Word Level

```text
Artificial
Intelligence
is
amazing
```

Each word becomes one unit.

---

### Token Level

An LLM may split it as:

```text
Artificial
Intelligence
is
amaz
ing
.
```

Notice:

A token is **not always a complete word**.

It can be:

* One character
* One word
* Part of a word
* Punctuation
* Whitespace

---

# 5. Tokenization Workflow

The complete workflow looks like this:

```text
User Input
      │
      ▼
Tokenizer
      │
      ▼
Tokens
      │
      ▼
Token IDs
      │
      ▼
Embeddings
      │
      ▼
Transformer
```

The tokenizer acts as the bridge between human language and machine computation.

---

# 6. Types of Tokenization

## 6.1 Character Tokenization

Each character becomes a token.

Example:

```text
CAT
```

↓

```text
C
A
T
```

### Advantages

* Small vocabulary
* Handles unknown words

### Disadvantages

* Very long sequences
* Slow processing
* Poor semantic understanding

---

## 6.2 Word Tokenization

Each word becomes one token.

Example:

```text
Artificial Intelligence
```

↓

```text
Artificial
Intelligence
```

### Advantages

* Easy to understand
* Shorter sequences

### Disadvantages

* Huge vocabulary
* Unknown words become problematic

---

## 6.3 Subword Tokenization

Modern LLMs mostly use **subword tokenization**.

Example:

```text
unbelievable
```

↓

```text
un
believ
able
```

Advantages:

* Handles unknown words
* Smaller vocabulary
* Better efficiency
* Better multilingual support

Most modern LLMs use this approach.

---

# 7. Tokenization Algorithms

Modern tokenizers use specialized algorithms.

---

## Byte Pair Encoding (BPE)

Used by:

* GPT
* OpenAI models

### Working

1. Start with characters.
2. Find the most common adjacent pair.
3. Merge the pair.
4. Repeat until the vocabulary reaches the desired size.

Example:

```text
l o w

↓

lo w

↓

low
```

### Advantages

* Efficient
* Small vocabulary
* Handles unknown words

---

## WordPiece

Used by:

* BERT

Instead of merging frequent pairs, WordPiece chooses merges that maximize language modeling performance.

Advantages:

* Better handling of rare words
* Good language understanding

---

## SentencePiece

Used by:

* T5
* Llama
* Many multilingual models

Unlike BPE, SentencePiece works directly on raw text without requiring spaces between words.

Benefits:

* Language-independent
* Excellent for multilingual AI

---

# 8. Special Tokens

LLMs use reserved tokens with predefined meanings.

Examples:

| Token   | Purpose                     |
| ------- | --------------------------- |
| `<BOS>` | Beginning of sequence       |
| `<EOS>` | End of sequence             |
| `<PAD>` | Padding                     |
| `<UNK>` | Unknown token               |
| `<CLS>` | Classification token (BERT) |
| `<SEP>` | Separator token             |

These tokens help models understand sequence boundaries and special tasks.

---

# 9. Vocabulary

Every tokenizer has a **Vocabulary**.

Vocabulary = All tokens the model knows.

Example:

```text
Vocabulary

↓

Artificial
Intelligence
Machine
Learning
Code
Python
JavaScript
...
```

Modern LLMs typically have vocabularies containing tens of thousands to hundreds of thousands of tokens.

---

# 10. Token IDs

Each token receives a unique integer.

Example:

| Token        |   ID |
| ------------ | ---: |
| Artificial   | 4189 |
| Intelligence | 7345 |
| Python       | 2918 |
| JavaScript   | 5820 |

The model processes IDs rather than text.

---

# 11. Context Window

LLMs have a maximum number of tokens they can process at one time.

Example:

```text
Model

↓

Context Window

↓

8192 Tokens
```

If the prompt exceeds this limit:

* Older tokens may be truncated.
* Information can be lost.
* The model may no longer "remember" earlier parts of the conversation.

Token count—not character count—determines context usage.

---

# 12. Tokenization Examples

### Example 1

Input:

```text
Machine Learning
```

Possible Tokens:

```text
Machine
Learning
```

---

### Example 2

Input:

```text
internationalization
```

Possible Tokens:

```text
inter
national
ization
```

---

### Example 3

Input:

```text
ChatGPT
```

Possible Tokens:

```text
Chat
GPT
```

---

### Example 4

Input:

```text
JavaScriptDeveloper
```

Possible Tokens:

```text
Java
Script
Developer
```

Different models may tokenize the same text differently because they use different vocabularies and algorithms.

---

# 13. Why Tokenization Matters

Tokenization affects several important aspects of LLM performance.

## Cost

Many AI APIs charge **per token**.

More tokens → Higher cost.

---

## Speed

Fewer tokens generally lead to faster inference.

---

## Context Usage

Efficient tokenization allows more useful information to fit within the context window.

---

## Accuracy

Good tokenization preserves meaningful language units, improving model performance.

---

## Multilingual Support

Subword algorithms allow models to process many languages effectively without enormous vocabularies.

---

# 14. Best Practices

* Write prompts clearly and concisely.
* Avoid unnecessary repetition.
* Be aware that long prompts consume context.
* Use structured formatting when appropriate.
* Consider token limits when designing AI applications.
* For API usage, monitor token consumption to control cost.

---

# 15. Limitations

Tokenization is powerful but not perfect.

### Different Models Use Different Tokenizers

The same sentence may produce different token counts across models.

---

### Token Count ≠ Word Count

One word may become multiple tokens.

Conversely, one token may represent multiple characters or an entire word.

---

### Vocabulary Constraints

Very rare or newly coined words may be split into many subword tokens.

---

### Context Limits

Long prompts can exceed the model's maximum context window, requiring truncation or chunking.

---

# 16. Summary

Tokenization is the essential first step in every LLM pipeline. It converts human-readable text into machine-readable tokens that can be transformed into embeddings and processed by the Transformer architecture.

Modern LLMs primarily use **subword tokenization**, balancing vocabulary size, efficiency, and multilingual support. Algorithms such as **Byte Pair Encoding (BPE)**, **WordPiece**, and **SentencePiece** enable models to handle rare words and large vocabularies effectively.

Understanding tokenization is critical because it influences **model accuracy, inference speed, API cost, and context window utilization**.

---

# 17. Interview Questions

## Beginner

1. What is tokenization?
2. Why do LLMs require tokenization?
3. What is the difference between a word and a token?
4. What is a vocabulary?

## Intermediate

5. Compare character-, word-, and subword-level tokenization.
6. Explain Byte Pair Encoding (BPE).
7. What are special tokens?
8. Why does token count matter more than word count in LLMs?

## Advanced

9. Compare BPE, WordPiece, and SentencePiece.
10. How does tokenization influence API pricing and latency?
11. Why is subword tokenization preferred in modern LLMs?

---

# 18. Assignment

## Theory

1. Define tokenization and explain its role in an LLM.
2. Compare character, word, and subword tokenization with examples.
3. Explain how BPE builds a vocabulary.
4. Discuss how tokenization affects cost, speed, and context windows.

## Practical Thinking Exercise

Take the following sentence:

> **"Artificial Intelligence is transforming software engineering."**

For this sentence:

1. Estimate possible word-level tokens.
2. Suggest a possible subword tokenization.
3. Explain why an LLM would convert these tokens into IDs.
4. Describe how these token IDs eventually become embeddings and are processed by the Transformer.

---

# Key Takeaways

* **Tokenization** converts raw text into tokens that an LLM can process.
* Modern LLMs primarily use **subword tokenization** because it balances efficiency and flexibility.
* Common algorithms include **BPE**, **WordPiece**, and **SentencePiece**.
* Each token maps to a **unique token ID**, which is then converted into an **embedding**.
* Tokenization directly affects **cost, performance, context windows, and model accuracy**.
* Understanding tokenization is essential before learning **embeddings**, **attention**, and **context engineering**.

---

## 📖 What's Next?

**Lesson 1.7 – Embeddings & Vector Representations**

In the next lesson, we'll answer questions such as:

* What is an embedding?
* How do computers understand the meaning of words?
* What are vectors and vector spaces?
* Why are "King" and "Queen" mathematically similar?
* How do embeddings capture semantic relationships?
* What is cosine similarity?
* How are embeddings used in search, Retrieval-Augmented Generation (RAG), recommendations, and AI agents?

This lesson will bridge the gap between **tokenization** and the **attention mechanism**, giving you a deeper understanding of how LLMs represent language internally.
