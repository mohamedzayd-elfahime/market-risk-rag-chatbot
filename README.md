# Market Risk RAG Chatbot

Architecture reference for a controlled local RAG chatbot designed for market risk dashboards.

This repository focuses on the AI assistant layer: intent routing, retrieval-augmented generation, dashboard-state grounding, prompt construction, response policies, and post-generation guardrails. The design comes from a larger end-to-end risk platform where the chatbot explains forecasts, VaR, Expected Shortfall, volatility regimes, backtests, and model outputs without giving investment advice.

## Why This Project Matters

Financial dashboards often expose complex metrics, but users still need help interpreting them correctly. A generic LLM can easily hallucinate numbers, confuse metrics, or produce advice that should not be given.

This architecture treats the chatbot as a controlled decision-support interface:

- it grounds answers in live dashboard state and curated documentation;
- it routes questions with lightweight embeddings before generation;
- it retrieves only relevant risk knowledge through vector search;
- it constrains the LLM with explicit response policies;
- it validates answers before returning them to the user.

## Core Capabilities

- Local LLM integration through Ollama or another private inference backend.
- Lightweight embedding-based intent routing.
- Vector RAG over curated market risk documentation.
- Dashboard-state grounding for exact metrics displayed to the user.
- Prompt builder that combines policy, context, history, and retrieved passages.
- Guardrails against hallucinated metrics, financial advice, and VaR/ES confusion.
- Streaming-compatible response lifecycle.

## Architecture Overview

```text
User question
  -> chat API
  -> question/history validation
  -> embedding intent router
  -> routed context builder
  -> dashboard state + vector RAG
  -> response policy
  -> prompt builder
  -> local LLM
  -> answer repair + guardrails
  -> final controlled response
```

## Repository Structure

```text
docs/
  ARCHITECTURE.md       Complete chatbot system architecture
  RAG_PIPELINE.md       Offline indexing and runtime retrieval flow
  INTENT_ROUTING.md     Lightweight embedding intent classifier
  GUARDRAILS.md         Safety and answer validation layer
  DIAGRAMS.md           Mermaid architecture diagrams

examples/
  sample_context_payload.json
  sample_policy.json
  sample_prompt.txt
```

## Design Principles

1. Keep the model local when possible.
2. Use retrieval for domain knowledge, not for live dashboard numbers.
3. Treat displayed dashboard metrics as the highest-priority source of truth.
4. Avoid pure keyword matching for intent detection.
5. Let the LLM explain, but not invent.
6. Validate the final answer before sending it to the UI.
7. Refuse personalized buy/sell/allocation advice.

## Related Use Cases

This architecture can be adapted to:

- market risk dashboards;
- portfolio analytics tools;
- VaR/ES monitoring systems;
- model validation assistants;
- quantitative research dashboards;
- internal financial analytics copilots.

## Limitations

This repository documents an architecture pattern. It is not a financial advisor and does not provide investment recommendations. Any implementation should be validated against the target institution's data governance, compliance, model risk, and security requirements.

## License

MIT.
