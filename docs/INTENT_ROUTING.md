# Intent Routing

The intent router decides which context the chatbot needs before generation. It avoids sending every possible document and metric to the LLM.

## Why Not Pure Keyword Matching

Keyword matching is fast, but brittle. It can fail when users reformulate naturally:

```text
"Can you walk me through this risk result?"
"Why did the validation pass?"
"What does the tail loss mean here?"
```

A lightweight embedding router is more defensible because it compares semantic meaning, not only words.

## Recommended Approach

```text
Annotated examples per intent
  -> embedding model
  -> centroid per intent
  -> user question embedding
  -> cosine similarity
  -> best intent above threshold
```

This keeps routing:

- local;
- fast;
- auditable;
- testable;
- less brittle than keyword rules.

## Example Intents

| Intent | Meaning | Context |
| --- | --- | --- |
| help_request | User asks for guidance | Forecast + backtest summary |
| forecast_query | Forecast, VaR, ES, volatility | Forecast context |
| backtest_query | Kupiec, Christoffersen, violations, ES diagnostics | Backtest context |
| strategy_query | Economic backtest or simulated strategy | Economic context |
| model_query | Model architecture, training, artifacts | Model metadata + RAG |
| data_query | Dataset, source, preprocessing | Data metadata + RAG |
| definition_query | Conceptual explanation | RAG |
| out_of_scope | Unsupported request | Controlled fallback |

## Fallback

A lexical fallback can exist only for degraded operation when the embedding model is unavailable. It should not be the main routing strategy.
