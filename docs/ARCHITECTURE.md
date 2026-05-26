# Chatbot Architecture

The chatbot is designed as a controlled local assistant for market risk dashboards. Its main role is to explain risk metrics and model outputs while staying grounded in trusted context.

## System Layers

```text
Presentation layer
  Dashboard chat panel or API client

API layer
  Chat endpoint, streaming endpoint, request validation

Orchestration layer
  Conversation turn lifecycle, history handling, response mode selection

Routing layer
  Lightweight embedding intent router

Context layer
  Dashboard state, model metadata, backtest summaries, vector RAG passages

Generation layer
  Prompt builder and local LLM backend

Safety layer
  Answer repair, guardrails, final validation
```

## Request Lifecycle

1. The user sends a question from the dashboard or an API client.
2. The API validates the request and normalizes the recent conversation history.
3. The intent router classifies the question using lightweight embeddings.
4. The context builder selects the smallest useful context for that intent.
5. The response policy defines what the assistant may or may not say.
6. The prompt builder combines instructions, context, retrieved passages, and history.
7. The local LLM generates an answer when generation is allowed.
8. The answer repair layer fixes terminology and formatting problems.
9. Guardrails detect unsupported numbers, advice, or metric confusion.
10. The final controlled response is returned to the UI.

## Context Priority

The chatbot uses a clear hierarchy of evidence:

```text
1. Live dashboard state
2. Structured backtest / forecast / metadata files
3. Curated RAG documentation
4. Conversation history
5. General language model knowledge
```

Live dashboard state is the strongest source because it reflects what the user is seeing. RAG is used for definitions, methodology, interpretation rules, and explanations.

## Main Components

| Component | Responsibility |
| --- | --- |
| Chat API | Receives chat requests and streams responses |
| Chat service | Orchestrates one conversation turn |
| Intent router | Classifies the question without calling the LLM |
| Context builder | Selects forecast, backtest, model, or RAG context |
| RAG retriever | Fetches relevant passages from a local vector database |
| Policy router | Builds constraints for each answer type |
| Prompt builder | Assembles the final model prompt |
| Local LLM client | Calls the local generation backend |
| Answer repair | Cleans common wording and metric issues |
| Guardrails | Blocks or corrects unsafe or unsupported answers |

## What Makes It Defensible

The system does not rely on one large prompt alone. It separates routing, retrieval, policy, generation, and validation. This makes the assistant easier to test, audit, and improve.
