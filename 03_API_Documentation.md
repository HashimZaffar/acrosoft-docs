
---
# Acrosoft API Documentation

**Product:** Acrosoft Platform – Public/Partner API  
**Version:** v1.0  
**Last Updated:** November 2025  
**Author:** Hashim Zaffar

---

## 1. Overview
The **Acrosoft API** allows developers and clients to interact with the Acrosoft platform programmatically.  
It supports operations such as creating projects, managing tasks, tracking analytics, and integrating with external systems like GitHub and Slack.

### Base URLs
| Environment | URL |
|--------------|------|
| Production | `https://api.acrosoft.io/v1` |
| Staging | `https://staging-api.acrosoft.io/v1` |
| Local Dev | `http://localhost:4000/v1` |

### Key Features
- RESTful endpoints with JSON payloads  
- OAuth2 and JWT-based authentication  
- Role-based access control (RBAC)  
- Pagination, sorting, and filtering  
- Webhooks for real-time notifications  

---

## 2. Authentication

### JWT Authentication
All endpoints require authentication except `/auth/login` and `/auth/register`.

#### Login Request
```http
POST /auth/login
Content-Type: application/json

{
  "email": "client@example.com",
  "password": "strongpassword123"
}


#### Response

```json
{
  "token": "<jwt_token>",
  "refreshToken": "<refresh_token>",
  "expiresIn": 3600
}
```

Use this token in the `Authorization` header for all subsequent requests:

```
Authorization: Bearer <jwt_token>
```

### Refresh Token

```http
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "<refresh_token>"
}
```

---

## 3. Common Conventions

### Headers

| Header          | Value              |
| --------------- | ------------------ |
| `Content-Type`  | `application/json` |
| `Authorization` | `Bearer <token>`   |

### Pagination

Use query params:
`?page=1&limit=20`

### Sorting and Filtering

Examples:

```
/projects?sort=createdAt:desc&status=active
```

---

## 4. API Endpoints

### 4.1 Auth Service

#### POST `/auth/register`

Register a new client or team member.

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

Retrieve authenticated user profile.

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

#### PATCH `/users/:id`

Update user details (Admin or Self).

**Body Example**

```json
{
  "name": "John D.",
  "role": "manager"
}
```

---

### 4.3 Projects

#### GET `/projects`

Fetch all projects for the authenticated organization.

**Query Parameters**

| Param    | Type    | Description                                      |
| -------- | ------- | ------------------------------------------------ |
| `status` | string  | Filter by project status (`active`, `completed`) |
| `page`   | integer | Page number                                      |
| `limit`  | integer | Results per page                                 |

**Response**

```json
{
  "page": 1,
  "total": 2,
  "projects": [
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

Create a new project.

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

#### PATCH `/projects/:id`

Update project status or metadata.

```json
{
  "status": "completed"
}
```

#### DELETE `/projects/:id`

Archive or delete a project (Admin only).

---

### 4.4 Tasks

#### GET `/projects/:id/tasks`

Retrieve all tasks for a project.

**Response**

```json
[
  {
    "id": "t1",
    "title": "Design wireframes",
    "status": "in-progress",
    "assignee": "u1001",
    "dueDate": "2025-11-10"
  }
]
```

#### POST `/projects/:id/tasks`

Create a new task.

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

---

### 4.5 Notifications

#### GET `/notifications`

Retrieve all notifications for the authenticated user.

**Response**

```json
[
  {
    "id": "n12",
    "message": "Project ‘Website Revamp’ was updated",
    "read": false,
    "createdAt": "2025-11-06T09:00:00Z"
  }
]
```

#### PATCH `/notifications/:id/read`

Mark a notification as read.

---

### 4.6 Analytics

#### GET `/analytics/projects/:id`

Fetch performance data for a project.

**Response**

```json
{
  "projectId": "p101",
  "velocity": 85,
  "completionRate": 92,
  "bugsResolved": 40,
  "openTasks": 6
}
```

---

## 5. Webhooks

You can register webhooks to get real-time updates for events like:

* Project created or updated
* Task assigned
* New comment or file upload

### Example Registration

```http
POST /webhooks
Content-Type: application/json

{
  "url": "https://clientapp.io/webhook",
  "events": ["project.updated", "task.assigned"]
}
```

**Event Payload Example**

```json
{
  "event": "project.updated",
  "timestamp": "2025-11-06T10:15:00Z",
  "data": {
    "projectId": "p109",
    "status": "in-progress"
  }
}
```

---

## 6. Error Handling

### Standard Response Format

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "The requested project does not exist."
  }
}
```

### Common Error Codes

| Code               | HTTP | Description              |
| ------------------ | ---- | ------------------------ |
| `UNAUTHORIZED`     | 401  | Invalid or expired token |
| `FORBIDDEN`        | 403  | Access denied            |
| `NOT_FOUND`        | 404  | Resource missing         |
| `VALIDATION_ERROR` | 422  | Invalid request payload  |
| `SERVER_ERROR`     | 500  | Unexpected server issue  |

---

## 7. Rate Limiting

* Standard limit: **120 requests/minute per user**
* Exceeding this limit returns `429 Too Many Requests`

Response Headers:

```
X-RateLimit-Limit: 120
X-RateLimit-Remaining: 100
X-RateLimit-Reset: 1709976000
```

---

## 8. Changelog

| Version | Date     | Changes                         |
| ------- | -------- | ------------------------------- |
| v1.0    | Nov 2025 | Initial public release          |
| v1.1    | Planned  | Add file upload + comments APIs |

---

## 11. Related Documents

* [01_Product_Requirements_Document.md](./01_Product_Requirements_Document.md)
* [02_System_Architecture.md](./02_System_Architecture.md)
* [03_API_Documentation.md](./03_API_Documentation.md)
* [04_User_Guide.md](./04_User_Guide.md)
* [05_Installation_Configuration_Guide.md](./05_Installation_Configuration_Guide.md)
* [06_Release_Notes.md](./06_Release_Notes.md)
* [08_Style_Guide.md](./08_Style_Guide.md)

---

**End of Document**