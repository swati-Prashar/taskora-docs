# 🔍 API Docs

# 🔍 API Docs

```
# Taskora API Documentation
```

<pre class="language-markdown"><code class="lang-markdown"><strong>## Base URL:
</strong>`https://api.taskora.dev/v1`

## Authentication
Send Bearer Token in header:
`Authorization: Bearer &#x3C;your-token>`

## Endpoints

### GET /tasks
Returns all tasks.

### POST /tasks
Creates a new task. Body:
```json
{
  "title": "Write Docs",
  "status": "To Do"
}
</code></pre>
