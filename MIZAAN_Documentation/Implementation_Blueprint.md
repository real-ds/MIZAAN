# MIZAAN | Implementation Blueprint

This document turns the architecture into an implementation-oriented module map.

## Backend

```text
backend/
├── app/
│   ├── api/
│   ├── agents/
│   │   ├── graph.py
│   │   ├── state.py
│   │   └── nodes/
│   ├── tools/
│   ├── retrieval/
│   │   ├── router.py
│   │   ├── structured.py
│   │   ├── fulltext.py
│   │   ├── hierarchical.py
│   │   ├── semantic.py
│   │   └── fusion.py
│   ├── ingestion/
│   ├── guardrails/
│   ├── evaluation/
│   ├── models/
│   ├── repositories/
│   ├── services/
│   └── config.py
├── tests/
└── migrations/
```

## Frontend

```text
frontend/
├── app/
├── components/
├── features/
│   ├── chat/
│   ├── stocks/
│   ├── investor-profile/
│   └── evidence/
└── lib/
```

## Retrieval Interface

All retrieval implementations should satisfy one conceptual interface:

```python
retrieve(query_plan) -> list[Evidence]
```

The agent should not know whether evidence came from SQL, FTS, hierarchy or embeddings.

## Evidence Contract

Every evidence item should contain:

- source ID
- stock/ticker
- source type
- timestamp
- content reference
- retrieval method
- score
- metadata

## Repository Boundary

The agent must never construct raw SQL directly.

Use:

`Agent Tool → Service → Repository → Database`

## LLM Boundary

Use:

`Agent Node → LLM Service → Provider`

This makes model/provider changes easier and improves testing.

## Deterministic vs Probabilistic Components

### Deterministic

- database queries
- filters
- ranking formulas
- date calculations
- currency normalization
- citation mapping
- deduplication
- idempotency

### Probabilistic

- intent classification
- sentiment extraction
- event extraction
- semantic query understanding
- response generation

The architecture should minimize probabilistic operations when deterministic computation is sufficient.
