# MIZAAN | Development Roadmap

## Phase 0 — Foundation

- repository setup
- architecture
- coding standards
- environment configuration
- Docker
- CI pipeline

**Exit:** application builds and tests run.

## Phase 1 — Data Layer

- PostgreSQL schema
- stock universe
- fundamentals ingestion
- news ingestion
- normalization
- deduplication
- idempotency

**Exit:** one stock can be ingested repeatedly without duplicates.

## Phase 2 — Structured RAG

- metadata indexing
- PostgreSQL full-text search
- hierarchical document structure
- evidence objects
- retrieval router

**Exit:** factual financial questions return source-backed evidence.

## Phase 3 — Agent

- LangGraph state
- intent routing
- query planning
- retrieval tools
- analysis tools
- citation tool

**Exit:** multi-step agentic research works.

## Phase 4 — Investor Memory

- persona extraction
- structured preference storage
- deterministic screening
- personalized ranking

**Exit:** recommendations respect investor constraints.

## Phase 5 — Guardrails

- prompt-injection defense
- claim verification
- citation validation
- numerical checks
- refusal behavior

**Exit:** unsupported claims are blocked.

## Phase 6 — Evaluation

- benchmark dataset
- retrieval metrics
- RAG metrics
- agent metrics
- financial safety metrics
- performance metrics

**Exit:** automated evaluation report generated.

## Phase 7 — Semantic Fallback

- embedding model
- semantic retrieval
- optional reranking
- evidence fusion

**Exit:** ambiguous conceptual questions improve without replacing structured retrieval.

## Phase 8 — Production Cloud

- Terraform
- AWS
- ECS/RDS
- monitoring
- CI/CD
- smoke tests

**Exit:** reproducible production deployment.
