# BlackRoad OS Documentation

Architecture documentation, governance, brand system, roadmap, and technical specifications for BlackRoad OS, Inc.

## Navigation

### Architecture

- [System Overview](architecture/overview.md) — High-level architecture and component relationships
- [Gateway Architecture](architecture/gateway.md) — Tokenless gateway design and request flow
- [Agent System](architecture/agent-system.md) — Agent definitions, orchestration, and routing
- [Security Model](architecture/security-model.md) — Security architecture and threat model

### Architecture Decisions

- [ADR-001: TypeScript Rewrite](architecture/decisions/ADR-001-typescript-rewrite.md)
- [ADR-002: Tokenless Gateway](architecture/decisions/ADR-002-tokenless-gateway.md)
- [ADR-003: Vitest Testing](architecture/decisions/ADR-003-vitest-testing.md)

### Brand

- [Design System](brand/design-system.md) — Complete design system overview
- [Colors](brand/colors.md) — Color palette and usage guidelines
- [Typography](brand/typography.md) — Font families, sizes, and line heights
- [Spacing](brand/spacing.md) — Golden ratio spacing scale

### Governance

- [Decision Log](governance/decision-log.md) — Index of all architectural decisions
- [Release Process](governance/release-process.md) — Versioning, tagging, and deployment
- [Incident Response](governance/incident-response.md) — Severity levels, roles, and procedures

### Developer Guides

- [Getting Started](guides/getting-started.md) — Prerequisites and initial setup
- [Local Development](guides/local-development.md) — Running services locally
- [Adding an Agent](guides/adding-an-agent.md) — Step-by-step agent creation
- [Adding a Provider](guides/adding-a-provider.md) — Integrating a new AI provider

### API

- [Gateway API](api/gateway-api.md) — REST API specification
- [Protocol Spec](api/protocol-spec.md) — Internal communication protocol
- [Error Codes](api/error-codes.md) — Error code reference

### Roadmap

- [Q1 2026](roadmap/2026-q1.md) — Current quarter priorities
- [Backlog](roadmap/backlog.md) — Future work items

### Runbooks

- [Gateway Outage](runbooks/gateway-outage.md) — Response and recovery
- [Provider Failover](runbooks/provider-failover.md) — Failover procedures
- [Key Rotation](runbooks/key-rotation.md) — API key rotation process

## Repository Structure

```
blackroad-docs/
├── architecture/        System design and ADRs
│   └── decisions/       Architecture Decision Records
├── brand/               Design system guidelines
├── governance/          Processes and policies
├── guides/              Developer how-to guides
├── api/                 API specifications
├── roadmap/             Planning documents
└── runbooks/            Operational procedures
```

## Contributing

1. Follow the conventions in [CLAUDE.md](CLAUDE.md)
2. Use Mermaid for diagrams — they render natively on GitHub
3. Keep documents focused — one topic per file
4. Cross-reference related documents with relative links

## License

Copyright (c) 2025-2026 BlackRoad OS, Inc. All Rights Reserved. See [LICENSE](LICENSE).
