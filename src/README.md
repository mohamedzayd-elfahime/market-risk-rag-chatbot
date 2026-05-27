# Reference Implementation

This folder contains the chatbot implementation extracted from the larger Market Risk Forecasting AI Dashboard project.

It preserves the original package structure under `src/backend/` so the main orchestration code remains readable:

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

The generated Chroma vector database is intentionally not included. Rebuild it from the RAG documents when integrating the implementation in an application.

The code is provided as a reference implementation for the architecture described in `docs/`. It is not packaged as a standalone product server in this repository; the full runnable dashboard lives in the companion application repository.
