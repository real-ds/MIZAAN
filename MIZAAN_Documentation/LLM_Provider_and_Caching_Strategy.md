# MIZAAN | LLM Provider, OpenRouter & Caching Strategy

## 1. Objective

MIZAAN uses **cloud-hosted LLM APIs only** and must remain operational when a provider becomes unavailable, rate-limited, slow, or quota constrained.

## 2. Provider Architecture

```text
LangGraph
   ↓
LLM Service
   ↓
Provider Router
   ├── Groq API
   ├── OpenRouter API
   │     └── openrouter/free / selected free models
   └── Hugging Face Inference API
```

All inference is performed over HTTPS APIs.

## 3. Provider Roles

### Groq — Primary

Use Groq for interactive agent requests where low latency is important.

### OpenRouter — Free Model Gateway

OpenRouter provides a unified API to multiple models/providers and exposes a free-model routing option.

Recommended default:

```text
openrouter/free
```

This allows MIZAAN to use an available free model without coupling the application to a single model.

For deterministic evaluation, explicit model IDs may be configured.

### Hugging Face — Fallback

Use Hugging Face Inference API as a separate fallback provider and for hosted open-weight models where appropriate.

## 4. OpenRouter Model Strategy

Do not permanently hard-code a single free model.

Instead, maintain:

```env
OPENROUTER_FREE_MODEL=openrouter/free
```

and optionally:

```env
OPENROUTER_BENCHMARK_MODEL=
OPENROUTER_REASONING_MODEL=
OPENROUTER_EXTRA_MODEL=
```

The exact model IDs should be selected after checking current availability, tool-calling support, context size, structured-output support and evaluation performance.

## 5. Failover

```text
Request
  ↓
Groq
  │ timeout / rate-limit / provider outage
  ▼
OpenRouter
  │ unavailable / quota exhausted
  ▼
Hugging Face
  │ unavailable
  ▼
Graceful failure
```

Fallback conditions:

- timeout
- rate-limit response
- provider outage
- transient 5xx
- configured quota exhaustion

Do not trigger failover for ordinary validation or authorization errors.

## 6. Provider Interface

```python
class LLMProvider(Protocol):
    async def generate(
        self,
        messages: list[dict],
        *,
        temperature: float = 0.0,
        max_tokens: int = 1000,
    ) -> str:
        ...
```

Implementations:

```text
GroqProvider
OpenRouterProvider
HuggingFaceProvider
```

The LangGraph agent depends only on the provider interface.

## 7. Model Capability Routing

Before selecting a provider/model, the router may consider:

- tool/function calling
- structured outputs
- context-window requirements
- expected latency
- token budget
- reasoning complexity
- current provider availability
- free-tier quota

For example:

```text
Simple extraction
    → OpenRouter free model

Interactive agent
    → Groq

Complex/long-context benchmark
    → explicitly configured OpenRouter model

Groq unavailable
    → OpenRouter

OpenRouter unavailable
    → Hugging Face
```

## 8. Redis Caching

Use Upstash Redis for:

- LLM response caching
- fundamentals
- news search results
- stable analysis results
- rate limiting
- short-lived state
- ingestion locks

Example keys:

```text
mizan:llm:{provider}:{model}:{prompt_hash}:{context_hash}
mizan:fundamentals:{ticker}:{metric}:{period}
mizan:news:{ticker}:{date}:{query_hash}
```

Personalized responses must include user/profile/version information in cache keys.

## 9. Cache Safety

Redis is never the source of truth.

If Redis fails:

```text
Redis unavailable
      ↓
PostgreSQL / provider
      ↓
Continue without cache
```

## 10. Cost Control

- Cache reusable responses.
- Avoid LLM calls for deterministic operations.
- Batch ingestion.
- Deduplicate before LLM processing.
- Use structured retrieval first.
- Use embeddings only when beneficial.
- Limit tokens/output.
- Route simple tasks to free models.
- Reserve higher-capability calls for tasks that need them.

## 11. No Local Inference

Explicitly out of scope:

- local model servers
- local GPU inference
- local LLM runtimes

All model and embedding inference is cloud/API based.
