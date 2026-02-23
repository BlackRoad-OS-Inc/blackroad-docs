# RFC Template — BlackRoad OS, Inc.

## RFC-XXXX: [Title]

| Field | Value |
|-------|-------|
| RFC Number | XXXX |
| Status | Draft / Review / Accepted / Rejected / Superseded |
| Author | [Name] |
| Created | YYYY-MM-DD |
| Last Updated | YYYY-MM-DD |
| Supersedes | N/A |
| Related | RFC-YYYY, Issue #NNN |

---

## Abstract

*One paragraph summary of what this RFC proposes and why.*

---

## Motivation

### Problem Statement

*What problem does this solve? Why is the status quo insufficient?*

### Goals

- [ ] Goal 1
- [ ] Goal 2

### Non-Goals

- Not trying to solve X
- Not changing Y

---

## Proposal

### Overview

*High-level description of the proposed solution.*

### Technical Details

#### Architecture

```
[Diagram or pseudocode here]
```

#### API / Interface Changes

```typescript
// New or changed interfaces
interface ExampleChange {
  field: string;
}
```

#### Data Model Changes

```sql
-- New tables or columns
ALTER TABLE agents ADD COLUMN new_field TEXT;
```

### Migration Path

*How do we get from current state to proposed state?*
*Is this backwards compatible?*

---

## Drawbacks

*What are the costs of this approach? Trade-offs?*

---

## Alternatives Considered

### Alternative A: [Name]
- **Pros**: ...
- **Cons**: ...
- **Why rejected**: ...

### Alternative B: [Name]
- **Pros**: ...
- **Cons**: ...
- **Why rejected**: ...

---

## Security Considerations

*Does this affect the threat model? Authentication? Data privacy?*

---

## Open Questions

- [ ] Question 1 — assigned to @person
- [ ] Question 2 — needs stakeholder input

---

## Decision Record

| Date | Decision | Rationale |
|------|----------|-----------|
| YYYY-MM-DD | Accepted | ... |

---

## References

- [Link 1](url)
- [Link 2](url)
