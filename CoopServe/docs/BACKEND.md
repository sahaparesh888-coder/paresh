# Backend Development Guide

## Project Structure

```
backend/
├── app/
│   ├── routes/
│   │   ├── __init__.py
│   │   └── users.py           # User endpoints
│   ├── __init__.py
│   ├── main.py                # FastAPI app entry
│   ├── database.py            # Database configuration
│   ├── models.py              # SQLAlchemy models
│   ├── schemas.py             # Pydantic schemas
│   └── security.py            # Auth utilities
├── tests/
│   ├── __init__.py
│   ├── test_health.py         # Health check tests
│   └── test_users.py          # User endpoint tests
├── alembic/                   # Database migrations
├── requirements.txt           # Python dependencies
├── .env.example               # Configuration template
└── .gitignore
```

## Installation

1. **Create virtual environment**:
```bash
python -m venv venv
```

2. **Activate virtual environment**:
   - Windows: `venv\Scripts\activate`
   - macOS/Linux: `source venv/bin/activate`

3. **Install dependencies**:
```bash
pip install -r requirements.txt
```

4. **Setup environment variables**:
```bash
cp .env.example .env
# Edit .env with your configuration
```

## Running the Application

### Development Mode
```bash
uvicorn app.main:app --reload
```

The API will be available at: `http://127.0.0.1:8000`

### API Documentation
- Swagger UI: `http://127.0.0.1:8000/docs`
- ReDoc: `http://127.0.0.1:8000/redoc`

## Testing

### Run all tests
```bash
pytest
```

### Run specific test file
```bash
pytest tests/test_users.py
```

### Run tests with coverage
```bash
pytest --cov=app tests/
```

### Run tests in verbose mode
```bash
pytest -v
```

## Database Setup

### PostgreSQL Configuration
1. Install PostgreSQL 16+
2. Create database:
```sql
CREATE DATABASE coopserve;
```

3. Update `.env` with database URL:
```
DATABASE_URL=postgresql://username:password@localhost:5432/coopserve
```

### Running Migrations
```bash
alembic upgrade head
```

### Creating New Migrations
```bash
alembic revision --autogenerate -m "Description of changes"
```

## Code Structure

### Models (`app/models.py`)
SQLAlchemy ORM models representing database tables:
- `User` - System users
- `Cooperative` - Cooperative organizations
- `Member` - Members of cooperatives

### Schemas (`app/schemas.py`)
Pydantic models for request/response validation:
- `UserCreate`, `UserUpdate`, `User`
- `LoginRequest`, `TokenResponse`

### Routes (`app/routes/`)
API endpoint implementations:
- `users.py` - User management and authentication

### Security (`app/security.py`)
Authentication and authorization utilities:
- Password hashing (bcrypt)
- JWT token generation and validation

## Environment Variables

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/coopserve

# API Configuration
API_HOST=127.0.0.1
API_PORT=8000

# Security
SECRET_KEY=your-secret-key-here

# Firebase
FIREBASE_CREDENTIALS=path/to/firebase-credentials.json
```

## API Endpoints

See `docs/API.md` for complete API documentation.

### Quick Reference
- `GET /` - Health check
- `POST /api/users/register` - Register new user
- `POST /api/users/login` - User login
- `GET /api/users/{user_id}` - Get user details
- `PUT /api/users/{user_id}` - Update user
- `DELETE /api/users/{user_id}` - Delete user (soft delete)

## Best Practices

1. **Always use virtual environment**
2. **Keep dependencies up to date**: `pip list --outdated`
3. **Write tests for new features**
4. **Use meaningful commit messages**
5. **Follow PEP 8 code style**
6. **Use type hints in function definitions**
7. **Document complex logic with docstrings**

## Common Issues

### PostgreSQL Connection Error
- Ensure PostgreSQL service is running
- Check DATABASE_URL is correct
- Verify database exists

### Module Import Error
- Ensure virtual environment is activated
- Run `pip install -r requirements.txt`
- Check Python path includes project root

### Port Already in Use
```bash
uvicorn app.main:app --reload --port 8001
```

## Debugging

Enable debug logging:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

Use FastAPI debugging in development by adding to `main.py`:
```python
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, debug=True)
```

## Additional Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/)
- [Alembic Documentation](https://alembic.sqlalchemy.org/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
