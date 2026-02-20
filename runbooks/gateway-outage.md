# Runbook: Gateway Outage

Procedure for responding to a BlackRoad Gateway outage.

## Symptoms

- Health check endpoint (`GET /v1/health`) returns non-200 or times out
- All agent invocations fail
- Web dashboard shows "Gateway Unreachable"
- CLI `br status` reports connection error

## Severity

**SEV-1** — Complete service outage. All agents are offline.

## Diagnosis

### Step 1: Verify the outage

```bash
# Check health endpoint
curl -s -o /dev/null -w "%{http_code}" http://localhost:8787/v1/health

# Check if process is running
lsof -i :8787

# Check from outside (if deployed)
curl -s -o /dev/null -w "%{http_code}" https://gateway.blackroad.io/v1/health
```

### Step 2: Check logs

```bash
# Local logs
cat /var/log/blackroad/gateway.log | tail -50

# Cloudflare Worker logs
wrangler tail

# Railway logs
railway logs --service blackroad-core
```

### Step 3: Check infrastructure

- **Cloudflare status:** https://www.cloudflarestatus.com/
- **Railway status:** https://status.railway.app/
- **DNS resolution:** `dig gateway.blackroad.io`

### Step 4: Check recent changes

```bash
# Recent deployments
git log --oneline -10 --since="1 hour ago"

# Recent config changes
git diff HEAD~3 config/
```

## Mitigation

### Process crashed — Restart

```bash
# Local
npm start

# Railway
railway up --service blackroad-core

# Cloudflare Worker
wrangler deploy
```

### Port conflict

```bash
# Find what's using port 8787
lsof -i :8787

# Kill the conflicting process
kill -9 <PID>

# Restart gateway
npm start
```

### Bad deployment — Rollback

```bash
# Find last working version
git log --oneline -10

# Rollback to specific commit
git checkout <commit-sha>
npm run build
npm start
```

### Configuration error

```bash
# Validate config
node -e "require('./config/default.json')"

# Check environment variables
env | grep BLACKROAD

# Revert config changes
git checkout HEAD~1 -- config/
```

## Recovery

1. Confirm gateway responds: `curl http://localhost:8787/v1/health`
2. Verify all providers are available in health response
3. Test an agent invocation: `br invoke octavia "health check"`
4. Monitor for 30 minutes for recurrence

## Prevention

- Health check monitoring with alerts
- Automated rollback on deployment failure
- Rate limiting to prevent cascade failures
- Regular load testing
