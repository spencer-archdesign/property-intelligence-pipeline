# ADR-0001: BBL as Primary Identifier

## Status
Accepted

## Context

Building data is sourced from multiple public datasets with inconsistent addressing and naming conventions.

Reliable joins require a stable, canonical identifier.

---

## Decision

The system uses **BBL (Borough–Block–Lot)** as the primary identifier for buildings.

- BIN is stored where available
- Addresses are treated as descriptive, not authoritative

---

## Consequences

### Benefits
- deterministic joins across datasets
- avoidance of fuzzy address matching
- long-term identifier stability

### Tradeoffs
- records without BBLs require exclusion or manual handling
