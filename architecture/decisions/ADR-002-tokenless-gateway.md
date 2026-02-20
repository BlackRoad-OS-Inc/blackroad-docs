# ADR-002: Tokenless Gateway Pattern

**Status:** Accepted
**Date:** 2026-01-15
**Deciders:** Alexa Amundson

## Context

BlackRoad OS orchestrates multiple AI agents that communicate with various AI providers (Anthropic, OpenAI, Ollama, Gemini). The original architecture had each agent holding its own API keys, which created several problems:

- API keys scattered across agent configurations and environment variables
- No centralized control over which agents could access which providers
- Key rotation required updating every agent
- No unified audit trail for provider usage
- Risk of key leakage through agent logs or errors

## Decision

Adopt a **tokenless gateway pattern** where:

1. No agent ever holds a provider API key
2. All provider communication flows through a central gateway
3. Agents authenticate to the gateway with Bearer tokens that identify the agent (not a provider key)
4. The gateway holds all provider keys and injects them at request time
5. A policy engine at the gateway controls which agents can access which providers

## Consequences

### Positive

- Single point of key management — rotate once, applies everywhere
- Fine-grained access control per agent per provider
- Complete audit trail at the gateway
- Agents cannot accidentally leak provider keys
- Agents are provider-agnostic — they request capabilities, not specific APIs

### Negative

- Gateway is a single point of failure (mitigated by health checks and failover)
- Additional network hop for every request (mitigated by co-located deployment)
- Gateway must be highly available and performant

### Mitigations

- Gateway deployed on Cloudflare Workers for edge performance
- Health check endpoints for monitoring
- Fallback chains: if gateway is unreachable, agents queue requests
- Rate limiting at the gateway prevents cascade failures
