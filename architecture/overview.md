# System Overview

BlackRoad OS is a distributed AI infrastructure platform. This document describes the high-level architecture and how components interact.

## Component Map

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[blackroad-web<br/>Next.js Dashboard]
        CLI[blackroad-operator<br/>br CLI]
    end

    subgraph "Gateway Layer"
        GW[blackroad-core<br/>Tokenless Gateway]
    end

    subgraph "Agent Layer"
        AGT[blackroad-agents<br/>Orchestration]
        OCT[Octavia<br/>Architect]
        LUC[Lucidia<br/>Dreamer]
        ALI[Alice<br/>Operator]
        CIP[Cipher<br/>Sentinel]
        PRI[Prism<br/>Analyst]
        PLN[Planner<br/>Strategist]
    end

    subgraph "Provider Layer"
        ANT[Anthropic]
        OAI[OpenAI]
        OLL[Ollama]
        GEM[Gemini]
    end

    subgraph "Infrastructure"
        INF[blackroad-infra<br/>Terraform + Docker]
        CF[Cloudflare]
        RW[Railway]
        DO[DigitalOcean]
        PI[Raspberry Pi Fleet]
    end

    WEB --> GW
    CLI --> GW
    GW --> AGT
    AGT --> OCT & LUC & ALI & CIP & PRI & PLN
    GW --> ANT & OAI & OLL & GEM
    INF --> CF & RW & DO & PI
```

## Repositories

| Repository | Purpose | Stack |
|------------|---------|-------|
| `blackroad-core` | Gateway + runtime engine | TypeScript, Hono, Zod |
| `blackroad-agents` | Agent definitions + orchestration | TypeScript, Zod, Commander |
| `blackroad-web` | Frontend dashboard | Next.js 15, React 19, Tailwind |
| `blackroad-operator` | CLI tooling (`br` command) | TypeScript, Commander, Chalk |
| `blackroad-infra` | IaC + CI/CD + containers | Terraform, Docker, GitHub Actions |
| `blackroad-docs` | Documentation (this repo) | Markdown, Mermaid |

## Core Principles

1. **Tokenless agents** — No agent ever holds an API key. All provider communication flows through the gateway.
2. **Policy-driven access** — Agent permissions are defined declaratively in JSON. The policy engine evaluates every request.
3. **Provider agnostic** — Agents request capabilities, not specific providers. The gateway routes to the best available provider.
4. **Fallback chains** — Every agent has an ordered list of providers. If one fails, the next is tried automatically.
5. **Observable** — Every request is logged, metered, and traceable.

## Data Flow

```mermaid
sequenceDiagram
    participant C as Client (Web/CLI)
    participant G as Gateway
    participant P as Policy Engine
    participant A as Agent Orchestrator
    participant R as Provider

    C->>G: POST /v1/chat/completions
    G->>P: Evaluate permissions
    P-->>G: Allow / Deny
    G->>A: Route to agent
    A->>A: Build prompt context
    A->>G: Select provider
    G->>R: Forward request (with API key)
    R-->>G: Response
    G-->>C: Response (sanitized)
```

## Network Topology

All services communicate over HTTPS. The gateway binds to localhost by default and is exposed through Cloudflare Tunnel or direct deployment. See [networking documentation](../guides/local-development.md) for local setup.

## Related Documents

- [Gateway Architecture](gateway.md)
- [Agent System](agent-system.md)
- [Security Model](security-model.md)
