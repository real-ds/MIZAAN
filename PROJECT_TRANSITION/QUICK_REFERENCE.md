# ⚡ QUICK REFERENCE CARD

**Complete Agentic Development System**

---

## 📋 FILES IN THIS SYSTEM

| File | Purpose | Key Points |
|------|---------|-----------|
| **AGENTIC_SYSTEM_PROMPT.md** | Main instructions for AI agent | Read this first; all others depend on it |
| **DEVELOPMENT_GUIDE.md** | Coding standards & best practices | Style guide, patterns, examples |
| **PROJECT_STRUCTURE.md** | Project architecture & inventory | Fill this with YOUR project details |
| **CHANGELOG.md** | Change tracking (auto-filled) | Required documentation of every change |
| **CODE_REVIEW_CHECKLIST.md** | Quality assurance standards | Use before merging anything |
| **WORKFLOW_GUIDE.md** | How to use this system | Practical examples & workflows |
| **QUICK_REFERENCE.md** | This file | One-page summary |

---

## 🚀 QUICK START (2 minutes)

```
1. Upload your project files
2. Send Claude this prompt:
   "I have a project [description]. Please:
    1. Analyze the structure
    2. Update PROJECT_STRUCTURE.md
    3. Tell me what you found"
3. Describe your task
4. Claude handles everything
```

---

## 💬 COMMON PROMPTS

### New Feature
```
I need [FEATURE] with:
- [Requirement 1]
- [Requirement 2]
Follow AGENTIC_SYSTEM_PROMPT.md throughout.
```

### Bug Fix
```
Bug: [Description]
Symptoms: [How it breaks]
Files: [Where to look]
Please fix and update CHANGELOG.md.
```

### Performance
```
[THING] is slow ([current time] → target: [target time])
Please optimize and document improvements in CHANGELOG.md.
```

### Refactoring
```
Refactor [COMPONENT] because [reason].
Maintain all existing functionality.
Update CHANGELOG.md when done.
```

---

## ✅ CLAUDE'S AUTOMATIC WORKFLOW

```
EVERY TASK:
1. ✓ Read AGENTIC_SYSTEM_PROMPT.md
2. ✓ Review PROJECT_STRUCTURE.md (understand codebase)
3. ✓ Plan changes (show CHANGE_PLAN.txt)
4. ✓ Implement with comments
5. ✓ Write/update tests
6. ✓ Update CHANGELOG.md
7. ✓ Verify everything works

DELIVERABLES:
- Modified source files (with comments)
- Updated tests
- Updated CHANGELOG.md
- Summary of changes
- Verification proof
```

---

## 🎯 EXAMPLE CONVERSATION

```
YOU:
"Add JWT authentication with login/logout endpoints"

CLAUDE:
"Planning... [shows CHANGE_PLAN.txt]"

YOU:
"Yes, implement"

CLAUDE:
"✓ Added auth_service.py
 ✓ Added login route (/api/auth/login)
 ✓ Added logout route (/api/auth/logout)
 ✓ Added 12 tests
 ✓ All tests pass
 ✓ CHANGELOG.md updated"

YOU:
"Looks great! Merged."
```

---

## 📊 KEY CONCEPTS

### CHANGE_PLAN.txt
Temporary file showing Claude's plan BEFORE implementation:
```
What will be modified
Execution order
Why each change
Testing strategy
```

### Comments in Code
Every change includes:
```python
# ===== CHANGE: [Description] =====
# REASON: Why this was added
# IMPACT: What this affects
# DATE: When it was changed
# TASK: Related issue/task ID
```

### CHANGELOG.md Entries
Every task updates CHANGELOG:
```
## [DATE] - Version X.Y.Z

### Changed
- **File**: path/to/file.py
  **Change**: What was modified
  **Why**: Business reason
  **Impact**: Effect on system
  **Lines**: 45-67
  **Task**: TASK-123
```

---

## 🔍 BEFORE MERGING CHECKLIST

Use this before accepting any changes:

```
□ Code compiles/runs without errors
□ All tests pass
□ No console.log/print statements left
□ Comments explain WHY
□ No hardcoded values (except constants)
□ Performance acceptable
□ CHANGELOG.md updated
□ Documentation updated
□ Security reviewed
□ Error handling complete
```

---

## 🚨 COMMON PROBLEMS & FIXES

| Problem | Solution |
|---------|----------|
| Claude not planning first | Start prompt with "Follow AGENTIC_SYSTEM_PROMPT.md" |
| Changes don't match style | Update PROJECT_STRUCTURE.md with code examples |
| Tests failing | Include "Run tests first" in prompt |
| CHANGELOG not updated | End prompt with "Update CHANGELOG.md" |
| Too many changes at once | Request smaller, incremental changes |
| Missing documentation | Include "Update documentation" in prompt |

---

## 📈 METRICS TO TRACK

```
EFFICIENCY:
- Task time: [Hours to complete]
- Code quality: [Issues found in review]
- Test coverage: [% of code tested]
- Regression bugs: [0 is target]

EXAMPLE IMPROVEMENT:
Week 1 (manual): 8 features, 15 review issues, 82% coverage
Week 2 (agentic): 12 features, 2 review issues, 94% coverage
Result: +50% productivity, -87% defects
```

---

## 🎓 EXAMPLE FEATURES

### Feature 1: User Authentication (4 hours)
```
✓ Login endpoint
✓ Register endpoint
✓ JWT token generation
✓ Password hashing
✓ Token validation middleware
✓ Logout endpoint
✓ Tests: 15+ tests, 95% coverage
✓ CHANGELOG.md documented
```

### Feature 2: User Profile API (2 hours)
```
✓ GET /api/users/{id} endpoint
✓ PUT /api/users/{id} for updates
✓ DELETE /api/users/{id} soft delete
✓ Authorization checks
✓ Tests: 8 tests, 92% coverage
✓ CHANGELOG.md documented
```

### Feature 3: Email Notifications (6 hours)
```
✓ Email service class
✓ HTML email templates
✓ Send on signup
✓ Send on password reset
✓ Send on important events
✓ Scheduled email queue
✓ Tests: 12 tests, 88% coverage
✓ CHANGELOG.md documented
```

---

## 🔐 SECURITY CHECKLIST

Always verify:
```
□ No hardcoded secrets
□ Input validation
□ SQL injection prevented (parameterized queries)
□ XSS prevention (proper escaping)
□ Authentication required where needed
□ Authorization verified
□ Sensitive data not logged
□ CSRF tokens (if applicable)
```

---

## 📚 REFERENCE MATRIX

### When to Use Each File

**Start Project**:
```
1. PROJECT_STRUCTURE.md (fill with your details)
2. DEVELOPMENT_GUIDE.md (know the standards)
3. CODE_REVIEW_CHECKLIST.md (understand quality expectations)
```

**During Development**:
```
1. AGENTIC_SYSTEM_PROMPT.md (Claude uses this)
2. DEVELOPMENT_GUIDE.md (follow patterns)
3. Write code with proper comments
```

**After Task Completion**:
```
1. Check CHANGELOG.md updated
2. Run CODE_REVIEW_CHECKLIST.md
3. Verify all tests pass
4. Merge changes
```

---

## 💡 PRO TIPS

### Tip 1: Specific Requirements
```
❌ "Add user search"
✅ "Add user search with:
    - Endpoint: GET /api/users/search?q={query}
    - Search fields: name, email
    - Pagination: limit=20, offset=0
    - Response: {results: [], total: int}"
```

### Tip 2: Reference Previous Work
```
✅ "Add similar to the login endpoint we built earlier"
✅ "Follow the pattern used in user_service.py"
```

### Tip 3: Mention Edge Cases
```
✅ "Handle: empty input, invalid email, rate limiting"
```

### Tip 4: Performance Requirements
```
✅ "Must complete in <100ms for 1000 records"
✅ "Optimize for mobile (low bandwidth)"
```

### Tip 5: Incremental Changes
```
✓ Instead of "build API"
✓ Do: POST endpoint → GET endpoint → PUT endpoint → DELETE endpoint
(One at a time, each fully tested)
```

---

## 🎯 SUCCESS CRITERIA

Your task is complete when:

```
✓ Feature implemented per specifications
✓ All tests pass (>80% coverage)
✓ Code follows DEVELOPMENT_GUIDE.md
✓ Comments explain WHY
✓ CHANGELOG.md updated completely
✓ No hardcoded values (except constants)
✓ Error handling implemented
✓ Security reviewed
✓ Performance acceptable
✓ Documentation updated
```

---

## 📞 WHEN TO USE EACH PART

| Situation | Use This |
|-----------|----------|
| Starting new task | WORKFLOW_GUIDE.md |
| Writing code | DEVELOPMENT_GUIDE.md |
| Reviewing code | CODE_REVIEW_CHECKLIST.md |
| Documenting change | CHANGELOG.md |
| Understanding project | PROJECT_STRUCTURE.md |
| AI needs instructions | AGENTIC_SYSTEM_PROMPT.md |
| Quick answer needed | This file |

---

## 🚀 YOU'RE READY!

This system gives you:
- ✅ Professional code quality
- ✅ Automatic documentation
- ✅ Comprehensive testing
- ✅ Efficient development
- ✅ Production-ready output

**Next Step**: Upload your project and describe your first task!

---

**Version**: 1.0  
**Last Updated**: August 2026  
**Status**: Ready for Production

---

## 📌 QUICK LINKS

1. **Main System**: AGENTIC_SYSTEM_PROMPT.md
2. **Coding Rules**: DEVELOPMENT_GUIDE.md  
3. **Project Info**: PROJECT_STRUCTURE.md
4. **Track Changes**: CHANGELOG.md
5. **Quality Check**: CODE_REVIEW_CHECKLIST.md
6. **How to Use**: WORKFLOW_GUIDE.md
7. **Summary**: This file (QUICK_REFERENCE.md)

**All files work together as a complete professional development system.**
