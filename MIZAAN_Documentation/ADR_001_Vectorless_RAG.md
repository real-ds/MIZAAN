# ADR-001 | Structured-First / Vector-Optional RAG

## Status

Accepted

## Context

The original challenge specifies embeddings and a vector store for RAG. However, MIZAAN is intended to evolve beyond a learning-oriented implementation into a production-grade financial research system.

Financial information contains many exact, structured relationships:

- stock → metric
- metric → period
- stock → news
- article → event
- event → sentiment
- investor → constraint

Forcing every retrieval operation through vector similarity can reduce determinism for exact financial questions.

## Decision

MIZAAN will use a **structured-first, vector-optional RAG architecture**.

Retrieval order is determined by query characteristics:

1. SQL/exact structured retrieval
2. metadata filtering
3. full-text/BM25-style retrieval
4. hierarchical/entity retrieval
5. optional embedding retrieval
6. evidence fusion
7. verification
8. generation

## Why

### Advantages

- Exact numerical retrieval
- Better temporal correctness
- Lower unnecessary embedding/storage dependence
- Transparent retrieval paths
- Easier debugging
- Testable deterministic filters
- Efficient structured screening
- Semantic retrieval remains available for ambiguous queries

### Trade-off

Pure vector retrieval can be simpler for arbitrary semantic documents. Therefore embeddings remain an optional fallback rather than being prohibited.

## Consequences

### Positive

MIZAAN becomes retrieval-strategy-aware and better aligned with financial data.

### Negative

The retrieval layer becomes more complex and requires stronger query routing and evaluation.

## Rejected Alternative

**Vector DB for every document and query**

Rejected as the sole retrieval strategy because exact financial facts and deterministic constraints are better represented through structured retrieval.

## Validation

The decision must be validated experimentally by comparing:

- structured-only
- vector-only
- hybrid
- structured-first/vector-optional

using the same evaluation benchmark.


---

# ADR-002 | Free-Tier Deployment

## Status

Accepted

## Decision

Use:

- Vercel — frontend
- Render — FastAPI backend
- Neon PostgreSQL — database
- Upstash Redis — cache
- Groq — primary LLM provider
- Hugging Face — LLM fallback
- cloud fallback provider — additional cloud fallback
- GitHub Actions — CI/CD
- Docker — backend packaging

AWS is excluded from the current release.

## Mandatory operational requirement

The Render backend must expose:

`GET /health`

CI/CD must perform a post-deployment smoke test against this endpoint.

## Rationale

The project should demonstrate production engineering without requiring paid cloud infrastructure during the current development phase.


## Cloud-Only LLM Decision

All LLM inference is performed through hosted APIs. Local model runtimes are intentionally excluded. This keeps deployment architecture uniform between development, staging and production and allows the fallback chain to be tested against real provider APIs.


---

# ADR-003 | Cloud-Only Multi-Provider LLM Strategy

## Status

Accepted

## Decision

MIZAAN uses a cloud/API-only LLM architecture:

1. **Groq API** — primary interactive inference
2. **OpenRouter API** — free-model gateway and fallback
3. **Hugging Face Inference API** — secondary fallback

OpenRouter should default to:

`openrouter/free`

for workloads where automatic selection from the available free model pool is acceptable.

Explicit OpenRouter model IDs may be configured for deterministic evaluation and specialized workloads.

## Rationale

- avoids vendor lock-in;
- supports free/open-weight model access;
- provides provider failover;
- keeps development, staging and production architecture consistent;
- avoids local GPU/model-server requirements.

## Consequence

Provider quotas and model availability can change. The provider router and configuration layer must therefore remain dynamic.
