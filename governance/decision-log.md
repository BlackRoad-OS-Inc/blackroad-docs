# Architecture Decision Log

Index of all Architecture Decision Records (ADRs) for BlackRoad OS.

## Decision Records

| ID | Title | Status | Date |
|----|-------|--------|------|
| [ADR-001](../architecture/decisions/ADR-001-typescript-rewrite.md) | TypeScript Rewrite | Accepted | 2026-01-15 |
| [ADR-002](../architecture/decisions/ADR-002-tokenless-gateway.md) | Tokenless Gateway Pattern | Accepted | 2026-01-15 |
| [ADR-003](../architecture/decisions/ADR-003-vitest-testing.md) | Vitest for Testing | Accepted | 2026-01-15 |

## Status Definitions

| Status | Meaning |
|--------|---------|
| **Proposed** | Under discussion, not yet decided |
| **Accepted** | Decision made, implementation underway or complete |
| **Deprecated** | Superseded by a newer decision |
| **Rejected** | Considered but not adopted |

## How to Add a Decision

1. Copy the template below
2. Create a new file: `architecture/decisions/ADR-NNN-short-title.md`
3. Fill in the sections
4. Add a row to the table above
5. Submit a pull request for team review

## ADR Template

```markdown
# ADR-NNN: Title

**Status:** Proposed
**Date:** YYYY-MM-DD
**Deciders:** Names

## Context

What is the issue we are deciding on? What forces are at play?

## Decision

What is the change that we are proposing and/or doing?

## Consequences

### Positive
- What becomes easier?

### Negative
- What becomes harder?

### Mitigations
- How do we address the negatives?
```
