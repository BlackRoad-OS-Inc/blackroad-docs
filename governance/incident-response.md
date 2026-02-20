# Incident Response

Procedures for handling service incidents affecting BlackRoad OS.

## Severity Levels

| Level | Definition | Response Time | Examples |
|-------|-----------|---------------|----------|
| **SEV-1** | Complete service outage | 15 minutes | Gateway down, all agents offline |
| **SEV-2** | Major feature degraded | 1 hour | One provider unreachable, auth broken |
| **SEV-3** | Minor feature degraded | 4 hours | Slow responses, one agent failing |
| **SEV-4** | Cosmetic / low impact | Next business day | UI glitch, non-critical log error |

## Roles

| Role | Responsibility |
|------|---------------|
| **Incident Commander** | Coordinates response, makes decisions, communicates status |
| **Technical Lead** | Diagnoses root cause, implements fixes |
| **Communications** | Updates stakeholders, writes status page updates |

For BlackRoad OS (small team), all roles may be filled by the same person.

## Response Procedure

### 1. Detect

Incidents are detected through:

- Health check failures (automated monitoring)
- Error rate spikes in gateway telemetry
- User reports
- Agent invocation failures

### 2. Acknowledge

1. Confirm the incident is real (not a false alarm)
2. Assign severity level
3. Start an incident log (timestamped entries)

### 3. Investigate

1. Check gateway health: `GET /v1/health`
2. Check provider status: review each provider's status page
3. Check rate limits: review telemetry for quota exhaustion
4. Check recent deployments: `git log --oneline -10` on affected service
5. Check infrastructure: Cloudflare dashboard, Railway dashboard

### 4. Mitigate

Priority is restoring service, not finding root cause:

- **Provider down?** → Trigger fallback chain (see [provider failover](../runbooks/provider-failover.md))
- **Gateway down?** → Restart service, check deploy logs
- **Config error?** → Revert last config change
- **Rate limited?** → Increase limits temporarily

### 5. Resolve

1. Confirm service is restored
2. Verify with health checks
3. Monitor for 30 minutes for recurrence

### 6. Post-Mortem

Within 48 hours of resolution, write a post-mortem:

## Post-Mortem Template

```markdown
# Incident Post-Mortem: [Title]

**Date:** YYYY-MM-DD
**Duration:** X hours Y minutes
**Severity:** SEV-N
**Author:** Name

## Summary
One-paragraph description of what happened.

## Timeline
- HH:MM — Event description
- HH:MM — Event description

## Root Cause
What caused the incident.

## Impact
What was affected and for how long.

## Resolution
What fixed the issue.

## Lessons Learned
What we learned.

## Action Items
- [ ] Action — Owner — Due date
```

## Communication

| Severity | Internal | External |
|----------|----------|----------|
| SEV-1 | Immediate notification | Status page update within 15 min |
| SEV-2 | Notification within 1 hour | Status page if customer-facing |
| SEV-3 | Daily standup mention | No external communication |
| SEV-4 | Tracked in issue tracker | No external communication |
