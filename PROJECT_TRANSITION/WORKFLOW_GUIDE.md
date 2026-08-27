# 🔄 AGENTIC WORKFLOW GUIDE

**How to Use This Development System**

---

## 📦 What You Have

A complete professional development system consisting of:

1. **AGENTIC_SYSTEM_PROMPT.md** ← Main instructions for AI agent
2. **DEVELOPMENT_GUIDE.md** ← Coding standards & patterns
3. **PROJECT_STRUCTURE.md** ← Project inventory & architecture
4. **CHANGELOG.md** ← Change tracking
5. **CODE_REVIEW_CHECKLIST.md** ← QA guidelines
6. **WORKFLOW_GUIDE.md** ← This file

---

## 🚀 QUICK START

### Step 1: Upload Your Project

Place your project files in `/mnt/user-data/uploads/` or tell Claude about your project structure.

### Step 2: Initial Setup

Ask Claude to:
1. Analyze your project structure
2. Fill in PROJECT_STRUCTURE.md with your actual project details
3. Identify the tech stack and dependencies
4. Document the architecture

Example prompt:
```
I'm uploading my React + Flask project. 
Please:
1. Analyze the structure
2. Update PROJECT_STRUCTURE.md with my actual architecture
3. Document the tech stack
4. Identify any areas that need work
```

### Step 3: Define Your Changes

Describe what you want built/fixed:

Example:
```
I need to add a user authentication system with:
- JWT token-based auth
- Login/register endpoints
- Protected routes
- Password hashing with bcrypt

Please use the agentic system to:
1. Plan the changes
2. Make the modifications
3. Add tests
4. Update documentation
```

### Step 4: Claude Works

Claude will automatically:
1. Read AGENTIC_SYSTEM_PROMPT.md
2. Understand your project (from PROJECT_STRUCTURE.md)
3. Plan the changes efficiently
4. Implement with professional quality
5. Add comprehensive comments
6. Update CHANGELOG.md
7. Verify everything works

### Step 5: Review & Merge

Review the changes using CODE_REVIEW_CHECKLIST.md and merge.

---

## 💬 PROMPT TEMPLATES

### For New Features

```
I need to implement [FEATURE] with these requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Technical details:
- Technology: [e.g., Flask/React]
- Database: [if applicable]
- API endpoint: [if applicable]

Please follow the agentic system:
1. Review PROJECT_STRUCTURE.md
2. Plan the implementation
3. Make all necessary changes with comments
4. Add tests
5. Update CHANGELOG.md
6. Verify it compiles/runs
```

### For Bug Fixes

```
I found a bug:
- Description: [What's broken]
- Symptoms: [How it appears to users]
- Files affected: [Which files are involved]
- Reproduce: [Steps to see the bug]

Please:
1. Locate the bug
2. Write a test that fails (proves the bug exists)
3. Fix the bug
4. Verify the test passes
5. Update CHANGELOG.md
6. Document what was wrong and why
```

### For Refactoring

```
I want to refactor [COMPONENT/MODULE] because:
- Reason: [Why it needs refactoring]
- Current state: [Brief description]
- Goals: [What should improve]

Please:
1. Analyze current implementation
2. Design improved structure
3. Implement with minimal behavior change
4. Ensure all tests pass
5. Document changes
6. Update CHANGELOG.md
```

### For Performance Optimization

```
[COMPONENT] is slow:
- Current speed: [e.g., 2000ms for X operation]
- Target speed: [e.g., 500ms]
- Bottleneck: [e.g., N+1 queries, large bundle]

Please:
1. Identify root causes
2. Optimize most impactful areas
3. Add performance tests
4. Measure improvement
5. Document all optimizations
6. Update CHANGELOG.md
```

---

## 📊 EXECUTION EXAMPLES

### Example 1: Adding User Profile API

**Your Prompt**:
```
I need to add a user profile API endpoint that:
- Returns: { id, name, email, created_at, updated_at }
- Requires: JWT authentication
- Endpoint: GET /api/users/{user_id}
- Should handle: Not found (404), Unauthorized (401)

Please implement using the agentic system.
```

**What Claude Does** (automatically):

1. **UNDERSTANDING PHASE**
   ```
   ✓ Read AGENTIC_SYSTEM_PROMPT.md
   ✓ Review PROJECT_STRUCTURE.md (find existing patterns)
   ✓ Examine current user model
   ✓ Check authentication middleware
   ```

2. **PLANNING PHASE**
   ```
   CHANGE_PLAN.txt:
   
   TASK: Add user profile API endpoint
   
   FILES TO MODIFY:
   - src/api/routes.py (add route)
   - src/api/handlers.py (add handler logic)
   - src/services/user_service.py (add get_user method)
   - tests/unit/test_api.py (add tests)
   
   EXECUTION ORDER:
   1. Add get_user to UserService
   2. Add API handler
   3. Add route to routes.py
   4. Add tests
   5. Verify end-to-end
   
   TESTING STRATEGY:
   - Unit test: get_user returns correct data
   - Unit test: get_user raises 404 for missing user
   - Integration test: /api/users/1 returns 200 with data
   - Integration test: without auth returns 401
   ```

3. **IMPLEMENTATION PHASE**
   ```
   # In user_service.py:
   def get_user(self, user_id: int) -> Dict:
       """
       ===== CHANGE: Added user profile retrieval =====
       REASON: Support GET /api/users/{user_id} endpoint
       DATE: 2026-08-21
       TASK: API-001
       """
       user = UserRepository.get_by_id(user_id)
       if not user:
           raise UserNotFoundError(f"User {user_id} not found")
       
       return {
           'id': user.id,
           'name': user.name,
           'email': user.email,
           'created_at': user.created_at,
           'updated_at': user.updated_at
       }
   
   # In routes.py:
   @app.route('/api/users/<int:user_id>', methods=['GET'])
   @require_jwt_token  # Authentication required
   def get_user_profile(user_id):
       """Get user profile by ID."""
       try:
           user_service = UserService()
           user_data = user_service.get_user(user_id)
           return jsonify(user_data), 200
       except UserNotFoundError:
           return jsonify({'error': 'User not found'}), 404
       except Exception as e:
           logger.error(f"Error fetching user {user_id}: {e}")
           return jsonify({'error': 'Internal server error'}), 500
   
   # In test_api.py:
   def test_get_user_profile_success():
       """Test successful profile retrieval."""
       response = client.get(
           '/api/users/1',
           headers={'Authorization': 'Bearer valid_token'}
       )
       assert response.status_code == 200
       assert response.json['id'] == 1
       assert response.json['name'] == 'Test User'
   
   def test_get_user_profile_not_found():
       """Test 404 for missing user."""
       response = client.get(
           '/api/users/9999',
           headers={'Authorization': 'Bearer valid_token'}
       )
       assert response.status_code == 404
   
   def test_get_user_profile_unauthorized():
       """Test 401 without authentication."""
       response = client.get('/api/users/1')
       assert response.status_code == 401
   ```

4. **DOCUMENTATION PHASE**
   ```
   CHANGELOG.md entry:
   
   ## [2026-08-21] - Version 1.1.0
   
   ### Added
   - **File**: src/api/routes.py  
     **Endpoint**: GET /api/users/{user_id}  
     **Purpose**: Retrieve user profile  
     **Auth Required**: Yes (JWT token)  
     **Response**: { id, name, email, created_at, updated_at }  
     **Error Codes**: 401 (unauthorized), 404 (not found)  
     **Lines Added**: 45-62  
     **Task**: API-001  
   
   - **File**: src/services/user_service.py  
     **Method**: get_user(user_id: int) -> Dict  
     **Purpose**: Fetch user from database  
     **Complexity**: Low  
     **Test Coverage**: 95%  
   
   ### Changed
   - **File**: tests/unit/test_api.py  
     **Change**: Added 3 new test cases for user profile endpoint  
     **Coverage Before**: 82%  
     **Coverage After**: 89%  
   ```

5. **VERIFICATION PHASE**
   ```
   ✓ Code compiles without errors
   ✓ All tests pass (pytest tests/)
   ✓ Test coverage >80% (89%)
   ✓ Manual verification: curl request returns correct data
   ✓ Error handling verified
   ✓ Authentication verified
   ✓ CHANGELOG.md updated
   ✓ Documentation complete
   ```

**Result**: Professional, production-ready feature implemented!

---

## 🔄 TYPICAL WORKFLOW

### Morning: Start a Task

```
1. Claude reads AGENTIC_SYSTEM_PROMPT.md
2. Asks you to describe the task
3. Reviews PROJECT_STRUCTURE.md to understand codebase
4. Creates a CHANGE_PLAN.txt (temporary)
5. Awaits approval to proceed
```

**Your Response**:
```
Yes, proceed with the implementation. 
Here's the exact requirement: [details]
```

### Mid-Day: Implementation

```
1. Claude modifies files one by one
2. Adds professional comments
3. Runs tests after each file
4. Fixes any issues found
5. Verifies no regressions
```

**You monitor**: Just check the changes are happening correctly

### Late Day: Completion

```
1. Claude updates CHANGELOG.md
2. Creates summary of changes
3. Shows all modified files
4. Provides testing instructions
5. Asks for verification
```

**Your Action**: Review changes and merge

---

## 🎯 COMMON WORKFLOWS

### Workflow 1: Bug Fix (1-2 hours)

```
You:     "There's a bug in user login - it fails with special characters in password"
Claude:  "I'll reproduce the bug with a test"
You:     "Yes, do that"
Claude:  "✓ Test fails as expected (confirms bug)"
Claude:  "Fixing now... ✓ Test passes"
Claude:  "CHANGELOG.md updated"
You:     "Looks good, merged!"
```

### Workflow 2: Feature Addition (4-8 hours)

```
You:     "Add email verification during signup"
Claude:  "Planning implementation... [CHANGE_PLAN.txt shown]"
You:     "Looks good, implement"
Claude:  "Implementing... [files being modified]"
Claude:  "✓ All tests pass"
Claude:  "✓ CHANGELOG.md updated"
You:     "Review code... looks professional, merged!"
```

### Workflow 3: Performance Optimization (2-4 hours)

```
You:     "User search is slow - takes 3 seconds for 1000 users"
Claude:  "Analyzing... root cause: no database index"
Claude:  "Adding index and optimizing query..."
Claude:  "Before: 3000ms, After: 150ms (95% improvement!)"
Claude:  "✓ Tests pass, CHANGELOG.md updated"
You:     "Excellent! Verified and merged."
```

### Workflow 4: Refactoring (6-12 hours)

```
You:     "User model is getting huge, need to split into multiple models"
Claude:  "Designing new structure... [architecture shown]"
You:     "Looks good!"
Claude:  "Refactoring... [multiple files being changed]"
Claude:  "✓ All tests pass (100% compatibility)"
Claude:  "✓ CHANGELOG.md updated"
You:     "Reviewed thoroughly, looks great! Merged."
```

---

## 🛠️ CUSTOMIZING THE SYSTEM

### Add Your Own Prompts

Create a `CUSTOM_PROMPTS.md`:

```markdown
# Custom Development Prompts

## Company Standards
- All errors must be logged to [your logging service]
- All API responses must include request_id
- All timestamps in UTC
- Rate limiting: 1000 requests/minute per user

## Project-Specific Guidelines
- Use [your ORM], not [other ORM]
- Frontend components must be in functional style with hooks
- All data fetched through DataService, not directly from API
- Database queries must be analyzed before deployment
```

### Customize CODE_REVIEW_CHECKLIST.md

Add your company-specific requirements:

```markdown
## Company-Specific Checks

□ All changes logged to Sentry
□ Feature flag added (if feature)
□ Analytics event tracked (if user-facing)
□ Support documentation updated
□ Marketing notified (if public feature)
□ Performance metrics added to monitoring
```

---

## 📈 MEASURING SUCCESS

Track these metrics:

```
DEVELOPMENT EFFICIENCY:
- Task completion time: ↓ (should decrease with system)
- Code quality issues: ↓ (fewer bugs)
- Test coverage: ↑ (should increase)
- CHANGELOG accuracy: ↑ (should be 100%)
- Review comments: ↓ (fewer issues in review)
- Regression bugs: ↓ (should decrease)

Example:
Week 1 (without system): 8 features, 15 issues in PR review
Week 2 (with system): 10 features, 2 issues in PR review
Improvement: +25% features, -87% review issues
```

---

## 🚨 TROUBLESHOOTING

### Issue: Claude Not Following Format

**Solution**: Start every prompt with:
```
Please follow AGENTIC_SYSTEM_PROMPT.md:

1. [Your task description]
2. [Any specific requirements]
```

### Issue: Changes Don't Match Project Style

**Solution**: Ensure PROJECT_STRUCTURE.md includes:
- Code style examples
- Naming conventions
- Patterns used in project
- Technology-specific standards

### Issue: Tests Failing After Changes

**Solution**: In prompt, include:
```
Before implementing:
1. Run existing test suite
2. Report any failures
3. Do not proceed if tests fail
```

### Issue: CHANGELOG Not Updated

**Solution**: Always end prompts with:
```
Important: After all changes, update CHANGELOG.md with:
- Files modified
- What changed
- Why
- Test coverage
```

---

## 🎓 BEST PRACTICES

### 1. Be Specific in Requests

```
❌ "Add login feature"

✅ "Add JWT-based login with:
   - Endpoint: POST /api/auth/login
   - Input: {email, password}
   - Output: {access_token, expires_in}
   - Requirements: Hash passwords with bcrypt, 24-hour expiration
   - Error codes: 401 (invalid), 422 (validation)"
```

### 2. Include Context

```
❌ "Add user deletion"

✅ "Add user deletion endpoint:
   - DELETE /api/users/{user_id}
   - Only admins can delete
   - Soft delete (set is_deleted=true, don't remove from DB)
   - Cascade: delete user_posts, user_comments
   - Audit log deletion event
   - Return 204 No Content on success"
```

### 3. Verify Changes

```
After Claude completes:
1. Review CHANGELOG.md entries
2. Check modified files exist
3. Run tests locally
4. Test manually
5. Review code quality
```

### 4. Iterate Incrementally

```
✓ Instead of: "Build entire authentication system"
✓ Do: 
  1. "Add login endpoint"
  2. "Add logout endpoint"
  3. "Add token refresh"
  4. "Add password reset"
  (One at a time, each verified before next)
```

---

## 📚 FILE REFERENCE

| File | Purpose | When to Use |
|------|---------|------------|
| AGENTIC_SYSTEM_PROMPT.md | Main system instructions | Always |
| DEVELOPMENT_GUIDE.md | Coding standards | During implementation |
| PROJECT_STRUCTURE.md | Project reference | Before/after changes |
| CHANGELOG.md | Change history | After each task |
| CODE_REVIEW_CHECKLIST.md | Quality assurance | Before merging |
| WORKFLOW_GUIDE.md | This guide | For process help |

---

## 🎯 SUMMARY

1. **Upload your project** → Claude analyzes it
2. **Describe what you want** → Claude plans changes
3. **Claude implements** → Professional, tested code
4. **Claude documents** → CHANGELOG.md automatically updated
5. **You review** → Using CODE_REVIEW_CHECKLIST.md
6. **You merge** → Production-ready code

**Result**: Software engineering team efficiency in a single AI agent.

---

**The system is now ready. Upload your project and describe your first task!**
