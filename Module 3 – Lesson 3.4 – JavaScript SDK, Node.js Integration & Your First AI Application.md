# Module 3 – LLM APIs, SDKs & AI Application Development

# Lesson 3.4 – JavaScript SDK, Node.js Integration & Your First AI Application

> **Estimated Study Time:** 18–22 Hours
> **Difficulty:** Intermediate
> **Learning Outcome:** Learn how to build your first production-ready AI application using **JavaScript (Node.js)**. You'll understand SDK architecture, project setup, API client creation, environment configuration, async programming, streaming, error handling, Express.js integration, and production best practices.

> **Tech Stack Used Throughout This Module**
>
> * JavaScript (ES Modules)
> * Node.js
> * Express.js
> * dotenv
> * Modern OpenAI JavaScript SDK
> * REST APIs

---

# Table of Contents

1. Introduction
2. Why Use an SDK?
3. Project Structure
4. Installing Dependencies
5. Environment Configuration
6. Creating Your First AI Client
7. Making Your First AI Request
8. Understanding Responses
9. Async/Await
10. Error Handling
11. Building an Express API
12. Streaming Responses
13. Production Project Structure
14. Best Practices
15. Common Mistakes
16. Summary
17. Interview Questions
18. Assignment

---

# 1. Introduction

Up to now, you've learned:

* LLMs
* Prompt Engineering
* APIs
* Security
* Authentication

Now we'll actually build software.

Our application architecture:

```text
Browser

↓

Express Backend

↓

OpenAI SDK

↓

AI Provider

↓

Response

↓

Browser
```

This is how thousands of production AI applications work.

---

# 2. Why Use an SDK?

Without SDK:

```text
Application

↓

Manual HTTPS

↓

Headers

↓

JSON

↓

Authentication

↓

Parsing

↓

Response
```

Lots of repetitive work.

---

With SDK:

```javascript
const response = await client.responses.create(...)
```

SDK automatically handles:

* HTTPS
* Authentication
* JSON
* Serialization
* Response parsing
* Errors

---

Advantages

* Less code
* Better readability
* Easier maintenance
* Built-in updates
* Streaming support
* Better developer experience

---

# 3. Project Structure

A beginner project:

```text
ai-chatbot/

│

├── package.json

├── .env

├── server.js
```

---

A production project:

```text
ai-chatbot/

│

├── src/

│   ├── config/

│   ├── controllers/

│   ├── routes/

│   ├── services/

│   ├── middleware/

│   ├── utils/

│   ├── prompts/

│   └── app.js

│

├── server.js

├── .env

├── package.json

└── README.md
```

Notice that prompts are separated into their own directory.

---

# 4. Installing Dependencies

Initialize project:

```bash
npm init -y
```

Install Express:

```bash
npm install express
```

Install dotenv:

```bash
npm install dotenv
```

Install the OpenAI SDK:

```bash
npm install openai
```

Project now has:

```text
Express

+

dotenv

+

OpenAI SDK

+

Node.js
```

---

# 5. Environment Configuration

Create:

```text
.env
```

Example:

```env
OPENAI_API_KEY=your_api_key_here
PORT=5000
```

Never commit `.env`.

Create:

```text
.gitignore
```

Include:

```text
node_modules

.env
```

---

Load environment variables:

```javascript
import dotenv from "dotenv";

dotenv.config();
```

Access variables:

```javascript
process.env.OPENAI_API_KEY
```

---

Workflow:

```text
.env

↓

dotenv

↓

process.env

↓

Application
```

---

# 6. Creating Your First AI Client

Install SDK first.

Create client:

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});
```

The client object manages communication with the AI provider.

Architecture:

```text
Application

↓

Client Object

↓

HTTPS

↓

Provider
```

---

# 7. Making Your First AI Request

Basic request:

```javascript
const response = await client.responses.create({
  model: "gpt-5.5",
  input: "Explain REST APIs in simple words.",
});

console.log(response.output_text);
```

Let's understand every part.

---

## model

Determines which AI model processes your request.

---

## input

The prompt sent to the model.

---

## response

Contains:

* Generated text
* Metadata
* Usage information
* Other response fields depending on the request

---

Flow:

```text
Input

↓

SDK

↓

HTTPS

↓

LLM

↓

Generated Output

↓

JavaScript Object
```

---

# 8. Understanding Responses

Conceptually, a response object contains:

```text
Response

│

├── Output

├── Usage

├── Metadata

└── Status
```

Example:

```javascript
console.log(response.output_text);
```

Prints:

```text
REST API stands for...
```

Applications may also inspect additional response fields depending on their needs.

---

# 9. Async/Await

AI requests are asynchronous.

Bad:

```javascript
const response = client.responses.create(...);
```

Good:

```javascript
const response = await client.responses.create(...);
```

Why?

Because the request travels across the internet.

Workflow:

```text
Application

↓

Send Request

↓

Wait

↓

Receive Response

↓

Continue
```

Without async/await, your program won't correctly wait for the response.

---

# 10. Error Handling

Network requests can fail.

Example:

```javascript
try {
  const response = await client.responses.create({
    model: "gpt-5.5",
    input: "Hello",
  });

  console.log(response.output_text);

} catch (error) {
  console.error(error.message);
}
```

Possible failures:

* Invalid API key
* Rate limits
* Network timeout
* Invalid request
* Service unavailable

Never assume every request succeeds.

---

# 11. Building an Express API

Instead of exposing your API key,

the frontend calls your backend.

Architecture:

```text
Browser

↓

POST /chat

↓

Express

↓

OpenAI

↓

Express

↓

Browser
```

Example:

```javascript
import express from "express";

const app = express();

app.use(express.json());

app.post("/chat", async (req, res) => {

    const { message } = req.body;

    try {

        const response = await client.responses.create({
            model: "gpt-5.5",
            input: message,
        });

        res.json({
            reply: response.output_text,
        });

    } catch (err) {

        res.status(500).json({
            error: "Something went wrong",
        });

    }

});
```

The browser never communicates directly with the AI provider.

---

# 12. Streaming Responses

Normal request:

```text
User

↓

Wait

↓

Entire Response
```

Streaming:

```text
User

↓

First Tokens

↓

More Tokens

↓

More Tokens

↓

Completed Response
```

Streaming creates a much more responsive experience for chat interfaces.

The SDK typically provides streaming helpers that let your application process generated content incrementally.

Benefits:

* Better UX
* Faster perceived speed
* Real-time updates

---

# 13. Production Project Structure

Professional AI applications separate responsibilities.

```text
src/

│

├── config/

│      openai.js

│

├── controllers/

│      chatController.js

│

├── services/

│      aiService.js

│

├── prompts/

│      supportPrompt.js

│      codingPrompt.js

│

├── middleware/

│      auth.js

│

├── routes/

│      chatRoutes.js

│

└── app.js
```

Responsibilities:

Config

↓

Create SDK client

---

Service

↓

Talk to AI

---

Controller

↓

Handle request

---

Route

↓

Map URL

---

Middleware

↓

Authentication

---

This separation improves maintainability and testability.

---

# 14. Best Practices

### Use one shared SDK client.

Avoid creating a new client for every request.

---

### Keep prompts outside controllers.

Store reusable prompts in dedicated modules.

---

### Validate user input.

Reject invalid requests before calling the AI.

---

### Handle errors gracefully.

Return meaningful API responses.

---

### Log requests responsibly.

Track latency and failures without exposing sensitive information.

---

### Use environment variables.

Never hardcode secrets.

---

### Keep AI logic inside services.

Controllers should remain lightweight.

---

# 15. Common Mistakes

## Hardcoding API keys

Security risk.

---

## Calling AI directly from frontend

Exposes credentials.

---

## Ignoring async

Requests will not behave correctly.

---

## No input validation

Users may send invalid data.

---

## Creating huge controller files

Business logic belongs in services.

---

## Mixing prompts with routing logic

Makes maintenance difficult.

---

# 16. Summary

In this lesson, you built the foundation of a production AI backend.

You learned:

* SDK installation
* Environment configuration
* Client creation
* Making requests
* Reading responses
* Async programming
* Error handling
* Express integration
* Streaming concepts
* Project organization

This is the starting point for almost every Node.js AI application.

---

# 17. Interview Questions

## Beginner

1. Why use an SDK instead of raw HTTP?
2. What is the purpose of `.env`?
3. Why is `await` required?
4. Why should the frontend not call the AI provider directly?

---

## Intermediate

5. Explain the role of the OpenAI client.
6. Describe the request lifecycle in an Express application.
7. How should errors be handled?
8. Why separate controllers from services?

---

## Advanced

9. Design a production AI backend architecture.
10. Explain how streaming improves UX.
11. Describe how you would organize prompts in a large enterprise application.

---

# 18. Assignment

## Theory

1. Explain the role of the JavaScript SDK.
2. Describe the structure of a production AI backend.
3. Explain async/await in the context of API requests.
4. Discuss why environment variables are essential.

---

## Practical Exercise

Build a simple AI chatbot backend with:

* Express.js
* `.env`
* OpenAI JavaScript SDK
* `/chat` endpoint
* Input validation
* Error handling
* Logging
* Service layer
* Prompt module
* Controller
* Route separation

Draw the complete request lifecycle and explain how each component contributes to security, maintainability, and scalability.

---

# Key Takeaways

* The **JavaScript SDK** abstracts away low-level HTTP communication, making AI integration simpler and safer.
* A production AI backend should separate **configuration, routing, controllers, services, prompts, and middleware** into distinct modules.
* **Async/await** is essential because AI API calls involve network communication and are inherently asynchronous.
* The frontend should communicate only with your backend, which securely manages API credentials and business logic.
* Reusable prompts, centralized error handling, and proper project organization are critical for building maintainable AI applications.

---

# 🏗 AI Engineering Deep Dive

A beginner's AI project often looks like this:

```text
server.js

↓

Everything
```

As the project grows, this quickly becomes difficult to maintain.

A scalable architecture evolves into:

```text
                Browser
                   │
                   ▼
            Express Router
                   │
                   ▼
             Chat Controller
                   │
                   ▼
               AI Service
                   │
          ┌────────┴────────┐
          ▼                 ▼
   Prompt Builder     OpenAI Client
          │                 │
          └────────┬────────┘
                   ▼
              AI Provider
                   │
                   ▼
            Response Formatter
                   │
                   ▼
              JSON Response
```

This layered architecture makes it easier to:

* Replace AI providers
* Add caching
* Add retrieval (RAG)
* Introduce tool calling
* Write automated tests
* Scale individual components

This is the architectural style used in many production AI systems.

---

# 📖 Next Lesson (3.5)

## **Understanding Requests, Responses, Parameters & Model Configuration**

In the next lesson, you'll learn how to control AI behavior through API parameters, including:

* Request anatomy
* Input vs instructions
* Model selection
* Temperature
* Top-p
* Max output tokens
* Stop sequences
* Structured outputs
* Metadata
* Response objects
* Usage statistics
* Cost calculation
* Choosing the right configuration for different applications

This lesson teaches you how to fine-tune AI API requests for **quality, consistency, latency, and cost**, making it one of the most practical parts of AI application development.
