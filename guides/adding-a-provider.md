# Adding a Provider

Step-by-step guide to integrating a new AI provider into the BlackRoad gateway.

## Overview

A provider adapter translates between the BlackRoad internal protocol and the provider's API. Each adapter implements the `Provider` interface.

## Step 1: Implement the Provider Interface

Create a new file in `blackroad-core/src/providers/`:

```typescript
// src/providers/mistral.ts
// Copyright (c) 2025-2026 BlackRoad OS, Inc. All Rights Reserved.

import type { Provider, ChatRequest, ChatResponse } from './types.js'
import { ProviderError } from '../protocol/errors.js'
import { logger } from '../telemetry/logger.js'

export class MistralProvider implements Provider {
  name = 'mistral'
  private baseUrl: string
  private apiKey: string

  constructor(config: { baseUrl: string; apiKey: string }) {
    this.baseUrl = config.baseUrl
    this.apiKey = config.apiKey
  }

  async chat(request: ChatRequest): Promise<ChatResponse> {
    const response = await fetch(`${this.baseUrl}/v1/chat/completions`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${this.apiKey}`,
      },
      body: JSON.stringify({
        model: request.model,
        messages: request.messages,
        temperature: request.temperature,
        max_tokens: request.maxTokens,
      }),
    })

    if (!response.ok) {
      throw new ProviderError(
        `Mistral API error: ${response.status}`,
        'PROVIDER_REQUEST_FAILED',
        response.status,
      )
    }

    const data = await response.json()

    return {
      id: data.id,
      content: data.choices[0].message.content,
      model: data.model,
      usage: {
        promptTokens: data.usage.prompt_tokens,
        completionTokens: data.usage.completion_tokens,
        totalTokens: data.usage.total_tokens,
      },
    }
  }

  async isAvailable(): Promise<boolean> {
    try {
      const response = await fetch(`${this.baseUrl}/v1/models`, {
        headers: { Authorization: `Bearer ${this.apiKey}` },
      })
      return response.ok
    } catch {
      return false
    }
  }
}
```

## Step 2: Register in the Provider Registry

Update `src/providers/registry.ts`:

```typescript
import { MistralProvider } from './mistral.js'

// In createProviderRegistry():
if (config.providers.mistral?.enabled) {
  registry.register(
    new MistralProvider({
      baseUrl: config.providers.mistral.baseUrl,
      apiKey: process.env.BLACKROAD_MISTRAL_API_KEY ?? '',
    }),
  )
}
```

## Step 3: Add Configuration

Update `config/default.json`:

```json
{
  "providers": {
    "mistral": {
      "enabled": false,
      "baseUrl": "https://api.mistral.ai"
    }
  }
}
```

## Step 4: Add Environment Variable

Document the new environment variable:

```bash
BLACKROAD_MISTRAL_API_KEY=your-key-here
```

## Step 5: Write Tests

Create `test/providers/mistral.test.ts`:

```typescript
import { describe, it, expect, vi } from 'vitest'
import { MistralProvider } from '../../src/providers/mistral.js'

describe('MistralProvider', () => {
  it('should have correct name', () => {
    const provider = new MistralProvider({
      baseUrl: 'https://api.mistral.ai',
      apiKey: 'test-key',
    })
    expect(provider.name).toBe('mistral')
  })

  it('should format chat requests correctly', async () => {
    // Mock fetch and test request formatting
  })
})
```

## Step 6: Update Agent Permissions

If any agents should use the new provider, update `config/agent-permissions.json`:

```json
{
  "agents": {
    "planner": {
      "providers": ["anthropic", "openai", "gemini", "mistral"]
    }
  }
}
```

## Checklist

- [ ] Provider class implements `Provider` interface
- [ ] `chat()` method handles request/response translation
- [ ] `isAvailable()` method checks provider health
- [ ] Error handling uses `ProviderError` class
- [ ] Registered in provider registry
- [ ] Configuration added to `default.json`
- [ ] Environment variable documented
- [ ] Tests written and passing
- [ ] Agent permissions updated (if applicable)
