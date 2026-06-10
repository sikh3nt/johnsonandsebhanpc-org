# API Documentation

## Base URL

```
http://localhost:5000/api
```

## Authentication

All authenticated endpoints require a Bearer token in the Authorization header:

```http
Authorization: Bearer <your_jwt_token>
```

## Response Format

All responses follow this format:

```json
{
  "success": true,
  "data": { /* response data */ },
  "error": null,
  "meta": {
    "timestamp": "2026-06-10T12:00:00Z",
    "version": "1.0"
  }
}
```

## Error Handling

Error responses include an error object:

```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request",
    "details": {
      "field": "error details"
    }
  }
}
```

### Error Codes

| Code | Status | Description |
|------|--------|-------------|
| VALIDATION_ERROR | 400 | Request validation failed |
| UNAUTHORIZED | 401 | Missing or invalid token |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |
| CONFLICT | 409 | Resource already exists |
| RATE_LIMIT | 429 | Too many requests |
| SERVER_ERROR | 500 | Internal server error |

## Endpoints

### Authentication

#### Register

```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "user-123",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

#### Login

```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

**Response:** Same as register

#### Refresh Token

```http
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

#### Logout

```http
POST /auth/logout
Authorization: Bearer <token>
```

---

### NPC Management

#### Get All NPCs

```http
GET /npcs?page=1&limit=10&search=&sort=-createdAt
Authorization: Bearer <token>
```

**Query Parameters:**
- `page` (number): Page number (default: 1)
- `limit` (number): Results per page (default: 10)
- `search` (string): Search by name or description
- `sort` (string): Sort field (prefix with `-` for descending)

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "npc-123",
      "name": "Elara",
      "description": "A skilled mage",
      "type": "mage",
      "level": 15,
      "status": "active",
      "createdAt": "2026-06-10T12:00:00Z",
      "updatedAt": "2026-06-10T12:00:00Z"
    }
  ],
  "meta": {
    "total": 42,
    "page": 1,
    "limit": 10,
    "pages": 5
  }
}
```

#### Get NPC by ID

```http
GET /npcs/:id
Authorization: Bearer <token>
```

**Response:** Single NPC object (same as above)

#### Create NPC

```http
POST /npcs
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Elara",
  "description": "A skilled mage",
  "type": "mage",
  "level": 15,
  "stats": {
    "strength": 8,
    "dexterity": 14,
    "constitution": 10,
    "intelligence": 16,
    "wisdom": 12,
    "charisma": 13
  },
  "attributes": {
    "color": "blue",
    "alignment": "neutral_good"
  }
}
```

**Response:** Created NPC object

#### Update NPC

```http
PUT /npcs/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Elara the Wise",
  "level": 16,
  "status": "inactive"
}
```

**Response:** Updated NPC object

#### Delete NPC

```http
DELETE /npcs/:id
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": { "id": "npc-123" }
}
```

---

### Organization

#### Get Dashboard Data

```http
GET /org/dashboard
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "totalNPCs": 42,
    "activeNPCs": 38,
    "totalMembers": 12,
    "recentActivity": [],
    "statistics": {
      "npcsByType": {},
      "npcsByLevel": {}
    }
  }
}
```

#### Get Analytics

```http
GET /org/analytics?period=month&metric=npc_creation
Authorization: Bearer <token>
```

**Query Parameters:**
- `period`: day, week, month, year
- `metric`: npc_creation, user_activity, etc.

#### Get Members

```http
GET /org/members?page=1&limit=20
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "user-123",
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "role": "admin",
      "joinedAt": "2026-06-10T12:00:00Z"
    }
  ]
}
```

#### Add Member

```http
POST /org/members
Authorization: Bearer <token>
Content-Type: application/json

{
  "email": "newuser@example.com",
  "role": "member"
}
```

---

## Rate Limiting

- **Limit**: 1000 requests per hour per IP
- **Headers**:
  - `X-RateLimit-Limit`: Total requests allowed
  - `X-RateLimit-Remaining`: Requests remaining
  - `X-RateLimit-Reset`: Unix timestamp when limit resets

---

## Pagination

Paginated endpoints return data with:

```json
{
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "pages": 10
  }
}
```

**Example Usage:**
```bash
# Get page 2 with 20 items per page
curl "http://localhost:5000/api/npcs?page=2&limit=20"
```

---

## Filtering & Searching

### Operators

- `eq`: Equal
- `ne`: Not equal
- `gt`: Greater than
- `gte`: Greater than or equal
- `lt`: Less than
- `lte`: Less than or equal
- `in`: In array
- `nin`: Not in array

### Examples

```bash
# Filter by level greater than 10
GET /npcs?level[gt]=10

# Filter by type in array
GET /npcs?type[in]=mage,wizard

# Search by name
GET /npcs?search=Elara
```

---

## Sorting

Use the `sort` parameter with field names:

```bash
# Sort by name ascending
GET /npcs?sort=name

# Sort by createdAt descending
GET /npcs?sort=-createdAt

# Multiple sorts
GET /npcs?sort=-level,name
```

---

## Webhook Events

Subscribe to webhook events for real-time updates:

```http
POST /webhooks
Authorization: Bearer <token>
Content-Type: application/json

{
  "event": "npc.created",
  "url": "https://your-domain.com/webhook",
  "active": true
}
```

### Supported Events

- `npc.created`
- `npc.updated`
- `npc.deleted`
- `member.joined`
- `member.removed`

---

## Examples

### Create and Retrieve NPC

```bash
# 1. Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password"}'

# Response: { "data": { "token": "eyJ..." } }

# 2. Create NPC
curl -X POST http://localhost:5000/api/npcs \
  -H "Authorization: Bearer eyJ..." \
  -H "Content-Type: application/json" \
  -d '{
    "name":"Elara",
    "type":"mage",
    "level":15
  }'

# 3. Get NPC
curl http://localhost:5000/api/npcs/npc-123 \
  -H "Authorization: Bearer eyJ..."
```

---

## Versioning

Current API version: **v1.0**

The API is versioned via header or URL:

```bash
# Via header
curl -H "API-Version: 1.0" http://localhost:5000/api/...

# Via URL
curl http://localhost:5000/api/v1/...
```

---

## Support

For API issues or questions:
- [Open an issue](https://github.com/sikh3nt/johnsonandsebhanpc-org/issues)
- [Check Discussions](https://github.com/sikh3nt/johnsonandsebhanpc-org/discussions)

---

**Last Updated**: June 10, 2026
