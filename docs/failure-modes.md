# Failure Modes and Guardrails

## Goal

Public building datasets are useful but imperfect. This system is designed to:
- avoid false certainty
- preserve traceability
- support correction without corrupting raw inputs

---

## Data Quality Failure Modes

### Missing or Invalid BBL
- Some records may be missing BBL or contain malformed identifiers.
- These records cannot be deterministically joined.

Mitigation:
- exclude from canonical building creation, or
- route to an exceptions queue / manual review list

---

### Conflicting Entity Records
A building may have:
- multiple listed managing entities
- conflicting roles across sources

Mitigation:
- store multiple links with roles
- rank links by confidence and recency
- allow manual overrides in derived layers

---

### Name Fragmentation
The same entity may appear as:
- punctuation variants
- abbreviations
- LLC/INC differences

Mitigation:
- normalized canonical names
- alias table
- resolution explanations and confidence scoring

---

### Stale Outputs After Refresh
Refreshes can cause:
- entity merges/splits due to alias changes
- building counts shifting

Mitigation:
- treat read models as disposable
- regenerate handoff lists per ingestion run
- include ingestion run metadata on exports

---

## Resolution Failure Modes

### Over-Merging (False Positives)
Two distinct entities may normalize to a similar name.

Mitigation:
- require alias confirmation before merging
- preserve manual override capability
- keep match explanations and confidence

---

### Under-Merging (False Negatives)
One entity may remain fragmented across multiple normalized variants.

Mitigation:
- maintain alias expansion process
- identify “near duplicate” entity candidates for review
- track manual consolidation decisions as derived data

---

## Guardrails

- raw records are immutable
- derived layers are rebuildable
- entity resolution is explainable
- confidence is recorded and visible
- manual overrides do not overwrite sources

---

## What This System Does Not Guarantee

- legal ownership correctness
- completeness of public datasets
- real-time updates

This is a targeting index, not a legal registry.
