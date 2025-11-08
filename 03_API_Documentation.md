
---

# **Acrosoft API Documentation**

## **Document Control**

| Field                | Detail                                 |
| -------------------- | -------------------------------------- |
| **Product**          | Acrosoft Platform — Public/Partner API |
| **API Version**      | v1.0                                   |
| **Document Version** | 1.1 (Enterprise Revision)              |
| **Last Updated**     | November 2025                          |
| **Author**           | Hashim Zaffar                          |
| **Reviewed By**      | —                                      |
| **Approved By**      | —                                      |
| **Status**           | Draft / Reviewed / Approved            |
| **Confidentiality**  | Public / Partner / Internal            |

---

## **1. Overview**

The **Acrosoft API** lets clients and partners integrate with the Acrosoft platform to create and manage projects, tasks, notifications, and analytics.
It uses **REST over HTTPS** with **JSON** request and response bodies. Authentication uses **OAuth 2.0** and **JWT**.

### 1.1 Base URLs

| Environment | Base URL                             |
| ----------- | ------------------------------------ |
| Production  | `https://api.acrosoft.io/v1`         |
| Staging     | `https://staging-api.acrosoft.io/v1` |
| Local Dev   | `http://localhost:4000/v1`           |

### 1.2 Key Features

* RESTful endpoints with JSON
* OAuth2 or JWT authentication with RBAC
* Pagination, sorting, filtering
* Idempotency for POST/PUT where applicable
* Webhooks with signed events
* Standardized error model (RFC 7807)
* Rate limiting headers

### 1.3 Versioning Policy

* URI-versioned: `/v1`
* Backward-compatible changes may be added without a new major version
* Breaking changes require a new major version `/v2`

---

## **2. Authentication and Authorization**

### 2.1 JWT Authentication

All endpoints require authentication except `/auth/login` and `/auth/register`.

**Login Request**

```http
POST /auth/login
Content-Type: application/json

{
  "email": "client@example.com",
  "password": "strongpassword123"
}
```

**Login Response**

```json
{
  "token": "<jwt_token>",
  "refreshToken": "<refresh_token>",
  "expiresIn": 3600,
  "tokenType": "Bearer"
}
```

**Use the token**

```
Authorization: Bearer <jwt_token>
```

**Refresh**

```http
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "<refresh_token>"
}
```

### 2.2 OAuth 2.0 (Partners)

* **Grant Types**: Client Credentials, Authorization Code
* **Scopes**:

  * `projects:read`, `projects:write`
  * `tasks:read`, `tasks:write`
  * `users:read`, `users:write`
  * `analytics:read`
  * `notifications:read`, `notifications:write`

**Token Request (Client Credentials)**

```http
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=<id>&client_secret=<secret>&scope=projects:read
```

### 2.3 RBAC Roles

| Role        | Typical Use                 | Allowed Scopes                                          |
| ----------- | --------------------------- | ------------------------------------------------------- |
| **admin**   | Platform administration     | All scopes                                              |
| **manager** | Project and team management | `projects:*`, `tasks:*`, `users:read`, `analytics:read` |
| **client**  | View and collaborate        | `projects:read`, `tasks:read`, `notifications:*`        |

---

## **3. Conventions**

### 3.1 Headers

| Header            | Value                                           |
| ----------------- | ----------------------------------------------- |
| `Content-Type`    | `application/json`                              |
| `Accept`          | `application/json`                              |
| `Authorization`   | `Bearer <token>`                                |
| `Idempotency-Key` | UUID for safe retries on POST/PUT (recommended) |

### 3.2 Pagination

* Query: `?page=1&limit=20`
* Response envelope:

```json
{
  "page": 1,
  "limit": 20,
  "total": 42,
  "items": [ ... ]
}
```

### 3.3 Sorting and Filtering

* Sorting: `?sort=createdAt:desc,name:asc`
* Filtering examples:

  * `/projects?status=active`
  * `/tasks?assignee=u1001&dueBefore=2025-12-01`

### 3.4 Date and Time

* **ISO 8601** with `Z` suffix, for example `2025-11-15T09:00:00Z`.

### 3.5 Idempotency

* For **POST** and **PUT**, supply `Idempotency-Key`. Retries with the same key will not duplicate resources.

---

## **4. API Endpoints**

> The examples below show representative requests and responses. Full schemas are in **Section 9**.

### 4.1 Auth

#### POST `/auth/register`

Registers a new client or team member.

**Request**

```json
{
  "name": "John Doe",
  "email": "john@acrosoft.io",
  "password": "securePass123!",
  "role": "client"
}
```

**Response**

```json
{
  "message": "User registered successfully",
  "userId": "u1234567"
}
```

---

### 4.2 Users

#### GET `/users/me`

Returns the authenticated user profile.

**Response**

```json
{
  "id": "u123",
  "name": "John Doe",
  "email": "john@acrosoft.io",
  "role": "client",
  "createdAt": "2025-10-15T09:32:00Z"
}
```

#### PATCH `/users/{id}`

Update user details. Admins can update any user. Users can update their own basic fields.

**Request**

```json
{
  "name": "John D.",
  "role": "manager"
}
```

**Responses**

* `200 OK` updated user
* `403 FORBIDDEN` if role change without permission

---

### 4.3 Projects

#### GET `/projects`

Fetch projects for the authenticated organization.

**Query**

| Param    | Type    | Description                       |
| -------- | ------- | --------------------------------- |
| `status` | string  | `active`, `completed`, `archived` |
| `page`   | integer | Page number                       |
| `limit`  | integer | Items per page                    |

**Response**

```json
{
  "page": 1,
  "limit": 20,
  "total": 2,
  "items": [
    {
      "id": "p101",
      "name": "Website Revamp",
      "status": "active",
      "manager": "Jane Doe",
      "deadline": "2025-12-20T00:00:00Z"
    }
  ]
}
```

#### POST `/projects`

Create a project.

**Request**

```json
{
  "name": "Acrosoft Client Portal",
  "description": "Web dashboard for clients",
  "startDate": "2025-11-15",
  "deadline": "2026-01-15"
}
```

**Response**

```json
{
  "id": "p109",
  "message": "Project created successfully"
}
```

#### PATCH `/projects/{id}`

Partial update.

**Request**

```json
{
  "status": "completed"
}
```

#### DELETE `/projects/{id}`

Archive or delete a project. Admin only.

---

### 4.4 Tasks

#### GET `/projects/{id}/tasks`

List tasks for a project.

**Response**

```json
{
  "page": 1,
  "limit": 50,
  "total": 1,
  "items": [
    {
      "id": "t1",
      "title": "Design wireframes",
      "status": "in-progress",
      "assignee": "u1001",
      "dueDate": "2025-11-10"
    }
  ]
}
```

#### POST `/projects/{id}/tasks`

Create a task.

**Request**

```json
{
  "title": "Implement dashboard API",
  "assignee": "u1003",
  "dueDate": "2025-11-30"
}
```

**Response**

```json
{
  "id": "t103",
  "message": "Task created successfully"
}
```

#### PATCH `/tasks/{taskId}`

Update task attributes such as status and assignee.

---

### 4.5 Notifications

#### GET `/notifications`

Fetch notifications for the authenticated user.

**Response**

```json
{
  "items": [
    {
      "id": "n12",
      "message": "Project 'Website Revamp' was updated",
      "read": false,
      "createdAt": "2025-11-06T09:00:00Z"
    }
  ]
}
```

#### PATCH `/notifications/{id}/read`

Marks a notification as read.

---

### 4.6 Analytics

#### GET `/analytics/projects/{id}`

Project health metrics.

**Response**

```json
{
  "projectId": "p101",
  "velocity": 85,
  "completionRate": 92,
  "bugsResolved": 40,
  "openTasks": 6,
  "sampleWindowDays": 30
}
```

---

## **5. Webhooks**

### 5.1 Events

* `project.created`, `project.updated`, `project.archived`
* `task.created`, `task.updated`, `task.assigned`
* `comment.created`, `file.uploaded` (planned)

### 5.2 Register a Webhook

```http
POST /webhooks
Content-Type: application/json

{
  "url": "https://clientapp.io/webhook",
  "events": ["project.updated", "task.assigned"],
  "secret": "optional-shared-secret"
}
```

### 5.3 Delivery and Security

* **Signing:** `X-Acrosoft-Signature: sha256=<HMAC>` where HMAC is computed over the raw body using your webhook secret
* **Idempotency:** `X-Acrosoft-Event-ID`
* **Retries:** Exponential backoff up to 6 attempts on 5xx
* **Timeout:** 5 seconds per delivery

**Event Payload**

```json
{
  "id": "evt_01HF0T8R",
  "type": "project.updated",
  "created": "2025-11-06T10:15:00Z",
  "data": {
    "projectId": "p109",
    "status": "in-progress"
  }
}
```

---

## **6. Errors and Problem Details**

### 6.1 Standard Error Model (RFC 7807)

```json
{
  "type": "https://docs.acrosoft.io/errors/resource-not-found",
  "title": "Resource not found",
  "status": 404,
  "detail": "The requested project does not exist.",
  "instance": "/projects/p999"
}
```

### 6.2 Common Error Codes

| Code               | HTTP | Meaning                   |
| ------------------ | ---- | ------------------------- |
| `UNAUTHORIZED`     | 401  | Token missing or expired  |
| `FORBIDDEN`        | 403  | Insufficient privileges   |
| `NOT_FOUND`        | 404  | Resource does not exist   |
| `VALIDATION_ERROR` | 422  | Invalid request payload   |
| `CONFLICT`         | 409  | Version or state conflict |
| `RATE_LIMITED`     | 429  | Too many requests         |
| `SERVER_ERROR`     | 500  | Unexpected error          |

---

## **7. Rate Limiting**

* Default: **120 requests per minute per user**
* Headers:

```
X-RateLimit-Limit: 120
X-RateLimit-Remaining: 100
X-RateLimit-Reset: 1709976000
```

* `429 Too Many Requests` on limit breach

---

## **8. Security and Compliance**

### 8.1 Transport and Data

* HTTPS required
* TLS 1.2 or later
* AES-256 at rest (platform policy)

### 8.2 Input Validation and Protections

* Strict JSON schema validation
* Size limits for payloads and arrays
* Anti-automation: reCAPTCHA on auth endpoints (configurable)
* OWASP API top 10 controls applied

### 8.3 Auditing

* Audit trails for admin actions, permission changes, and data exports

---

## **9. Schemas (Selected)**

### 9.1 Project

```json
{
  "type": "object",
  "required": ["id", "name", "status"],
  "properties": {
    "id": { "type": "string", "example": "p101" },
    "name": { "type": "string" },
    "description": { "type": "string" },
    "status": { "type": "string", "enum": ["active", "completed", "archived"] },
    "manager": { "type": "string" },
    "deadline": { "type": "string", "format": "date-time" },
    "createdAt": { "type": "string", "format": "date-time" },
    "updatedAt": { "type": "string", "format": "date-time" }
  }
}
```

### 9.2 Task

```json
{
  "type": "object",
  "required": ["id", "title", "status"],
  "properties": {
    "id": { "type": "string", "example": "t1" },
    "title": { "type": "string" },
    "status": { "type": "string", "enum": ["todo", "in-progress", "blocked", "done"] },
    "assignee": { "type": "string", "nullable": true },
    "dueDate": { "type": "string", "format": "date" },
    "projectId": { "type": "string" }
  }
}
```

### 9.3 Problem Details (Error)

```json
{
  "type": "object",
  "required": ["title", "status"],
  "properties": {
    "type": { "type": "string", "format": "uri" },
    "title": { "type": "string" },
    "status": { "type": "integer" },
    "detail": { "type": "string" },
    "instance": { "type": "string" },
    "errors": {
      "type": "array",
      "items": { "type": "object", "properties": {
        "path": { "type": "string" },
        "message": { "type": "string" }
      } }
    }
  }
}
```

---

## **10. Example cURL Snippets**

### 10.1 Create a Project

```bash
curl -X POST "https://api.acrosoft.io/v1/projects" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: $(uuidgen)" \
  -d '{
    "name":"Acrosoft Client Portal",
    "description":"Web dashboard for clients",
    "startDate":"2025-11-15",
    "deadline":"2026-01-15"
  }'
```

### 10.2 List Tasks

```bash
curl "https://api.acrosoft.io/v1/projects/p101/tasks?page=1&limit=50" \
  -H "Authorization: Bearer $TOKEN"
```

---

## **11. Changelog**

| Version | Date     | Changes                                            |
| ------- | -------- | -------------------------------------------------- |
| v1.0    | Nov 2025 | Initial public release                             |
| v1.1    | Planned  | Add file upload and comments APIs; expand webhooks |

---

## **12. OpenAPI Seed (Publish in your repo)**

> Minimal, valid seed. Expand with all paths in CI.

```yaml
openapi: 3.1.0
info:
  title: Acrosoft API
  version: "1.0"
  description: Public/Partner API for Acrosoft Platform
servers:
  - url: https://api.acrosoft.io/v1
  - url: https://staging-api.acrosoft.io/v1
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
    oauth2:
      type: oauth2
      flows:
        clientCredentials:
          tokenUrl: https://api.acrosoft.io/v1/oauth/token
          scopes:
            projects:read: Read projects
            projects:write: Write projects
security:
  - bearerAuth: []
paths:
  /projects:
    get:
      summary: List projects
      parameters:
        - in: query
          name: status
          schema: { type: string, enum: [active, completed, archived] }
        - in: query
          name: page
          schema: { type: integer, minimum: 1, default: 1 }
        - in: query
          name: limit
          schema: { type: integer, minimum: 1, maximum: 100, default: 20 }
      responses:
        "200":
          description: OK
    post:
      summary: Create project
      headers:
        Idempotency-Key:
          schema: { type: string }
          required: false
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/ProjectCreate"
      responses:
        "201":
          description: Created
  /auth/login:
    post:
      summary: Login and get JWT
      responses:
        "200":
          description: OK
components:
  schemas:
    Project:
      type: object
      properties:
        id: { type: string }
        name: { type: string }
        status: { type: string, enum: [active, completed, archived] }
    ProjectCreate:
      type: object
      required: [name]
      properties:
        name: { type: string }
        description: { type: string }
        startDate: { type: string, format: date }
        deadline: { type: string, format: date }
```

---

## **13. Related Documents**

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

## **14. Standards and Best Practices**

* **OpenAPI 3.1** for API description
* **RFC 7807** for error responses
* **OAuth 2.0**, **JWT (RFC 7519)**
* **OWASP API Security Top 10**
* **ISO/IEC 27001** controls for security management

---

**End of Document**
