# MIZAAN | Security & Privacy

## 1. Authentication

Use OAuth for user identity.

Do not store Google passwords.

## 2. Authorization

Every user-owned resource must be scoped by authenticated user ID.

A user must not access another user's:

- investor profile
- conversations
- preferences
- private evaluation traces

## 3. Secrets

Never commit:

- API keys
- OAuth client secrets
- database passwords
- AWS credentials

Use environment variables locally and AWS Secrets Manager/secure CI secrets in production.

## 4. Data Protection

Use HTTPS in production.

Encrypt sensitive data at rest where applicable.

## 5. Prompt/Data Separation

Retrieved financial documents are untrusted content. They must not be treated as executable instructions.

## 6. Logging

Do not log:

- access tokens
- secrets
- full private conversations unless explicitly required and protected

Prefer:

- trace ID
- user pseudonymous ID
- tool name
- latency
- success/failure
- evaluation metadata

## 7. Financial Disclaimer

MIZAAN is an analytical/research assistant and must clearly avoid representing generated analysis as guaranteed investment advice.

## 8. Source Compliance

Data ingestion must respect:

- robots.txt
- source terms
- rate limits
- licensing requirements

The original challenge specifically calls for responsible scraping of supported sources.
