# MIZAAN | Agentic Task Allocation

## 1. Principle

The LLM agent should handle tasks requiring language understanding, planning and synthesis. Deterministic services should handle exact computation, filtering, persistence and validation.

## 2. Agent vs Tool Boundary

| Task | Agent | Deterministic Tool |
|---|---:|---:|
| Understand natural-language query | ✓ | |
| Identify user intent | ✓ | |
| Decide retrieval strategy | ✓ | |
| SQL metric lookup | | ✓ |
| Date/period filtering | | ✓ |
| Stock eligibility filtering | | ✓ |
| Investor rule evaluation | | ✓ |
| Full-text search | | ✓ |
| News deduplication | | ✓ |
| Sentiment extraction | ✓ | |
| Evidence verification | ✓ + deterministic checks | ✓ |
| Citation mapping | | ✓ |
| Final explanation | ✓ | |
| Authentication | | ✓ |
| Database writes | | ✓ |
| Evaluation scoring | | ✓ |

## 3. Tool Contracts

### `get_stock_fundamentals`
Input: ticker, metric, period  
Output: value, unit, period, source_id

### `search_news`
Input: ticker/entities, date range, query terms  
Output: ranked articles with source metadata

### `get_stock_events`
Input: ticker, date range  
Output: normalized events

### `get_investor_profile`
Input: user_id  
Output: structured persona

### `screen_stocks`
Input: universe, constraints  
Output: eligible candidates

### `compare_stocks`
Input: tickers, metrics  
Output: normalized comparison table

### `retrieve_evidence`
Input: query plan  
Output: evidence objects

### `verify_claim`
Input: claim, evidence  
Output: support status and supporting source IDs

### `generate_citations`
Input: verified claims  
Output: citation map

## 4. Anti-Over-Agentification Rule

The agent must not:

- call an LLM to perform simple arithmetic;
- call an LLM for deterministic filtering;
- re-process unchanged documents;
- retrieve the entire corpus for a narrow query;
- fabricate missing data;
- write directly to the database without a service contract.
