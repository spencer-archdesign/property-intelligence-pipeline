# ADR-0004: Read Models for Sales Handoff

## Status
Accepted

## Context

Sales workflows require:
- clarity
- prioritization
- justification

Raw or canonical data is not sales-friendly.

---

## Decision

The system produces **read-only sales models**, including:
- territory-based building lists
- management entity portfolios
- opportunity flags and notes

Read models are derived and disposable.

---

## Consequences

### Benefits
- clean handoff
- reduced misinterpretation
- separation of concerns

### Tradeoffs
- additional rebuild logic
