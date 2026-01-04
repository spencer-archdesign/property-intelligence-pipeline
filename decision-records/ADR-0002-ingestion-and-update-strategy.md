# ADR-0002: Ingestion and Update Strategy

## Status
Accepted

## Context

Public datasets are updated periodically and may change structure over time.

Incremental ingestion increases complexity early.

---

## Decision

The system uses **full refresh ingestion**:
- all raw records are reloaded per ingestion run
- normalized and resolved layers are rebuilt

---

## Consequences

### Benefits
- simpler reasoning
- deterministic rebuilds
- easier schema change handling

### Tradeoffs
- increased ingestion cost
- higher compute and storage usage
