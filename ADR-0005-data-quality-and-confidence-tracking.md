# ADR-0005: Data Quality and Confidence Tracking

## Status
Accepted

## Context

Public datasets contain errors, omissions, and conflicts.

Pretending otherwise creates false certainty.

---

## Decision

The system:
- tracks match confidence
- stores match explanations
- allows manual overrides as a derived layer

Raw data is never overwritten.

---

## Consequences

### Benefits
- honest representations
- defensible targeting
- easier correction

### Tradeoffs
- increased model complexity
