# MIZAAN | Retrieval Architecture

## 1. Goal

Build a retrieval system optimized for financial accuracy rather than forcing every query through vector similarity.

## 2. Retrieval Tiers

### Tier 1 — Exact Structured Retrieval

Use SQL for:

- revenue
- profit
- debt
- ROE
- valuation metrics
- quarterly results
- dates
- prices
- stock identifiers

### Tier 2 — Metadata Retrieval

Filter by:

- ticker
- sector
- source
- date
- event type
- sentiment
- document type
- financial period

### Tier 3 — Full-Text Retrieval

Use PostgreSQL full-text search/BM25-style ranking for exact terms and concepts.

### Tier 4 — Hierarchical Retrieval

Navigate:

```text
Company
 → Document
   → Section
     → Entity
       → Event
         → Evidence
```

### Tier 5 — Optional Semantic Retrieval

Embeddings are used only when:

- the question is conceptually phrased;
- lexical retrieval has low confidence;
- synonyms/paraphrases are important;
- the agent determines semantic retrieval is justified.

## 3. Retrieval Router

The router chooses the retrieval path based on query characteristics.

Example:

```text
"What was Infosys revenue in FY2025?"
→ SQL

"Recent negative developments around Infosys?"
→ Metadata + full text + news

"Why is investor sentiment deteriorating?"
→ News + sentiment + event graph

"Which companies fit my conservative profile?"
→ Structured screening + fundamentals + persona
```

## 4. Evidence Object

```json
{
  "evidence_id": "ev_123",
  "ticker": "TCS",
  "source_id": "src_456",
  "source_type": "financial_news",
  "published_at": "2026-08-01T10:00:00Z",
  "section": "earnings",
  "text": "...",
  "metric": null,
  "value": null,
  "unit": null,
  "retrieval_method": "full_text",
  "relevance_score": 0.0
}
```

## 5. Evidence Ranking

Suggested ranking features:

- entity match
- metric match
- period match
- source quality
- recency
- lexical relevance
- semantic relevance
- event relevance

The final score should be deterministic and testable.

## 6. Why Optional Embeddings?

Embeddings remain useful for semantic ambiguity. They should not be required for exact financial facts.

This produces a **vector-optional hybrid RAG** architecture rather than a vector-only architecture.
