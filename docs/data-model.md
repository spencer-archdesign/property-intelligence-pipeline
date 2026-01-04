# Data Model

## Goal

The data model is designed to support:
- deterministic joins across public datasets
- explainable entity resolution
- reproducible read models for downstream workflows

The model separates raw source data from derived layers to preserve traceability and allow rebuilds.

---

## Layers

### 1) Raw Layer

Raw records are stored as close to source as practical.

Characteristics:
- immutable
- versioned by ingestion run
- not edited for correctness

Entities (logical):
- `RawPlutoRecord`
- `RawRegistrationRecord`

Key fields:
- `ingestion_run_id`
- `source_row_id` (if available)
- `bbl` (critical join key)
- raw text fields (entity names, addresses)
- selected numeric attributes (sqft, UnitsRes, etc.)

---

### 2) Normalized Layer

Normalized records represent cleaned, standardized forms of raw inputs.

Characteristics:
- regenerable
- standardizes numeric types and formatting
- retains source references

Entities (logical):
- `CanonicalBuilding`
- `NormalizedEntityName`

Canonical Building (BBL-keyed) includes:
- `bbl` (primary key)
- `bin` (secondary identifier where present)
- `canonical_address` (descriptive)
- `gross_sqft`
- `residential_units`
- `year_built`
- `source_ingestion_run_ids[]`

---

### 3) Resolved Layer (Entity Resolution)

Resolved records link buildings to management entities and preserve the reasoning behind matches.

Characteristics:
- explainable
- confidence-scored
- supports aliasing and manual overrides

Entities (logical):
- `ManagementEntity`
- `EntityAlias`
- `BuildingEntityLink`

**ManagementEntity**
- `entity_id`
- `entity_name_canonical`

**EntityAlias**
- `alias_text_raw`
- `alias_text_normalized`
- `entity_id`

**BuildingEntityLink**
- `bbl`
- `entity_id`
- `entity_role` (e.g., Managing Agent)
- `match_type` (e.g., DIRECT_BBL_JOIN, ALIAS_MATCH, MANUAL_OVERRIDE)
- `confidence_score`
- `match_explanation[]`
- `resolved_at`

---

### 4) Read Models

Read models are derived, query-friendly views used by downstream teams.

Characteristics:
- disposable
- reproducible
- optimized for search and prioritization

Entities (logical):
- `TerritoryTargetList`
- `ManagementEntityProfile`
- `SalesHandoffList`

Examples:
- targets filtered by borough/zip/territory + unit thresholds
- management entity rollups (building count, total units, total sqft)
- opportunity flags (rule-based fields used for prioritization)

---

## Identifiers

- **BBL** is the canonical identifier for buildings across all layers.
- **BIN** is stored as secondary metadata where present.
- `entity_id` is internally generated and stable within the system.

Addresses are treated as descriptive, not authoritative join keys.
