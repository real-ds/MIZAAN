# MIZAAN | Evaluation Framework

## 1. Purpose

Evaluation is a first-class engineering component of MIZAAN.

## 2. Evaluation Dataset

Create a versioned benchmark containing:

- factual lookup questions
- multi-hop financial questions
- news/sentiment questions
- stock comparisons
- personalized screening questions
- missing-data questions
- adversarial/prompt-injection questions

Do not reuse evaluation questions as training examples.

## 3. Retrieval Metrics

### Recall@K
Measures whether relevant evidence appears in top K results.

### Precision@K
Measures the proportion of top K results that are relevant.

### MRR
Measures ranking quality based on the position of the first relevant result.

### nDCG
Measures ranking quality with graded relevance.

## 4. Generation Metrics

- Faithfulness
- Answer relevancy
- Context precision
- Context recall
- Citation correctness
- Citation completeness

## 5. Agent Metrics

- Intent classification accuracy
- Tool-selection accuracy
- Task completion rate
- Invalid tool-call rate
- Recovery success rate
- Average tool calls/query

## 6. Financial Safety Metrics

- Hallucination rate
- Unsupported numerical claim rate
- Unsupported recommendation rate
- Wrong-ticker rate
- Wrong-period rate
- Citation coverage

## 7. Performance Metrics

- Retrieval p50/p95 latency
- End-to-end p50/p95 latency
- Ingestion throughput
- Database query latency
- LLM calls/query
- Tokens/query

## 8. Evaluation Record

Each evaluation run should record:

- dataset version
- application commit
- prompt version
- model name/version
- retrieval configuration
- guardrail configuration
- timestamp
- metric results

## 9. Quantitative Resume Metrics

Only report numbers after actual measurement.

Examples:

- `100+ benchmark queries`
- `50+ NSE/BSE stocks`
- `1K+ financial documents`
- `10K+ indexed evidence records`
- `92% citation coverage`
- `90% Precision@5`
- `95% grounded-response rate`

These are examples of reporting formats, not assumed MIZAAN results.
