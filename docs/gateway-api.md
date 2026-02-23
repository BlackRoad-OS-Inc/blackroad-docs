---
title: Gateway API
description: BlackRoad tokenless AI gateway — all provider communication goes here
sidebar_position: 2
---

# BlackRoad Gateway API

**Base URL:** `http://127.0.0.1:8787` (local) · `https://gateway.blackroad.ai` (cloud)

All agents communicate **only** through this gateway. No agent ever holds API keys.

## Authentication

All protected endpoints require a Bearer token:

```http
Authorization: Bearer your-agent-token
```

## Endpoints

### Health

#### `GET /v1/health`

No auth required. Returns gateway status.

```json
{
  "status": "healthy",
  "version": "0.1.0",
  "uptime": 3600.5,
  "providers": {
    "ollama": "connected",
    "openai": "configured",
    "anthropic": "configured"
  }
}
```

#### `GET /v1/health/ready`

Kubernetes-compatible readiness probe.

```json
{ "ready": true }
```

### Chat Completions

#### `POST /v1/chat`

Route a chat completion to any provider via a unified interface.

**Request:**
```json
{
  "model": "qwen2.5:3b",
  "messages": [
    { "role": "system", "content": "You are LUCIDIA." },
    { "role": "user", "content": "What is trinary logic?" }
  ],
  "temperature": 0.7,
  "max_tokens": 2048,
  "stream": false
}
```

**Response:**
```json
{
  "id": "chatcmpl-abc123",
  "model": "qwen2.5:3b",
  "provider": "ollama",
  "choices": [
    {
      "message": { "role": "assistant", "content": "Trinary logic uses 1, 0, -1..." },
      "finish_reason": "stop"
    }
  ],
  "usage": { "prompt_tokens": 45, "completion_tokens": 120, "total_tokens": 165 }
}
```

**Provider Routing:**

| Model prefix | Routes to |
|-------------|-----------|
| `gpt-*` | OpenAI |
| `claude-*` | Anthropic |
| `gemini-*` | Google |
| `*/` | Together AI |
| Everything else | Ollama (local) |

### Agents

#### `GET /v1/agents`

List all registered agents.

```json
{
  "agents": [
    { "id": "lucidia-001", "name": "LUCIDIA", "type": "reasoning", "status": "active" },
    { "id": "alice-001",   "name": "ALICE",   "type": "worker",    "status": "active" }
  ],
  "total": 6
}
```

#### `POST /v1/agents/invoke`

Invoke an agent with a task.

```json
{
  "agent": "LUCIDIA",
  "task": "Analyze the Pi fleet telemetry and suggest optimizations",
  "context": { "fleet_size": 2, "worlds_generated": 14 }
}
```

### Memory

#### `POST /v1/memory`

Store a memory entry.

```json
{
  "content": "aria64 has 174GB free disk space",
  "key": "aria64_disk",
  "type": "observation"
}
```

**Response:**
```json
{
  "hash": "a3f2b1c4...",
  "prev_hash": "GENESIS",
  "key": "aria64_disk",
  "content": "aria64 has 174GB free disk space",
  "type": "observation",
  "truth_state": 0,
  "timestamp": "2026-02-23T03:11:32Z"
}
```

## Rate Limits

| Tier | Requests/min | Burst |
|------|-------------|-------|
| Agent (default) | 60 | 10 |
| Agent (elevated) | 300 | 50 |
| Internal | Unlimited | — |

## Errors

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Try again in 45 seconds.",
    "retry_after": 45
  }
}
```

| Code | HTTP | Description |
|------|------|-------------|
| `UNAUTHORIZED` | 401 | Missing or invalid Bearer token |
| `FORBIDDEN` | 403 | Agent lacks permission for this operation |
| `RATE_LIMIT_EXCEEDED` | 429 | Rate limit hit |
| `PROVIDER_UNAVAILABLE` | 503 | AI provider unreachable |
| `INVALID_REQUEST` | 422 | Request body validation failed |
