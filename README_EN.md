# Building Systems with ChatGPT API: Architectural Patterns & Best Practices

This repository outlines the best practices for developing production-ready systems based on Large Language Models (LLMs), following the curriculum developed by **DeepLearning.AI** and **OpenAI**.

### 📋 Table of Contents
1. [LLM Architecture Overview](#1-llm-architecture-overview)
2. [Chat Format & Security](#2-chat-format--security)
3. [Evaluating Inputs](#3-evaluating-inputs)
4. [Processing Methods: CoT & Chaining](#4-processing-methods)
5. [Checking Outputs](#5-checking-outputs)
6. [Evaluation Strategies](#6-evaluation-strategies)
7. [Case Study: Customer Support Assistant](#7-case-study)

---

### 1. LLM Architecture Overview
Modern LLMs are trained to predict the next token. It is crucial to distinguish between:

*   **Base LLM:** Trained on massive datasets to complete text.
*   **Instruction Tuned LLM:** Optimized via SFT and RLHF to follow specific user commands.

#### Tokenization
*   1 token ≈ 4 characters (English).
*   Context window limits (e.g., 4000 tokens for GPT-3.5) include both input and output.

### 2. Chat Format & Security
Dialogue management relies on three distinct roles:
*   `System`: Sets global behavior and constraints.
*   `User`: The current user query.
*   `Assistant`: Stores previous model responses to maintain context.

> **Security Tip:** Never hardcode API keys. Use `.env` files and the `python-dotenv` library for environment variable management.

### 3. Evaluating Inputs
*   **Classification:** Categorizing queries (e.g., `billing`, `technical_support`) using JSON for programmatic routing.
*   **Moderation API:** A free tool to check inputs for hate speech, violence, or self-harm.
*   **Prompt Injection Prevention:** Using delimiters (e.g., `####`) and verification prompts to detect attempts to bypass system instructions.

### 4. Processing Methods
#### Chain of Thought (CoT)
Forcing the model to generate intermediate reasoning steps. Use an "Inner Monologue" approach to hide technical reasoning from the end-user.

#### Chaining Prompts
Breaking complex tasks into a sequence of simpler, modular prompts.
*   **Benefits:** Lower latency, reduced costs, easier debugging, and seamless API integration between steps.

### 5. Checking Outputs
Before displaying the result:
1. Perform a second moderation check.
2. **Fact-Verification:** Compare the assistant's response against the provided context to prevent hallucinations.

### 6. Evaluation Strategies
Development is iterative:
*   **Rubric-based Evaluation:** Using a stronger model (like GPT-4) to grade responses based on specific criteria.
*   **OpenAI Evals:** An open-source framework for comparing model outputs against "gold standard" human answers.

### 7. Case Study: Customer Support Assistant
A standard system pipeline:
1. `Input Moderation` → 2. `Classification` → 3. `Information Retrieval` → 4. `Response Generation` → 5. `Output Verification`.

---
### 💡 Key Takeaways
*   Treat the LLM as a **Reasoning Agent**, not a static database.
*   Modular chains of short, specialized prompts are more reliable than a single "super-prompt."
*   System quality is defined by systematic evaluation, not subjective impressions.