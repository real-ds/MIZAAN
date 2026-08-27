# MIZAAN | Engineering Contribution Guide

## 1. Code Quality

- Use Python type hints.
- Prefer small, testable functions.
- Keep LLM calls behind service interfaces.
- Keep database access behind repositories/services.
- Do not mix prompt construction with persistence logic.

## 2. Agent Tool Rules

Every tool must define:

- input schema
- output schema
- failure modes
- timeout
- authorization requirements
- deterministic behavior where possible

## 3. Pull Requests

Every PR should include:

- problem description
- implementation summary
- tests
- evaluation impact
- migration notes if applicable
- deployment impact

## 4. Prompt Changes

Prompt changes are code changes.

Record:

- prompt version
- expected behavior
- evaluation impact

## 5. Evaluation Changes

Any change affecting retrieval or generation should run the regression benchmark.

## 6. Commit Style

Examples:

- `feat: add hierarchical retrieval`
- `feat: add investor persona memory`
- `fix: prevent duplicate news ingestion`
- `test: add citation coverage benchmark`
- `infra: add ECS terraform module`

## 7. Never

- commit secrets;
- bypass tests to merge;
- fabricate evaluation results;
- silently change financial units;
- allow retrieved text to execute instructions.
