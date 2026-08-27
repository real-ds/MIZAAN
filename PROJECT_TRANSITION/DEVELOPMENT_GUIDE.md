# 📖 DEVELOPMENT GUIDE

**Version**: 1.0  
**Last Updated**: August 2026  
**Purpose**: Detailed coding standards, patterns, and best practices

---

## 🎨 CODE STYLE & CONVENTIONS

### Python Standards

**Naming Conventions**:
```python
# Variables and functions: snake_case
user_count = 0
def calculate_total_price():
    pass

# Classes: PascalCase
class UserAccount:
    pass

# Constants: UPPER_SNAKE_CASE
MAX_RETRY_ATTEMPTS = 3
DEFAULT_TIMEOUT = 30

# Private methods/variables: _leading_underscore
class DataHandler:
    def _internal_process(self):
        pass
    
    _cache = {}
```

**Docstring Format** (Google Style):
```python
def process_user_data(users: List[Dict], filter_inactive: bool = True) -> Dict[str, int]:
    """
    Process user data and return statistics.
    
    Aggregates user information and applies optional filtering.
    Performance: O(n) where n = number of users.
    
    Args:
        users: List of user dictionaries with 'id', 'name', 'status' keys
        filter_inactive: If True, excludes users with status='inactive' (default: True)
    
    Returns:
        Dictionary with keys:
            - 'total': Total number of users processed
            - 'active': Count of active users
            - 'inactive': Count of inactive users (if filter_inactive=False)
    
    Raises:
        ValueError: If users list is empty
        TypeError: If users contains non-dict elements
    
    Examples:
        >>> users = [{'id': 1, 'name': 'Alice', 'status': 'active'}]
        >>> result = process_user_data(users)
        >>> result['active']
        1
    
    Note:
        This function assumes all user dicts have required keys.
        Validation happens in calling function.
    
    Changed: 2026-08-21 - Added filter_inactive parameter
    """
    # Implementation here
    pass
```

**Type Hints** (Required for new code):
```python
from typing import List, Dict, Optional, Tuple, Callable, Union

# Function signatures
def fetch_user(user_id: int) -> Optional[Dict[str, any]]:
    pass

def process_batch(items: List[str], processor: Callable[[str], str]) -> List[str]:
    pass

def complex_return() -> Tuple[str, int, List[Dict]]:
    pass

# Class attributes
class UserService:
    users: List[Dict[str, any]]
    cache: Dict[int, any]
    
    def __init__(self, db_url: str) -> None:
        self.db_url = db_url
```

### JavaScript/TypeScript Standards

**Naming Conventions**:
```javascript
// Variables and functions: camelCase
const userName = "Alice";
function calculateTotalPrice() { }

// Classes/Components: PascalCase
class UserAccount { }
function UserProfile() { }

// Constants: UPPER_SNAKE_CASE
const MAX_RETRY_ATTEMPTS = 3;
const DEFAULT_TIMEOUT = 30000; // milliseconds

// Private: Leading underscore
class DataHandler {
  _internalCache = {};
  
  _processData() { }
}
```

**JSDoc Format** (TypeScript):
```typescript
/**
 * Process user data and return statistics.
 * 
 * Aggregates user information and applies optional filtering.
 * Performance: O(n) where n = number of users.
 *
 * @param {Array<UserObject>} users - List of user objects with id, name, status
 * @param {boolean} [filterInactive=true] - Whether to exclude inactive users
 * 
 * @returns {UserStats} Object with total, active, inactive counts
 * @returns {number} UserStats.total - Total users processed
 * @returns {number} UserStats.active - Active user count
 * @returns {number} UserStats.inactive - Inactive user count
 * 
 * @throws {Error} If users array is empty
 * @throws {TypeError} If users contains non-objects
 * 
 * @example
 * const users = [{id: 1, name: 'Alice', status: 'active'}];
 * const stats = processUserData(users);
 * console.log(stats.active); // 1
 * 
 * @since 1.0.0
 * @updated 2026-08-21 - Added filterInactive parameter
 */
function processUserData(
  users: UserObject[],
  filterInactive: boolean = true
): UserStats { }
```

---

## 🏗️ ARCHITECTURAL PATTERNS

### Error Handling

**Python Pattern**:
```python
# ❌ BAD: Silent failures
def fetch_data(url):
    response = requests.get(url)
    return response.json()

# ✅ GOOD: Explicit error handling
def fetch_data(url: str) -> Dict:
    """Fetch data from URL with proper error handling."""
    if not url or not isinstance(url, str):
        raise ValueError(f"Invalid URL: {url}")
    
    try:
        response = requests.get(url, timeout=30)
        response.raise_for_status()  # Raise for 4xx/5xx
        
        data = response.json()
        if not data:
            raise ValueError("Empty response received")
        
        return data
        
    except requests.Timeout:
        logger.error(f"Timeout fetching {url}")
        raise TimeoutError(f"Request to {url} exceeded 30s") from None
        
    except requests.HTTPError as e:
        logger.error(f"HTTP error {e.response.status_code}: {url}")
        raise ConnectionError(f"Failed to fetch {url}: {e.response.status_code}") from e
        
    except requests.RequestException as e:
        logger.error(f"Request failed: {url} - {str(e)}")
        raise ConnectionError(f"Connection failed for {url}") from e
        
    except json.JSONDecodeError as e:
        logger.error(f"Invalid JSON from {url}")
        raise ValueError(f"Invalid JSON response from {url}") from e
```

**JavaScript Pattern**:
```javascript
// ❌ BAD: Silent failures
async function fetchData(url) {
  const response = await fetch(url);
  return response.json();
}

// ✅ GOOD: Explicit error handling
async function fetchData(url: string): Promise<any> {
  // Validate input
  if (!url || typeof url !== 'string') {
    throw new TypeError(`Invalid URL: ${url}`);
  }
  
  try {
    const response = await fetch(url, {
      timeout: 30000,
      headers: { 'Accept': 'application/json' }
    });
    
    // Check response status
    if (!response.ok) {
      throw new HTTPError(
        `HTTP ${response.status}: ${response.statusText}`,
        response.status
      );
    }
    
    // Parse and validate JSON
    const data = await response.json();
    
    if (!data || Object.keys(data).length === 0) {
      throw new Error('Empty response received');
    }
    
    return data;
    
  } catch (error) {
    if (error instanceof TypeError) {
      logger.error(`Network error: ${error.message}`);
      throw new Error(`Connection failed: ${error.message}`) from error;
    }
    
    if (error instanceof SyntaxError) {
      logger.error(`Invalid JSON from ${url}`);
      throw new Error(`Invalid JSON response from ${url}`) from error;
    }
    
    logger.error(`Fetch failed for ${url}:`, error);
    throw error;
  }
}
```

### Logging Strategy

**Python**:
```python
import logging

logger = logging.getLogger(__name__)

# Different levels for different situations
logger.debug("Processing item_id: 123")  # Development troubleshooting
logger.info("User login successful: user_id=456")  # Important events
logger.warning("Retry attempt 2/3 for API call")  # Degraded conditions
logger.error("Database connection failed", exc_info=True)  # Errors with stack trace
logger.critical("Authentication service down - ALERT REQUIRED")  # Severe issues

# Never log sensitive data
# ❌ NEVER: logger.info(f"User login: {username}:{password}")
# ✓ DO: logger.info(f"User login: {username}")
```

**JavaScript**:
```javascript
// Use appropriate console methods
console.debug("Processing item_id:", 123);  // Development only
console.log("User login successful: user_id=456");  // Important events
console.warn("Retry attempt 2/3 for API call");  // Degraded conditions
console.error("Database connection failed", error);  // Errors
console.error("Authentication service down - ALERT REQUIRED");  // Severe

// Production: Use logger service
logger.info("User login", { userId: 456 });
logger.error("Connection failed", { 
  url: apiUrl, 
  statusCode: 503,
  duration: 2500 
});
```

### Data Validation

**Python**:
```python
from typing import List, Dict
from dataclasses import dataclass

@dataclass
class User:
    """Validated user data."""
    id: int
    name: str
    email: str
    age: int
    
    def __post_init__(self):
        """Validate data after initialization."""
        if not isinstance(self.id, int) or self.id <= 0:
            raise ValueError(f"Invalid user id: {self.id}")
        
        if not self.name or not isinstance(self.name, str):
            raise ValueError(f"Invalid name: {self.name}")
        
        if '@' not in self.email:
            raise ValueError(f"Invalid email: {self.email}")
        
        if not (0 <= self.age <= 150):
            raise ValueError(f"Invalid age: {self.age}")

def validate_user_list(users: List[Dict]) -> List[User]:
    """Convert and validate list of user dicts."""
    validated = []
    
    for idx, user_dict in enumerate(users):
        try:
            user = User(**user_dict)
            validated.append(user)
        except (TypeError, ValueError) as e:
            logger.error(f"Invalid user at index {idx}: {e}")
            raise ValueError(f"User validation failed at index {idx}") from e
    
    return validated
```

**JavaScript**:
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

function validateUser(user: unknown): User {
  if (typeof user !== 'object' || user === null) {
    throw new TypeError('User must be an object');
  }
  
  const u = user as Record<string, unknown>;
  
  // Validate each field
  if (!Number.isInteger(u.id) || u.id <= 0) {
    throw new Error(`Invalid user id: ${u.id}`);
  }
  
  if (typeof u.name !== 'string' || !u.name.trim()) {
    throw new Error(`Invalid name: ${u.name}`);
  }
  
  if (typeof u.email !== 'string' || !u.email.includes('@')) {
    throw new Error(`Invalid email: ${u.email}`);
  }
  
  if (!Number.isInteger(u.age) || u.age < 0 || u.age > 150) {
    throw new Error(`Invalid age: ${u.age}`);
  }
  
  return u as User;
}

function validateUserList(users: unknown[]): User[] {
  if (!Array.isArray(users)) {
    throw new TypeError('Input must be an array');
  }
  
  return users.map((user, idx) => {
    try {
      return validateUser(user);
    } catch (error) {
      logger.error(`Invalid user at index ${idx}:`, error);
      throw new Error(`User validation failed at index ${idx}`);
    }
  });
}
```

---

## 📊 PERFORMANCE GUIDELINES

### Complexity Analysis

Always consider:
```
Time Complexity: O(?) - What's the worst-case runtime?
Space Complexity: O(?) - How much memory is needed?
```

**Examples**:
```python
# O(n) - Linear search
def find_user(users, target_id):
    for user in users:
        if user.id == target_id:
            return user
    return None

# O(n log n) - Efficient sorting
def sort_by_name(users):
    return sorted(users, key=lambda u: u.name)

# O(1) - Dictionary lookup (average case)
user_dict = {user.id: user for user in users}
user = user_dict.get(target_id)

# O(n²) - AVOID if possible (nested loops)
def find_duplicates_naive(users):
    duplicates = []
    for i, user1 in enumerate(users):
        for j, user2 in enumerate(users):
            if i != j and user1.email == user2.email:
                duplicates.append((user1, user2))
    return duplicates

# O(n) - Better approach
def find_duplicates_optimized(users):
    seen = {}
    duplicates = []
    for user in users:
        if user.email in seen:
            duplicates.append((seen[user.email], user))
        else:
            seen[user.email] = user
    return duplicates
```

### Optimization Checklist

```
PERFORMANCE REVIEW:
□ No unnecessary database queries (N+1 problem)
□ No unnecessary API calls
□ Efficient data structures chosen (dict vs list, etc.)
□ Algorithms optimized (O(n) better than O(n²))
□ Caching implemented where beneficial
□ Loop operations minimized
□ No memory leaks (proper cleanup)
□ Lazy loading implemented where appropriate
```

---

## 🧪 TESTING PATTERNS

### Unit Tests - Python

```python
import unittest
from unittest.mock import patch, MagicMock

class TestUserService(unittest.TestCase):
    """Test UserService functionality."""
    
    def setUp(self):
        """Set up test fixtures."""
        self.service = UserService()
        self.test_user = {'id': 1, 'name': 'Test', 'email': 'test@example.com'}
    
    def tearDown(self):
        """Clean up after tests."""
        self.service.close()
    
    def test_create_user_success(self):
        """Test successful user creation."""
        result = self.service.create_user(self.test_user)
        self.assertIsNotNone(result)
        self.assertEqual(result['id'], 1)
    
    def test_create_user_invalid_email(self):
        """Test creation fails with invalid email."""
        invalid_user = {**self.test_user, 'email': 'invalid'}
        with self.assertRaises(ValueError):
            self.service.create_user(invalid_user)
    
    @patch('requests.get')
    def test_fetch_remote_user(self, mock_get):
        """Test fetching user from remote API."""
        mock_response = MagicMock()
        mock_response.json.return_value = self.test_user
        mock_get.return_value = mock_response
        
        result = self.service.fetch_remote_user(1)
        self.assertEqual(result['name'], 'Test')
        mock_get.assert_called_once()
```

### Unit Tests - JavaScript

```javascript
import { describe, it, expect, beforeEach, afterEach, vi } from 'vitest';
import { UserService } from './userService';

describe('UserService', () => {
  let service: UserService;
  
  const testUser = {
    id: 1,
    name: 'Test',
    email: 'test@example.com'
  };
  
  beforeEach(() => {
    service = new UserService();
  });
  
  afterEach(() => {
    service.close();
  });
  
  it('should create user successfully', async () => {
    const result = await service.createUser(testUser);
    expect(result).toBeDefined();
    expect(result.id).toBe(1);
  });
  
  it('should reject user with invalid email', async () => {
    const invalidUser = { ...testUser, email: 'invalid' };
    await expect(
      service.createUser(invalidUser)
    ).rejects.toThrow('Invalid email');
  });
  
  it('should fetch remote user', async () => {
    const mockFetch = vi.fn().mockResolvedValue({
      json: async () => testUser
    });
    
    global.fetch = mockFetch;
    
    const result = await service.fetchRemoteUser(1);
    expect(result.name).toBe('Test');
    expect(mockFetch).toHaveBeenCalledOnce();
  });
});
```

---

## 🔒 SECURITY BEST PRACTICES

### Input Validation
```python
# ✅ Always validate user input
from typing import Optional

def process_user_input(user_data: str) -> str:
    # Check type
    if not isinstance(user_data, str):
        raise TypeError(f"Expected string, got {type(user_data)}")
    
    # Check length
    if len(user_data) > 1000:
        raise ValueError("Input too long (max 1000 chars)")
    
    # Check content
    if not user_data.strip():
        raise ValueError("Input cannot be empty")
    
    # Sanitize
    cleaned = user_data.strip()
    
    return cleaned
```

### SQL Injection Prevention
```python
# ❌ BAD: String concatenation (SQL injection risk)
query = f"SELECT * FROM users WHERE id = {user_id}"
cursor.execute(query)

# ✅ GOOD: Parameterized queries
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))
```

### Secrets Management
```python
# ❌ BAD: Hardcoded secrets
API_KEY = "sk_live_abc123def456"
db_password = "MyPassword123"

# ✅ GOOD: Environment variables
import os
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv('API_KEY')
DB_PASSWORD = os.getenv('DB_PASSWORD')

if not API_KEY:
    raise ValueError("API_KEY not set in environment")
```

---

## 📦 DEPENDENCY MANAGEMENT

### Python (requirements.txt format)
```
# Core dependencies
flask==2.3.0
sqlalchemy==2.0.0
pydantic==2.0.0

# API & HTTP
requests==2.31.0
httpx==0.24.0

# Database
psycopg2-binary==2.9.0

# Data processing
pandas==2.0.0
numpy==1.24.0

# Testing
pytest==7.0.0
pytest-cov==4.0.0

# Utilities
python-dotenv==1.0.0
pyyaml==6.0

# Always pin versions to ensure reproducibility!
```

### JavaScript (package.json format)
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "next": "^14.0.0",
    "axios": "^1.6.0",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "typescript": "^5.0.0",
    "vitest": "^0.34.0",
    "@testing-library/react": "^14.0.0",
    "eslint": "^8.0.0"
  }
}
```

---

## 🎯 COMMON PITFALLS TO AVOID

```
DO NOT:
❌ Use bare except clauses
❌ Modify mutable default arguments
❌ Forget to handle edge cases (None, empty, etc.)
❌ Log sensitive data (passwords, tokens, SSNs)
❌ Leave debugging console.logs in production code
❌ Use magic numbers (hard-coded constants)
❌ Ignore performance implications
❌ Write overly complex logic without comments
❌ Forget to update docstrings/comments
❌ Leave TODO comments without context

DO:
✓ Specific exception handling
✓ Immutable defaults
✓ Comprehensive edge case handling
✓ Log non-sensitive context
✓ Clean code before commit
✓ Named constants
✓ Optimize before scaling
✓ Keep functions focused
✓ Keep documentation in sync
✓ Reference issues/PRs in TODOs
```

---

## 📋 CODE REVIEW CHECKLIST

Before requesting code review:

```
SELF-REVIEW CHECKLIST:
□ Code compiles/runs without errors
□ All tests pass
□ No console warnings/errors
□ Follows coding standards
□ Naming is clear and consistent
□ Comments explain WHY not WHAT
□ No hardcoded values (except constants)
□ Error handling is comprehensive
□ Performance acceptable
□ Security reviewed (input validation, injection prevention)
□ Documentation updated
□ CHANGELOG.md updated
□ No debugging statements left
□ No sensitive data in code/logs
□ Commit message is clear
```

---

**These guidelines ensure code quality, maintainability, and professional standards.**
