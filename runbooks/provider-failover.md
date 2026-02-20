# Runbook: Provider Failover

Procedure for handling AI provider outages and failover.

## Symptoms

- Specific provider returns errors (5xx, timeouts)
- Agent invocations fail for agents using that provider
- Health check shows provider as `unavailable`
- Increased latency for affected agents

## Automatic Failover

The gateway has built-in fallback chains. When a provider fails:

1. Gateway detects the failure (HTTP error or timeout)
2. Request is retried with the next provider in the agent's fallback chain
3. Metrics are updated to reflect the failover
4. Health check marks the provider as degraded

### Fallback Chains

```
Octavia:  anthropic → openai → ollama
Lucidia:  anthropic → ollama
Alice:    ollama (no fallback)
Cipher:   anthropic (no fallback)
Prism:    openai → ollama
Planner:  anthropic → openai → gemini
```

Agents with a single provider (Alice, Cipher) cannot fail over automatically.

## Manual Intervention

### When automatic failover is insufficient

If all providers in an agent's chain are down, or if the fallback produces poor results:

### Step 1: Check provider status

```bash
# Check health endpoint
curl http://localhost:8787/v1/health | jq '.providers'

# Check provider status pages
# Anthropic: https://status.anthropic.com/
# OpenAI: https://status.openai.com/
# Google: https://status.cloud.google.com/
```

### Step 2: Temporarily redirect agents

Update `config/agent-permissions.json` to change provider assignments:

```json
{
  "cipher": {
    "providers": ["openai"],
    "comment": "TEMP: anthropic down, using openai fallback"
  }
}
```

Restart the gateway to apply:

```bash
npm start
```

### Step 3: Monitor the original provider

```bash
# Poll provider health every 60 seconds
watch -n 60 'curl -s http://localhost:8787/v1/health | jq .providers.anthropic'
```

### Step 4: Restore original configuration

Once the provider recovers:

1. Revert the config changes
2. Restart the gateway
3. Verify the provider shows as `available`
4. Test agent invocations

## Ollama-Specific Issues

Ollama runs locally and can fail for different reasons:

```bash
# Check if Ollama is running
curl http://localhost:11434/api/tags

# Restart Ollama
ollama serve

# Check available models
ollama list

# Pull a missing model
ollama pull qwen2.5:7b
```

## Prevention

- Monitor all provider status pages
- Set up alerts for provider health changes
- Maintain diverse fallback chains (mix cloud and local providers)
- Keep Ollama models up to date on local machines
- Test failover paths regularly
