# NYR Project Zero — Logical Database Schema

## 1. Purpose

This document defines the logical data model for the NYR opportunity research system.

It is technology-independent. It does not prescribe a database engine, programming language, or deployment architecture.

## 2. Core Entities

### 2.1 Divisions

Stores the 12 NYR research divisions.

Fields:
- division_id (Primary Key)
- name
- research_scope
- research_questions
- priority
- status

### 2.2 Sources

Stores research source information and provenance.

Fields:
- source_id (Primary Key)
- source_name
- source_type
- canonical_url
- publisher
- published_at
- accessed_at
- author_or_owner
- license_or_terms
- reliability_rating
- archival_reference
- source_class
- access_method
- authentication_required
- access_cost
- rate_limit
- license_status
- commercial_use
- collection_policy
- last_checked
- source_status

### 2.3 Signals

Stores initial observations that may indicate problems or opportunities.

Fields:
- signal_id (Primary Key)
- title
- description
- division_id (Foreign Key)
- source_id (Foreign Key)
- geography
- industry
- signal_type
- observed_at
- collected_at
- status

### 2.4 Claims

Stores individual factual or analytical claims and their evidence.

Fields:
- claim_id (Primary Key)
- statement
- claim_type
- source_id (Foreign Key)
- supporting_or_opposing
- evidence_strength
- verification_status
- notes

### 2.5 Problems

Stores structured descriptions of identified problems.

Fields:
- problem_id (Primary Key)
- title
- problem_statement
- affected_users
- root_cause
- current_workaround
- frequency
- severity
- geography
- division_id (Foreign Key)
- confidence_level
- status

### 2.6 Opportunities

Stores potential solutions and commercial opportunities.

Fields:
- opportunity_id (Primary Key)
- title
- proposed_solution
- problem_id (Foreign Key)
- differentiation
- target_customer
- value_proposition
- business_model
- estimated_cost
- market_hypothesis
- novelty_classification
- opportunity_score
- confidence_level
- status

### 2.7 Competitors

Stores existing products, organizations, alternatives, and competing solutions.

Fields:
- competitor_id (Primary Key)
- name
- website
- solution_description
- pricing_model
- strengths
- limitations
- evidence_source

### 2.8 Experiments

Stores validation experiments associated with opportunities.

Fields:
- experiment_id (Primary Key)
- opportunity_id (Foreign Key)
- hypothesis
- methodology
- success_criteria
- budget_limit
- results
- outcome
- lessons_learned

### 2.9 Decisions

Stores formal research and opportunity decisions.

Fields:
- decision_id (Primary Key)
- opportunity_id (Foreign Key)
- decision
- rationale
- evidence_snapshot
- decided_at
- reviewer

## 3. Relationship Entities

The following relationships support many-to-many connections.

### Evidence Links
Connects claims to one or more sources.

### Problem Claims
Connects problems to supporting or opposing claims.

### Opportunity Competitors
Connects opportunities to relevant competitors and alternatives.

### Opportunity Claims
Connects opportunities to supporting or opposing claims.

### Experiment Observations
Optional relationship for recording individual measurements and observations within experiments.

### Problem Signals
Connects multiple signals to the same underlying problem.

## 4. Identifier Convention

Use stable, human-readable identifiers:

- Divisions: R01 through R12
- Signals: SIG-000001
- Sources: SRC-000001
- Claims: CLM-000001
- Problems: PRB-000001
- Opportunities: OPP-000001
- Competitors: CMP-000001
- Experiments: EXP-000001
- Decisions: DEC-000001

Identifiers must never be silently reused after a record is retired.

## 5. Data Integrity Rules

1. Every signal must have at least one traceable source.
2. Every opportunity must reference at least one documented problem.
3. Important claims must have supporting or opposing evidence.
4. Every experiment must have a falsifiable hypothesis.
5. Every decision must contain a written rationale.
6. Preserve original source URLs and access dates.
7. Link duplicate signals rather than silently deleting them.
8. Preserve historical opportunity scores and their evaluation dates.
9. Record uncertainty and contradictory evidence.
10. Never treat a research score as proof of commercial viability.

## 6. Design Philosophy

- Preserve provenance.
- Separate observations from interpretations.
- Support many-to-many evidence relationships.
- Maintain historical traceability.
- Allow future expansion.
- Avoid technology-specific dependencies.

## 7. Implementation Status

This is a logical design only.

Database engine selection, physical schema, indexes, APIs, migrations, and deployment architecture are intentionally deferred until justified by actual research operations.
