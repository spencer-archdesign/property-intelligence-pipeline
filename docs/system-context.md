# System Context

## Overview

The Property Intelligence Pipeline transforms public building datasets into an internal **property intelligence index** that supports repeatable portfolio targeting.

This system is intentionally **not** a CRM and does not own outreach workflows. It produces read-only views and exports that downstream teams (e.g., sales) can consume.

---

## Actors

- **Operations / Admin**
  - configures ingestion runs
  - monitors data quality
  - maintains entity alias rules and overrides

- **Sales / Business Development**
  - consumes targeting lists
  - searches management entities and portfolios
  - uses exported lists to drive outreach

- **System Owner**
  - maintains the pipeline logic and internal schema
  - manages refresh cadence and schema drift

---

## External Systems and Data Sources

- **Public Data Sources**
  - PLUTO (selected building and portfolio-relevant attributes)
  - Registration contact datasets (management / ownership linkage signals)

These sources are treated as:
- imperfect
- periodically refreshed
- subject to schema drift

---

## Internal System Boundary

The system boundary includes:

- **Ingestion Layer**
  - imports raw records into internal storage
  - tags each run with an ingestion run identifier

- **Normalization Layer**
  - standardizes identifiers and selected attributes
  - normalizes entity names into canonical and alias forms

- **Entity Resolution Layer**
  - links buildings to entities using deterministic joins (BBL) and alias rules
  - records confidence and match explanations

- **Read Models**
  - produces sales-facing lists and rollups
  - provides query-friendly views without exposing raw complexity

---

## Outputs

Primary outputs are read-only:
- **Territory Target Lists** (by geography + size filters)
- **Management Entity Profiles** (portfolio rollups and distributions)
- **Sales Handoff Lists** (curated targets with rationale)

Outputs may be delivered as:
- internal views (search and filters)
- exports (CSV / PDF / list reports)

---

## Non-Goals

This subsystem does not:
- send emails or automate outreach
- serve as a system of record for legal ownership
- replace CRM functionality
- provide guarantees about completeness or correctness of public data
