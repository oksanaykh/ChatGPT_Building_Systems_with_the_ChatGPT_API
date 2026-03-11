# Building Systems with the ChatGPT API -- Course Report

This repository contains structured notes and best practices from the
course **"Building Systems with the ChatGPT API"** by **DeepLearning.AI
and OpenAI**.

The report summarizes architectural patterns and engineering practices
for integrating **Large Language Models (LLMs)** into production systems
with a focus on **safety, accuracy, and scalability**.

------------------------------------------------------------------------

# 1. Introduction to Large Language Models (LLMs)

Modern LLMs are algorithms trained using **supervised learning** to
repeatedly predict the next token in a sequence.

## Base LLM vs Instruction-Tuned LLM

  -----------------------------------------------------------------------
  Feature                 Base LLM                Instruction-Tuned LLM
  ----------------------- ----------------------- -----------------------
  Training principle      Predicts next word from Fine-tuned on
                          large datasets          instruction--response
                                                  pairs

  Optimization            Supervised Learning     RLHF (Reinforcement
                                                  Learning from Human
                                                  Feedback)

  Typical behavior        Continues text          Follows user
                          logically               instructions and aims
                                                  to be helpful
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## Tokenization and Context Limits

LLMs process **tokens**, not words.

Example:

`lollipop → l + oll + ipop`

Empirical rule:

-   **1 token ≈ 4 characters**
-   **1 token ≈ ¾ word**

Example context window:

-   **gpt-3.5-turbo ≈ 4000 tokens**

Tokens also determine **API cost**.

------------------------------------------------------------------------

# 2. Chat Format Architecture and Security

LLM systems use structured messages with roles.

### System

Defines assistant behavior and constraints.

### User

Contains the user query.

### Assistant

Stores previous responses and maintains conversation context.

------------------------------------------------------------------------

## API Key Security

Best practices:

1.  Never hardcode keys in scripts
2.  Store keys in `.env` files
3.  Load them using `python-dotenv`

------------------------------------------------------------------------

# 3. Evaluating Inputs

## Query Classification

Requests can be routed to different modules:

-   billing
-   technical support
-   product information

Using **JSON output** simplifies routing.

------------------------------------------------------------------------

## Content Moderation

The **OpenAI Moderation API** classifies content into categories such
as:

-   hate
-   violence
-   self-harm
-   sexual content

------------------------------------------------------------------------

## Preventing Prompt Injection

Example attack:

"Ignore previous instructions."

Strategies:

### Delimiters

    ####
    User input
    ####

### Detection Prompt

A separate prompt checks whether user input attempts to override system
instructions.

------------------------------------------------------------------------

# 4. Processing Inputs

## Chain of Thought

Encourages the model to generate intermediate reasoning steps.

### Inner Monologue Pattern

Applications hide reasoning from users and only show final answers.

------------------------------------------------------------------------

## Prompt Chaining

Complex tasks are split into smaller prompts.

Advantages:

-   better state management
-   lower cost
-   easier debugging
-   easier integration with external tools

------------------------------------------------------------------------

# 5. Checking Outputs

### Output Moderation

Verify response safety using the Moderation API.

### Fact Verification

Compare generated responses with trusted context to detect
hallucinations.

------------------------------------------------------------------------

# 6. Evaluation Strategies

Typical workflow:

1.  Prompt tuning
2.  Collect difficult cases
3.  Build a development dataset
4.  Run automated evaluation

------------------------------------------------------------------------

## Rubric-Based Evaluation

LLMs (often GPT‑4) can evaluate answers using criteria:

-   factual correctness
-   completeness
-   tone

------------------------------------------------------------------------

## OpenAI Evals

Responses are compared with a reference answer:

  Score   Meaning
  ------- ---------------------------------------
  A       Correct subset
  B       Correct but contains extra info
  C       Slightly different but mostly correct
  D/E     Factually incorrect

------------------------------------------------------------------------

# 7. Example System: Customer Support Assistant

Pipeline:

1.  Input moderation
2.  Intent classification
3.  Retrieval of product data
4.  Response generation
5.  Output moderation

------------------------------------------------------------------------

# 8. Key Takeaways

-   Prompt engineering reduces development time dramatically
-   Modular prompt chains are more reliable than single prompts
-   System evaluation must be systematic
-   LLMs should be treated as **reasoning agents**, not static knowledge
    bases
