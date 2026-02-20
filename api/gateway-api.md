# Gateway API

REST API specification for the BlackRoad Gateway.

**Base URL:** `http://localhost:8787` (development) or configured deployment URL.

All requests require `Authorization: Bearer <agent-token>` unless noted.

## Endpoints

### Health Check

```
GET /v1/health
```

No authentication required.

**Response:**

```json
{
  "status": "healthy",
  "version": "0.1.0",
  "uptime": 3600,
  "providers": {
    "anthropic": { "available": true, "latency": 120 },
    "openai": { "available": true, "latency": 95 },
    "ollama": { "available": true, "latency": 15 },
    "gemini": { "available": false, "latency": null }
  }
}
```

### Chat Completions

```
POST /v1/chat/completions
```

Send a chat request through the gateway. The gateway selects the provider based on agent permissions and availability.

**Request:**

```json
{
  "model": "claude-sonnet-4-20250514",
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "Explain the tokenless gateway pattern." }
  ],
  "temperature": 0.7,
  "max_tokens": 1024
}
```

**Response:**

```json
{
  "id": "resp_abc123",
  "content": "The tokenless gateway pattern...",
  "model": "claude-sonnet-4-20250514",
  "provider": "anthropic",
  "usage": {
    "prompt_tokens": 42,
    "completion_tokens": 256,
    "total_tokens": 298
  }
}
```

### Invoke Agent

```
POST /v1/invoke
```

Invoke a named agent with a task. The gateway routes the request through the agent's orchestration pipeline.

**Request:**

```json
{
  "agent": "octavia",
  "task": "Design a microservices architecture for user authentication",
  "intent": "architect",
  "context": {
    "project": "blackroad-core",
    "constraints": ["must support OAuth2", "must be stateless"]
  }
}
```

**Response:**

```json
{
  "id": "inv_xyz789",
  "agent": "octavia",
  "result": "Here is the proposed architecture...",
  "model": "claude-sonnet-4-20250514",
  "provider": "anthropic",
  "usage": {
    "prompt_tokens": 128,
    "completion_tokens": 512,
    "total_tokens": 640
  }
}
```

### List Agents

```
GET /v1/agents
```

**Response:**

```json
{
  "agents": [
    {
      "name": "octavia",
      "title": "The Architect",
      "role": "Systems design, strategy, architecture",
      "status": "available",
      "providers": ["anthropic", "openai"],
      "capabilities": ["architecture", "design", "review", "strategy"]
    }
  ]
}
```

### Get Agent

```
GET /v1/agents/:name
```

**Response:**

```json
{
  "name": "octavia",
  "title": "The Architect",
  "role": "Systems design, strategy, architecture",
  "description": "Octavia specializes in systems architecture...",
  "status": "available",
  "color": "#9C27B0",
  "providers": ["anthropic", "openai"],
  "maxTokens": 8192,
  "capabilities": ["architecture", "design", "review", "strategy"],
  "fallbackChain": ["anthropic", "openai", "ollama"]
}
```

### Metrics

```
GET /v1/metrics
```

**Response:**

```json
{
  "requests": { "total": 1024, "success": 1010, "error": 14 },
  "latency": { "p50": 85, "p95": 250, "p99": 500 },
  "providers": {
    "anthropic": { "requests": 600, "errors": 5 },
    "openai": { "requests": 300, "errors": 8 },
    "ollama": { "requests": 124, "errors": 1 }
  }
}
```

## Error Responses

All errors follow a consistent format:

```json
{
  "error": {
    "code": "GATEWAY_AUTH_FAILED",
    "message": "Invalid or missing authorization token",
    "status": 401
  }
}
```

See [error codes](error-codes.md) for the complete reference.

## Rate Limiting

Responses include rate limit headers:

```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1706000000
```

When rate limited, the gateway returns `429 Too Many Requests`.
