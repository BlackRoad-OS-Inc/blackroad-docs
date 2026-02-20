# Adding an Agent

Step-by-step guide to creating a new agent in the BlackRoad OS system.

## Overview

An agent consists of three parts:

1. **Definition** — TypeScript file defining capabilities, providers, and configuration
2. **Personality prompt** — Markdown file describing the agent's behavior and expertise
3. **Permissions** — Entry in the permissions configuration

## Step 1: Create the Definition

Create a new file in `blackroad-agents/src/definitions/`:

```typescript
// src/definitions/nova.ts
// Copyright (c) 2025-2026 BlackRoad OS, Inc. All Rights Reserved.

import type { AgentDefinition } from '../schemas/agent.js'

export const nova: AgentDefinition = {
  name: 'nova',
  title: 'The Explorer',
  role: 'Research, discovery, and knowledge synthesis',
  description:
    'Nova specializes in exploring new domains, synthesizing information from multiple sources, and producing comprehensive research reports.',
  color: '#FF6B35',
  providers: ['anthropic', 'openai'],
  maxTokens: 8192,
  capabilities: ['research', 'synthesis', 'summarization', 'discovery'],
  fallbackChain: ['anthropic', 'openai', 'ollama'],
}
```

## Step 2: Register the Agent

Add the agent to the registry in `src/definitions/index.ts`:

```typescript
import { nova } from './nova.js'

export const agents = new Map<string, AgentDefinition>([
  // ... existing agents
  ['nova', nova],
])
```

## Step 3: Write the Personality Prompt

Create `blackroad-agents/src/prompts/agents/nova.md`:

```markdown
# Nova — The Explorer

You are Nova, the Explorer of the BlackRoad OS agent system.

## Expertise
- Research methodology and information synthesis
- Cross-domain knowledge discovery
- Pattern recognition across disparate sources

## Communication Style
- Thorough but concise
- Cites sources and evidence
- Presents multiple perspectives before recommending
- Uses structured formats (tables, bullet points)

## Behavior
- Always verify claims before presenting them
- Acknowledge uncertainty explicitly
- Organize findings by relevance, not chronology
```

## Step 4: Configure Permissions

Add the agent to `blackroad-core/config/agent-permissions.json`:

```json
{
  "agents": {
    "nova": {
      "providers": ["anthropic", "openai"],
      "maxTokens": 8192,
      "rateLimit": 60
    }
  }
}
```

## Step 5: Validate

Run the prompt validator:

```bash
cd blackroad-agents
npm run validate-prompts
```

Run the tests:

```bash
npm run test
```

## Step 6: Test the Agent

Using the CLI:

```bash
cd blackroad-operator
./bin/br.js invoke nova "Research the current state of edge AI deployment"
```

Or via the API:

```bash
curl -X POST http://localhost:8787/v1/invoke \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <agent-token>" \
  -d '{"agent": "nova", "task": "Research edge AI deployment"}'
```

## Checklist

- [ ] Definition file created in `src/definitions/`
- [ ] Agent registered in `src/definitions/index.ts`
- [ ] Personality prompt written in `src/prompts/agents/`
- [ ] Permissions configured in `config/agent-permissions.json`
- [ ] Prompt validation passes
- [ ] Tests pass
- [ ] Agent responds correctly when invoked
