# FastAPI CRUD Patterns

## Description
Patterns for building clean, maintainable CRUD APIs with FastAPI, including routing, validation, and database integration.

## When to Use
- Building REST APIs with FastAPI
- Setting up database models with SQLAlchemy
- Implementing create, read, update, delete operations
- Adding request validation with Pydantic

## How to Use

1. **Define Pydantic models**: Separate input (schemas) from output (responses)
2. **Use dependency injection**: Database sessions, authentication
3. **Follow REST conventions**: GET/POST/PUT/DELETE for resources
4. **Validate with Pydantic**: Use Field for constraints
5. **Handle errors explicitly**: Return appropriate HTTP status codes

## Examples

### Pydantic Schemas
```python
from pydantic import BaseModel, EmailStr, Field
from datetime import datetime
from typing import Optional

class UserBase(BaseModel):
    email: EmailStr
    username: str = Field(..., min_length=3, max_length=50)

class UserCreate(UserBase):
    password: str = Field(..., min_length=8)

class UserUpdate(BaseModel):
    email: Optional[EmailStr] = None
    username: Optional[str] = Field(None, min_length=3, max_length=50)

class UserResponse(UserBase):
    id: int
    created_at: datetime
    class Config:
        from_attributes = True
```

### CRUD Router Pattern
```python
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session
from typing import List

router = APIRouter(prefix="/users", tags=["users"])

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@router.get("/", response_model=List[UserResponse])
def list_users(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    users = db.query(User).offset(skip).limit(limit).all()
    return users

@router.get("/{user_id}", response_model=UserResponse)
def get_user(user_id: int, db: Session = Depends(get_db)):
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user

@router.post("/", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
def create_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = User(**user.model_dump())
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

@router.put("/{user_id}", response_model=UserResponse)
def update_user(user_id: int, user: UserUpdate, db: Session = Depends(get_db)):
    db_user = db.query(User).filter(User.id == user_id).first()
    if not db_user:
        raise HTTPException(status_code=404, detail="User not found")
    for key, value in user.model_dump(exclude_unset=True).items():
        setattr(db_user, key, value)
    db.commit()
    db.refresh(db_user)
    return db_user

@router.delete("/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_user(user_id: int, db: Session = Depends(get_db)):
    db_user = db.query(User).filter(User.id == user_id).first()
    if not db_user:
        raise HTTPException(status_code=404, detail="User not found")
    db.delete(db_user)
    db.commit()
```

## Best Practices

- Use separate schemas for create/update vs response (exclude password from response)
- Add pagination with `skip`/`limit` and return total count in metadata
- Use `BackgroundTasks` for heavy operations
- Implement optimistic locking with version fields
- Add OpenAPI documentation through docstrings and response models

## Related Skills
- [API Design](../api-design/) — General API patterns
- [pytest Best Practices](../testing/) — Testing FastAPI endpoints
