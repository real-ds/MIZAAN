# MIZAAN | Environment Configuration

## Vercel Frontend

```env
NEXT_PUBLIC_API_URL=https://<render-backend>.onrender.com
```

## Render Backend

```env
APP_ENV=production
APP_NAME=MIZAAN
LOG_LEVEL=INFO
PORT=8000
```

## Neon PostgreSQL

```env
DATABASE_URL=
DATABASE_POOL_SIZE=
DATABASE_MAX_OVERFLOW=
```

## Upstash Redis

```env
REDIS_URL=
REDIS_TOKEN=
```

## Groq

```env
GROQ_API_KEY=
GROQ_MODEL=
```

## OpenRouter

```env
OPENROUTER_API_KEY=
OPENROUTER_FREE_MODEL=openrouter/free
OPENROUTER_BENCHMARK_MODEL=
OPENROUTER_REASONING_MODEL=
```

## Hugging Face

```env
HF_TOKEN=
HF_MODEL=
```

## LLM Router

```env
PRIMARY_LLM_PROVIDER=groq
FALLBACK_LLM_PROVIDER=openrouter
SECONDARY_FALLBACK_PROVIDER=huggingface
LLM_TIMEOUT_SECONDS=30
LLM_MAX_RETRIES=2
```

## Google OAuth

```env
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REDIRECT_URI=
```

## Observability

```env
LANGSMITH_API_KEY=
LANGSMITH_PROJECT=
OTEL_EXPORTER_OTLP_ENDPOINT=
```

No local LLM environment variables are required.

Never commit actual secrets. Commit only `.env.example`.
