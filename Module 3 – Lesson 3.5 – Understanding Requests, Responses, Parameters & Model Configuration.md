# Module 3 – LLM APIs, SDKs & AI Application Development

# Lesson 3.5 – Understanding Requests, Responses, Parameters & Model Configuration

> **Estimated Study Time:** 20–24 Hours
> **Difficulty:** Intermediate → Advanced
> **Learning Outcome:** Learn how to construct high-quality API requests, understand every important request parameter, configure models for different workloads, interpret API responses, estimate token costs, and choose optimal settings for production AI systems.

> **Important Principle**
>
> **A prompt is only one part of an API request.**
>
> Professional AI engineers optimize the **entire request configuration**, not just the prompt.

---

# Table of Contents

1. Introduction
2. Anatomy of an API Request
3. Request Parameters
4. Model Selection
5. Instructions vs Input
6. Temperature
7. Top-p Sampling
8. Max Output Tokens
9. Stop Sequences
10. Metadata
11. Response Object
12. Usage Statistics
13. Cost Estimation
14. Choosing Configurations
15. Best Practices
16. Common Mistakes
17. Summary
18. Interview Questions
19. Assignment

---

# 1. Introduction

When beginners call an AI API, they often write:

```javascript
const response = await client.responses.create({
  model: "gpt-5.5",
  input: "Explain Docker."
});
```

That works.

But production systems configure many more aspects of the request.

Think of an API request like ordering a pizza.

You don't just say:

> Pizza

You specify:

* Size
* Crust
* Toppings
* Cheese
* Delivery Address
* Payment Method

Similarly, AI requests include much more than just the prompt.

---

# 2. Anatomy of an API Request

Conceptually:

```text
Application

↓

Request Object

│

├── Model

├── Instructions

├── Input

├── Tools

├── Metadata

├── Limits

└── Configuration

↓

AI Provider

↓

Response Object
```

The request object completely defines how the model should behave.

---

# 3. Request Parameters

A conceptual request contains:

```javascript
{
    model: "...",

    instructions: "...",

    input: "...",

    tools: [...],

    metadata: {...},

    max_output_tokens: ...,

    stream: true
}
```

Every parameter changes the behavior of the request.

---

# 4. Model Selection

One of the most important decisions.

Different models optimize for:

* Speed
* Cost
* Reasoning
* Coding
* Vision
* Audio
* Large context windows

---

Example decision tree:

```text
Simple FAQ

↓

Fast Model

────────────────────

Complex Architecture

↓

Reasoning Model

────────────────────

Image Analysis

↓

Vision Model
```

Using the biggest model for every task wastes money.

---

# 5. Instructions vs Input

Many modern APIs separate **instructions** from **user input**.

Conceptually:

```text
Instructions

↓

Persistent Behavior

────────────────────

Input

↓

Current User Request
```

Example:

Instructions:

```text
You are an experienced Java mentor.
```

Input:

```text
Explain polymorphism.
```

Instructions remain consistent.

Input changes every request.

---

Architecture:

```text
Instructions

+

Conversation

+

Current Input

↓

LLM
```

---

# 6. Temperature

Temperature controls **randomness**.

Lower temperature:

```text
Deterministic

↓

Consistent

↓

Predictable
```

Higher temperature:

```text
Creative

↓

Diverse

↓

Less Predictable
```

---

Imagine rolling dice.

Low temperature:

```text
Almost always:

4
```

High temperature:

```text
1

6

2

5

3
```

More variation.

---

Typical use cases:

| Task             | Temperature |
| ---------------- | ----------- |
| Code generation  | Low         |
| JSON extraction  | Low         |
| Summarization    | Low         |
| Translation      | Low         |
| Creative writing | Higher      |
| Storytelling     | Higher      |
| Brainstorming    | Higher      |

---

Important:

Higher temperature **does not** mean higher intelligence.

It means more randomness.

---

# 7. Top-p Sampling

Another sampling technique.

Instead of randomness,

Top-p limits the candidate token pool.

Conceptually:

```text
All Possible Words

↓

Most Probable Words

↓

Top-p Selection

↓

Next Token
```

Both temperature and top-p influence generation diversity.

In practice, many applications adjust one while leaving the other at its default value unless there is a specific need.

---

# 8. Max Output Tokens

Limits response length.

Without limit:

```text
Question

↓

Very Long Response
```

With limit:

```text
Question

↓

Short Response
```

Benefits:

* Lower cost
* Faster responses
* Predictable output size

---

Example use cases:

Chatbot:

Medium response

---

Headline Generator:

Very short response

---

Research Assistant:

Long response

---

# 9. Stop Sequences

Sometimes you want generation to stop at specific markers.

Conceptually:

```text
Generate

↓

STOP Token Found

↓

End Generation
```

Useful for:

* Structured formats
* Custom protocols
* Delimited outputs

Modern APIs also provide structured output mechanisms that reduce the need for manual stop sequences in many scenarios.

---

# 10. Metadata

Metadata is information **about the request**, not part of the prompt.

Example:

```text
Request

│

├── User ID

├── Feature

├── Session

├── Environment

└── Experiment
```

Uses:

* Analytics
* Debugging
* A/B testing
* Monitoring
* Cost reporting

The model does not use metadata as instructions unless you explicitly include it in the model's input.

---

# 11. Response Object

A response is more than generated text.

Conceptually:

```text
Response

│

├── Output

├── Usage

├── Status

├── Metadata

└── Other Provider Fields
```

Applications often inspect:

* Generated content
* Completion status
* Usage metrics
* Errors (if any)

---

# 12. Usage Statistics

Providers typically report usage information.

Conceptually:

```text
Prompt Tokens

+

Output Tokens

↓

Total Tokens
```

Applications track usage for:

* Billing
* Optimization
* Analytics
* Capacity planning

Dashboard example:

```text
Today's Usage

Prompt Tokens:
152,000

Output Tokens:
84,000

Total:
236,000
```

---

# 13. Cost Estimation

Cost depends on:

```text
Prompt Size

+

Output Size

+

Selected Model

↓

API Cost
```

Reducing unnecessary context can significantly lower costs.

Example:

```text
3000 Tokens

↓

Optimize Prompt

↓

900 Tokens

↓

Lower Bill
```

Large-scale applications can save substantial amounts by optimizing token usage.

---

# 14. Choosing Configurations

Different applications require different settings.

| Application        | Typical Configuration Goals |
| ------------------ | --------------------------- |
| Customer Support   | Consistency, speed          |
| Coding Assistant   | Accuracy, reasoning         |
| Translator         | Low randomness              |
| JSON Extractor     | Structured output           |
| Story Generator    | Creativity                  |
| Marketing Copy     | Balanced creativity         |
| Research Assistant | Longer responses            |

There is no universal "best" configuration.

Configuration should match the task.

---

# 15. Best Practices

### Match the model to the workload.

Don't overpay for simple tasks.

---

### Keep prompts concise.

Remove unnecessary instructions.

---

### Set sensible output limits.

Prevent runaway responses.

---

### Monitor token usage.

Optimization reduces long-term costs.

---

### Prefer structured outputs when appropriate.

Especially for machine-readable workflows.

---

### Test different configurations.

Measure quality, latency, and cost before deploying.

---

# 16. Common Mistakes

## Using the same configuration everywhere

Different tasks have different requirements.

---

## Extremely high temperature for factual tasks

Produces inconsistent answers.

---

## Unlimited outputs

Can increase cost unexpectedly.

---

## Ignoring usage metrics

Makes optimization difficult.

---

## Confusing metadata with prompt content

Metadata is for your application, not necessarily for the model.

---

# 17. Summary

Professional AI engineering is about configuring requests intelligently.

A complete request includes:

* Model selection
* Instructions
* User input
* Parameters
* Output limits
* Metadata
* Tools
* Streaming preferences

Understanding these controls lets you optimize for:

* Quality
* Cost
* Latency
* Consistency
* User experience

---

# 18. Interview Questions

## Beginner

1. What is the purpose of the `model` parameter?
2. What does temperature control?
3. Why set output token limits?
4. What is metadata?

---

## Intermediate

5. Compare instructions and input.
6. Explain top-p sampling.
7. Why should usage metrics be monitored?
8. How do request parameters affect cost?

---

## Advanced

9. Design request configurations for:

   * AI Tutor
   * Customer Support Bot
   * Coding Assistant
10. Explain how configuration choices affect scalability.
11. Describe how you would optimize an AI application serving millions of requests daily.

---

# 19. Assignment

## Theory

1. Explain the anatomy of an AI request.
2. Compare instructions and user input.
3. Discuss temperature and top-p.
4. Explain the purpose of metadata.
5. Describe how usage metrics support optimization.

---

## Practical Thinking Exercise

Design configuration profiles for the following systems:

* AI Resume Reviewer
* AI Medical Information Assistant
* AI Story Generator
* AI Programming Mentor
* AI Customer Support Chatbot

For each profile, justify:

* Model choice
* Temperature
* Output length
* Use of structured outputs
* Monitoring strategy
* Cost optimization approach

---

# Key Takeaways

* A production AI request consists of much more than a prompt—it includes **model selection, instructions, parameters, limits, tools, and metadata**.
* **Temperature** controls output randomness, while **top-p** influences token selection diversity. Choose settings that fit the task rather than using one configuration everywhere.
* Monitoring **token usage** and response metrics is essential for controlling costs and improving application performance.
* Selecting the appropriate model and limiting unnecessary output can significantly reduce latency and API expenses.
* Effective AI engineering involves continuously tuning request configurations based on measurable outcomes such as quality, consistency, speed, and cost.

---

# 🏗 AI Engineering Deep Dive

A mature AI platform often generates requests through multiple layers before they reach the model:

```text
                  User Request
                       │
                       ▼
              Business Logic Layer
                       │
                       ▼
              Prompt Builder Service
                       │
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
   Instructions     User Input    Retrieved Context
         │             │             │
         └─────────────┼─────────────┘
                       ▼
             Request Configuration
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
      Model      Token Limits   Tool Settings
         │             │             │
         └─────────────┼─────────────┘
                       ▼
                  SDK Client
                       │
                       ▼
                  AI Provider
                       │
                       ▼
               Response Processing
                       │
                       ▼
                Application Output
```

This layered approach allows teams to change prompts, models, limits, or tools independently without rewriting business logic, making large AI systems easier to maintain and evolve.

---

# 📖 Next Lesson (3.6)

## **Streaming Responses, Server-Sent Events (SSE) & Real-Time AI Applications**

In the next lesson, you'll learn how to build ChatGPT-like real-time experiences, including:

* What streaming is
* Server-Sent Events (SSE)
* HTTP streaming fundamentals
* Token-by-token generation
* Streaming with the JavaScript SDK
* Express.js streaming endpoints
* Frontend streaming consumption
* Cancellation and abort controllers
* Error handling during streams
* Performance optimization
* Building production-grade real-time AI chat applications

This lesson will move from standard request/response APIs to **interactive, real-time AI applications**, which are now the standard for modern chat interfaces.
