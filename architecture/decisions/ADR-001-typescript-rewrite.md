# ADR-001: TypeScript Rewrite

**Status:** Accepted
**Date:** 2026-01-15
**Deciders:** Alexa Amundson

## Context

The original BlackRoad gateway and agent system were written in plain JavaScript (Node.js). As the system grew in complexity — multiple providers, a policy engine, structured protocol schemas — the lack of type safety became a significant source of bugs and slowed development.

Key pain points:

- Runtime type errors in provider adapters (different response shapes)
- No compile-time validation of configuration files
- Difficulty refactoring across modules without breaking contracts
- No IDE autocompletion for complex nested types

## Decision

Rewrite all Node.js services (blackroad-core, blackroad-agents, blackroad-operator) in **TypeScript 5** targeting **Node.js 22**.

Specific choices:

- **TypeScript 5.7+** for the latest type system features
- **Node.js 22 LTS** for native ESM, top-level await, and stable APIs
- **ESM modules** (`"type": "module"`) — no CommonJS
- **Strict mode** enabled (`"strict": true`)
- **Zod** for runtime schema validation (complements compile-time types)
- **Hono** for HTTP framework (lightweight, TypeScript-first)

## Consequences

### Positive

- Compile-time type checking catches errors before deployment
- Zod schemas provide both runtime validation and TypeScript type inference
- Better IDE support with autocompletion and refactoring tools
- Easier onboarding — types serve as documentation
- Hono's type-safe routing catches route/handler mismatches at compile time

### Negative

- Build step required (`tsc` before running)
- Slightly more verbose code (type annotations)
- Team must learn TypeScript idioms
- Migration period where old JS and new TS coexist

### Mitigations

- Use `tsx` for development (JIT compilation, no build step in dev)
- Gradual migration — new code in TS, old code migrated module by module
- Vitest for testing (native TypeScript support, no separate config)
