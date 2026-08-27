# MIZAAN | OpenRouter Free Model Strategy

## 1. Purpose

OpenRouter is MIZAAN's unified gateway for accessing free/open-weight models through one API.

## 2. Default

```env
OPENROUTER_FREE_MODEL=openrouter/free
```

The `openrouter/free` route should be treated as a dynamic free-model pool rather than a permanently fixed model.

## 3. Explicit Models

For evaluation or workload-specific routing, configure an explicit model ID:

```env
OPENROUTER_BENCHMARK_MODEL=
OPENROUTER_REASONING_MODEL=
```

Model IDs must be validated against the currently available OpenRouter catalog before deployment.

## 4. Capability Requirements

Before selecting a free model, verify:

- tool/function calling
- structured output
- context length
- JSON reliability
- instruction following
- latency
- evaluation score
- availability

## 5. MIZAAN Workloads

### Agentic tool use
Prefer models that reliably support tool/function calling.

### Structured extraction
Prefer smaller/efficient models where JSON compliance is strong.

### Long-context analysis
Prefer currently available free models with sufficient context length.

### Evaluation
Pin a model ID so benchmark results remain reproducible.

## 6. Important

Do not hard-code today's free-model roster into the architecture.

OpenRouter's free catalog can change. The system should query/configure the currently supported model pool and record the model used in every evaluation trace.

## 7. Observability

Record:

```text
provider
model
request_id
latency
input_tokens
output_tokens
failure_type
fallback_count
```

This allows MIZAAN to compare providers objectively.
