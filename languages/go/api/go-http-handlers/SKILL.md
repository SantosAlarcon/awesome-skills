# Go HTTP Handlers

## Description
Patterns for writing clean, idiomatic HTTP handlers in Go, including routing, middleware, error handling, and response formatting.

## When to Use
- Building HTTP APIs with net/http or Gin
- Creating middleware for authentication, logging
- Handling request validation and response formatting
- Structuring handler functions for testability

## How to Use

1. **Use http.HandlerFunc**: Keep handlers simple and focused
2. **Decode request into struct**: Use encoding/json or validator
3. **Return structured responses**: JSON with consistent format
4. **Handle errors explicitly**: Return appropriate status codes
5. **Inject dependencies**: Use middleware or struct receivers

## Examples

### Handler with JSON Response
```go
package handler

import (
    "encoding/json"
    "net/http"
)

type Response struct {
    Data    any    `json:"data,omitempty"`
    Error   string `json:"error,omitempty"`
    Message string `json:"message,omitempty"`
}

func JSON(w http.ResponseWriter, status int, data any) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(status)
    json.NewEncoder(w).Encode(data)
}

func (h *UserHandler) GetUser(w http.ResponseWriter, r *http.Request) {
    id := r.PathValue("id")
    
    user, err := h.service.GetUser(r.Context(), id)
    if err != nil {
        if errors.Is(err, ErrNotFound) {
            JSON(w, http.StatusNotFound, Response{Error: "user not found"})
            return
        }
        JSON(w, http.StatusInternalServerError, Response{Error: "internal error"})
        return
    }
    
    JSON(w, http.StatusOK, Response{Data: user})
}
```

### Middleware Pattern
```go
func Logger(next http.HandlerFunc) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        next.ServeHTTP(w, r)
        log.Printf("%s %s %v", r.Method, r.URL.Path, time.Since(start))
    }
}

func Auth(next http.HandlerFunc) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        token := r.Header.Get("Authorization")
        if token == "" {
            JSON(w, http.StatusUnauthorized, Response{Error: "missing token"})
            return
        }
        
        claims, err := validateToken(token)
        if err != nil {
            JSON(w, http.StatusUnauthorized, Response{Error: "invalid token"})
            return
        }
        
        ctx := context.WithValue(r.Context(), "user", claims)
        next.ServeHTTP(w, r.WithContext(ctx))
    }
}

// Usage
http.HandleFunc("/api/users", Logger(Auth(userHandler.List)))
```

### Request Validation
```go
type CreateUserRequest struct {
    Email    string `json:"email"`
    Name     string `json:"name"`
    Password string `json:"password"`
}

func (r *CreateUserRequest) Validate() []string {
    var errs []string
    if r.Email == "" || !strings.Contains(r.Email, "@") {
        errs = append(errs, "invalid email")
    }
    if len(r.Password) < 8 {
        errs = append(errs, "password must be at least 8 characters")
    }
    return errs
}

func (h *UserHandler) Create(w http.ResponseWriter, r *http.Request) {
    var req CreateUserRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        JSON(w, http.StatusBadRequest, Response{Error: "invalid request body"})
        return
    }
    
    if errs := req.Validate(); len(errs) > 0 {
        JSON(w, http.StatusBadRequest, Response{Error: strings.Join(errs, ", ")})
        return
    }
    
    // proceed with creation
}
```

## Best Practices

- Keep handlers thin — delegate business logic to services
- Use context.Context for cancellation and timeouts
- Log errors with structured logging (don't just print)
- Return consistent error format across all endpoints
- Write handler tests using httptest

## Related Skills
- [Go Testing Patterns](../testing/) — Testing HTTP handlers
