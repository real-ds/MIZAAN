# CODE REVIEW CHECKLIST

**Purpose**: Ensure all code changes meet professional engineering standards  
**When to Use**: Before committing changes and during code review  
**Audience**: Developers and Code Reviewers

---

## 🔍 SELF-REVIEW CHECKLIST

Complete this BEFORE requesting code review or merging:

### Functional Correctness

```
□ Feature works as specified
□ No new console.log/print statements left in code
□ No TODO/FIXME comments without context
□ All error cases handled
□ Edge cases considered:
  □ Empty inputs
  □ Null/None values
  □ Very large inputs
  □ Negative numbers (if applicable)
  □ Special characters
  □ Concurrent access (if applicable)
```

### Code Quality & Style

```
□ Code follows project style guide
□ Variable names are clear and descriptive
□ Function names indicate purpose
□ No magic numbers (use named constants)
□ DRY principle followed (no code duplication)
□ SOLID principles applied (where appropriate)
□ Consistent indentation and formatting
□ Max line length respected (e.g., 100 chars)
□ Comments explain WHY, not WHAT
□ No unnecessary comments
□ Function/class length reasonable (<50 lines ideal)
```

### Documentation

```
□ Docstrings/JSDoc present for all public functions
□ Function signatures documented
□ Parameters explained with types
□ Return values documented
□ Exceptions/errors documented
□ Example usage provided (if complex)
□ Complex algorithms explained
□ README updated (if applicable)
□ API documentation updated (if applicable)
□ CHANGELOG.md updated
```

### Testing

```
□ New unit tests written for new code
□ Existing tests updated (if needed)
□ All tests pass locally
□ Test coverage adequate (>80% target)
□ Edge cases tested
□ Error cases tested
□ No flaky/intermittent tests
□ Test names are descriptive
□ Test data realistic
□ Mocks used appropriately
```

### Performance & Security

```
PERFORMANCE:
□ No obvious performance regressions
□ No N+1 database queries
□ No unnecessary API calls
□ Efficient algorithms used
□ Caching considered (if appropriate)
□ Memory leaks prevented
□ No infinite loops
□ Async operations used where needed

SECURITY:
□ No hardcoded secrets/passwords
□ User input validated
□ SQL injection prevented (parameterized queries)
□ XSS prevention (proper escaping)
□ CSRF token validation (if applicable)
□ Authentication checked
□ Authorization verified
□ Sensitive data not logged
□ No directory traversal vulnerabilities
□ Dependencies checked for vulnerabilities
```

### Error Handling

```
□ All exceptions handled explicitly
□ No bare except clauses
□ Appropriate exception types used
□ Error messages helpful
□ Stack traces logged (but not exposed to users)
□ Graceful degradation where applicable
□ User-facing errors clear
□ Technical errors logged for debugging
```

### Dependencies & Imports

```
□ No unused imports
□ No circular dependencies
□ New dependencies justified
□ Dependencies checked for security issues
□ Version pinning appropriate
□ Dependency documentation updated
□ No unnecessary external dependencies (reinventing wheels avoided)
```

### Database (if applicable)

```
□ Database migrations created (if schema changed)
□ SQL queries optimized
□ Indexes added where needed
□ No unnecessary queries
□ Connection pooling configured
□ Transactions used appropriately
□ Data migration strategy clear
```

### API Changes (if applicable)

```
□ API versioning followed (if breaking change)
□ Backward compatibility maintained (if possible)
□ API documentation updated
□ Request validation complete
□ Response format consistent
□ Error responses standardized
□ Rate limiting considered
□ Pagination implemented (if needed)
```

---

## 👥 CODE REVIEWER CHECKLIST

Use this when reviewing others' code:

### Architecture & Design

```
□ Change fits overall architecture
□ Design patterns followed
□ Solution is most straightforward approach
□ No over-engineering for edge cases
□ Modularity maintained
□ Component responsibilities clear
□ Coupling minimized
□ No unnecessary abstraction
□ Scalability considerations addressed
```

### Business Logic

```
□ Implementation matches requirements
□ Edge cases handled per requirements
□ No unnecessary features added
□ Performance acceptable for use case
□ User experience considered
□ Accessibility considerations (if UI)
□ Internationalization considered (if applicable)
```

### Code Review Comments

```
WHEN COMMENTING:
□ Be specific (don't just say "this is wrong")
□ Explain why it's a problem
□ Suggest a fix or approach
□ Ask questions instead of demanding changes
□ Praise good solutions
□ Learn from different approaches
□ Stay professional and kind

COMMENT EXAMPLES:

❌ BAD:
"This is inefficient."

✅ GOOD:
"This function does O(n²) comparison for each user. 
With 10k+ users, this will be slow. Consider using 
a dictionary/set for O(1) lookups instead. See lines 45-50."

❌ BAD:
"Add more tests."

✅ GOOD:
"I notice the error handling path isn't tested. 
Can you add a test for when the API returns 500? 
Example: test_fetch_user_api_error()"

❌ BAD:
"This is wrong."

✅ GOOD:
"This SQL query is vulnerable to injection. 
Use parameterized queries instead:
cursor.execute('SELECT * FROM users WHERE id = %s', (user_id,))"
```

### Common Issues to Look For

```
□ Resource leaks (files not closed, connections not released)
□ Off-by-one errors
□ Null pointer/None exceptions
□ Incorrect data types
□ Logic errors (wrong boolean conditions)
□ Regex escaping issues
□ Timezone issues (if time involved)
□ Float precision issues (if needed)
□ Thread safety (if concurrent)
□ Race conditions
□ Deadlocks
```

### Question the Author

```
□ Why was this approach chosen?
□ Have you tested the failure scenario?
□ Is this consistent with other parts of the codebase?
□ What are the performance implications?
□ Could this impact other components?
□ Is there any technical debt being added?
□ Are there any breaking changes?
```

---

## 📋 AUTOMATED CHECKS

Set these up to run automatically:

### Linting & Formatting

**Python**:
```bash
# Check code style
flake8 src/

# Format code
black src/

# Check type hints
mypy src/

# Security checks
bandit src/
```

**JavaScript/TypeScript**:
```bash
# Check code style
eslint src/

# Format code
prettier --write src/

# Type checking
tsc --noEmit

# Security checks
npm audit
```

### Testing

```bash
# Run all tests
pytest tests/  # Python
npm test       # JavaScript

# With coverage
pytest --cov=src tests/
npm run test:coverage

# Only unit tests
pytest tests/unit/
npm run test:unit
```

### Build Checks

```bash
# Build backend
python -m py_compile src/

# Build frontend
npm run build

# Docker build
docker build .
```

### Pre-commit Hook

Set up `.git/hooks/pre-commit`:
```bash
#!/bin/bash

# Run linter
npm run lint || exit 1

# Run tests
npm test || exit 1

# Check for secrets
if grep -r "password\|secret\|api_key" src/ --include="*.py" --include="*.js" --include="*.jsx"; then
  echo "❌ Potential secrets found in code!"
  exit 1
fi

echo "✅ Pre-commit checks passed"
```

---

## 🚀 REVIEW DECISION MATRIX

### Approve ✅
```
Criteria:
- Meets all checklist items
- No major concerns
- Minor issues noted and acknowledged
- Tests comprehensive
- Documentation complete
- Performance acceptable
- Security reviewed

Action: Merge to develop/main
```

### Request Changes ❌
```
Criteria:
- Critical issues found that must be fixed
- Tests insufficient
- Security concerns
- Performance problematic
- Documentation incomplete
- Breaking changes not handled

Action: 
1. List specific issues
2. Explain why it's important
3. Suggest solutions
4. Request re-review after fixes
```

### Comment 💭
```
Criteria:
- Minor suggestions
- Not blockers
- Nice-to-have improvements
- Questions to clarify understanding

Action:
- Author can address or leave as future improvement
- Not required to merge
- Useful for future refactoring
```

---

## 🔄 REVIEW PROCESS

### Step 1: Initial Check (5 min)
```
□ Code compiles/syntax valid
□ Tests pass
□ No obvious red flags
```

### Step 2: Understand Changes (10-15 min)
```
□ Read commit message/description
□ Understand what changed and why
□ Trace through the logic
□ Identify all affected files
```

### Step 3: Detailed Review (20-30 min)
```
□ Review architecture/design
□ Check code quality
□ Verify tests adequately cover changes
□ Look for performance issues
□ Check security
□ Verify documentation
```

### Step 4: Provide Feedback (5-10 min)
```
□ Summarize findings
□ Highlight strengths
□ List concerns/issues
□ Ask clarifying questions
□ Provide suggestions
```

### Step 5: Decision (2 min)
```
□ Approve
□ Request Changes
□ Comment
```

---

## 📊 CODE REVIEW METRICS

Track these to improve process:

```
REVIEW METRICS:
- Average review time: [target: <30 min]
- Issues found per review: [target: 2-4]
- Critical issues: [target: 0-1]
- Test coverage maintained: [target: 100%]
- Performance regressions: [target: 0]
- Security issues: [target: 0]
- Reviewer turnaround: [target: <24 hours]
```

---

## 🎓 REVIEW BEST PRACTICES

### As an Author:
```
✅ DO:
- Make small, focused PRs (easier to review)
- Write clear commit messages
- Provide context in PR description
- Link to related issues
- Mark as draft if not ready
- Respond to comments with clarity
- Update based on feedback
- Show appreciation for feedback

❌ DON'T:
- Submit massive PRs (>400 lines)
- Leave vague commit messages
- Be defensive about feedback
- Push back without understanding
- Ignore test failures
- Leave debugging code
- Request emergency reviews
```

### As a Reviewer:
```
✅ DO:
- Be kind and constructive
- Ask questions instead of demanding
- Explain the reasoning
- Suggest alternatives
- Learn from good code
- Review promptly
- Be specific in feedback
- Acknowledge good work

❌ DON'T:
- Be condescending
- Block on style preferences
- Demand perfection
- Skip security/performance review
- Review while tired/rushed
- Leave vague comments
- Approve without understanding
```

---

## 🎯 REVIEW FOCUS AREAS

### High Priority (Always Check)
```
1. Security vulnerabilities
2. Performance regressions
3. Test coverage
4. Breaking changes
5. Database migrations
6. Error handling
```

### Medium Priority (Usually Check)
```
1. Code style consistency
2. Documentation
3. Function complexity
4. Algorithm efficiency
5. API design
```

### Low Priority (Nice-to-Check)
```
1. Comment quality
2. Variable naming
3. Code organization
4. Future improvements
```

---

## 📝 REVIEW COMMENT TEMPLATE

Use this template for comprehensive reviews:

```markdown
## Code Review Summary

### ✅ Strengths
- [What was done well]
- [Clean implementation]

### ❌ Issues (Blocking)
1. **Issue**: [Description]
   **Why**: [Impact]
   **Suggestion**: [How to fix]
   **Severity**: Critical/High

### ⚠️ Suggestions (Non-Blocking)
1. **Suggestion**: [Description]
   **Benefit**: [Why it matters]

### 💭 Questions
1. **Question**: Why was this approach chosen?
   **Consider**: [Alternative approach]

### 📊 Metrics
- Lines changed: X
- Files affected: Y
- Test coverage: Z%
- Performance impact: None/Positive/Negative

### 🎯 Decision
- [✅ Approve / ❌ Request Changes / 💭 Comment]

### 📝 Next Steps
- [What needs to happen before merge]
```

---

## 🔗 RELATED DOCUMENTATION

- **AGENTIC_SYSTEM_PROMPT.md** - System instructions for AI development
- **DEVELOPMENT_GUIDE.md** - Coding standards and patterns
- **CHANGELOG.md** - Change tracking
- **PROJECT_STRUCTURE.md** - Project overview

---

**Code review is not about finding fault — it's about collective code ownership and continuous improvement.**
