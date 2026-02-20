# Local Development

Detailed guide for running BlackRoad OS services locally.

## Environment Variables

Create a `.env` file in `blackroad-core`:

```bash
# Gateway
BLACKROAD_GATEWAY_PORT=8787
BLACKROAD_GATEWAY_HOST=127.0.0.1

# Providers (only needed if using external providers)
BLACKROAD_ANTHROPIC_API_KEY=sk-ant-...
BLACKROAD_OPENAI_API_KEY=sk-...

# Ollama (local, no key needed)
BLACKROAD_OLLAMA_URL=http://localhost:11434
```

Never commit `.env` files. They are in `.gitignore`.

## Running Services

### Gateway (blackroad-core)

```bash
cd blackroad-core
npm install
npm run dev          # Starts with hot reload via tsx
```

The gateway runs at `http://localhost:8787`. Test it:

```bash
curl http://localhost:8787/v1/health
```

### Web Dashboard (blackroad-web)

```bash
cd blackroad-web
npm install
npm run dev          # Next.js dev server
```

Dashboard runs at `http://localhost:3000`.

### Using Docker Compose

To run everything together:

```bash
cd blackroad-infra/docker
docker compose up
```

This starts the gateway (8787) and web app (3000) in containers.

## Development Workflow

### TypeScript Projects (core, agents, operator)

```bash
npm run dev          # Watch mode with tsx (auto-recompile)
npm run typecheck    # Check types without building
npm run test         # Run tests once
npm run test:watch   # Run tests in watch mode
npm run lint         # ESLint
npm run format       # Prettier
```

### Build for Production

```bash
npm run build        # Compile TypeScript to dist/
npm start            # Run compiled code
```

## Testing

All TypeScript repos use Vitest:

```bash
npm run test                  # Run all tests
npm run test -- --coverage    # With coverage report
npm run test -- -t "pattern"  # Filter by test name
```

## Debugging

### VS Code

Add to `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Gateway",
      "type": "node",
      "request": "launch",
      "runtimeExecutable": "npx",
      "runtimeArgs": ["tsx", "src/index.ts"],
      "cwd": "${workspaceFolder}",
      "console": "integratedTerminal"
    }
  ]
}
```

### Logging

The gateway uses Pino for structured logging. Set the log level:

```bash
BLACKROAD_LOG_LEVEL=debug npm run dev
```

Log levels: `trace`, `debug`, `info`, `warn`, `error`, `fatal`.

## Common Issues

| Issue | Solution |
|-------|---------|
| Port 8787 in use | Kill the process: `lsof -ti:8787 \| xargs kill` |
| TypeScript errors after pull | Run `npm install` then `npm run typecheck` |
| Tests fail on fresh clone | Ensure Node 22+: `node --version` |
| Ollama not responding | Start Ollama: `ollama serve` |
