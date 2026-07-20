# Module 1 – Large Language Models (LLMs)

# Lesson 1.4 – How Large Language Models Work Internally

> **Estimated Study Time:** 6–8 Hours
> **Difficulty:** Intermediate
> **Learning Outcome:** Understand the complete internal working of a Large Language Model (LLM), from user input to generated response. By the end of this lesson, you will know how text is transformed into tokens, processed through Transformer layers, and converted back into meaningful output.

---

# Table of Contents

1. Introduction
2. High-Level Workflow
3. Step 1 – Receiving User Input
4. Step 2 – Tokenization
5. Step 3 – Token IDs
6. Step 4 – Embeddings
7. Step 5 – Positional Encoding
8. Step 6 – Transformer Layers
9. Step 7 – Self-Attention
10. Step 8 – Feed Forward Network
11. Step 9 – Logits
12. Step 10 – Softmax
13. Step 11 – Sampling
14. Step 12 – Next Token Prediction
15. Step 13 – Response Generation
16. Example Walkthrough
17. Performance Considerations
18. Limitations
19. Summary
20. Interview Questions
21. Assignment

---

# 1. Introduction

When you type a prompt such as:

> Explain Artificial Intelligence.

it appears that the AI instantly understands the question and generates an answer.

In reality, the model **never sees letters or words** the way humans do.

Instead, it performs a sequence of mathematical operations.

The complete pipeline is:

```text
User Prompt
      │
      ▼
Tokenization
      │
      ▼
Token IDs
      │
      ▼
Embeddings
      │
      ▼
Positional Encoding
      │
      ▼
Transformer Layers
      │
      ▼
Self-Attention
      │
      ▼
Feed Forward Networks
      │
      ▼
Logits
      │
      ▼
Softmax
      │
      ▼
Token Selection
      │
      ▼
Generated Response
```

Everything inside the model is ultimately represented as **numbers**.

---

# 2. High-Level Workflow

The internal workflow of an LLM consists of the following stages:

| Step | Process                             |
| ---- | ----------------------------------- |
| 1    | User enters text                    |
| 2    | Text is tokenized                   |
| 3    | Tokens become token IDs             |
| 4    | Token IDs become embeddings         |
| 5    | Position information is added       |
| 6    | Transformer processes all tokens    |
| 7    | Attention finds relationships       |
| 8    | Feed Forward layers refine features |
| 9    | Model calculates probabilities      |
| 10   | Next token is selected              |
| 11   | Process repeats until completion    |

---

# 3. Step 1 – Receiving User Input

Suppose the user enters:

```text
What is Artificial Intelligence?
```

To humans, this is a sentence.

To the computer, it is simply a sequence of characters.

Before any intelligence can occur, the text must be converted into a machine-readable form.

---

# 4. Step 2 – Tokenization

The first operation is **Tokenization**.

A tokenizer breaks the input into smaller units called **tokens**.

Example:

```text
What
is
Artificial
Intelligence
?
```

Sometimes tokens are entire words.

Sometimes they are parts of words.

Example:

```text
Artificial
Intelli
gence
```

Different models use different tokenization algorithms, but the objective is the same: represent text efficiently.

---

# 5. Step 3 – Token IDs

Computers cannot process text directly.

Each token is mapped to a unique integer.

Example:

| Token        | Token ID |
| ------------ | -------: |
| What         |      125 |
| is           |       42 |
| Artificial   |     4189 |
| Intelligence |     7345 |
| ?            |       31 |

Now the sentence becomes:

```text
125
42
4189
7345
31
```

The model works with these IDs, not the original text.

---

# 6. Step 4 – Embeddings

Token IDs are still just numbers.

They do not convey meaning.

Each token ID is converted into a dense numerical vector called an **embedding**.

Example (simplified):

| Token | Embedding (3D example) |
| ----- | ---------------------- |
| King  | [0.82, 0.11, -0.43]    |
| Queen | [0.80, 0.14, -0.39]    |
| Car   | [-0.12, 0.91, 0.55]    |

Real embeddings often contain hundreds or thousands of dimensions.

Words with similar meanings have embeddings that are close together in vector space.

---

# 7. Step 5 – Positional Encoding

Transformers process tokens in parallel.

Without additional information, the model would not know the order of words.

Example:

```text
Dog bites man

Man bites dog
```

Both contain the same words, but the meanings are completely different.

To preserve word order, the model adds **positional information** to each embedding.

Example:

| Position | Token |
| -------- | ----- |
| 1        | Dog   |
| 2        | bites |
| 3        | man   |

Now the model knows both **what** the token is and **where** it appears.

---

# 8. Step 6 – Transformer Layers

After embeddings are prepared, they pass through multiple Transformer layers.

Each layer performs two main operations:

1. Self-Attention
2. Feed Forward Network

Modern LLMs may contain dozens or even hundreds of Transformer layers.

Each layer progressively builds a richer understanding of the input.

---

# 9. Step 7 – Self-Attention

Self-Attention is the most important innovation in modern LLMs.

It allows every token to examine every other token in the input.

Example:

```text
The animal didn't cross the street because it was tired.
```

What does **"it"** refer to?

The model uses attention to determine that **"it"** most likely refers to **"animal"**, not **"street."**

---

## Why Self-Attention Matters

Without self-attention:

* Long sentences become difficult to understand.
* Important context may be lost.

With self-attention:

* Long-range relationships are captured.
* Dependencies across the sentence are preserved.
* Context becomes much richer.

---

## Simplified Attention Flow

```text
Every Token
      │
      ▼
Looks at Every Other Token
      │
      ▼
Assigns Importance Scores
      │
      ▼
Builds Context-Aware Representation
```

---

# 10. Step 8 – Feed Forward Network (FFN)

After attention, each token passes through a Feed Forward Network.

The FFN is a small neural network applied independently to every token.

Its purpose is to:

* Learn more complex patterns.
* Refine token representations.
* Increase model capacity.

Every Transformer block alternates between:

```text
Attention

↓

Feed Forward

↓

Attention

↓

Feed Forward
```

This repeated processing enables increasingly sophisticated representations.

---

# 11. Step 9 – Logits

After all Transformer layers, the model predicts the next token.

It first produces **logits**, which are raw numerical scores for every token in the vocabulary.

Example:

| Token  | Logit |
| ------ | ----: |
| Paris  |   8.7 |
| London |   4.2 |
| Rome   |   3.5 |
| Berlin |   2.8 |

Logits are not probabilities yet.

They simply indicate relative preference.

---

# 12. Step 10 – Softmax

The Softmax function converts logits into probabilities that sum to 100%.

Example:

| Token  | Probability |
| ------ | ----------: |
| Paris  |         96% |
| London |          2% |
| Rome   |          1% |
| Berlin |          1% |

Now the model knows which token is most likely.

---

# 13. Step 11 – Sampling

The highest probability token is not always selected.

Different decoding strategies produce different behaviors.

## Greedy Decoding

Always choose the highest-probability token.

**Advantage:** Predictable.

**Disadvantage:** Can produce repetitive or less creative output.

---

## Temperature Sampling

Controls randomness.

* Low temperature → More deterministic.
* High temperature → More diverse and creative.

---

## Top-k Sampling

Choose only from the top **k** most probable tokens.

Example:

If k = 5, the model considers only the five highest-probability candidates.

---

## Top-p (Nucleus Sampling)

Instead of a fixed number of tokens, choose from the smallest set whose cumulative probability exceeds a threshold (e.g., 90%).

This often produces more natural text than Top-k alone.

---

# 14. Step 12 – Next Token Prediction

Suppose the prompt is:

```text
The capital of France is
```

The model predicts:

```text
Paris
```

The sentence becomes:

```text
The capital of France is Paris
```

The updated sentence is then fed back into the model to predict the next token.

This loop repeats until the response is complete or a stopping condition is reached.

---

# 15. Step 13 – Response Generation

The model continues predicting one token at a time:

```text
The

↓

capital

↓

of

↓

France

↓

is

↓

Paris

↓

.
```

Although generated sequentially, this process is optimized to be extremely fast on modern hardware.

---

# 16. Example Walkthrough

Prompt:

```text
What is AI?
```

Pipeline:

```text
Prompt

↓

Tokenizer

↓

Token IDs

↓

Embeddings

↓

Position Encoding

↓

Transformer Layers

↓

Attention

↓

Feed Forward

↓

Logits

↓

Softmax

↓

Next Token

↓

Repeat

↓

Final Answer
```

This same workflow applies whether the task is writing code, summarizing a document, translating text, or answering questions.

---

# 17. Performance Considerations

Several factors affect LLM performance:

### Model Size

More parameters generally improve capability but require more memory and computation.

### Context Window

Longer context windows allow the model to process more information at once.

### Hardware

GPUs and TPUs accelerate the large matrix operations required by Transformer models.

### Quantization

Reducing numerical precision (e.g., 16-bit to 8-bit or 4-bit) can reduce memory usage and improve inference speed, often with only a modest impact on quality.

### Caching

Inference systems often reuse previously computed internal states to generate subsequent tokens more efficiently.

---

# 18. Limitations

Even with sophisticated internal processing, LLMs have limitations:

* They predict statistically likely tokens rather than "understanding" in a human sense.
* They can hallucinate incorrect information.
* Performance depends heavily on prompt quality and available context.
* They may struggle with very long inputs beyond the context window.
* Inference requires significant computational resources.

Understanding these limitations helps engineers design systems that combine LLMs with retrieval, tools, validation, and human oversight.

---

# 19. Summary

Large Language Models operate through a carefully designed pipeline:

1. Text is converted into **tokens**.
2. Tokens become **token IDs**.
3. Token IDs are transformed into **embeddings**.
4. **Positional encoding** preserves word order.
5. Multiple **Transformer layers** process the sequence.
6. **Self-attention** models relationships between tokens.
7. **Feed Forward Networks** refine representations.
8. **Logits** assign scores to possible next tokens.
9. **Softmax** converts scores into probabilities.
10. A **sampling strategy** selects the next token.
11. The process repeats until the complete response is generated.

This pipeline enables LLMs to perform a wide variety of language tasks using the same underlying architecture.

---

# 20. Interview Questions

## Beginner

1. What is the first step after an LLM receives user input?
2. Why are tokens used instead of raw text?
3. What is an embedding?
4. Why is positional encoding necessary?

## Intermediate

5. Explain the role of Self-Attention.
6. What does a Feed Forward Network do?
7. Differentiate between logits and probabilities.
8. Compare Greedy, Top-k, and Top-p sampling.

## Advanced

9. Why do Transformers process tokens more efficiently than recurrent architectures?
10. How does caching improve inference performance?
11. What trade-offs arise when increasing model size or context window?

---

# 21. Assignment

## Theory

1. Explain the complete internal workflow of an LLM from prompt to response.
2. Differentiate between tokens, token IDs, embeddings, logits, and probabilities.
3. Why is Self-Attention considered the core innovation of the Transformer?
4. Compare different token selection strategies and discuss their trade-offs.

## Practical Thinking Exercise

Take the prompt:

> **"Explain Machine Learning in simple terms."**

For each stage below, describe what happens conceptually:

* Tokenization
* Token IDs
* Embeddings
* Positional Encoding
* Transformer Processing
* Self-Attention
* Feed Forward Network
* Logits
* Softmax
* Token Selection
* Final Response

---

# Key Takeaways

* An LLM transforms text into **tokens**, **token IDs**, and **embeddings** before processing it.
* **Positional encoding** ensures that token order is preserved.
* **Self-attention** allows every token to consider every other token, enabling rich contextual understanding.
* **Transformer layers** repeatedly apply attention and feed-forward networks to build increasingly sophisticated representations.
* **Logits**, **Softmax**, and **sampling** determine which token is generated next.
* Responses are produced **one token at a time**, forming coherent text through repeated prediction.

---

## Next Lesson (1.5)

**Transformer Architecture**

In the next lesson, we will dive deeply into the architecture that powers every modern LLM. You'll learn about:

* Why Transformers replaced RNNs and LSTMs
* Encoder vs. Decoder architectures
* Query, Key, and Value (QKV)
* Multi-Head Attention
* Residual Connections
* Layer Normalization
* Decoder Blocks
* Training vs. Inference
* Why Transformers scale so effectively

This lesson will provide the architectural foundation needed to understand GPT, Claude, Gemini, Llama, and other state-of-the-art language models.
