# Ingestion and Refresh

## Overview

Public datasets are imported on a periodic cadence and rebuilt into an internal property intelligence index.

The ingestion strategy prioritizes:
- determinism
- simplicity
- resilience to schema drift

---

## Ingestion Runs

Each import is tagged with an `ingestion_run_id`, such as:
- `pluto-YYYY-MM-DD`
- `reg-YYYY-MM-DD`

The run identifier is stored alongside raw records and propagated to derived layers for traceability.

---

## Strategy: Full Refresh

The system uses a **full refresh** approach:
1. ingest raw PLUTO records
2. ingest raw registration records
3. rebuild normalized canonical buildings
4. rebuild entity resolution links
5. rebuild read models

Rationale:
- avoids incremental complexity early
- allows deterministic rebuilds
- makes schema changes easier to handle

---

## Field Selection

Only selected high-value attributes are imported from PLUTO, typically including:
- BBL, BIN
- address fields (for display)
- `gross_sqft`
- `residential_units` (UnitsRes)
- optional: year built, land use / class signals

Registration contacts import includes:
- BBL
- entity name (raw)
- entity role
- contact coordinates (address/phone) when applicable

---

## Schema Drift Handling

Public datasets may change (renamed columns, added/removed fields).

Guardrails:
- ingestion validates required fields (e.g., BBL)
- optional fields are nullable
- schema changes trigger an ingestion warning and are logged for review

---

## Rebuild Guarantees

A rebuild should be:
- repeatable for a given source snapshot
- traceable to ingestion runs
- explainable via match explanations and confidence

---

## Operational Notes

This subsystem intentionally does not prescribe implementation tooling.
The key requirement is that ingestion runs are:
- identifiable
- repeatable
- observable (basic logging and counts)
