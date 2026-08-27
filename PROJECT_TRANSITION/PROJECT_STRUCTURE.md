# PROJECT STRUCTURE

**Project Name**: [To be filled]  
**Version**: [To be filled]  
**Last Updated**: [YYYY-MM-DD]  
**Tech Stack**: [To be filled]

---

## 📂 Directory Structure

Fill this section with your actual project structure:

```
project-root/
│
├── README.md                 # Project overview
├── CHANGELOG.md              # All changes (auto-updated)
├── AGENTIC_SYSTEM_PROMPT.md  # System instructions
├── DEVELOPMENT_GUIDE.md      # Coding standards
├── PROJECT_STRUCTURE.md      # This file
├── CODE_REVIEW_CHECKLIST.md  # QA guidelines
│
├── src/                      # Source code
│   ├── __init__.py          # Package initialization
│   ├── main.py              # Application entry point
│   ├── config.py            # Configuration management
│   │
│   ├── api/                 # API/Backend logic
│   │   ├── __init__.py
│   │   ├── routes.py        # API endpoint definitions
│   │   ├── handlers.py      # Request handlers
│   │   └── middleware.py    # API middleware
│   │
│   ├── database/            # Database layer
│   │   ├── __init__.py
│   │   ├── models.py        # ORM/Database models
│   │   ├── queries.py       # Common database queries
│   │   ├── migrations/      # Database migrations
│   │   └── schema.sql       # Schema definition
│   │
│   ├── services/            # Business logic
│   │   ├── __init__.py
│   │   ├── user_service.py
│   │   ├── data_service.py
│   │   └── auth_service.py
│   │
│   ├── utils/               # Utility functions
│   │   ├── __init__.py
│   │   ├── validators.py    # Input validation
│   │   ├── helpers.py       # Helper functions
│   │   ├── logger.py        # Logging setup
│   │   └── exceptions.py    # Custom exceptions
│   │
│   ├── frontend/            # Frontend code (if full-stack)
│   │   ├── public/
│   │   ├── src/
│   │   │   ├── components/  # React components
│   │   │   ├── pages/       # Page components
│   │   │   ├── hooks/       # Custom React hooks
│   │   │   ├── utils/       # Frontend utilities
│   │   │   └── App.jsx
│   │   └── package.json
│   │
│   └── tests/               # Test suite
│       ├── __init__.py
│       ├── unit/            # Unit tests
│       ├── integration/     # Integration tests
│       ├── e2e/             # End-to-end tests
│       ├── fixtures/        # Test data
│       └── conftest.py      # Pytest configuration
│
├── docs/                    # Documentation
│   ├── API.md              # API documentation
│   ├── ARCHITECTURE.md     # Architecture decisions
│   ├── SETUP.md            # Setup instructions
│   └── TROUBLESHOOTING.md  # Common issues
│
├── config/                  # Configuration files
│   ├── development.env      # Dev environment variables
│   ├── production.env       # Prod environment variables
│   ├── database.config      # Database configuration
│   └── logging.config       # Logging configuration
│
├── scripts/                 # Utility scripts
│   ├── setup.sh            # Initial setup
│   ├── migrate.py          # Database migration runner
│   ├── seed_data.py        # Seed test data
│   └── deploy.sh           # Deployment script
│
├── requirements.txt         # Python dependencies (if backend)
├── package.json            # Node.js dependencies (if frontend)
├── Dockerfile              # Docker configuration
├── docker-compose.yml      # Docker compose setup
├── .gitignore             # Git ignore rules
└── .env.example           # Example environment variables
```

---

## 🏗️ Architecture Overview

### High-Level Architecture

```
[Client/Frontend]
       ↓
[API Gateway / Load Balancer]
       ↓
[REST/GraphQL API Server]
       ↓
[Business Logic Services]
       ↓
[Database Layer]
       ↓
[Data Storage (PostgreSQL, MongoDB, etc.)]
```

Replace this with your actual architecture.

### Component Interactions

```
[Describe how major components interact]

Example:
1. User submits form in Frontend
2. Frontend sends HTTP POST to /api/users
3. API Gateway validates request
4. UserController receives request
5. UserService processes business logic
6. UserRepository executes database query
7. Database returns user record
8. Response flows back through layers
```

---

## 📦 Key Dependencies

### Backend Dependencies (Python)
```
- Flask (or Django/FastAPI): Web framework
- SQLAlchemy: ORM for database
- Pydantic: Data validation
- Pytest: Testing framework
- python-dotenv: Environment variable management
- Requests: HTTP client
- Gunicorn: WSGI server
```

### Frontend Dependencies (JavaScript/React)
```
- React: UI framework
- Next.js: React framework with SSR
- Axios: HTTP client
- Redux (or Context API): State management
- Tailwind CSS: Styling
- React Router: Client-side routing
```

### Database
```
- PostgreSQL (or MySQL/MongoDB)
- Redis: Caching layer
- Alembic: Database migrations
```

---

## 🔄 Data Flow

### Example: User Login Flow

```
1. USER ACTION
   └─ User enters credentials in login form

2. FRONTEND
   └─ LoginForm.jsx validates input
   └─ Sends POST /api/auth/login with credentials

3. API LAYER
   └─ auth_routes.py receives request
   └─ Calls AuthController.handle_login()

4. SERVICE LAYER
   └─ AuthService.authenticate_user()
   └─ Validates credentials against database
   └─ Generates JWT token

5. DATABASE LAYER
   └─ UserRepository.get_by_email()
   └─ Queries users table
   └─ Returns user record with hashed password

6. VALIDATION
   └─ Compare provided password with stored hash
   └─ Return success/failure

7. RESPONSE
   └─ API returns JWT token
   └─ Frontend stores token in localStorage/cookie
   └─ User logged in ✓

8. PROTECTED REQUESTS
   └─ Frontend includes JWT in Authorization header
   └─ API middleware validates token
   └─ Request proceeds or returns 401 Unauthorized
```

---

## 🧪 Testing Strategy

### Unit Tests
- **Location**: `tests/unit/`
- **Coverage Target**: >80%
- **Tools**: pytest (Python), Jest/Vitest (JavaScript)
- **When to Run**: Before every commit

### Integration Tests
- **Location**: `tests/integration/`
- **Coverage Target**: Critical user flows
- **Tools**: pytest, Postman
- **When to Run**: Before deployment

### End-to-End Tests
- **Location**: `tests/e2e/`
- **Coverage Target**: Main user journeys
- **Tools**: Playwright, Cypress
- **When to Run**: Before production release

### Running Tests
```bash
# All tests
pytest tests/

# Specific test file
pytest tests/unit/test_auth.py

# With coverage
pytest --cov=src tests/

# Integration tests only
pytest tests/integration/
```

---

## 🚀 Deployment Architecture

### Development Environment
```
Local Machine
├─ Python venv / Node.js
├─ Local Database (SQLite or Docker PostgreSQL)
└─ npm run dev / python app.py
```

### Staging Environment
```
Staging Server
├─ Docker container
├─ Staging database
├─ Environment variables (.env)
└─ Health checks enabled
```

### Production Environment
```
Production Server
├─ Kubernetes / Docker Swarm (if scaled)
├─ Load Balancer (nginx)
├─ Multiple replicas for redundancy
├─ Production database (managed service)
├─ Redis cache layer
├─ CDN for static assets
└─ Monitoring & logging
```

---

## 🔐 Environment Configuration

### Required Environment Variables

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
DATABASE_POOL_SIZE=10

# API Server
API_PORT=8000
API_HOST=0.0.0.0
API_DEBUG=False

# Authentication
JWT_SECRET_KEY=your-secret-key-here
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24

# External Services
AWS_ACCESS_KEY_ID=xxx
AWS_SECRET_ACCESS_KEY=xxx
S3_BUCKET_NAME=my-bucket

# Logging
LOG_LEVEL=INFO
LOG_FORMAT=json

# Frontend (if React)
REACT_APP_API_URL=http://localhost:8000/api
REACT_APP_ENV=development
```

### Setup Instructions
```bash
# 1. Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Set up environment variables
cp .env.example .env
# Edit .env with your values

# 4. Initialize database
python scripts/setup.py
python scripts/migrate.py

# 5. Run application
python src/main.py
```

---

## 📊 Database Schema

### Core Tables

#### Users Table
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(100) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);

CREATE INDEX idx_users_email ON users(email);
```

#### Other Tables
[Document your actual table structures]

### Relationships
[Show foreign key relationships and data model]

---

## 🔍 Key Modules Explained

### api/routes.py
- **Purpose**: Define API endpoints
- **Key Functions**: 
  - `@app.route('/api/users', methods=['GET'])` - List users
  - `@app.route('/api/users', methods=['POST'])` - Create user
- **Dependencies**: Flask, UserController
- **Modified**: [When last changed]

### services/user_service.py
- **Purpose**: Business logic for user operations
- **Key Functions**:
  - `create_user(data)` - Validate and create user
  - `get_user_by_id(id)` - Retrieve user
  - `update_user(id, data)` - Update user
- **Dependencies**: UserRepository, validators
- **Modified**: [When last changed]

### database/models.py
- **Purpose**: Database models/ORM definitions
- **Key Classes**:
  - `User` - User model with fields
  - `Post` - Post model
- **Dependencies**: SQLAlchemy
- **Modified**: [When last changed]

[Add descriptions for your actual modules]

---

## 🐛 Known Issues & Technical Debt

### Current Issues
1. **Performance Issue**: User search is slow with >10k records
   - **Impact**: Search page loads in 2+ seconds
   - **Priority**: High
   - **Solution**: Add database index on username

2. **Missing Feature**: Email notifications
   - **Impact**: Users don't receive alerts
   - **Priority**: Medium
   - **Planned**: Q3 2026

### Technical Debt
1. **Legacy Code**: Old authentication module (auth_v1.py)
   - **Impact**: Duplicate code, maintenance burden
   - **Effort**: 3 days to refactor
   - **Status**: Backlog

2. **Missing Tests**: File upload module (coverage: 20%)
   - **Impact**: Bugs introduced easily
   - **Effort**: 2 days to add tests
   - **Status**: Backlog

---

## 📈 Scaling Considerations

### Current Capacity
- **Max Users**: ~10,000 concurrent
- **Database**: Single PostgreSQL instance
- **API Servers**: 1 instance
- **Storage**: Single filesystem

### Scaling Strategy
1. **Vertical Scaling** (first stage):
   - Increase server resources (CPU, RAM)
   - Upgrade database
   - Add Redis cache layer

2. **Horizontal Scaling** (second stage):
   - Multiple API server instances
   - Load balancer (nginx)
   - Database replication
   - Separate read replicas

3. **Advanced Scaling**:
   - Kubernetes orchestration
   - Microservices architecture
   - Database sharding
   - CDN for static assets

---

## 🔄 CI/CD Pipeline

### Workflow
```
1. Developer pushes to GitHub
2. GitHub Actions triggered
3. Lint checks (ESLint, Pylint)
4. Unit tests run
5. Integration tests run
6. Build Docker image
7. Push to registry
8. Deploy to staging (auto)
9. Manual approval to production
10. Deploy to production
```

### Status Checks Required
- [x] All tests pass
- [x] Code coverage >80%
- [x] No security vulnerabilities
- [x] Code style checks pass
- [x] Build succeeds

---

## 📚 Important Files at a Glance

| File | Purpose | Priority |
|------|---------|----------|
| `src/main.py` | Application entry point | Critical |
| `src/database/models.py` | Database schema | Critical |
| `src/api/routes.py` | API endpoints | High |
| `src/services/*` | Business logic | High |
| `tests/*` | Test suite | High |
| `docs/API.md` | API documentation | Medium |
| `CHANGELOG.md` | Change history | Medium |
| `requirements.txt` | Dependencies | Critical |

---

## 🚨 Common Tasks

### How to add a new feature?
1. Create feature branch: `git checkout -b feature/name`
2. Update DATABASE (if needed): Edit `database/migrations/`
3. Add MODEL: Update `database/models.py`
4. Add SERVICE: Create/update `services/feature_service.py`
5. Add API ROUTE: Update `api/routes.py`
6. Add TESTS: Create `tests/unit/test_feature.py`
7. UPDATE CHANGELOG.md
8. Create PR and request review

### How to fix a bug?
1. Create bug branch: `git checkout -b fix/bug-name`
2. Reproduce bug with test
3. Add test case to `tests/unit/`
4. Fix in source code
5. Verify test passes
6. Update CHANGELOG.md
7. Create PR with bug reference

### How to deploy to production?
1. Ensure all tests pass: `npm test`
2. Update version: Edit `__version__` in `src/main.py`
3. Update CHANGELOG.md with release notes
4. Create release: `git tag v1.0.0`
5. Push to GitHub
6. CI/CD pipeline runs automatically
7. Manual approval triggers production deployment

---

**Update this document as your project evolves. It's a living reference for developers.**
