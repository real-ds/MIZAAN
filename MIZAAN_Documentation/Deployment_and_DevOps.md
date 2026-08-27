# MIZAAN | Deployment & DevOps

## 1. Deployment Philosophy

MIZAAN follows a **free-tier / zero-cost-first cloud architecture** for the current release.

There is no AWS dependency and no local LLM inference.

> Provider quotas and free-tier policies can change. The system is designed for low-cost operation and provider portability rather than assuming unlimited free usage.

## 2. Production Topology

```text
                     Internet
                        │
                        ▼
              ┌──────────────────┐
              │ Vercel Frontend  │
              │ Next.js / React  │
              └────────┬─────────┘
                       │ HTTPS
                       ▼
              ┌──────────────────┐
              │ Render Backend   │
              │ FastAPI          │
              │ GET /health      │
              └───────┬──────────┘
                      │
          ┌───────────┼────────────────────┐
          ▼           ▼                    ▼
       Neon DB     Upstash Redis       LLM Router
                                          ├─ Groq API
                                          ├─ OpenRouter API
                                          └─ Hugging Face API
```

## 3. Frontend — Vercel

- Next.js
- React
- TypeScript
- Tailwind CSS

```env
NEXT_PUBLIC_API_URL=https://<render-backend>.onrender.com
```

## 4. Backend — Render

- Python
- FastAPI
- Uvicorn
- Pydantic
- LangGraph
- SQLAlchemy
- Alembic

### Mandatory endpoint

```http
GET /health
```

Response:

```json
{
  "status": "ok",
  "service": "mizaaan-api",
  "version": "0.1.0"
}
```

`/health` must not call an LLM.

Optional:

```http
GET /ready
```

may validate database and Redis connectivity.

## 5. Database — Neon PostgreSQL

PostgreSQL stores:

- users
- stocks
- fundamentals
- news
- events
- investor profiles
- conversations
- citations
- ingestion jobs
- evaluations

## 6. Cache — Upstash Redis

Redis is used for:

- LLM response cache
- query cache
- fundamentals cache
- news cache
- rate limiting
- short-lived state
- distributed locks

Redis is not authoritative.

## 7. LLM Cloud Stack

### Primary

**Groq API**

### Free model gateway

**OpenRouter API**

Default configurable free route:

```env
OPENROUTER_FREE_MODEL=openrouter/free
```

### Fallback

**Hugging Face Inference API**

The application accesses all providers through a common abstraction.

## 8. LLM Failover

```text
Groq
  ↓ failure/rate-limit/timeout
OpenRouter
  ↓ unavailable/quota exhausted
Hugging Face
  ↓ unavailable
Graceful failure
```

Provider/model selection must be configurable.

## 9. Docker

Containerize the FastAPI backend.

```text
Dockerfile
  ↓
Docker Image
  ↓
Render Web Service
```

## 10. CI/CD

GitHub Actions:

```text
Push / Pull Request
        ↓
Lint
        ↓
Type Check
        ↓
Unit Tests
        ↓
Integration Tests
        ↓
RAG / Agent Evaluation
        ↓
Docker Build
        ↓
Deploy
        ↓
Smoke Test /health
```

Frontend:

```text
GitHub → Vercel
```

Backend:

```text
GitHub → Render
```

## 11. Health Smoke Test

```bash
curl https://<render-backend>.onrender.com/health
```

CI should fail when this endpoint is unavailable after deployment.

## 12. Database Migrations

Use Alembic.

```text
Deploy
  ↓
Run migration
  ↓
Start application
  ↓
/health smoke test
```

## 13. Free-Tier Engineering Rules

- Cache provider responses.
- Minimize LLM calls.
- Batch ingestion.
- Deduplicate before LLM processing.
- Use deterministic filtering/ranking.
- Index database queries.
- Use embeddings selectively.
- Bound background workloads.
- Use provider failover.
- Monitor provider quotas.

## 14. No AWS / No Local LLM

AWS deployment and local LLM inference are intentionally excluded from the current release.
