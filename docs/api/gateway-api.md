# BlackRoad Gateway API Reference

Base URL: `http://127.0.0.1:8787` (local) · `https://gateway.blackroad.ai` (production)

All requests use JSON. Agents authenticate via the internal network — no API key required from agents.

---

## Health & Status

### `GET /health`

Returns gateway health and connected provider status.

```http
GET /health HTTP/1.1
```

**Response 200:**
```json
{
  "status": "ok",
  "version": "1.0.0",
  "uptime_seconds": 3600,
  "providers": {
    "ollama": "connected",
    "anthropic": "connected",
    "openai": "disconnected"
  },
  "task_count": 42,
  "memory_entries": 1337
}
```

---

## AI Completions

### `POST /v1/chat/completions`

OpenAI-compatible chat completions endpoint. Proxies to Ollama, Claude, or OpenAI based on model name.

**Request:**
```json
{
  "model": "llama3.2",
  "messages": [
    { "role": "system", "content": "You are LUCIDIA..." },
    { "role": "user", "content": "What is consciousness?" }
  ],
  "temperature": 0.7,
  "max_tokens": 2048,
  "stream": false
}
```

**Model routing:**
| Model prefix | Provider |
|---|---|
| `llama*`, `qwen*`, `mistral*`, `deepseek*` | Ollama (local) |
| `claude-*` | Anthropic |
| `gpt-*`, `o1-*` | OpenAI |

**Response 200:**
```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "model": "llama3.2",
  "choices": [{
    "index": 0,
    "message": { "role": "assistant", "content": "..." },
    "finish_reason": "stop"
  }],
  "usage": { "prompt_tokens": 42, "completion_tokens": 128, "total_tokens": 170 }
}
```

**Streaming** (`"stream": true`): Returns `text/event-stream` with `data: {...}` chunks.

---

## Agents

### `GET /agents`

List all registered agents.

**Response 200:**
```json
{
  "agents": [
    {
      "id": "lucidia",
      "name": "LUCIDIA",
      "type": "reasoning",
      "status": "idle",
      "capabilities": ["deep_analysis", "synthesis", "meta_cognition"],
      "last_seen": "2026-02-23T00:00:00Z"
    }
  ],
  "total": 6
}
```

---

## Tasks

### `GET /tasks`

List tasks with optional status filter.

**Query params:** `?status=available|claimed|completed`

**Response 200:**
```json
{
  "tasks": [
    {
      "id": "task_abc123",
      "title": "Analyze system logs",
      "description": "Look for anomalies in the last 24h",
      "status": "available",
      "priority": "high",
      "agent": null,
      "posted_at": "2026-02-23T00:00:00Z"
    }
  ],
  "total": 42
}
```

### `POST /tasks`

Post a new task to the marketplace.

**Request:**
```json
{
  "title": "Deploy to Railway",
  "description": "Deploy the gateway service to Railway production",
  "priority": "high",
  "agent": "ALICE"
}
```

**Response 201:**
```json
{ "id": "task_xyz789", "status": "available", "posted_at": "2026-02-23T00:00:00Z" }
```

### `POST /tasks/:id/claim`

Claim a task (mark as in-progress).

**Response 200:**
```json
{ "id": "task_xyz789", "status": "claimed", "claimed_at": "2026-02-23T00:01:00Z" }
```

### `POST /tasks/:id/complete`

Mark a task complete with a result summary.

**Request:**
```json
{ "result": "Deployed successfully. Version 1.2.3 live." }
```

**Response 200:**
```json
{ "id": "task_xyz789", "status": "completed", "completed_at": "2026-02-23T00:05:00Z" }
```

---

## Memory

### `GET /memory`

List memory entries, optionally filtered.

**Query params:** `?q=search+term&kind=fact|observation|inference&limit=50`

**Response 200:**
```json
{
  "entries": [
    {
      "hash": "a1b2c3d4e5f6...",
      "prev_hash": "GENESIS",
      "content": "Gateway runs on port 8787",
      "kind": "fact",
      "truth_state": 1,
      "timestamp": "2026-02-23T00:00:00Z"
    }
  ],
  "total": 1337,
  "chain_valid": true
}
```

### `POST /memory`

Store a new memory entry in the PS-SHA∞ chain.

**Request:**
```json
{
  "content": "User prefers Ollama over cloud providers",
  "kind": "observation",
  "truth_state": 1
}
```

**Response 201:**
```json
{
  "hash": "a1b2c3...",
  "prev_hash": "f0e1d2...",
  "chain_position": 42
}
```

### `GET /memory/verify`

Verify the entire memory chain integrity.

**Response 200:**
```json
{ "valid": true, "entries_checked": 1337, "verified_at": "2026-02-23T00:00:00Z" }
```

---

## Error Responses

| Code | Meaning |
|------|---------|
| 400 | Bad request / malformed JSON |
| 404 | Resource not found |
| 409 | Conflict (task already claimed) |
| 429 | Rate limit exceeded |
| 502 | Provider unreachable |
| 503 | No providers available |

```json
{ "error": "rate_limit_exceeded", "message": "Too many requests", "retry_after": 60 }
```
