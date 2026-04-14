# OpenInvestigator Architecture

## Purpose

OpenInvestigator is a local-first analysis system for complex case material.

It turns raw documents into structured, source-grounded artifacts and supports multiple investigative runs on the same case data.

**Core idea:**  
**Prepare data once. Investigate many times. Compare runs deliberately.**

## System model

OpenInvestigator separates three layers:

### 1. Case data
Persistent artifacts derived from source material.

Examples:
- documents
- chunks
- entities
- events
- claims
- time references
- provenance links

### 2. Investigation runs
Run-specific interpretations built on the same case data.

A run can produce:
- timelines
- contradictions
- hypotheses
- reports

Each run uses its own profile and configuration snapshot.

### 3. Run comparison
Runs of the same case can be compared to reveal:
- stable findings
- changed assumptions
- divergent interpretations
- sensitive claims

This is a core product feature, not an add-on.

## MVP scope

The MVP should be able to:

1. create a case
2. ingest a small set of documents
3. extract text and stable chunks
4. derive structured artifacts:
   - entities
   - events
   - claims
   - time references
5. preserve provenance
6. execute investigation profiles against the same case
7. generate contradictions, hypotheses, and reports
8. compare multiple runs of the same case
9. reproduce runs from stored inputs and configuration

## Non-goals

The MVP is not:

- a generic chat-with-documents tool
- a free-form agent framework
- a full probabilistic reasoning engine
- a multi-tenant enterprise platform
- a UI-first product
- an OCR product by itself

## Architecture principles

### Local-first by default
The system should work locally without requiring hosted infrastructure.

### Parser-first, LLM-assisted
Use deterministic processing where possible. Use LLMs where structured interpretation adds real value.

### Structured artifacts over prose
The system should emit typed artifacts, not just summaries.

### Provenance is mandatory
Every important output must be traceable to source material.

### Stable case data, variable runs
Persistent case knowledge and run-specific interpretation must remain separate.

### Reproducibility is a feature
Runs must be recreatable from stored inputs, configuration, and model metadata.

## High-level flow

```text
[ Ingest documents ]
        |
        v
[ Parse / OCR / Extract text ]
        |
        v
[ Chunk and normalize ]
        |
        v
[ Extract structured case data ]
  -> entities
  -> events
  -> claims
  -> time references
  -> provenance
        |
        v
[ Store stable case data ]
        |
        v
[ Run investigation profile ]
  -> timeline
  -> contradictions
  -> hypotheses
  -> report
        |
        v
[ Compare runs ]
```

## Core domain concepts

* **Case** — top-level container for one investigation
* **SourceDocument** — original imported file
* **Chunk** — stable text unit derived from a document
* **SourceAnchor** — canonical provenance reference
* **Entity** — person, organization, place, object, account, or identifier
* **Event** — something that happened or is alleged to have happened
* **Claim** — source-grounded assertion
* **AnalysisRun** — reproducible run with profile and config snapshot
* **Contradiction** — structured conflict between artifacts inside a run
* **Hypothesis** — source-grounded explanatory scenario
* **ReportArtifact** — exported run output

## Critical data boundary

The most important schema rule is:

* **case-level data = persistent extracted artifacts**
* **run-level data = interpretations built from those artifacts**

This separation is essential for trust, comparison, and reproducibility.

## Provenance model

Every important artifact should be traceable through this chain:

**Hypothesis -> Claim/Event -> Chunk -> SourceDocument**

Outputs without source grounding should not be treated as trustworthy.

## Investigation runs

A run applies one profile to the same stable case data.

Example profile styles:

* conservative
* skeptic
* exploratory
* timeline-focused

A profile may influence:

* source weighting
* contradiction sensitivity
* hypothesis style
* reasoning strictness

Each run should be:

* traceable
* reproducible
* comparable

## Reporting

MVP outputs:

* JSON
* Markdown

A report should include:

* case overview
* source inventory
* entities
* timeline
* claims
* contradictions
* hypotheses
* run metadata

Comparison reports should highlight:

* stable findings
* changed hypotheses
* differing assumptions
* profile-driven differences

## Technical baseline

Recommended stack:

* Python 3.12+
* FastAPI
* Typer
* Pydantic v2
* SQLAlchemy 2.x
* PostgreSQL
* Alembic
* pytest

## Security baseline

Minimum controls:

* file type and size validation
* safe file handling
* schema validation of model outputs
* redaction-safe logging
* explicit local-only vs cloud-enabled mode

Treat document content as untrusted input.

## Summary

OpenInvestigator is built around five non-negotiables:

* grounded extraction
* first-class provenance
* stable case data
* reproducible investigation runs
* comparable perspectives over the same evidence

This is not a system for generating one answer.

It is a system for producing structured, source-grounded, and comparable ways of thinking about the same case.
