# Module 4 – AI Agents, Agentic AI & Multi-Agent Systems

# Lesson 4.6 – Agent Frameworks – LangGraph, OpenAI Agents SDK, AutoGen, CrewAI & Semantic Kernel

> **Estimated Study Time:** 36–45 Hours
> **Difficulty:** Advanced → Expert
> **Learning Outcome:** Understand the major AI agent frameworks, how they differ, when to use each one, and how to select the right framework for enterprise AI systems.

> **Important Principle**
>
> **An AI framework does not make an agent intelligent.**
>
> **It provides the infrastructure that allows intelligent agents to be built reliably, safely, and at scale.**

---

# Table of Contents

1. Introduction
2. Why Agent Frameworks Exist
3. What Makes a Good Agent Framework?
4. Framework Landscape
5. LangGraph
6. OpenAI Agents SDK
7. CrewAI
8. AutoGen
9. Semantic Kernel
10. Other Emerging Frameworks
11. Feature Comparison
12. Choosing the Right Framework
13. Enterprise Architecture Patterns
14. Framework-Agnostic Design
15. Best Practices
16. Common Mistakes
17. Summary
18. Interview Questions
19. Assignment

---

# 1. Introduction

Imagine building a large AI agent manually.

You need to implement:

* Memory
* Tool execution
* Planning
* Retries
* Logging
* State management
* Multi-agent communication
* Human approvals
* Monitoring

Everything from scratch.

```text
Developer

↓

Thousands of Lines of Code

↓

Complex Agent
```

This quickly becomes difficult to maintain.

Agent frameworks provide reusable building blocks.

---

# 2. Why Agent Frameworks Exist

Traditional LLM applications look like:

```text
User

↓

Prompt

↓

LLM

↓

Answer
```

Modern agents require much more.

```text
User

↓

Planner

↓

Memory

↓

Tools

↓

Workflow

↓

Human Approval

↓

Monitoring

↓

Answer
```

Frameworks manage these components.

---

## Responsibilities

A framework usually provides:

* Agent lifecycle
* Workflow engine
* State management
* Tool execution
* Memory integration
* Observability
* Human approval
* Multi-agent coordination

---

# 3. What Makes a Good Agent Framework?

An enterprise-ready framework should support:

```text
Planning

↓

Memory

↓

Tools

↓

State

↓

Logging

↓

Retry

↓

Security

↓

Deployment
```

---

Important characteristics:

| Capability       | Why It Matters        |
| ---------------- | --------------------- |
| State Management | Resume long workflows |
| Tool Support     | External integrations |
| Memory           | Persistent context    |
| Multi-Agent      | Team collaboration    |
| Human Approval   | Safety                |
| Observability    | Debugging             |
| Extensibility    | Future growth         |
| Scalability      | Production deployment |

---

# 4. Framework Landscape

Today's ecosystem contains several major frameworks.

```text
               AI Agent Frameworks

                     │

    ┌─────────┬────────┬────────┬──────────┐

    ▼         ▼        ▼        ▼

LangGraph CrewAI AutoGen OpenAI Agents SDK

                │

                ▼

         Semantic Kernel

                │

                ▼

      Other Emerging Frameworks
```

Each framework emphasizes different strengths.

---

# 5. LangGraph

LangGraph is designed around **graph-based workflows**.

Instead of linear execution,

agents move through a directed graph.

Architecture:

```text
Node

↓

Decision

↓

Next Node

↓

Decision

↓

Next Node
```

Example:

```text
Start

↓

Research

↓

Code

↓

Test

↓

Deploy

↓

Finish
```

---

### Core Concepts

* Nodes
* Edges
* Shared State
* Conditional Routing
* Checkpointing

---

### Strengths

✔ Long-running workflows

✔ Complex branching

✔ State persistence

✔ Recovery after failure

✔ Human approval

---

### Ideal Use Cases

* Enterprise workflows
* Software engineering agents
* Research systems
* Approval pipelines

---

# 6. OpenAI Agents SDK

The OpenAI Agents SDK focuses on building production-ready AI agents with a consistent programming model.

Typical architecture:

```text
User

↓

Agent

↓

Tools

↓

Model

↓

Response
```

The SDK emphasizes:

* Clear agent definitions
* Tool integration
* Structured outputs
* Multi-step execution
* Tracing and observability
* Guardrails through application logic

---

### Strengths

✔ Simple developer experience

✔ Tight integration with OpenAI models

✔ Production-oriented abstractions

✔ Built-in tracing support

✔ Strong tool ecosystem

---

Ideal for:

* Business assistants
* Customer support
* Enterprise automation
* Internal productivity tools

---

# 7. CrewAI

CrewAI models AI systems like a human organization.

Each agent has:

* Role
* Goal
* Responsibility

Example:

```text
CEO

↓

Manager

↓

Developer

↓

Tester

↓

Reviewer
```

Each agent contributes independently.

---

Strengths:

✔ Easy multi-agent development

✔ Role specialization

✔ Task delegation

✔ Human-like collaboration

---

Example organization:

```text
Researcher

↓

Writer

↓

Reviewer

↓

Editor
```

---

Ideal for:

* Content generation
* Business workflows
* Research automation
* Team simulation

---

# 8. AutoGen

AutoGen emphasizes **agent-to-agent conversation**.

Architecture:

```text
Agent A

↓

Message

↓

Agent B

↓

Message

↓

Agent C
```

Instead of executing workflows,

agents primarily communicate.

---

Strengths

✔ Multi-agent discussions

✔ Collaborative reasoning

✔ Debate

✔ Negotiation

✔ Planning

---

Example

```text
Planner

↓

Developer

↓

Reviewer

↓

Planner
```

The conversation itself drives progress.

---

Best suited for:

* Research
* Brainstorming
* Collaborative coding
* Design discussions

---

# 9. Semantic Kernel

Semantic Kernel integrates AI into enterprise software ecosystems.

Architecture:

```text
Business Logic

↓

Semantic Kernel

↓

AI

↓

Enterprise Systems
```

Key capabilities:

* AI plugins
* Memory connectors
* Planning
* Prompt templates
* Enterprise integrations

---

Strong integrations include:

* Microsoft ecosystem
* Azure
* Microsoft 365
* Enterprise APIs

---

Best for:

* Enterprise applications
* Business automation
* Internal copilots

---

# 10. Other Emerging Frameworks

The ecosystem evolves rapidly.

Examples include frameworks focused on:

* Workflow automation
* Distributed agents
* Browser automation
* Robotics
* Scientific research
* Autonomous coding

The underlying concepts remain similar:

```text
Goal

↓

Planning

↓

Memory

↓

Tools

↓

Execution

↓

Observation
```

Frameworks differ primarily in implementation details and developer experience.

---

# 11. Feature Comparison

| Feature                | LangGraph | OpenAI Agents SDK | CrewAI    | AutoGen   | Semantic Kernel   |
| ---------------------- | --------- | ----------------- | --------- | --------- | ----------------- |
| Workflow Engine        | Excellent | Good              | Good      | Moderate  | Good              |
| Multi-Agent            | Excellent | Good              | Excellent | Excellent | Moderate          |
| Tool Support           | Excellent | Excellent         | Good      | Good      | Excellent         |
| State Management       | Excellent | Good              | Moderate  | Moderate  | Good              |
| Enterprise Integration | Good      | Good              | Moderate  | Moderate  | Excellent         |
| Human Approval         | Excellent | Good              | Moderate  | Moderate  | Good              |
| Learning Curve         | Moderate  | Easy–Moderate     | Easy      | Moderate  | Moderate–Advanced |

No framework is "best" for every project.

---

# 12. Choosing the Right Framework

Decision tree:

```text
Need Complex Workflow?

↓

Yes

↓

LangGraph

────────────

Need Simple Production Agent?

↓

OpenAI Agents SDK

────────────

Need Team Collaboration?

↓

CrewAI

────────────

Need Agent Conversations?

↓

AutoGen

────────────

Need Enterprise Microsoft Integration?

↓

Semantic Kernel
```

Framework selection depends on project requirements rather than popularity.

---

# 13. Enterprise Architecture Patterns

Production systems rarely rely on a single framework feature.

Typical architecture:

```text
Frontend

↓

API Gateway

↓

Business Layer

↓

Agent Framework

↓

Memory

↓

Tools

↓

Monitoring

↓

Database
```

Frameworks are one layer within a broader system.

---

# 14. Framework-Agnostic Design

One of the most important engineering principles is to avoid tightly coupling business logic to a specific framework.

Poor architecture:

```text
Business Logic

↓

Specific Framework

↓

LLM
```

Better architecture:

```text
Business Logic

↓

Agent Interface

↓

Framework Adapter

↓

Framework

↓

LLM
```

Benefits:

* Easier migration
* Better testing
* Reduced vendor lock-in
* Greater flexibility

---

# 15. Best Practices

### Separate business logic from framework code.

---

### Use interfaces or adapters.

Avoid hard dependencies.

---

### Keep workflows modular.

---

### Monitor every agent execution.

---

### Version prompts and workflows.

---

### Design for framework replacement.

---

### Evaluate frameworks through prototypes before committing.

---

# 16. Common Mistakes

## Choosing a framework because it is popular

Popularity does not guarantee suitability.

---

## Rebuilding framework functionality

Use the framework's strengths instead of duplicating them.

---

## Ignoring observability

Debugging long-running agents becomes difficult.

---

## Overengineering

Simple applications may not need a sophisticated framework.

---

## Framework lock-in

Avoid embedding business rules directly into framework-specific APIs.

---

# 17. Summary

Agent frameworks provide the infrastructure needed to build reliable AI systems.

Major frameworks include:

* **LangGraph** for graph-based workflows
* **OpenAI Agents SDK** for production-oriented agents with strong tool integration
* **CrewAI** for role-based collaboration
* **AutoGen** for conversational multi-agent systems
* **Semantic Kernel** for enterprise AI integration

Choosing the right framework depends on architecture, workflows, integrations, and operational requirements—not on which framework is newest.

---

# 18. Interview Questions

## Beginner

1. Why do AI agent frameworks exist?
2. What problems do they solve?
3. What is workflow orchestration?
4. What is state management?

---

## Intermediate

5. Compare LangGraph and CrewAI.
6. Compare AutoGen and OpenAI Agents SDK.
7. Why is framework abstraction important?
8. Explain framework lock-in.

---

## Advanced

9. Design a framework-agnostic enterprise AI platform.
10. Compare graph-based workflows with conversational multi-agent systems.
11. Explain how to migrate from one agent framework to another while minimizing business disruption.

---

# 19. Assignment

## Theory

1. Explain the purpose of agent frameworks.
2. Compare LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, and Semantic Kernel.
3. Explain workflow orchestration.
4. Discuss framework abstraction.
5. Describe enterprise architecture patterns.

---

## Practical Thinking Exercise

Design an **Enterprise AI Platform** that supports:

* Customer support agents
* Software engineering agents
* HR assistants
* Finance assistants
* Research agents

Your design should include:

* Framework abstraction layer
* Agent runtime
* Workflow engine
* Tool registry
* Memory subsystem
* Monitoring
* Security
* Human approval
* Logging
* Deployment pipeline

For each component, explain:

* Responsibilities
* Interactions
* Scalability strategy
* Failure recovery
* Monitoring approach

---

# Key Takeaways

* Agent frameworks provide reusable infrastructure for **planning**, **memory**, **tool execution**, **workflow orchestration**, and **observability**.
* Different frameworks optimize for different scenarios: graph workflows, role-based collaboration, conversational agents, or enterprise integration.
* Production systems should use **framework-agnostic architectures** to reduce vendor lock-in and improve maintainability.
* Framework selection should be based on technical requirements, ecosystem fit, and operational needs rather than trends.
* Enterprise AI platforms often combine framework capabilities with custom infrastructure for governance, monitoring, and scalability.

---

# 🏗 AI Engineering Deep Dive

A modern enterprise AI platform often layers business logic above an interchangeable agent framework:

```text
                         Client Applications
                                │
                                ▼
                           API Gateway
                                │
                                ▼
                       Business Service Layer
                                │
                                ▼
                    Agent Abstraction Interface
                                │
        ┌───────────────────────┼───────────────────────┐
        ▼                       ▼                       ▼
   LangGraph Adapter     OpenAI Agents SDK      CrewAI Adapter
                                │
                                ▼
                         Agent Runtime Layer
                                │
          ┌─────────────────────┼─────────────────────┐
          ▼                     ▼                     ▼
     Workflow Engine      Memory Manager      Tool Orchestrator
          │                     │                     │
          └─────────────────────┼─────────────────────┘
                                ▼
                      Monitoring & Observability
                                │
                                ▼
                     Databases / Vector Stores /
                   Enterprise APIs / External Tools
```

This architecture allows organizations to adopt new frameworks over time without rewriting business logic, making the platform more resilient to the rapidly evolving AI ecosystem.

---

# 📖 Next Lesson (4.7)

## **Agent Memory + RAG + Tool Use – Building Complete Agentic Workflows**

So far, we've studied the major building blocks separately:

* Planning
* Memory
* Tool use
* Multi-agent systems
* Frameworks

In the next lesson, you'll learn how these pieces work together in a **complete production agent pipeline**. Topics include:

* End-to-end agent execution lifecycle
* Planning → Retrieval → Tool Use → Reflection loop
* Memory-enhanced RAG
* Retrieval-aware planning
* Tool-aware reasoning
* Human-in-the-loop checkpoints
* Long-running workflows
* Failure recovery
* Agent state machines
* Production orchestration patterns

This lesson is where individual concepts come together into a cohesive **agentic architecture** capable of solving real-world enterprise tasks.
