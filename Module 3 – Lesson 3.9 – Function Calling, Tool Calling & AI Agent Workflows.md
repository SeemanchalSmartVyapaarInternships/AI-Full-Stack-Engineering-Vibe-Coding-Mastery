# Module 3 – LLM APIs, SDKs & AI Application Development

# Lesson 3.9 – Function Calling, Tool Calling & AI Agent Workflows

> **Estimated Study Time:** 28–34 Hours
> **Difficulty:** Advanced
> **Learning Outcome:** Learn how modern LLMs interact with external tools, APIs, databases, web search, email systems, file systems, and business applications. Understand function calling, tool calling, multi-step execution, agent loops, orchestration, safety, and production architectures.

> **Important Principle**
>
> **LLMs don't magically know everything.**
>
> They become truly useful when they can **use tools**, **retrieve information**, and **take actions**.

---

# Table of Contents

1. Introduction
2. Why Tool Calling Exists
3. Function Calling vs Tool Calling
4. Tool Architecture
5. Tool Definitions
6. Tool Selection
7. Function Execution Lifecycle
8. Multi-Step Tool Workflows
9. Agent Loops
10. Tool Orchestration
11. Real-World Tools
12. Safety & Guardrails
13. Error Handling
14. Production Architecture
15. Best Practices
16. Common Mistakes
17. Summary
18. Interview Questions
19. Assignment

---

# 1. Introduction

Imagine asking:

> What is the weather in Delhi today?

Can an LLM answer?

Only if:

* It memorized outdated information
* Or it can use a weather API

Better workflow:

```text
User

↓

Weather Tool

↓

Current Temperature

↓

LLM

↓

Answer
```

The LLM reasons.

The tool retrieves facts.

---

Another example:

User:

> Send an email to John.

LLM cannot send emails by itself.

Instead:

```text
User

↓

Email Tool

↓

SMTP / Gmail API

↓

Email Sent

↓

LLM

↓

Confirmation
```

---

# 2. Why Tool Calling Exists

LLMs have limitations.

They cannot inherently:

* Access today's weather
* Query databases
* Read your emails
* Execute SQL
* Call REST APIs
* Send WhatsApp messages
* Book meetings
* Process payments

Tool calling solves this.

---

Without tools:

```text
Question

↓

LLM Guess
```

---

With tools:

```text
Question

↓

Reason

↓

Choose Tool

↓

Execute Tool

↓

Receive Result

↓

Generate Answer
```

---

# 3. Function Calling vs Tool Calling

These terms are related but not identical.

## Function Calling

The model requests that your application execute a predefined function.

Example:

```text
LLM

↓

bookFlight()

↓

Application

↓

Result

↓

LLM
```

---

## Tool Calling

A broader concept.

Tools may include:

* Functions
* APIs
* Databases
* Search engines
* File systems
* Code execution
* OCR
* External services

---

Comparison

| Function Calling            | Tool Calling                 |
| --------------------------- | ---------------------------- |
| Calls application functions | Uses any external capability |
| Usually local               | May be local or remote       |
| Narrower concept            | Broader ecosystem            |

---

# 4. Tool Architecture

A production architecture:

```text
                 User

                   │

                   ▼

                  LLM

                   │

          Needs External Data?

                   │

         Yes ─────────── No

          │              │

          ▼              ▼

      Tool Manager      Respond

          │

 ┌────────┼─────────┐

 ▼        ▼         ▼

Search   Database  Email

          ▼

     Tool Result

          ▼

         LLM

          ▼

      Final Answer
```

Notice:

The LLM never directly talks to the database.

Your application controls execution.

---

# 5. Tool Definitions

Before the model can use a tool,

it must know:

* Tool name
* Purpose
* Inputs
* Outputs

Conceptually:

```text
Tool

Name:
searchCustomer

Description:
Find customer by ID

Parameters:

customerId
```

The model uses this definition to determine when the tool is appropriate.

---

# 6. Tool Selection

Suppose several tools exist.

```text
Weather

Calendar

Email

Database

Calculator

Search
```

Question:

> What's 345 × 924?

Model selects:

Calculator.

---

Question:

> Send an email.

Model selects:

Email Tool.

---

Question:

> Find customer #4182.

Model selects:

Database Tool.

---

Workflow:

```text
Question

↓

Reason

↓

Choose Best Tool

↓

Execute
```

---

# 7. Function Execution Lifecycle

Complete lifecycle:

```text
User

↓

LLM

↓

Tool Request

↓

Application

↓

Execute Tool

↓

Result

↓

LLM

↓

Final Response
```

The application—not the LLM—executes the function.

---

Example:

User:

```text
Calculate 245 × 786
```

Workflow:

```text
LLM

↓

calculator()

↓

Application

↓

192570

↓

LLM

↓

"The result is 192,570."
```

---

# 8. Multi-Step Tool Workflows

Many tasks require multiple tools.

Example:

User:

> Email everyone attending tomorrow's meeting.

Workflow:

```text
User

↓

Calendar Tool

↓

Meeting Attendees

↓

Contacts Tool

↓

Email Addresses

↓

Email Tool

↓

Confirmation

↓

LLM
```

This is a multi-step workflow.

---

# 9. Agent Loops

Modern AI agents operate in loops.

Architecture:

```text
Goal

↓

Reason

↓

Need Tool?

↓

Execute

↓

Observe Result

↓

Goal Achieved?

↓

No

↓

Repeat

↓

Yes

↓

Answer
```

This cycle is sometimes called the **Reason → Act → Observe** loop.

---

Example:

```text
Research Company

↓

Search Website

↓

Need Annual Report

↓

Download

↓

Read

↓

Need Revenue

↓

Search Again

↓

Answer
```

---

# 10. Tool Orchestration

Large systems coordinate many tools.

Architecture:

```text
                    LLM

                      │

        ┌─────────────┼─────────────┐

        ▼             ▼             ▼

 Search Tool    Database Tool    Email Tool

        ▼             ▼             ▼

          Tool Results Combined

                      │

                      ▼

                     LLM
```

The orchestrator:

* Executes tools
* Handles retries
* Resolves conflicts
* Combines outputs

---

# 11. Real-World Tools

Modern AI systems commonly integrate:

---

## Search

Retrieve:

* Documentation
* Websites
* Company knowledge

---

## SQL Database

Execute queries.

Retrieve customer information.

---

## Email

Send:

* Reports
* Notifications
* Confirmations

---

## Calendar

Schedule meetings.

Check availability.

---

## File System

Read:

* PDFs
* CSV
* Word files

---

## OCR

Extract text from images.

---

## Payments

Initiate payment requests.

Generate invoices.

---

## Maps

Find routes.

Estimate travel time.

---

## CRM

Retrieve:

* Leads
* Customers
* Opportunities

---

## ERP

Retrieve:

* Inventory
* Orders
* Employees

---

# 12. Safety & Guardrails

Tool execution can have consequences.

Example:

```text
Delete Customer
```

Should never execute automatically.

Production systems implement:

```text
LLM

↓

Permission Check

↓

Human Approval

↓

Execute
```

Examples requiring confirmation:

* Delete files
* Send money
* Cancel bookings
* Delete database records
* Send mass emails

---

Other safeguards:

* Input validation
* Rate limits
* Access control
* Audit logs

---

# 13. Error Handling

Tools can fail.

Example:

```text
Database Offline
```

Workflow:

```text
Tool

↓

Failure

↓

Retry?

↓

Fallback?

↓

Notify User
```

Never assume tool execution always succeeds.

---

# 14. Production Architecture

Enterprise architecture:

```text
                  User
                    │
                    ▼
                   LLM
                    │
            Tool Decision Engine
                    │
      ┌─────────────┼─────────────┐
      ▼             ▼             ▼
 Search API    CRM Service    Calendar API
      │             │             │
      └─────────────┼─────────────┘
                    ▼
            Tool Orchestrator
                    │
                    ▼
            Response Composer
                    │
                    ▼
               Final Answer
```

Additional layers often include:

* Authentication
* Authorization
* Monitoring
* Logging
* Rate limiting

---

# 15. Best Practices

### Keep tools focused.

One tool should perform one responsibility.

---

### Validate tool inputs.

Reject invalid parameters.

---

### Confirm destructive actions.

Never automatically delete or modify important data.

---

### Separate reasoning from execution.

The LLM decides.

Your application executes.

---

### Log tool usage.

Useful for debugging and auditing.

---

### Handle failures gracefully.

Provide meaningful user feedback.

---

### Restrict permissions.

Tools should have the minimum access required.

---

# 16. Common Mistakes

## Giving tools excessive permissions

Violates the principle of least privilege.

---

## Allowing unrestricted execution

Can damage systems.

---

## Ignoring tool failures

Results in confusing user experiences.

---

## Combining unrelated responsibilities

One tool should not perform many unrelated actions.

---

## Letting the model execute code directly

The application must remain in control.

---

## Skipping human approval

Dangerous for high-impact operations.

---

# 17. Summary

Tool calling transforms an LLM from a text generator into an intelligent assistant capable of interacting with the outside world.

A production tool-calling system includes:

* Tool definitions
* Tool selection
* Secure execution
* Error handling
* Monitoring
* Permission checks
* Human approval for sensitive actions

These capabilities form the foundation of modern AI agents.

---

# 18. Interview Questions

## Beginner

1. What is tool calling?
2. Why can't an LLM directly send emails?
3. What is function calling?
4. Why is tool execution handled by the application?

---

## Intermediate

5. Compare function calling and tool calling.
6. Explain a multi-step tool workflow.
7. What is an agent loop?
8. Why are permission checks important?

---

## Advanced

9. Design an AI assistant that integrates email, calendar, CRM, and database tools.
10. Explain how to safely execute AI-requested actions.
11. Describe an orchestration layer for enterprise AI systems.

---

# 19. Assignment

## Theory

1. Define function calling.
2. Define tool calling.
3. Explain tool orchestration.
4. Describe the agent loop.
5. Explain why guardrails are necessary.

---

## Practical Thinking Exercise

Design an **AI Executive Assistant** that can:

* Search company documents
* Read calendars
* Schedule meetings
* Send emails
* Generate reports
* Query CRM
* Retrieve ERP data

Your architecture should include:

* LLM
* Tool registry
* Tool orchestrator
* Authentication
* Authorization
* Human approval system
* Logging
* Monitoring
* Error handling

Explain how each component contributes to security, reliability, and usability.

---

# Key Takeaways

* **Function calling** allows an LLM to request the execution of predefined application functions, while **tool calling** encompasses a broader range of external capabilities such as APIs, databases, and search systems.
* The **application** always executes tools—the LLM only decides when and how they should be used.
* **Multi-step workflows** enable AI systems to combine multiple tools to accomplish complex tasks.
* Production AI systems require **guardrails**, **permission checks**, **logging**, and **human approval** for sensitive operations.
* Tool calling is the foundation of **agentic AI**, where reasoning and action are combined to solve real-world problems.

---

# 🏗 AI Engineering Deep Dive

A mature AI agent platform typically separates reasoning from execution:

```text
                     User Goal
                         │
                         ▼
                  Planner (LLM)
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
     Search Tool     CRM Tool      Calendar Tool
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                 Tool Orchestrator
                         │
                Validation & Policies
                         │
                         ▼
                 Execute Safe Actions
                         │
                         ▼
                  Observation Results
                         │
                         ▼
                    Planner (LLM)
                         │
                         ▼
                    Final Response
```

This separation allows organizations to enforce business rules, monitor every action, and safely integrate AI into enterprise workflows.

---

# 📖 Next Lesson (3.10)

## **Building Production AI Applications – Architecture, Patterns & Best Practices**

The final lesson of Module 3 brings everything together. You'll learn:

* End-to-end AI application architecture
* Layered service design
* AI service abstraction
* Multi-provider support
* Prompt management systems
* AI middleware
* Logging & observability
* Cost monitoring
* Caching strategies
* Rate limiting
* Resilience and retries
* Testing AI applications
* Deployment patterns
* Scaling to millions of users

This lesson synthesizes everything from Modules 1–3 into a comprehensive blueprint for designing and operating **production-grade AI applications**.
