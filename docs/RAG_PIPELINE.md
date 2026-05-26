# Vector RAG Pipeline

The RAG layer provides stable domain knowledge for market risk concepts, model interpretation rules, dashboard usage, and common questions.

## Offline Indexing

```text
Curated documents
  -> cleaning
  -> section-aware splitting
  -> overlapping chunks
  -> lightweight embeddings
  -> normalized vectors
  -> local vector database
```

Recommended embedding model for low-latency local use:

```text
sentence-transformers/all-MiniLM-L6-v2
```

This model is small enough for CPU inference while remaining more robust than keyword matching for semantic retrieval.

## Runtime Retrieval

```text
User question
  -> intent router
  -> RAG needed?
  -> question embedding
  -> vector similarity search
  -> top-k passages
  -> formatted context
  -> prompt builder
```

## What Belongs In RAG

Good RAG content:

- definitions of VaR, Expected Shortfall, backtesting, and volatility regimes;
- model methodology;
- interpretation rules;
- dashboard usage guidance;
- common user questions;
- known limitations.

Bad RAG content:

- live forecast values;
- changing dashboard metrics;
- temporary logs;
- private local paths;
- raw notebooks;
- large intermediate datasets.

## RAG vs Dashboard State

RAG should explain concepts. Dashboard state should provide exact numbers.

Example:

```text
Dashboard state:
  VaR = -1.45%

RAG:
  VaR is a loss threshold at a selected confidence level.
```

The assistant should not retrieve or invent live numbers from static documents.
