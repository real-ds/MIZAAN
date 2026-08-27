# MIZAAN | Development & Testing Plan

## 1. Development Method

Use incremental vertical slices:

1. Data model
2. Ingestion
3. Retrieval
4. Agent
5. Guardrails
6. Evaluation
7. UI
8. Deployment

Each phase must produce a runnable increment.

## 2. Branching

Suggested:

- `main` — production
- `develop` — integration
- `feature/*` — feature development
- `fix/*` — bug fixes

## 3. Testing Pyramid

### Unit Tests
Test:

- ticker normalization
- financial-value parsing
- metadata extraction
- date handling
- investor rule evaluation
- ranking functions
- citation validation
- deduplication
- idempotency keys

### Integration Tests
Test:

- API → database
- ingestion → PostgreSQL
- retrieval → evidence objects
- LangGraph → tools
- authentication → protected endpoints

### Agent Tests
Test:

- intent classification
- correct tool selection
- invalid tool recovery
- multi-step workflows
- missing evidence behavior

### RAG Tests
Test:

- Recall@K
- Precision@K
- MRR
- nDCG
- context precision
- context recall
- faithfulness
- answer relevancy

### Safety Tests
Test:

- unsupported numerical claims
- fabricated citations
- ticker confusion
- stale evidence
- prompt injection
- malicious tool requests
- conflicting evidence

### Performance Tests
Measure:

- p50/p95 retrieval latency
- p50/p95 end-to-end latency
- database query latency
- ingestion throughput
- LLM calls per query
- token consumption

## 4. Regression Suite

Maintain a fixed benchmark of representative financial questions.

Every production change should run the benchmark and compare results with the previous baseline.

## 5. Definition of Done

A feature is complete when:

- unit tests pass;
- integration tests pass;
- relevant agent evaluations pass;
- guardrails are covered;
- logs/metrics exist;
- documentation is updated;
- deployment configuration is reproducible.
