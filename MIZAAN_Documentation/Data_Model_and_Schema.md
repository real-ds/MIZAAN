# MIZAAN | Data Model & Schema

## 1. Core Entities

### users
- id
- email
- display_name
- oauth_provider
- created_at
- updated_at

### investor_profiles
- user_id
- risk_tolerance
- investment_horizon
- strategy
- preferred_sectors
- excluded_sectors
- debt_constraints
- dividend_preferences
- growth_preferences
- raw_preferences
- updated_at

### stocks
- id
- ticker
- company_name
- exchange
- nse_id
- bse_id
- sector
- market_cap
- active

### fundamentals
- id
- stock_id
- metric_name
- value
- unit
- period
- period_type
- source_id
- fetched_at

### news_articles
- id
- canonical_url
- title
- body
- source
- published_at
- content_hash
- fetched_at

### stock_news
- stock_id
- article_id
- relevance
- sentiment
- impact
- event_type

### sources
- id
- source_type
- source_name
- url
- retrieved_at
- checksum

### ingestion_jobs
- id
- job_type
- ticker
- idempotency_key
- status
- started_at
- completed_at
- error

### evidence
- id
- source_id
- stock_id
- content_reference
- retrieval_method
- retrieval_score
- created_at

### evaluations
- id
- dataset_version
- model_version
- metric_name
- metric_value
- run_id
- created_at

## 2. Important Constraints

- Unique stock ticker + exchange
- Unique article canonical URL/hash
- Unique ingestion idempotency key
- Foreign-key integrity
- Valid financial period
- Non-null source for financial claims

## 3. Indexes

Recommended:

- `stocks(ticker)`
- `stocks(nse_id)`
- `stocks(bse_id)`
- `fundamentals(stock_id, metric_name, period)`
- `news_articles(published_at)`
- `news_articles(content_hash)`
- `stock_news(stock_id, article_id)`
- full-text index on article title/body
- metadata indexes for source/date/ticker/event

## 4. Temporal Correctness

Financial data must preserve:

- publication time
- reporting period
- retrieval time
- effective date

The system must not confuse the date an article was retrieved with the period the article discusses.
