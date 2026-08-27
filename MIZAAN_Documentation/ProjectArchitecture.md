# MIZAAN | Project Architecture

## 1. Architecture Overview

MIZAAN uses a layered production architecture:

```text
                        ┌─────────────────────┐
                        │      Web Client     │
                        │   React / Next.js   │
                        └──────────┬──────────┘
                                   │ HTTPS
                                   ▼
                        ┌─────────────────────┐
                        │    API Gateway     │
                        │      FastAPI       │
                        └──────────┬──────────┘
                                   ▼
                        ┌─────────────────────┐
                        │     LangGraph      │
                        │  Agent Orchestrator│
                        └──────────┬──────────┘
                                   │
             ┌─────────────────────┼─────────────────────┐
             ▼                     ▼                     ▼
       Query Planner         Investor Memory       Analysis Tools
             │                     │                     │
             └─────────────────────┼─────────────────────┘
                                   ▼
                         Retrieval Orchestrator
                                   │
             ┌─────────────────────┼─────────────────────┐
             ▼                     ▼                     ▼
       SQL / Exact            FTS / BM25           Optional Embeddings
       Retrieval              Retrieval            Semantic Fallback
             │                     │                     │
             └─────────────────────┼─────────────────────┘
                                   ▼
                         Evidence Fusion/Ranking
                                   ▼
                         Verification + Guardrails
                                   ▼
                           LLM Generation
                                   ▼
                       Citation Validation
                                   ▼
                              Response
```

## 2. Data Architecture

```text
Sources
  ├── Fundamentals
  ├── Financial News
  ├── Prices
  └── Market Metadata
          ↓
     Ingestion Jobs
          ↓
  Normalize + Extract
          ↓
  Entity Resolution
          ↓
  Structured Knowledge
          ↓
  PostgreSQL
```

## 3. Core Services

### Frontend Service
React/Next.js application for authentication, watchlists, chat and evidence display.

### API Service
FastAPI service exposing authentication-aware endpoints, query endpoints, stock endpoints and evaluation/admin endpoints.

### Agent Service
LangGraph state machine responsible for query planning, tool selection, retrieval orchestration, verification and response generation.

### Ingestion Service
Scheduled/on-demand workers for fundamentals, news and price ingestion.

### Knowledge Store
PostgreSQL for users, investor profiles, stocks, financial metrics, news, events, citations, jobs and evaluation metadata.

### Retrieval Service
Provides structured, lexical, hierarchical and optional semantic retrieval.

### Evaluation Service
Runs benchmark queries and computes retrieval, generation, agent, safety and performance metrics.

## 4. LLM Provider Architecture

```text
LangGraph Agent
      ↓
LLM Provider Router
      ├── Groq API
      ├── OpenRouter API
      │      └── openrouter/free
      └── Hugging Face Inference API
```

All inference is cloud/API based.

## 5. Deployment Architecture

```text
Vercel
  │ HTTPS
  ▼
Render FastAPI
  ├── Neon PostgreSQL
  ├── Upstash Redis
  └── LLM Router
        ├── Groq
        ├── Hugging Face
        └── cloud fallback provider (local)
```

Mandatory backend endpoint: `GET /health`


Current target:

```text
CloudFront
    ↓
API Gateway / Load Balancer
    ↓
ECS Fargate
 ┌───────────────┐
 │ Web/API       │
 │ Agent         │
 │ Worker        │
 └───────┬───────┘
         ↓
      RDS PostgreSQL
         ↓
   Backup / Monitoring
```

deployment configuration provisions infrastructure. GitHub Actions performs automated CI/CD.

## 5. Architectural Principles

1. Deterministic operations before LLM operations.
2. Structured retrieval before semantic retrieval when exactness matters.
3. LLMs should plan and synthesize, not perform database work that SQL can perform.
4. Every generated financial claim should be traceable to evidence.
5. Ingestion must be idempotent.
6. Agent tools must have strict contracts.
7. Evaluation is part of development, not an afterthought.
