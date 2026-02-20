# Getting Started

Set up your development environment for BlackRoad OS.

## Prerequisites

| Tool | Version | Install |
|------|---------|---------|
| Node.js | 22+ | `brew install node@22` or [nodejs.org](https://nodejs.org) |
| Git | 2.40+ | `brew install git` |
| GitHub CLI | 2.40+ | `brew install gh` |
| Docker | 24+ | [docker.com](https://www.docker.com/products/docker-desktop/) |
| Terraform | 1.9+ | `brew install terraform` (for blackroad-infra) |

## Authentication

```bash
# GitHub — required for private repos
gh auth login

# Verify access to the org
gh repo list BlackRoad-OS-Inc --limit 5
```

## Clone the Repositories

```bash
mkdir -p ~/blackroad-inc && cd ~/blackroad-inc

# Clone in dependency order
gh repo clone BlackRoad-OS-Inc/blackroad-docs
gh repo clone BlackRoad-OS-Inc/blackroad-core
gh repo clone BlackRoad-OS-Inc/blackroad-agents
gh repo clone BlackRoad-OS-Inc/blackroad-infra
gh repo clone BlackRoad-OS-Inc/blackroad-operator
gh repo clone BlackRoad-OS-Inc/blackroad-web
```

## Quick Start

### 1. Start the Gateway

```bash
cd blackroad-core
npm install
npm run dev
# Gateway running at http://localhost:8787
```

### 2. Start the Web Dashboard

```bash
cd blackroad-web
npm install
npm run dev
# Dashboard at http://localhost:3000
```

### 3. Try the CLI

```bash
cd blackroad-operator
npm install
npm run build
./bin/br.js status
```

## Project Map

| Repository | What It Does | Port |
|-----------|--------------|------|
| `blackroad-core` | Gateway + runtime engine | 8787 |
| `blackroad-web` | Frontend dashboard | 3000 |
| `blackroad-agents` | Agent definitions + orchestration | Library (no server) |
| `blackroad-operator` | CLI (`br` command) | CLI (no server) |
| `blackroad-infra` | Terraform + Docker | N/A |
| `blackroad-docs` | Documentation (this repo) | N/A |

## Next Steps

- [Local Development](local-development.md) — Detailed development setup
- [Adding an Agent](adding-an-agent.md) — Create a new agent
- [Adding a Provider](adding-a-provider.md) — Integrate a new AI provider
- [Architecture Overview](../architecture/overview.md) — Understand the system
