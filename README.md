# Property Intelligence Pipeline

> *External data ingestion and entity-resolution pipeline for portfolio targeting*

## Purpose

This repository documents a **data ingestion and enrichment subsystem** designed to transform public building datasets into an internal, queryable **property intelligence index**.

The system enables:
- systematic discovery of building portfolios
- identification of repeat ownership and management entities
- geographic and size-based targeting
- defensible handoff lists for sales and business development teams

Rather than functioning as a CRM or outreach tool, this subsystem focuses on **data normalization, entity resolution, and read models** that downstream workflows can reliably consume.

---

## Problem Statement

Sales targeting often relies on:
- personal knowledge
- ad hoc searches
- incomplete or inconsistent datasets
- tribal knowledge of “who owns what”

In dense urban markets, this approach:
- does not scale
- misses portfolio-level opportunities
- makes territory planning opaque
- prevents systematic prioritization

Public datasets (e.g., PLUTO and registration contacts) contain the necessary signals, but:
- schemas differ
- naming is inconsistent
- ownership and management entities are fragmented
- raw data is difficult to query operationally

---

## System Overview

The Property Intelligence Pipeline is a **three-stage system**:

1. **Ingest**
   - Import public datasets (e.g., PLUTO, registration contacts)
   - Preserve raw source records

2. **Normalize & Resolve**
   - Normalize identifiers and names
   - Resolve buildings to management entities
   - Track match confidence and reasoning

3. **Serve Read Models**
   - Generate sales-facing views
   - Enable geographic and portfolio-based targeting
   - Support repeatable handoff to downstream teams

Sales workflows are **consumers**, not owners, of this system.

---

## Data Sources

This system ingests selected fields from:
- PLUTO (BBL, BIN, square footage, residential units, etc.)
- Registration contact datasets (entity names, building associations)

Only fields with durable business value are retained.

Sources are treated as:
- imperfect
- periodically refreshed
- subject to schema drift

---

## Architectural Principles

- **BBL is the canonical building identifier**
- Raw source data is immutable
- Normalized and resolved data is regenerable
- Entity resolution is explainable, not opaque
- Sales-facing outputs are read models, not source-of-truth records

---

## Data Model Philosophy

The system separates data into layers:

- **Raw**
  - direct source records
  - immutable
  - versioned by ingestion run

- **Normalized**
  - standardized names and identifiers
  - cleaned numeric fields

- **Resolved**
  - buildings linked to management entities
  - alias-aware
  - confidence-scored

- **Read Models**
  - territory lists
  - management entity profiles
  - portfolio rollups

---

## Outputs

Primary outputs include:
- Management Entity Profiles (normalized entity + building rollups)
- Territory Target Lists (filtered by geography and size)
- Sales Handoff Lists (curated targets with rationale)

All outputs are derived and reproducible.

---

## Repository Structure

```text
/
├── README.md
├── decision-records/
│   ├── ADR-0001-bbl-as-primary-identifier.md
│   ├── ADR-0002-ingestion-update-strategy.md
│   ├── ADR-0003-entity-resolution-and-normalization.md
│   ├── ADR-0004-read-models-for-sales-handoff.md
│   └── ADR-0005-data-quality-and-confidence.md
├── docs/
│   ├── system-context.md
│   ├── data-model.md
│   ├── ingestion-and-refresh.md
│   └── failure-modes.md
├── examples/
│   ├── sample-pluto-record.json
│   ├── sample-registration-contact-record.json
│   ├── sample-normalized-building.json
│   ├── sample-management-entity-profile.json
│   ├── sample-sales-handoff-list.json
│   └── sample-match-explanation.json
└── diagrams/
    ├── system-context.mmd
    ├── data-flow.mmd
    └── entity-resolution.mmd
```
---

# Status

This repository documents a production-proven architectural pattern implemented within an existing FileMaker-based platform.

Implementation details evolve independently.

---

# Non-Goals
   -	CRM replacement
   -	Outreach automation
   -	Ownership adjudication
   -	Marketing analytics
