# MIZAAN | Software Requirements Specification

## 1. Introduction

### 1.1 Purpose

This SRS defines functional, non-functional, architectural and operational requirements for MIZAAN, a production-grade Agentic AI stock-analysis platform for Indian equities.

### 1.2 Scope

The system covers authentication, stock following, financial data ingestion, news ingestion, structured knowledge indexing, retrieval, investor memory, agentic orchestration, evidence verification, grounded generation, citations, evaluation and cloud deployment.

### 1.3 Definitions

- **RAG:** Retrieval-Augmented Generation
- **Agent:** LLM-driven workflow capable of selecting tools and performing multi-step tasks
- **Investor Persona:** Persistent structured representation of investor preferences and constraints
- **Evidence:** Source-backed information used to support a generated claim
- **Structured-first RAG:** Retrieval architecture where exact structured retrieval is preferred over vector similarity
- **Guardrail:** Deterministic or model-assisted control that prevents unsafe, unsupported or invalid behavior

## 2. Functional Requirements

### FR-01 Authentication
The system shall support user authentication through Google OAuth.

### FR-02 User Profile
The system shall maintain a user profile and investor persona.

### FR-03 Stock Following
Users shall be able to follow and unfollow supported NSE/BSE tickers.

### FR-04 Fundamentals Ingestion
The system shall ingest supported company fundamentals and preserve source and period metadata.

### FR-05 News Ingestion
The system shall ingest recent Indian financial news from supported feeds/sources.

### FR-06 Data Normalization
The ingestion pipeline shall normalize ticker identifiers, timestamps, currencies, source identifiers and entities.

### FR-07 Deduplication
The pipeline shall detect duplicate or overlapping news articles.

### FR-08 Idempotency
Repeated ingestion of identical source data shall not create duplicate logical records.

### FR-09 Concurrent Ingestion
Concurrent ingestion jobs shall use safe database constraints/locking/transactional logic.

### FR-10 Investor Memory
The system shall extract explicit investor preferences from chat and update the investor persona.

### FR-11 Query Understanding
The agent shall classify user intent and identify entities, tickers, time periods and requested metrics.

### FR-12 Tool Selection
The agent shall select appropriate retrieval and analysis tools.

### FR-13 Structured Retrieval
The system shall support exact retrieval through SQL/metadata/hierarchical indexes.

### FR-14 Unstructured Retrieval
The system shall support lexical retrieval and optional semantic embedding retrieval.

### FR-15 Evidence Verification
The system shall validate that evidence supports generated factual claims.

### FR-16 Citation Generation
The system shall attach citations to relevant factual claims.

### FR-17 INR Correctness
Financial values presented to users shall use INR unless a different unit is explicitly required and clearly labeled.

### FR-18 Unsupported Claims
If required evidence cannot be found, the agent shall explicitly state that the information is unavailable rather than fabricate it.

### FR-19 Personalized Recommendations
The system shall apply investor constraints before ranking candidate stocks.

### FR-20 Sentiment/Impact Extraction
The ingestion pipeline may extract per-stock sentiment, impact and event information from financial articles.

### FR-21 Evaluation
The project shall provide repeatable evaluation datasets and automated metric calculation.

### FR-22 Observability
Production traces shall capture request, retrieval, tool and guardrail outcomes without storing sensitive secrets.

## 3. Non-Functional Requirements

### NFR-01 Reliability
Failures in one ingestion job shall not corrupt unrelated stock data.

### NFR-02 Consistency
Duplicate logical records shall be prevented through deterministic identifiers and database constraints.

### NFR-03 Performance
Retrieval and common deterministic operations should avoid unnecessary LLM calls.

### NFR-04 Scalability
The ingestion and query services should scale independently.

### NFR-05 Security
Authentication, authorization, secrets and API access shall follow least-privilege principles.

### NFR-06 Observability
Application errors, latency, tool failures and evaluation outcomes shall be observable.

### NFR-07 Reproducibility
Evaluation runs shall record model, prompt, dataset and configuration versions.

### NFR-08 Maintainability
Agent tools shall have clear contracts and deterministic responsibilities.

## 4. Constraints

- Financial data may be delayed or incomplete.
- Source licensing and robots/rate-limit policies must be respected.
- MIZAAN does not guarantee investment performance.
- Numerical claims require source evidence.

## 5. Acceptance Criteria

A release is acceptable when:

- authentication works;
- a supported stock can be ingested;
- repeated ingestion is idempotent;
- a query retrieves relevant evidence;
- answers contain valid citations;
- unsupported facts are refused;
- investor constraints affect recommendation filtering;
- evaluation tests pass agreed thresholds;
- the application is containerized and deployable.
