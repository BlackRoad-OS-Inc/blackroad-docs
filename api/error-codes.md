# Error Codes

Complete reference of error codes returned by the BlackRoad Gateway.

## Error Response Format

```json
{
  "error": {
    "code": "GATEWAY_AUTH_FAILED",
    "message": "Invalid or missing authorization token",
    "status": 401
  }
}
```

## Gateway Errors

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `GATEWAY_INTERNAL_ERROR` | 500 | Unexpected internal error |
| `GATEWAY_AUTH_FAILED` | 401 | Missing or invalid Bearer token |
| `GATEWAY_FORBIDDEN` | 403 | Agent lacks permission for this action |
| `GATEWAY_NOT_FOUND` | 404 | Requested resource does not exist |
| `GATEWAY_RATE_LIMITED` | 429 | Agent has exceeded its rate limit |
| `GATEWAY_TIMEOUT` | 504 | Request timed out waiting for provider |
| `GATEWAY_UNAVAILABLE` | 503 | Gateway is starting up or shutting down |

## Provider Errors

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `PROVIDER_UNAVAILABLE` | 502 | No providers are available for this request |
| `PROVIDER_REQUEST_FAILED` | 502 | Provider returned an error response |
| `PROVIDER_TIMEOUT` | 504 | Provider did not respond within timeout |
| `PROVIDER_RATE_LIMITED` | 429 | Provider rate limit reached (upstream) |
| `PROVIDER_AUTH_FAILED` | 502 | Provider rejected the API key |
| `PROVIDER_MODEL_NOT_FOUND` | 400 | Requested model not available on provider |
| `PROVIDER_OVERLOADED` | 503 | Provider is temporarily overloaded |

## Agent Errors

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `AGENT_NOT_FOUND` | 404 | No agent with the given name exists |
| `AGENT_UNAVAILABLE` | 503 | Agent's providers are all unavailable |
| `AGENT_TOKEN_LIMIT` | 400 | Request exceeds agent's max token budget |
| `AGENT_CAPABILITY_MISMATCH` | 400 | Agent does not have the requested capability |

## Policy Errors

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `POLICY_DENIED` | 403 | Policy engine denied the request |
| `POLICY_PROVIDER_NOT_ALLOWED` | 403 | Agent is not allowed to use this provider |
| `POLICY_TOKEN_BUDGET_EXCEEDED` | 400 | Request exceeds the agent's token budget |
| `POLICY_CONFIG_ERROR` | 500 | Policy configuration is invalid |

## Validation Errors

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `VALIDATION_FAILED` | 400 | Request body failed schema validation |
| `VALIDATION_MISSING_FIELD` | 400 | Required field is missing |
| `VALIDATION_INVALID_TYPE` | 400 | Field has wrong type |
| `VALIDATION_INVALID_VALUE` | 400 | Field value is out of range or invalid |

## Handling Errors

### Retry Strategy

| Error Category | Retry? | Strategy |
|---------------|--------|----------|
| Gateway 5xx | Yes | Exponential backoff, max 3 retries |
| Provider 502/503 | Yes | Try next provider in fallback chain |
| Provider 429 | Yes | Wait for `Retry-After` header |
| 4xx errors | No | Fix the request |
| Policy errors | No | Check agent permissions |

### Client Example

```typescript
async function callGateway(request: ChatRequest): Promise<ChatResponse> {
  const response = await fetch('/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify(request),
  })

  if (!response.ok) {
    const error = await response.json()
    if (error.error.status === 429) {
      const retryAfter = response.headers.get('Retry-After')
      // Wait and retry
    }
    throw new Error(`${error.error.code}: ${error.error.message}`)
  }

  return response.json()
}
```
