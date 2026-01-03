# REST API Flow - Interview Prep Edition
## What is REST?

**REST (Representational State Transfer)** - Architectural style for building APIs.

**Simple answer for interview:**
"REST is a way to build web APIs using standard HTTP methods. It treats everything as resources that can be accessed via URLs."

---

## 6 REST Constraints (Important!)

Just remember these 6:

1. **Client-Server** - Frontend and backend are separate
2. **Stateless** - Each request has all needed info, server doesn't remember previous requests
3. **Cacheable** - Responses can be cached for better performance
4. **Uniform Interface** - Consistent way to interact (same patterns everywhere)
5. **Layered System** - Can have multiple layers (load balancer, cache, etc.)
6. **Code on Demand** (optional) - Server can send executable code

**Interview tip:** Just explain first 4 well, that's enough!

---

## HTTP Methods (CRUD Operations)

| Method | Purpose | Example |
|--------|---------|---------|
| **GET** | Read/Retrieve | `GET /users/123` |
| **POST** | Create | `POST /users` |
| **PUT** | Update (full) | `PUT /users/123` |
| **PATCH** | Update (partial) | `PATCH /users/123` |
| **DELETE** | Delete | `DELETE /users/123` |

**Interview question:** "What's difference between PUT and PATCH?"
**Answer:** PUT replaces entire resource, PATCH updates only specific fields.

---

## Good URI Design

**✅ Good:**
```
GET /users
GET /users/123
GET /users/123/orders
POST /users
```

**❌ Bad:**
```
GET /getUsers
GET /user?id=123
POST /createUser
```

**Key points:**
- Use **nouns**, not verbs
- Use **plural** names (/users not /user)
- Use **hierarchy** for relationships

---

## Important Status Codes (Must Know!)

**Success:**
- `200 OK` - Request successful
- `201 Created` - New resource created
- `204 No Content` - Success but no data to return

**Client Errors:**
- `400 Bad Request` - Invalid input
- `401 Unauthorized` - Not authenticated
- `403 Forbidden` - Authenticated but no permission
- `404 Not Found` - Resource doesn't exist
- `422 Unprocessable Entity` - Validation failed
- `429 Too Many Requests` - Rate limit exceeded

**Server Errors:**
- `500 Internal Server Error` - Server crashed
- `503 Service Unavailable` - Server down

---

## Stateless Explained

**Interview question:** "What does stateless mean?"

**Answer:**
"Stateless means server doesn't store session data. Each request must contain all information needed (like auth token). This makes APIs scalable because any server can handle any request."

**Example:**
```
Request 1: GET /users/123
Headers: Authorization: Bearer token123

Request 2: GET /users/123/orders  
Headers: Authorization: Bearer token123

Each request is independent!
```

---

## Authentication (Quick)

**Most common: JWT (JSON Web Token)**

**Flow:**
1. User logs in with credentials
2. Server returns JWT token
3. Client includes token in every request
4. Server validates token

**Request example:**
```
GET /users/123
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Pagination

**Why?** Can't return 10,000 records in one response!

**Two types:**

**1. Offset-based (common):**
```
GET /users?page=1&per_page=20
GET /users?limit=20&offset=0
```

**2. Cursor-based (better for real-time):**
```
GET /users?limit=20&cursor=abc123
```

**Response example:**
```json
{
  "data": [...],
  "pagination": {
    "total": 1000,
    "page": 1,
    "per_page": 20
  }
}
```

---

## Filtering & Sorting

**Filtering:**
```
GET /products?category=electronics&price_min=100
```

**Sorting:**
```
GET /users?sort=name          (ascending)
GET /users?sort=-created_at   (descending, - means DESC)
```

---

## Versioning

**Why?** Need backward compatibility when making breaking changes.

**Most common: URI versioning**
```
/v1/users
/v2/users
/v3/users
```

**Interview tip:** Just mention this approach, it's most popular.

---

## Request/Response Example

**Request:**
```
POST /api/v1/users
Authorization: Bearer token123
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Response:**
```json
HTTP/1.1 201 Created
Location: /api/v1/users/124
Content-Type: application/json

{
  "id": 124,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2024-01-03T10:00:00Z"
}
```

---

## Rate Limiting

**Why?** Prevent abuse, ensure fair usage.

**Headers:**
```
X-RateLimit-Limit: 100        (max requests)
X-RateLimit-Remaining: 95     (requests left)
X-RateLimit-Reset: 1609459200 (reset time)
```

**When exceeded:**
```
429 Too Many Requests
Retry-After: 60
```

---

## Idempotency (Common Interview Question!)

**Question:** "What is idempotency?"

**Answer:** "An operation is idempotent if calling it multiple times has the same effect as calling it once."

**Examples:**
- `GET /users/123` - ✅ Idempotent (always returns same data)
- `PUT /users/123` - ✅ Idempotent (updating same data multiple times = same result)
- `DELETE /users/123` - ✅ Idempotent (deleting multiple times = already deleted)
- `POST /users` - ❌ NOT idempotent (creates new user each time)

---

## HATEOAS (Advanced but Good to Know)

**Hypermedia As The Engine Of Application State**

**Simple explanation:** API responses include links to related resources.

**Example:**
```json
{
  "id": 123,
  "name": "John",
  "links": {
    "self": "/users/123",
    "orders": "/users/123/orders",
    "friends": "/users/123/friends"
  }
}
```

Client can discover what actions are available!

---

## Common Interview Questions & Answers

### 1. "What is REST?"
**Answer:** "REST is an architectural style for building APIs that uses standard HTTP methods and treats everything as resources identified by URIs."

### 2. "What makes an API RESTful?"
**Answer:** "It should follow REST constraints: stateless communication, client-server architecture, cacheable responses, and uniform interface with resource-based URIs."

### 3. "Difference between PUT and PATCH?"
**Answer:** "PUT replaces the entire resource, PATCH updates only specific fields. PUT is idempotent, PATCH may or may not be."

### 4. "What is stateless?"
**Answer:** "Server doesn't store session data. Each request contains all information needed. Makes APIs scalable."

### 5. "How do you handle authentication?"
**Answer:** "Most commonly using JWT tokens. User logs in, gets token, includes it in Authorization header for subsequent requests."

### 6. "How do you version APIs?"
**Answer:** "Most common is URI versioning like /v1/users and /v2/users. Helps maintain backward compatibility."

### 7. "What status code for successful POST?"
**Answer:** "201 Created, with Location header pointing to new resource."

### 8. "What's the difference between 401 and 403?"
**Answer:** "401 means not authenticated (need to login), 403 means authenticated but not authorized (no permission)."

### 9. "How do you handle large datasets?"
**Answer:** "Use pagination with limit/offset or cursor-based, plus filtering and sorting options."

### 10. "What is rate limiting?"
**Answer:** "Limiting number of requests a client can make in a time window. Prevents abuse and ensures fair usage."

---

## Quick Checklist for Interviews

**Must know:**
- ✅ 6 REST constraints (at least first 4)
- ✅ HTTP methods (GET, POST, PUT, PATCH, DELETE)
- ✅ Common status codes (200, 201, 400, 401, 403, 404, 500)
- ✅ Stateless concept
- ✅ Good URI design (nouns, plural, hierarchy)
- ✅ PUT vs PATCH
- ✅ Authentication (JWT basics)
- ✅ Versioning

**Good to know:**
- ✅ Pagination
- ✅ Filtering & sorting
- ✅ Rate limiting
- ✅ Idempotency
- ✅ HATEOAS

---

## One Sentence Answers (For Quick Response)

- **REST?** "Architectural style for APIs using HTTP methods and resource-based URIs"
- **Stateless?** "Server doesn't remember previous requests, each request has all info needed"
- **PUT vs PATCH?** "PUT replaces entire resource, PATCH updates specific fields"
- **401 vs 403?** "401 is not logged in, 403 is logged in but no permission"
- **Idempotent?** "Multiple identical requests have same effect as single request"
- **Rate limiting?** "Limit requests per time window to prevent abuse"
- **Versioning?** "Maintain backward compatibility by versioning API endpoints"

---
 
