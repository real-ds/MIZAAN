# MIZAAN | AI Stock Analyzer

## Production-Grade Agentic RAG for Indian Equity Intelligence

MIZAAN is a production-oriented Agentic AI platform for Indian equity research. It combines structured financial data, financial news, investor memory, strategy-aware retrieval, evidence verification, guardrails, evaluation, and cloud-hosted open-weight LLMs.

## Final Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js / React / TypeScript |
| Frontend Hosting | Vercel |
| Backend | Python / FastAPI |
| Backend Hosting | Render |
| Health Check | `GET /health` |
| Database | PostgreSQL / Neon |
| Cache | Redis / Upstash |
| Agent Orchestration | LangGraph |
| Primary LLM | Groq API |
| LLM Gateway / Free Models | OpenRouter API |
| OpenRouter Routing | `openrouter/free` + selected free models |
| LLM Fallback | Hugging Face Inference API |
| RAG | Structured-first / vector-optional |
| Semantic Retrieval | Optional hosted embeddings |
| Authentication | Google OAuth |
| Containerization | Docker |
| CI/CD | GitHub Actions |
| Migrations | Alembic |

### Cloud-only inference

MIZAAN has **no local LLM runtime**. Every LLM/embedding inference call is made through a hosted API.

## LLM Reliability Architecture

```text
                    MIZAAN Agent
                         │
                         ▼
                  LLM Provider Router
                         │
              ┌──────────┼───────────┐
              ▼          ▼           ▼
          Groq API   OpenRouter   Hugging Face
                       API           API
                         │
              ┌──────────┼───────────────┐
              ▼          ▼               ▼
         openrouter/free  Selected     Hosted
         dynamic routing  free models  models
```

OpenRouter is used as a unified gateway for free/open-weight models. The model roster is deliberately configurable because free-model availability and quotas can change.

## Recommended OpenRouter Free Model Policy

MIZAAN should support the OpenRouter free pool through:

```text
openrouter/free
```

and allow explicit model IDs to be configured for benchmarked workloads.

The exact free model list must be discovered/configured at deployment time rather than hard-coded permanently.

## Fallback Policy

Recommended runtime order:

```text
Groq
  ↓ timeout / rate-limit / provider failure
OpenRouter
  ↓ unavailable / quota exhausted
Hugging Face
  ↓ unavailable
Graceful failure
```

A single agent request should not silently mix incompatible model outputs within one critical reasoning step.

## Core Retrieval Principle

> Use deterministic retrieval for exact financial information, semantic retrieval when language ambiguity requires it, and evidence verification/guardrails before generation.

## Main Capabilities

- NSE/BSE equity research
- Fundamentals and financial-news ingestion
- Investor persona memory
- Agentic query planning and tool selection
- Hierarchical and metadata-aware retrieval
- Optional semantic/embedding fallback
- Evidence verification and citation validation
- Financial anti-hallucination guardrails
- Automated RAG/agent/safety evaluation
- Vercel + Render deployment
- PostgreSQL persistence
- Redis caching
- Cloud-only LLM inference

## Important Rule

Do not publish numerical evaluation results, latency improvements, accuracy, dataset sizes, or cost savings until they are actually measured.
