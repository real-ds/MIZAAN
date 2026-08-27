# MIZAAN | Problem Statement

## 1. Project Title

**MIZAAN — AI Stock Analyzer**

## 2. Problem

Indian equity investors must combine information from multiple sources: company fundamentals, quarterly results, stock prices, financial news, market events, and their own investment preferences.

Traditional financial dashboards expose data but do not provide contextual reasoning. Generic LLM chatbots can explain finance but may hallucinate numbers, mix time periods, ignore investor-specific constraints, or provide recommendations without traceable evidence.

The original Sentellent challenge frames the target as a personal Agentic AI Indian Equity Analyst / Equity Research Chief of Staff for NSE/BSE equities. The system must ingest fundamentals and recent Indian financial news, personalize analysis using investor memory, and ground claims and recommendations in cited sources.

## 3. MIZAAN's Problem Definition

Build a production-grade Agentic AI system that can:

1. Understand a user's financial research question.
2. Determine which information and tools are required.
3. Retrieve exact financial facts using structured retrieval where possible.
4. Retrieve relevant news and unstructured evidence using lexical/semantic methods where required.
5. Maintain a persistent investor persona.
6. Rank or filter stocks using explicit, testable investment rules.
7. Verify evidence before generating an answer.
8. Cite the sources supporting factual claims.
9. Refuse unsupported claims instead of fabricating them.
10. Operate reliably under concurrent ingestion and repeated refreshes.
11. Measure retrieval, generation, agent, safety and performance quality.

## 4. Why Structured-First RAG?

Financial information is highly structured and time-sensitive. A question such as "What was TCS revenue in FY2025?" should not depend exclusively on semantic similarity.

MIZAAN therefore uses:

- SQL/structured retrieval for exact metrics
- Metadata and entity filtering
- Hierarchical navigation
- Full-text/BM25-style retrieval for lexical evidence
- Optional embeddings for semantic ambiguity
- Evidence fusion and verification before generation

This makes retrieval strategy-aware rather than vector-database-dependent.

## 5. Target Users

### Primary
- Retail investors
- Students and researchers studying equity markets
- Long-term investors
- Fundamental-analysis users

### Secondary
- Financial research teams
- Analysts who need rapid evidence collection
- Developers evaluating production Agentic RAG systems

## 6. Expected Outcome

MIZAAN should behave like a research assistant rather than a generic chatbot:

> Understand the investor → understand the question → retrieve appropriate evidence → verify evidence → apply investor constraints → generate a concise, cited answer.

## 7. Non-Goals

MIZAAN is not intended to:

- Guarantee investment returns.
- Execute trades automatically in the initial version.
- Replace licensed financial advisers.
- Provide unsupported real-time market claims.
- Predict stock prices as a guaranteed outcome.

## 8. Success Criteria

Success is measured through:

- Retrieval quality
- Citation coverage and correctness
- Answer faithfulness
- Hallucination/unsupported-claim rate
- Agent tool-selection accuracy
- Task completion rate
- Retrieval and end-to-end latency
- Ingestion idempotency
- Duplicate-data rate
- Reliability under concurrent ingestion

## 9. Architectural Philosophy

MIZAAN follows:

**Structured-first → semantic fallback → evidence verification → guarded generation**

rather than:

**Vector DB → similarity search → LLM**
