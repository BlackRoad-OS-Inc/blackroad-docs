# ADR-003: Vitest for Testing

**Status:** Accepted
**Date:** 2026-01-15
**Deciders:** Alexa Amundson

## Context

With the TypeScript rewrite (ADR-001), we needed a testing framework that works natively with ESM TypeScript without extra configuration. The main candidates were:

- **Jest** — Industry standard, large ecosystem, but requires `ts-jest` or `@swc/jest` for TypeScript
- **Vitest** — Vite-powered, native TypeScript/ESM support, Jest-compatible API
- **Node.js test runner** — Built-in, zero dependencies, but limited ecosystem

## Decision

Adopt **Vitest** as the testing framework for all TypeScript services.

## Consequences

### Positive

- Native TypeScript support — no `ts-jest`, no `babel`, no extra transforms
- Native ESM support — matches our `"type": "module"` configuration
- Jest-compatible API (`describe`, `it`, `expect`) — minimal learning curve
- Built-in coverage with `v8` provider
- Watch mode with HMR-style fast re-runs
- Built-in mocking compatible with ESM (no `jest.mock` ESM issues)

### Negative

- Smaller ecosystem than Jest (fewer community plugins)
- Team members familiar with Jest need minor adjustment
- Vitest-specific features (like in-source testing) may not transfer to other projects

### Mitigations

- API is 95% compatible with Jest — switching cost is near zero
- Vitest community is growing rapidly
- We use standard patterns (`describe`/`it`/`expect`) that work in both
