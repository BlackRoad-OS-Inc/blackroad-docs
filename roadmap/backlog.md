# Backlog

Future work items organized by theme. These are not scheduled for a specific quarter.

## Agent System

- **Agent memory** — Persistent memory for agents across sessions (PS-SHA-infinity)
- **Agent collaboration** — Multi-agent pipelines where agents hand off work
- **Agent learning** — Track agent performance and adjust routing
- **Custom agents** — Allow users to define their own agents via config
- **Agent marketplace** — Share and discover agent definitions

## Gateway

- **Streaming** — Server-Sent Events for real-time streaming responses
- **Caching** — Cache identical requests to reduce provider costs
- **Request batching** — Batch multiple small requests into single provider calls
- **Cost tracking** — Track per-agent, per-provider costs
- **WebSocket support** — Persistent connections for agent communication

## Web Dashboard

- **Real-time updates** — WebSocket-driven live metrics
- **Agent chat** — Interactive chat with agents from the dashboard
- **Task board** — Kanban-style task management
- **Cost dashboard** — Visualize provider spend over time
- **Dark/light theme** — Theme toggle with brand compliance

## Infrastructure

- **Multi-region** — Deploy gateway to multiple Cloudflare regions
- **Auto-scaling** — Scale based on request volume
- **Disaster recovery** — Automated failover and backup
- **Observability** — OpenTelemetry integration, distributed tracing
- **Secrets manager** — Integration with external secrets managers

## CLI

- **Plugin system** — Extend `br` with custom commands
- **Shell completion** — Bash/zsh/fish autocomplete
- **Interactive mode** — TUI dashboard with live updates
- **Config profiles** — Multiple named configurations

## Security

- **RBAC** — Role-based access control beyond per-agent policies
- **Audit export** — Export audit logs to external SIEM
- **mTLS** — Mutual TLS between gateway and agents
- **Key vault integration** — HashiCorp Vault, AWS Secrets Manager

## Developer Experience

- **SDK** — TypeScript SDK for building on BlackRoad OS
- **Playground** — Web-based prompt testing environment
- **Swagger/OpenAPI** — Auto-generated API documentation
- **VS Code extension** — Agent invocation from the editor
