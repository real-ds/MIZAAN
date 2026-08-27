# MIZAAN | Agentic Workflow

## 1. Query Workflow

```text
User Query
   ↓
Input Validation
   ↓
Intent + Entity Extraction
   ↓
Query Planner
   ↓
Tool Selection
   ↓
Retrieval
   ↓
Evidence Fusion
   ↓
Evidence Verification
   ↓
Investor Constraint Application
   ↓
Response Planning
   ↓
LLM Generation
   ↓
Citation Validation
   ↓
Output Guardrails
   ↓
Final Response
```

## 2. LangGraph State

Suggested state:

```python
class MizanState(TypedDict):
    user_id: str
    query: str
    intent: str | None
    tickers: list[str]
    time_range: dict
    investor_profile: dict
    selected_tools: list[str]
    retrieved_evidence: list[dict]
    verified_evidence: list[dict]
    claims: list[dict]
    draft_answer: str | None
    citations: list[dict]
    guardrail_results: dict
    evaluation_metadata: dict
```

## 3. Nodes

### input_guard
Validates request format and rejects obvious prompt-injection or unsupported requests.

### intent_router
Classifies the task:
- factual lookup
- company analysis
- news/sentiment
- comparison
- screening
- personalized recommendation

### entity_resolver
Resolves company names/tickers, periods and financial metrics.

### query_planner
Determines retrieval requirements.

### retrieval_router
Selects:
- SQL retrieval
- metadata filtering
- full-text/BM25
- hierarchical traversal
- optional semantic retrieval

### evidence_ranker
Ranks evidence using relevance, source quality, recency, entity match and query alignment.

### verifier
Checks whether evidence actually supports required claims.

### persona_filter
Applies explicit investor constraints.

### generator
Creates a response from verified evidence.

### citation_validator
Ensures claims have valid source references.

### output_guard
Blocks unsupported or invalid responses.

## 4. Example Recommendation Workflow

User:

> Recommend stocks for my conservative dividend-focused profile.

Flow:

1. Load investor persona.
2. Identify explicit constraints.
3. Retrieve candidate stocks.
4. Apply deterministic filters.
5. Retrieve recent supporting news/fundamentals.
6. Rank candidates.
7. Verify evidence.
8. Generate one-line reasons.
9. Attach citations.
10. Return uncertainty where evidence is incomplete.

## 5. Failure Path

If evidence is insufficient:

```text
Missing Evidence
      ↓
Attempt alternate retrieval
      ↓
Still missing?
      ↓
"I don't have that in the available data."
```

No fabricated financial value is allowed.
