# 🔍 API Docs

## Taskora API Documentation

### Base URL:

`https://api.taskora.dev/v1`

### Authentication

Send Bearer Token in header:`Authorization: Bearer <your-token>`

### Endpoints

#### GET /tasks

Returns all tasks.

#### POST /tasks

Creates a new task. Body:

```json
{
  "title": "Write Docs",
  "status": "To Do"
}
```
