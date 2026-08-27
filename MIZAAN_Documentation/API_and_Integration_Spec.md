# MIZAAN | API & Integration Specification

## 1. API Style

REST/JSON over HTTPS.

Base path:

`/api/v1`

## 2. Authentication

OAuth login establishes an authenticated user session/token.

Protected endpoints require authenticated identity.

## 3. User Endpoints

### GET `/users/me`
Returns current user profile.

### GET `/users/me/investor-profile`
Returns structured investor persona.

### PATCH `/users/me/investor-profile`
Updates explicit preferences.

## 4. Stock Endpoints

### GET `/stocks/search?q=`
Search supported equities.

### POST `/stocks/{ticker}/follow`
Follow a ticker and trigger ingestion if required.

### DELETE `/stocks/{ticker}/follow`
Unfollow a ticker.

### GET `/stocks/{ticker}`
Return stock metadata.

### GET `/stocks/{ticker}/fundamentals`
Return normalized fundamentals.

### GET `/stocks/{ticker}/news`
Return recent relevant news.

## 5. Agent Endpoint

### POST `/chat`

Request:

```json
{
  "message": "What is the recent sentiment around TCS?",
  "conversation_id": "conv_123"
}
```

Response:

```json
{
  "answer": "...",
  "citations": [],
  "confidence": 0.0,
  "trace_id": "trace_123"
}
```

## 6. Ingestion Endpoints

### POST `/ingestion/stocks/{ticker}`
Starts idempotent ingestion.

### GET `/ingestion/jobs/{job_id}`
Returns job status.

## 7. Evaluation Endpoints

### POST `/evaluations/run`
Runs a selected benchmark.

### GET `/evaluations/{run_id}`
Returns metrics.

## 8. Error Contract

```json
{
  "error": {
    "code": "EVIDENCE_NOT_FOUND",
    "message": "Required information is not available.",
    "trace_id": "trace_123"
  }
}
```

## 9. API Principles

- Version endpoints.
- Validate all input.
- Never expose internal stack traces.
- Use correlation/trace IDs.
- Enforce authorization on user-owned resources.
- Keep agent tools separate from public HTTP contracts.
