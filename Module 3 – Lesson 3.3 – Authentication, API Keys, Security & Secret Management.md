# Module 3 – LLM APIs, SDKs & AI Application Development

# Lesson 3.3 – Authentication, API Keys, Security & Secret Management

> **Estimated Study Time:** 16–18 Hours
> **Difficulty:** Intermediate → Advanced
> **Learning Outcome:** Learn how to securely authenticate with AI APIs, manage API keys, protect secrets, prevent credential leaks, implement backend proxy architectures, use environment variables, rotate secrets, handle rate limits, and deploy production-grade AI applications securely.

---

# Table of Contents

1. Introduction
2. Authentication Fundamentals
3. API Keys
4. API Keys vs OAuth vs JWT
5. Environment Variables
6. Secret Management
7. Backend Proxy Architecture
8. Key Rotation
9. Rate Limits & Quotas
10. Preventing Secret Leakage
11. Secure Deployment
12. Enterprise Secret Management
13. Best Practices
14. Common Mistakes
15. Summary
16. Interview Questions
17. Assignment

---

# 1. Introduction

Imagine you purchase access to an AI API.

The provider gives you:

```text
sk-xxxxxxxxxxxxxxxxxxxxxxxx
```

That single string gives access to your account.

If someone steals it:

* They can use your quota.
* They can generate expensive API requests.
* They may access resources associated with your account.
* Your monthly bill could increase significantly.

An API key should be treated like a password.

---

# 2. Authentication Fundamentals

## Formal Definition

Authentication is the process of verifying the identity of a client before allowing access to protected resources.

---

## Simple Definition

Authentication answers:

> **Who are you?**

---

Example

```text
Application

↓

API Key

↓

AI Provider

↓

Verified

↓

Access Granted
```

---

Without authentication:

```text
Application

↓

Unknown Client

↓

Access Denied
```

---

# 3. API Keys

API Keys are the simplest authentication mechanism.

Example:

```text
Application

↓

API Key

↓

Provider

↓

Model
```

The key identifies:

* Account
* Organization
* Billing
* Permissions

---

Typical request flow:

```text
Frontend

↓

Backend

↓

Attach API Key

↓

HTTPS Request

↓

Provider
```

Notice:

The **backend** attaches the key.

The browser never sees it.

---

# 4. API Keys vs OAuth vs JWT

These are often confused.

---

## API Key

Purpose:

Authenticate an application.

Example:

```text
Backend

↓

API Key

↓

OpenAI
```

---

## OAuth

Purpose:

Allow one application to access another user's account with permission.

Example:

```text
User

↓

Google Login

↓

Permission

↓

Application
```

Used for:

* Google Login
* GitHub Login
* Microsoft Login

---

## JWT (JSON Web Token)

Purpose:

Authenticate users inside your own application.

Example:

```text
User Login

↓

JWT

↓

Protected API
```

---

Comparison

| Technology | Purpose                      |
| ---------- | ---------------------------- |
| API Key    | Authenticate applications    |
| OAuth      | Authorize third-party access |
| JWT        | Authenticate users           |

They solve different problems.

---

# 5. Environment Variables

Never write secrets directly in code.

Bad:

```javascript
const API_KEY = "sk-123456789";
```

If pushed to GitHub,

the secret becomes public.

---

Correct approach:

```text
.env

↓

API_KEY=sk-xxxxxxxx
```

Application:

```javascript
process.env.API_KEY
```

---

Workflow:

```text
Environment Variables

↓

Application Startup

↓

Configuration

↓

API Client
```

Benefits:

* Easier deployment
* Better security
* Different keys per environment

---

# 6. Secret Management

Large organizations rarely use `.env` files in production.

Instead they use dedicated secret managers.

Architecture:

```text
Application

↓

Secret Manager

↓

Retrieve API Key

↓

Memory

↓

API Call
```

Benefits:

* Encryption
* Access control
* Audit logs
* Rotation
* Central management

---

Examples of secrets:

* API keys
* Database passwords
* JWT signing keys
* OAuth credentials
* Encryption keys

---

# 7. Backend Proxy Architecture

One of the most important production concepts.

❌ Wrong architecture:

```text
Browser

↓

OpenAI API
```

Problem:

API key exposed.

---

✅ Correct architecture:

```text
Browser

↓

Backend

↓

OpenAI API
```

The backend:

* Stores the API key
* Authenticates users
* Validates requests
* Applies business logic
* Logs usage
* Enforces limits

The frontend never knows the provider credentials.

---

Example request flow:

```text
User

↓

Frontend

↓

POST /api/chat

↓

Backend

↓

OpenAI API

↓

Backend

↓

Frontend
```

---

# 8. Key Rotation

Secrets should not remain unchanged forever.

Rotation process:

```text
Generate New Key

↓

Update Secret Store

↓

Deploy

↓

Verify

↓

Disable Old Key
```

Benefits:

* Reduced exposure
* Easier recovery after leaks
* Better compliance

---

Example schedule:

* Every 90 days
* Immediately after suspected compromise
* When employees leave
* After infrastructure incidents

---

# 9. Rate Limits & Quotas

AI providers protect their infrastructure.

Typical limits:

* Requests per minute
* Tokens per minute
* Daily quota
* Monthly spending limits

Workflow:

```text
Request

↓

Rate Limit Check

↓

Allowed?

↓

Yes → Continue

No → Reject / Retry
```

---

Application strategies:

* Retry with backoff
* Queue requests
* Cache responses
* Notify users

---

# 10. Preventing Secret Leakage

Common leak sources:

* GitHub
* Client-side JavaScript
* Screenshots
* Logs
* Chat messages
* Documentation

---

Example mistake:

```javascript
console.log(process.env.API_KEY);
```

Logs may expose secrets.

---

Another mistake:

```javascript
return {
  apiKey: process.env.API_KEY
};
```

Never return secrets to clients.

---

Checklist:

* Ignore `.env` in Git
* Never hardcode keys
* Never expose secrets in APIs
* Review logs
* Rotate compromised keys immediately

---

# 11. Secure Deployment

Development:

```text
.env.local
```

Production:

Use deployment platform secret storage.

Architecture:

```text
Cloud Platform

↓

Encrypted Secrets

↓

Application

↓

Runtime

↓

LLM API
```

Examples of secure environments:

* Docker secrets
* Kubernetes secrets
* Cloud secret managers
* CI/CD secret stores

---

# 12. Enterprise Secret Management

Large organizations often use centralized secret infrastructure.

Architecture:

```text
Developers

↓

CI/CD

↓

Secret Manager

↓

Application

↓

Runtime

↓

Provider
```

Features:

* Encryption at rest
* Encryption in transit
* Role-based access
* Audit logs
* Automatic rotation
* Version history

---

# 13. Best Practices

### Treat API keys like passwords.

---

### Store secrets outside source code.

---

### Use backend proxy architecture.

---

### Rotate secrets regularly.

---

### Limit permissions where possible.

Use the principle of least privilege.

---

### Monitor unusual usage.

Unexpected spikes may indicate compromised credentials.

---

### Separate environments.

Use different secrets for:

* Development
* Testing
* Staging
* Production

---

### Validate incoming requests.

Never trust client input directly.

---

# 14. Common Mistakes

## Hardcoding API keys

Most common beginner mistake.

---

## Committing `.env` to Git

Secrets become public.

---

## Using one key everywhere

Separate keys improve security and operational control.

---

## Exposing keys in frontend bundles

Anyone can inspect browser JavaScript.

---

## Ignoring rate limits

Causes failed requests and poor user experience.

---

## Logging sensitive data

Logs often persist longer than expected.

---

# 15. Summary

Security is foundational to production AI systems.

Professional applications:

* Store secrets securely.
* Authenticate through the backend.
* Rotate keys.
* Monitor usage.
* Handle rate limits.
* Separate environments.
* Protect user and provider credentials.

Good security practices prevent financial loss, service disruption, and unauthorized access.

---

# 16. Interview Questions

## Beginner

1. What is an API key?
2. Why should API keys never be stored in frontend code?
3. What is the purpose of `.env` files?
4. What is authentication?

---

## Intermediate

5. Compare API Keys, OAuth, and JWT.
6. Explain backend proxy architecture.
7. What is key rotation?
8. Why are rate limits important?

---

## Advanced

9. Design a secure AI backend architecture.
10. Explain enterprise secret management.
11. Describe how you would secure an AI SaaS platform handling thousands of users.

---

# 17. Assignment

## Theory

1. Explain authentication.
2. Compare API Keys, OAuth, and JWT.
3. Describe secure secret management.
4. Explain backend proxy architecture.

---

## Practical Thinking Exercise

You are building an **AI SaaS platform** similar to ChatGPT.

Design a security architecture that includes:

* User authentication
* JWT-based sessions
* API key storage
* Backend proxy
* Rate limiting
* Secret manager
* Request logging
* Monitoring
* Key rotation
* CI/CD deployment

Create an architecture diagram showing how requests flow from the user to the AI provider while keeping credentials secure.

---

# Key Takeaways

* **API keys authenticate applications**, while **OAuth** authorizes third-party access and **JWTs** authenticate users within your application.
* **Never expose API keys in frontend code**. Route AI requests through a secure backend that holds provider credentials.
* **Environment variables** are suitable for local development, while production systems should use dedicated **secret management** services.
* Regular **key rotation**, **rate limit handling**, and **monitoring** are essential parts of operating secure AI applications.
* Apply the **principle of least privilege**, separate environments, and avoid logging or exposing sensitive credentials.

---

# 🏗 AI Engineering Deep Dive

A secure production AI application typically follows this architecture:

```text
                    User
                      │
                      ▼
                Web / Mobile App
                      │
                 HTTPS (JWT)
                      │
                      ▼
                 Backend API
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
 User Auth     Rate Limiter    Request Validator
        │             │             │
        └─────────────┼─────────────┘
                      ▼
               Secret Manager
                      │
          Retrieve Provider API Key
                      │
                      ▼
                 AI API Client
                      │
                      ▼
                 AI Provider
                      │
                      ▼
                 AI Response
                      │
                      ▼
          Logging & Monitoring
                      │
                      ▼
                  User Response
```

Notice that **users authenticate to your backend**, not directly to the AI provider. Your backend authenticates to the provider using securely managed API credentials. This separation protects provider secrets, enables centralized monitoring, and allows you to enforce business rules before any AI request is made.

---

# 📖 Next Lesson (3.4)

## **JavaScript SDK, Node.js Integration & Your First AI Application**

In the next lesson, we'll start writing real code using **JavaScript (not TypeScript)**. Topics include:

* Installing the SDK
* Project structure
* Creating an AI client
* Making your first request
* Understanding request and response objects
* Environment configuration
* Async/await patterns
* Error handling
* Streaming responses
* Building a simple AI chatbot API with **Express.js**
* Production-ready project structure

This lesson marks the transition from **AI architecture** to **hands-on AI application development** using the JavaScript ecosystem.
