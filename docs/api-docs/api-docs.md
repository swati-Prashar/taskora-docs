# 🔍 API Docs

```markdown
# Taskora API Documentation

> Base URL: `https://api.taskora.dev/v1`

## Authentication
All endpoints require a Bearer token in the `Authorization` header.





```



***

Authorization: Bearer

````yaml

---

## Overview
This API manages tasks. The primary resource is `Task`. Standard RESTful endpoints are provided for create, read, update and delete operations. Responses use JSON.

---

## Task object
A `Task` has the following fields:

- `id` (string) — Unique task identifier (UUID or numeric string).
- `title` (string) — Short title of the task.
- `description` (string, optional) — Longer text describing the task.
- `status` (string) — One of: `todo`, `in_progress`, `done`, `archived`. (Example values: `To Do`, `In Progress`, `Done`)
- `priority` (string, optional) — One of: `low`, `medium`, `high`.
- `assignee` (object, optional) — Minimal assignee object: `{ "id": "u123", "name": "Alice" }`.
- `dueDate` (string, optional) — ISO 8601 date/time (e.g. `2025-11-10T14:30:00Z`).
- `createdAt` (string) — ISO 8601 timestamp when task was created.
- `updatedAt` (string) — ISO 8601 timestamp when task was last updated.
- `metadata` (object, optional) — Freeform key/value object for custom data.

---

## Common error format
Errors return JSON with the structure:

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task with id '123' not found"
  }
}

````

Status codes used:

* `200 OK` — success
* `201 Created` — resource created
* `204 No Content` — success with no response body (delete)
* `400 Bad Request` — validation or request syntax error
* `401 Unauthorized` — missing/invalid token
* `403 Forbidden` — insufficient permissions
* `404 Not Found` — resource not found
* `409 Conflict` — conflict (e.g., duplicate)
* `500 Internal Server Error` — server error

### Endpoints

#### GET /tasks

**Description:** List tasks with optional filtering and pagination.

**Query parameters:**

* `page` (int, optional) — page number (default `1`)
* `limit` (int, optional) — items per page (default `20`)
* `status` (string, optional) — filter by status
* `assigneeId` (string, optional) — filter by assignee
* `search` (string, optional) — text search on title/description
* `sort` (string, optional) — e.g. `createdAt:desc` or `dueDate:asc`

&#x20;

Example Request:

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.taskora.dev/v1/tasks?status=todo&page=1&limit=20"

```

Example Response (200):

```json
{
  "data": [
    {
      "id": "1",
      "title": "Write Docs",
      "description": "Write API documentation for Taskora",
      "status": "todo",
      "priority": "high",
      "assignee": { "id": "u1", "name": "Swati" },
      "dueDate": "2025-11-10T10:00:00Z",
      "createdAt": "2025-10-29T06:30:00Z",
      "updatedAt": "2025-10-29T06:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 1
  }
}

```

#### GET /tasks/{id}

**Description:** Retrieve a single task by ID.

**Path parameters:**

* `id` (string) — task id

**Example Request:**

```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.taskora.dev/v1/tasks/1"

```

Example Response (200):

```json
{
  "id": "1",
  "title": "Write Docs",
  "description": "Write API documentation for Taskora",
  "status": "todo",
  "priority": "high",
  "assignee": { "id": "u1", "name": "Swati" },
  "dueDate": "2025-11-10T10:00:00Z",
  "createdAt": "2025-10-29T06:30:00Z",
  "updatedAt": "2025-10-29T06:30:00Z"
}

```

**Errors**

* `404` if task not found.

***

#### POST /tasks

**Description:** Create a new task.

**Request body (JSON):**

```json
{
  "title": "Write Docs",
  "description": "Write API documentation for Taskora",
  "status": "todo",
  "priority": "high",
  "assigneeId": "u1",
  "dueDate": "2025-11-10T10:00:00Z",
  "metadata": { "project": "website" }
}

```

Example Response (201 Created):

```json
{
  "id": "3",
  "title": "Write Docs",
  "description": "Write API documentation for Taskora",
  "status": "todo",
  "priority": "high",
  "assignee": { "id": "u1", "name": "Swati" },
  "dueDate": "2025-11-10T10:00:00Z",
  "createdAt": "2025-10-29T07:00:00Z",
  "updatedAt": "2025-10-29T07:00:00Z"
}

```

**Errors**

* `400` when required fields missing or invalid.

***

#### PUT /tasks/{id}

**Description:** Update an existing task (full or partial updates allowed depending on implementation).

**Path parameters:**

* `id` (string) — task id

**Request body (JSON):** (example shows fields allowed)

```json
{
  "title": "Write Updated Docs",
  "description": "Expanded docs",
  "status": "in_progress",
  "priority": "medium",
  "assigneeId": "u2",
  "dueDate": "2025-11-12T12:00:00Z",
  "metadata": { "sprint": "5" }
}

```

Example Response (200):

```json
{
  "id": "1",
  "title": "Write Updated Docs",
  "status": "in_progress",
  "updatedAt": "2025-10-29T08:00:00Z"
}

```

**Errors**

* `400` validation errors
* `404` not found

***

#### DELETE /tasks/{id}

**Description:** Delete a task.

**Path parameters:**

* `id` (string)

**Example Request:**

```bash
curl -X DELETE -H "Authorization: Bearer <token>" \
  "https://api.taskora.dev/v1/tasks/1"

```

**Response:**

* `204 No Content` on success.
* `404` if not found.

***

### Optional / Additional endpoints (suggestions)

You may also provide endpoints for:

* `GET /users` — list users
* `GET /projects` — list projects
* `POST /auth/login` — obtain token (if you support username/password)
* `PATCH /tasks/{id}/status` — quick status updates
* Webhooks for task events: `POST /webhooks` / `DELETE /webhooks/{id}`

### Versioning & Changelog

Add a small changelog section in your docs and a `Last updated` timestamp. Example:

**Version:** `v1` — stable\
**Last updated:** `2025-10-29`

***

### Security & Rate Limits

Document any rate limits, e.g., `"X-RateLimit-Limit": 1000` and `"X-RateLimit-Remaining"`. If you support scopes/roles, list them.

***

### Examples & SDKs

Add short code examples (curl, JavaScript fetch, Python requests) and link to SDKs if available.

***

### FAQ / Troubleshooting

Short notes: token expired → 401, how to refresh token, request id logs for support.

````yaml

---

# 2) OpenAPI 3.0.3 spec (YAML)

Copy this text into a file `taskora-openapi.yaml` or paste into Swagger Editor / Redoc / GitBook plugin.

```yaml
openapi: 3.0.3
info:
  title: Taskora API
  version: "1.0.0"
  description: |
    Taskora REST API for managing tasks.
    Base URL: https://api.taskora.dev/v1
servers:
  - url: https://api.taskora.dev/v1
security:
  - bearerAuth: []
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
          example: "1"
        title:
          type: string
          example: "Write Docs"
        description:
          type: string
          example: "Write API documentation for Taskora"
        status:
          type: string
          enum: [todo, in_progress, done, archived]
          example: "todo"
        priority:
          type: string
          enum: [low, medium, high]
          example: "high"
        assignee:
          type: object
          properties:
            id:
              type: string
              example: "u1"
            name:
              type: string
              example: "Swati"
        assigneeId:
          type: string
          example: "u1"
        dueDate:
          type: string
          format: date-time
          example: "2025-11-10T10:00:00Z"
        metadata:
          type: object
          additionalProperties: true
          example: { "project": "website" }
        createdAt:
          type: string
          format: date-time
          example: "2025-10-29T06:30:00Z"
        updatedAt:
          type: string
          format: date-time
          example: "2025-10-29T07:00:00Z"
      required: [id, title, status, createdAt, updatedAt]

    TaskList:
      type: object
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/Task'
        pagination:
          type: object
          properties:
            page:
              type: integer
              example: 1
            limit:
              type: integer
              example: 20
            total:
              type: integer
              example: 1

    Error:
      type: object
      properties:
        error:
          type: object
          properties:
            code:
              type: string
              example: "TASK_NOT_FOUND"
            message:
              type: string
              example: "Task with id '1' not found"

paths:
  /tasks:
    get:
      summary: List tasks
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
        - name: status
          in: query
          schema:
            type: string
        - name: assigneeId
          in: query
          schema:
            type: string
        - name: search
          in: query
          schema:
            type: string
        - name: sort
          in: query
          schema:
            type: string
      security:
        - bearerAuth: []
      responses:
        '200':
          description: A paginated list of tasks
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TaskList'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    post:
      summary: Create a task
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                title:
                  type: string
                description:
                  type: string
                status:
                  type: string
                  enum: [todo, in_progress, done, archived]
                priority:
                  type: string
                  enum: [low, medium, high]
                assigneeId:
                  type: string
                dueDate:
                  type: string
                  format: date-time
                metadata:
                  type: object
                  additionalProperties: true
              required: [title, status]
      responses:
        '201':
          description: Task created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
        '400':
          description: Invalid input
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /tasks/{id}:
    get:
      summary: Get a single task
      security:
        - bearerAuth: []
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Task found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '404':
          description: Not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    put:
      summary: Update a task
      security:
        - bearerAuth: []
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                title:
                  type: string
                description:
                  type: string
                status:
                  type: string
                  enum: [todo, in_progress, done, archived]
                priority:
                  type: string
                  enum: [low, medium, high]
                assigneeId:
                  type: string
                dueDate:
                  type: string
                  format: date-time
                metadata:
                  type: object
                  additionalProperties: true
      responses:
        '200':
          description: Task updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '404':
          description: Not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    delete:
      summary: Delete a task
      security:
        - bearerAuth: []
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Deleted (no content)
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '404':
          description: Not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

````
