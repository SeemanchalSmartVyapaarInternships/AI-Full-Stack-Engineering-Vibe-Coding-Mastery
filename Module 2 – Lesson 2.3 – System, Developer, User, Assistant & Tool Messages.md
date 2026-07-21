# Module 2 – Prompt Engineering & AI Interaction

# Lesson 2.3 – System, Developer, User, Assistant & Tool Messages (Chat-Based LLM Architecture)

> **Estimated Study Time:** 10–12 Hours
> **Difficulty:** Intermediate → Advanced
> **Learning Outcome:** Understand how modern conversational AI systems (ChatGPT, Claude, Gemini, Copilot, Cursor, etc.) process messages internally, the purpose of each message role, instruction hierarchy, conversation management, and how tool calling works in production AI systems.

---

# Table of Contents

1. Introduction
2. Why Modern LLMs Use Messages
3. Message-Based Architecture
4. Types of Messages
5. System Messages
6. Developer Messages
7. User Messages
8. Assistant Messages
9. Tool Messages
10. Conversation History
11. Instruction Hierarchy
12. Message Processing Pipeline
13. Tool Calling Workflow
14. Real Production Architecture
15. Best Practices
16. Common Mistakes
17. Summary
18. Interview Questions
19. Assignment

---

# 1. Introduction

Early language models accepted only one input:

```text
Input

↓

Model

↓

Output
```

Example:

```text
Translate:

Hello

to French.
```

↓

```text
Bonjour
```

---

Modern AI systems are much more sophisticated.

Instead of a single prompt, they process an entire **conversation**.

```text
Conversation

↓

Messages

↓

LLM

↓

Response
```

Every ChatGPT conversation, AI coding assistant, and enterprise chatbot is fundamentally a **list of messages**.

---

# 2. Why Modern LLMs Use Messages

Suppose a conversation is:

User:

> My name is Rahul.

Assistant:

> Nice to meet you.

User:

> What's my name?

The model must remember the previous interaction.

Without message history:

```text
What's my name?
```

The model cannot answer.

With message history:

```text
My name is Rahul.

↓

What's my name?
```

It can answer correctly.

---

Messages provide:

* Conversation memory
* Context
* Roles
* Instructions
* Tool results
* System behavior

---

# 3. Message-Based Architecture

A modern chat application typically looks like:

```text
System Message
       │
Developer Message
       │
Conversation History
       │
User Message
       │
Tool Results (Optional)
       │
Final Prompt
       │
LLM
       │
Assistant Response
```

Notice:

The user sees only:

> "Explain Docker."

Internally, the model receives far more information.

---

# 4. Types of Messages

Modern LLM APIs commonly use several logical message types.

```text
Messages

│

├── System

├── Developer

├── User

├── Assistant

└── Tool
```

Each serves a different purpose.

---

# 5. System Messages

## Definition

A System Message defines the overall behavior and operating rules of the AI.

Think of it as the AI's **operating manual**.

---

Example:

```text
You are a professional software architect.

Always provide secure code.

Never reveal confidential information.

Respond in Markdown.
```

---

System messages usually define:

* Personality
* Behavior
* Safety
* Formatting
* Overall objectives

---

System messages are generally created by the application developer—not the end user.

---

## Example

Instead of every user writing:

```text
Be professional.
```

The application automatically includes:

```text
You are an enterprise customer support assistant.

Be polite.

Be concise.

Use company terminology.
```

---

# 6. Developer Messages

Developer messages are additional instructions supplied by the application developer.

They are more task-specific than system messages.

Example:

```text
Always answer using JSON.

Never expose internal database IDs.

Use metric units.

Prefer official documentation.
```

---

Difference:

System Message:

Defines overall behavior.

Developer Message:

Defines application-specific behavior.

---

Example:

System:

```text
You are an AI Tutor.
```

Developer:

```text
Teach beginners.

Avoid advanced mathematics.

Use simple examples.
```

---

# 7. User Messages

These are the actual requests from the user.

Examples:

```text
Explain AI.
```

```text
Generate SQL.
```

```text
Write Python code.
```

```text
Summarize this article.
```

The user message changes every request.

Everything else may remain unchanged.

---

# 8. Assistant Messages

Assistant messages are the model's previous responses.

Example:

```text
User:

Hello
```

↓

```text
Assistant:

Hi! How can I help?
```

↓

```text
User:

Explain Docker.
```

When generating the next response, the model also considers its own earlier replies.

This helps maintain continuity.

---

# 9. Tool Messages

Modern AI applications use external tools.

Examples:

* Calculator
* Weather API
* Database
* Web Search
* File Search
* Email
* Calendar
* Code Execution

Instead of guessing,

the model can ask a tool.

Example:

User:

```text
What's today's weather?
```

Workflow:

```text
LLM

↓

Weather Tool

↓

Temperature

↓

Tool Message

↓

LLM

↓

Answer
```

The model uses the tool's output as additional context.

---

## Example

Tool returns:

```json
{
  "city":"Delhi",
  "temperature":31,
  "condition":"Sunny"
}
```

Assistant:

> It is currently **31°C and sunny** in Delhi.

The AI did not memorize today's weather.

It used a tool.

---

# 10. Conversation History

The conversation history is simply:

```text
Message 1

↓

Message 2

↓

Message 3

↓

Message 4
```

Each new response depends on all previous messages still within the context window.

Example:

```text
User:

My favorite language is JavaScript.
```

Later:

```text
Recommend a framework.
```

The assistant can recommend:

* Next.js
* Express.js
* React

because earlier messages provide context.

---

# 11. Instruction Hierarchy

One of the most misunderstood concepts.

Suppose:

System:

```text
Always answer professionally.
```

Developer:

```text
Return JSON.
```

User:

```text
Tell a joke using emojis only.
```

The model attempts to satisfy all instructions, but when they conflict, higher-priority instructions generally take precedence.

A simplified hierarchy is:

```text
System

↓

Developer

↓

User

↓

Assistant History

↓

Tool Results
```

Higher-level instructions constrain lower-level ones.

**Important:** The exact instruction-following behavior varies by model and provider, but trusted system-level instructions are generally treated with higher priority than user requests.

---

# 12. Message Processing Pipeline

Production chat applications typically process requests like this:

```text
User Request

↓

Authentication

↓

Conversation Retrieval

↓

System Prompt

↓

Developer Prompt

↓

User Message

↓

RAG Retrieval (Optional)

↓

Tool Calls (Optional)

↓

LLM

↓

Response

↓

Store Conversation
```

This pipeline explains why enterprise AI systems are far more sophisticated than a simple "question → answer" interaction.

---

# 13. Tool Calling Workflow

Modern AI systems can reason about whether a tool is needed.

Example:

```text
User:

Convert

150 USD

to INR.
```

Workflow:

```text
Question

↓

Reason

↓

Need Exchange Rate?

↓

Yes

↓

Call Currency API

↓

Receive Data

↓

Generate Final Answer
```

The LLM is acting as an orchestrator rather than relying solely on memorized knowledge.

---

# 14. Real Production Architecture

A simplified enterprise AI assistant:

```text
                    User
                      │
                      ▼
              Authentication
                      │
                      ▼
            Conversation Manager
                      │
      ┌───────────────┼───────────────┐
      ▼               ▼               ▼
 System Msg     Developer Msg     Chat History
      │               │               │
      └───────────────┼───────────────┘
                      ▼
               Retrieval Layer
                      │
              (Vector Database)
                      │
                      ▼
                 Tool Manager
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
     Search API    Database     Calculator
                      │
                      ▼
                     LLM
                      │
                      ▼
             Assistant Response
```

Notice:

The LLM is only one component.

The surrounding infrastructure is equally important.

---

# 15. Best Practices

* Keep system instructions concise and unambiguous.
* Separate global behavior (system) from application logic (developer).
* Preserve relevant conversation history.
* Remove unnecessary old messages to save context.
* Use tools instead of guessing current facts.
* Validate tool outputs before presenting them.
* Log conversations responsibly while respecting privacy requirements.

---

# 16. Common Mistakes

## Mixing Instructions

Putting system-level rules into every user prompt.

---

## Repeating Instructions

Repeatedly sending identical instructions wastes tokens.

---

## Ignoring Conversation History

The AI loses context.

---

## Tool Misuse

Calling external tools unnecessarily increases cost and latency.

---

## Context Overflow

Too many historical messages cause important information to fall outside the context window.

---

# 17. Summary

Modern conversational AI systems operate on **structured message histories**, not isolated prompts.

The major message roles are:

* **System** → Overall behavior
* **Developer** → Application-specific instructions
* **User** → User requests
* **Assistant** → Previous AI responses
* **Tool** → Results from external systems

Together, these messages form the context used by the model to generate coherent, context-aware, and reliable responses.

Understanding this architecture is fundamental for building chatbots, coding assistants, AI agents, enterprise copilots, and production LLM applications.

---

# 18. Interview Questions

## Beginner

1. Why do modern LLMs use messages instead of a single prompt?
2. What is the purpose of a System Message?
3. What is the difference between a User Message and an Assistant Message?
4. What is a Tool Message?

---

## Intermediate

5. Compare System Messages and Developer Messages.
6. Explain how conversation history affects AI responses.
7. What is instruction hierarchy?
8. Why is tool calling preferred over guessing for live information?

---

## Advanced

9. Design the message architecture for an enterprise AI assistant.
10. Explain how production chat applications assemble messages before sending them to an LLM.
11. Discuss the trade-offs of retaining long conversation histories versus summarizing older interactions.

---

# 19. Assignment

## Theory

1. Explain the five primary message types used in modern LLM systems.
2. Compare System, Developer, User, Assistant, and Tool messages.
3. Describe the instruction hierarchy and why it matters.
4. Explain how conversation history influences response quality.

---

## Practical Thinking Exercise

Design the message flow for an **AI Coding Assistant** integrated into an IDE.

Your design should include:

* System instructions (coding standards and safety)
* Developer instructions (project-specific rules)
* User request
* Previous conversation history
* Tool calls (documentation search, code execution, file system)
* Assistant responses

Draw the complete message pipeline and explain how each message type contributes to generating accurate, context-aware coding assistance.

---

# Key Takeaways

* Modern LLM applications are built around **message-based conversations**, not single prompts.
* The primary message roles are **System**, **Developer**, **User**, **Assistant**, and **Tool**.
* **System messages** define overall behavior, while **Developer messages** add application-specific guidance.
* **Conversation history** provides continuity and context but must be managed within the model's context window.
* **Tool messages** allow models to incorporate reliable external information instead of relying solely on internal knowledge.
* Understanding message architecture is essential for building production-grade AI chatbots, copilots, and agentic systems.

---

# 📖 Next Lesson (2.4)

**Zero-Shot, One-Shot & Few-Shot Prompting**

In the next lesson, you'll learn one of the most influential prompting techniques in modern AI:

* What Zero-Shot, One-Shot, and Few-Shot prompting are
* How examples influence model behavior
* In-context learning
* Choosing the right prompting strategy
* Prompt length vs. performance trade-offs
* Real-world examples for coding, translation, classification, summarization, and structured data extraction
* Best practices and limitations of example-based prompting

This lesson introduces the concept of **in-context learning**, a cornerstone of modern prompt engineering and AI application design.
