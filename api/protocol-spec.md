# Protocol Specification

Internal communication protocol between BlackRoad OS components.

## Request Envelope

Every request to the gateway is wrapped in a standard envelope:

```typescript
interface GatewayRequest {
  // Provided by the client
  model?: string           // Preferred model (optional, gateway may override)
  messages: Message[]      // Chat messages
  temperature?: number     // 0.0 - 2.0 (default: 0.7)
  max_tokens?: number      // Max response tokens (default: agent limit)

  // Provided by the gateway (injected from auth)
  agent?: string           // Agent identity (from Bearer token)
  request_id?: string      // Unique request ID (generated)
}

interface Message {
  role: 'system' | 'user' | 'assistant'
  content: string
}
```

## Response Envelope

Every response from the gateway follows this structure:

```typescript
interface GatewayResponse {
  id: string               // Unique response ID
  content: string          // Response text
  model: string            // Model that generated the response
  provider: string         // Provider that served the request
  usage: Usage             // Token usage metrics
  metadata?: {
    duration_ms: number    // Processing time
    fallback_used: boolean // Whether a fallback provider was used
  }
}

interface Usage {
  prompt_tokens: number
  completion_tokens: number
  total_tokens: number
}
```

## Error Envelope

```typescript
interface ErrorResponse {
  error: {
    code: string           // Machine-readable error code
    message: string        // Human-readable description
    status: number         // HTTP status code
    details?: unknown      // Additional context (optional)
  }
}
```

## Streaming Protocol

For streaming responses, the gateway uses Server-Sent Events (SSE):

```
POST /v1/chat/completions
Content-Type: application/json

{ "model": "...", "messages": [...], "stream": true }
```

Response:

```
Content-Type: text/event-stream

data: {"id":"resp_1","delta":"Hello","done":false}

data: {"id":"resp_1","delta":" world","done":false}

data: {"id":"resp_1","delta":"","done":true,"usage":{"prompt_tokens":10,"completion_tokens":2,"total_tokens":12}}
```

Each SSE event contains a JSON chunk with:

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Response ID (same for all chunks) |
| `delta` | string | Text chunk |
| `done` | boolean | Whether this is the final chunk |
| `usage` | Usage | Token usage (only on final chunk) |

## Content Types

| Endpoint | Request | Response |
|----------|---------|----------|
| Chat completions | `application/json` | `application/json` or `text/event-stream` |
| Invoke | `application/json` | `application/json` |
| Health | N/A | `application/json` |
| Agents | N/A | `application/json` |
| Metrics | N/A | `application/json` |

## Authentication

All requests (except health check) require a Bearer token:

```
Authorization: Bearer <agent-token>
```

The token identifies the agent making the request. It is NOT a provider API key. The gateway uses this to look up agent permissions in the policy engine.

## Versioning

The API is versioned via URL path prefix: `/v1/`. Breaking changes will increment the version number. Non-breaking additions (new fields, new endpoints) do not require a version bump.
