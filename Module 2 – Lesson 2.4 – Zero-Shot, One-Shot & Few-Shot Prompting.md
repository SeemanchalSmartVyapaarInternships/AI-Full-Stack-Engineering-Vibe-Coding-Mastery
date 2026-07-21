# Module 2 – Prompt Engineering & AI Interaction

# Lesson 2.4 – Zero-Shot, One-Shot & Few-Shot Prompting (In-Context Learning)

> **Estimated Study Time:** 10–12 Hours
> **Difficulty:** Beginner → Advanced
> **Learning Outcome:** Learn how Large Language Models learn from examples inside a prompt without changing their parameters. Understand Zero-Shot, One-Shot, and Few-Shot prompting, In-Context Learning (ICL), when to use each technique, their advantages and limitations, and production best practices.

---

# Table of Contents

1. Introduction
2. What is In-Context Learning?
3. Zero-Shot Prompting
4. One-Shot Prompting
5. Few-Shot Prompting
6. Why Examples Improve Performance
7. How LLMs Learn from Examples
8. Comparing Zero-Shot, One-Shot & Few-Shot
9. Real-World Applications
10. Prompt Length vs Performance
11. Best Practices
12. Common Mistakes
13. Summary
14. Interview Questions
15. Assignment

---

# 1. Introduction

Suppose you ask a new employee:

> Categorize customer support tickets.

Without showing any examples, they might struggle.

Now suppose you first show:

```text
Email:
I forgot my password.

Category:
Password Reset
```

Then ask them to classify another email.

They immediately understand the expected pattern.

This is exactly how **Few-Shot Prompting** works.

Instead of retraining the AI,

we simply provide examples **inside the prompt**.

---

# 2. What is In-Context Learning (ICL)?

One of the biggest discoveries in modern LLM research is that models can often adapt to new tasks by observing examples provided in the prompt.

This behavior is called **In-Context Learning (ICL).**

---

## Formal Definition

In-Context Learning is the ability of a language model to adapt its behavior using examples contained within the input context, without updating its model parameters.

---

## Simple Definition

The AI **learns from examples you give it during the conversation**, not by changing its internal knowledge.

---

## Engineering Definition

ICL is an inference-time adaptation mechanism where demonstrations inside the prompt influence token prediction.

Notice:

No training occurs.

No fine-tuning occurs.

The model parameters remain exactly the same.

---

# 3. Zero-Shot Prompting

Zero-Shot means:

> **No examples are provided.**

You simply describe the task.

Example:

```text
Translate:

Good Morning

to Hindi.
```

Output:

```text
सुप्रभात
```

---

Another example:

```text
Summarize this article in 100 words.
```

No examples.

Only instructions.

---

## Workflow

```text
Instruction

↓

LLM

↓

Answer
```

---

## Advantages

* Simple
* Fast
* Small prompt
* Low token cost

---

## Limitations

The model must infer:

* Desired format
* Style
* Expected output

Sometimes it guesses incorrectly.

---

# 4. One-Shot Prompting

One-Shot means:

> **Exactly one example is provided.**

Example:

```text
Input:

I love this movie.

Sentiment:

Positive

-------------------

Input:

This food tastes terrible.

Sentiment:
```

Output:

```text
Negative
```

The model copies the demonstrated pattern.

---

Workflow:

```text
One Example

↓

New Input

↓

Pattern Matching

↓

Answer
```

---

Advantages:

* Better consistency
* Better formatting
* Small token overhead

---

# 5. Few-Shot Prompting

Few-Shot Prompting provides **multiple examples**.

Example:

```text
Sentence:

Amazing experience.

Sentiment:

Positive

------------------

Sentence:

Worst service ever.

Sentiment:

Negative

------------------

Sentence:

Highly recommended.

Sentiment:
```

Output:

```text
Positive
```

---

Workflow:

```text
Multiple Examples

↓

Pattern Recognition

↓

New Question

↓

Answer
```

---

The model identifies:

* Format
* Style
* Labels
* Output structure

before answering.

---

# 6. Why Examples Improve Performance

Consider asking:

```text
Extract information.
```

What information?

Unknown.

Now provide:

```text
Text:

John lives in Delhi.

Output:

{
"name":"John",
"city":"Delhi"
}
```

The model immediately understands:

* Required fields
* JSON format
* Naming convention

Examples reduce ambiguity.

---

# 7. How LLMs Learn from Examples

Remember:

The model **does not actually retrain itself.**

Instead:

```text
Prompt

↓

Examples

↓

Attention

↓

Pattern Detection

↓

Next Token Prediction
```

The examples influence the probability distribution of future tokens.

This temporary adaptation disappears once the conversation ends (or once the examples are no longer in context).

---

# 8. Comparing Zero-Shot, One-Shot & Few-Shot

| Feature       | Zero-Shot    | One-Shot              | Few-Shot                            |
| ------------- | ------------ | --------------------- | ----------------------------------- |
| Examples      | 0            | 1                     | 2 or more                           |
| Prompt Length | Short        | Medium                | Long                                |
| Token Cost    | Low          | Medium                | Higher                              |
| Consistency   | Moderate     | Better                | Best (generally)                    |
| Setup Time    | Very Low     | Low                   | Higher                              |
| Good For      | Simple tasks | Pattern demonstration | Complex formatting & classification |

---

## Visual Comparison

```text
Zero-Shot

Instruction

↓

Answer
```

---

```text
One-Shot

Example

↓

Question

↓

Answer
```

---

```text
Few-Shot

Example 1

↓

Example 2

↓

Example 3

↓

Question

↓

Answer
```

---

# 9. Real-World Applications

## A. Sentiment Analysis

Examples help define labels.

Example:

```text
Positive

Negative

Neutral
```

---

## B. Information Extraction

Example:

```text
Invoice

↓

JSON
```

The model learns the schema.

---

## C. Code Generation

Example:

```text
Input:

Sort array

↓

Output:

JavaScript Function
```

Future coding follows the same style.

---

## D. Translation

Provide one translation example.

The model follows the same translation style.

---

## E. Customer Support

Example responses teach:

* Tone
* Greeting style
* Closing style
* Company terminology

---

## F. SQL Generation

Examples define:

* Naming conventions
* Query style
* Formatting
* Aliases

---

# 10. Prompt Length vs Performance

Longer prompts often improve quality—but not always.

```text
Prompt Length

↓

Better Context

↓

Better Accuracy

↓

Higher Cost

↓

Higher Latency
```

Trade-offs:

Advantages:

* Better formatting
* Better consistency
* Reduced ambiguity

Disadvantages:

* More tokens
* Higher API cost
* Longer response time
* Increased context usage

The goal is **enough examples**, not **as many examples as possible**.

---

# 11. Best Practices

### Use representative examples.

Good examples should resemble real user inputs.

---

### Keep formatting consistent.

Bad:

```text
JSON

Markdown

Table

Paragraph
```

Mixed formats confuse the model.

---

### Use diverse examples.

Cover edge cases.

Don't repeat nearly identical examples.

---

### Minimize unnecessary text.

Every extra token increases cost.

---

### Test with real-world inputs.

Evaluate prompts on realistic data, not only ideal examples.

---

### Prefer Zero-Shot first.

If quality is insufficient,

then move to One-Shot or Few-Shot.

---

# 12. Common Mistakes

### Too Many Examples

Consumes valuable context.

---

### Contradictory Examples

Example 1:

```text
Happy

Positive
```

Example 2:

```text
Happy

Negative
```

The model receives conflicting guidance.

---

### Unrealistic Examples

Examples that don't match production data reduce effectiveness.

---

### Inconsistent Formatting

Changing output styles between examples leads to inconsistent responses.

---

### Copying Large Documents

Large examples increase latency and cost without proportional benefit.

---

# 13. Summary

Zero-Shot, One-Shot, and Few-Shot prompting are techniques that guide an LLM using instructions and examples rather than retraining.

The underlying capability is **In-Context Learning**, where the model adapts its behavior based on information present in the prompt.

The right strategy depends on:

* Task complexity
* Output consistency requirements
* Token budget
* Latency constraints

Production AI systems frequently combine these prompting techniques with retrieval, tool use, and structured outputs.

---

# 14. Interview Questions

## Beginner

1. What is Zero-Shot Prompting?
2. What is One-Shot Prompting?
3. What is Few-Shot Prompting?
4. What is In-Context Learning?

---

## Intermediate

5. Why do examples improve model performance?
6. Compare Zero-Shot and Few-Shot prompting.
7. What are the trade-offs of using many examples?
8. Why doesn't Few-Shot Prompting count as training?

---

## Advanced

9. Design a Few-Shot prompt for extracting invoice data into JSON.
10. When would you choose Zero-Shot over Few-Shot in a production API?
11. How does In-Context Learning differ from fine-tuning?

---

# 15. Assignment

## Theory

1. Explain In-Context Learning.
2. Compare Zero-Shot, One-Shot, and Few-Shot prompting.
3. Discuss why examples influence model behavior.
4. Explain the relationship between prompt length, token usage, latency, and cost.

---

## Practical Thinking Exercise

Design prompting strategies for the following applications:

### Application 1 – AI Resume Screener

Choose:

* Zero-Shot
* One-Shot
* Few-Shot

Justify your decision.

---

### Application 2 – AI Code Generator

Select the appropriate prompting strategy.

Explain why.

---

### Application 3 – AI Medical Report Extractor

Should it use examples?

How many?

What should those examples contain?

---

### Application 4 – AI SQL Generator

Design a Few-Shot prompt that teaches the model your company's SQL naming conventions and formatting style.

Explain how this improves consistency.

---

# Key Takeaways

* **Zero-Shot Prompting** relies only on task instructions.
* **One-Shot Prompting** teaches the model using a single example.
* **Few-Shot Prompting** provides multiple examples to improve consistency and accuracy.
* **In-Context Learning (ICL)** allows the model to adapt during inference without updating its parameters.
* More examples can improve quality but also increase **token usage, latency, and cost**.
* Effective Few-Shot prompts use **representative, consistent, and diverse examples**, avoiding unnecessary verbosity.

---

# 🎓 AI Engineering Insight

Many beginners believe:

> **"Few-Shot Prompting is a type of training."**

This is **incorrect**.

| Fine-Tuning                | Few-Shot Prompting                           |
| -------------------------- | -------------------------------------------- |
| Changes model parameters   | Does not change parameters                   |
| Takes hours or days        | Happens instantly during inference           |
| Requires GPUs and datasets | Requires only examples in the prompt         |
| Permanent until retrained  | Temporary and limited to the current context |
| Produces a new model       | Uses the existing model                      |

This distinction is fundamental and is frequently asked in AI engineering interviews.

---

# 📖 Next Lesson (2.5)

**Role Prompting & Persona Engineering**

In the next lesson, we'll explore:

* What Role Prompting really is
* Persona Engineering
* Role consistency across conversations
* Expert personas vs. generic assistants
* Multi-role prompting
* Dynamic role assignment
* Role switching
* Agent personas
* Enterprise prompt personas
* Best practices for designing AI assistants with specialized expertise

This lesson will show how assigning carefully designed roles and personas can significantly influence the style, depth, and usefulness of AI responses without changing the underlying model.
