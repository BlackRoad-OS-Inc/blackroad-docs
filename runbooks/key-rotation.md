# Runbook: API Key Rotation

Procedure for rotating AI provider API keys with zero downtime.

## When to Rotate

- **Scheduled:** Every 90 days (quarterly)
- **Immediate:** If a key is suspected to be compromised
- **Immediate:** If a team member with key access leaves
- **On alert:** If unusual API usage patterns are detected

## Pre-Rotation Checklist

- [ ] Identify which keys need rotation
- [ ] Verify access to provider dashboards
- [ ] Ensure gateway is healthy (`/v1/health`)
- [ ] Plan a low-traffic window (recommended, not required)
- [ ] Have rollback plan ready

## Rotation Procedure

### Step 1: Generate New Key

Generate the new key on the provider's dashboard. **Do not revoke the old key yet.**

| Provider | Dashboard |
|----------|-----------|
| Anthropic | https://console.anthropic.com/settings/keys |
| OpenAI | https://platform.openai.com/api-keys |
| Google (Gemini) | https://aistudio.google.com/apikey |

### Step 2: Update the Gateway

Update the environment variable with the new key:

```bash
# Local development
# Edit .env in blackroad-core
BLACKROAD_ANTHROPIC_API_KEY=sk-ant-new-key-here

# Railway
railway variables set BLACKROAD_ANTHROPIC_API_KEY=sk-ant-new-key-here

# Cloudflare Worker
wrangler secret put BLACKROAD_ANTHROPIC_API_KEY
# (paste new key when prompted)
```

### Step 3: Restart/Redeploy

```bash
# Local
npm start

# Railway
railway up

# Cloudflare
wrangler deploy
```

### Step 4: Verify

```bash
# Check health
curl http://localhost:8787/v1/health | jq '.providers'

# Test a request
curl -X POST http://localhost:8787/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <agent-token>" \
  -d '{
    "model": "claude-sonnet-4-20250514",
    "messages": [{"role": "user", "content": "Hello"}],
    "max_tokens": 10
  }'
```

Confirm the response is successful and the provider shows as `available`.

### Step 5: Revoke Old Key

Only after verifying the new key works:

1. Go to the provider dashboard
2. Revoke/delete the old key
3. Verify the gateway still works (it should — it's using the new key)

### Step 6: Document

Log the rotation:

```bash
# Record in memory/audit
echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) KEY_ROTATION provider=anthropic status=complete" >> /var/log/blackroad/key-rotation.log
```

## Rollback

If the new key doesn't work:

1. Immediately restore the old key in the environment
2. Restart the gateway
3. Investigate why the new key failed
4. Retry the rotation

The old key remains valid until explicitly revoked, so rollback is always possible.

## Emergency Rotation (Compromised Key)

If a key is compromised:

1. **Immediately revoke** the compromised key on the provider dashboard
2. Generate a new key
3. Update the gateway (Step 2-3 above)
4. Check provider usage logs for unauthorized activity
5. File an incident report per [incident response](../governance/incident-response.md)

## Automation

The `scripts/rotate-keys.sh` script in `blackroad-infra` automates steps 2-4:

```bash
cd blackroad-infra
./scripts/rotate-keys.sh --provider anthropic --key "sk-ant-new-key"
```

This updates the environment, restarts the service, and runs verification tests.
