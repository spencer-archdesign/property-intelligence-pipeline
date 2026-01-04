# ADR-0003: Entity Resolution and Name Normalization

## Status
Accepted

## Context

Management and ownership entities appear under many name variants:
- punctuation differences
- abbreviations
- LLC/INC variations

Naive string matching produces fragmentation.

---

## Decision

The system:
- stores original entity names
- generates normalized names
- maintains an alias table
- links buildings to entities with match reasoning

Resolution is **explainable**, not opaque.

---

## Consequences

### Benefits
- portfolio aggregation accuracy
- auditable matches
- controlled manual intervention

### Tradeoffs
- additional data structures
- resolution logic complexity
