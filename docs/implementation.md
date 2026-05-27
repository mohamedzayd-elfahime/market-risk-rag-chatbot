# Reference Implementation

The `src/backend/` folder contains the implementation extracted from the larger Market Risk Forecasting AI Dashboard project.

```text
src/backend/
  api/routes/chat.py          FastAPI chat endpoints
  chatbot/                    Turn orchestration, intent routing, prompts, RAG, guardrails
  core/                       Configuration and path helpers
  dashboard_state/            Context builders for forecast/backtest/dashboard outputs
  llm/                        Local Ollama client
  schemas/                    Pydantic request/response contracts
  services/chat_service.py    Stable service facade
```

The generated Chroma vector database is intentionally not committed. It should be rebuilt from the curated RAG documents when integrating this implementation into an application.

## Important Files

- `service.py`: orchestrates one chatbot turn.
- `intent_router.py`: routes questions with lightweight embeddings.
- `rag/retriever.py`: retrieves relevant context from Chroma.
- `prompt_builder.py`: builds the final prompt.
- `response_policy_router.py`: defines answer constraints.
- `response_guardrails.py`: validates and corrects unsafe or unsupported outputs.
- `local_ollama_client.py`: calls the local LLM backend.
