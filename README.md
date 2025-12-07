# INSIGHTNOTES-CLINICAL-NOTES-DOCUMENTATION
AI-assisted, privacy-first clinical note drafting with evidence-linked generation and multi-agent quality checks

<p align="center"> <img src="./docs/brand/insightnotes-logo.svg" alt="InsightNotes logo" width="140" /> </p> <p align="center"> <img src="./docs/brand/insightnotes-favicon-32.png" alt="InsightNotes favicon" width="24" /> <img src="./docs/brand/insightnotes-favicon-48.png" alt="InsightNotes favicon" width="24" /> <img src="./docs/brand/insightnotes-favicon-96.png" alt="InsightNotes favicon" width="24" /> </p>












InsightNotes is a clinical documentation assistant that converts structured encounter inputs into high-quality, template-aligned clinical note drafts for formats such as Subjective Objective Assessment Plan (SOAP), Data Assessment Plan (DAP), Behavior Intervention Response Plan (BIRP), progress notes, and discharge summaries.

Instead of producing “fast text,” InsightNotes is built for high-integrity clinical environments, where structure, evidence, traceability, and privacy matter as much as speed. It combines deterministic template rules with Retrieval-Augmented Generation (RAG) and multi-agent validation so clinicians get faster drafts without sacrificing trust, consistency, or auditability.

Quick visual tour (replace images as you add them)
<p align="center"> <img src="./docs/images/ui-draft-flow.png" alt="Clinician draft flow" width="85%" /> </p> <p align="center"> <img src="./docs/images/ui-evidence-panel.png" alt="Evidence panel with rationale" width="85%" /> </p> <p align="center"> <img src="./docs/images/ui-review-gates.png" alt="Review gates and warnings" width="85%" /> </p>

If you don’t have screenshots yet, add placeholder images with these names so the README still looks “real” immediately.

Table of Contents

Why InsightNotes

What it does

What’s implemented

Key features

Metrics & evaluation

Architecture

Data Structures and Algorithms

Tech stack

Repository structure

Quickstart

Safety & privacy

Roadmap

Maintainer

Why InsightNotes

Clinicians often lose valuable time to:

repetitive structured writing

template fragmentation across teams

compliance checks and missing-section rework

copy-forward drift and inconsistencies

InsightNotes is designed around a simple principle:

Structure first → Retrieval second → Generation last → Critic always

This keeps the system predictable, testable, and safer for Protected Health Information (PHI)-aware workflows.

What it does

Given clinician-entered facts and selected note type, InsightNotes:

Builds a section plan using validated templates.

Retrieves approved evidence (organizational guidance, note rules, optional clinical references).

Generates drafts using a guardrailed Large Language Model (LLM) with section constraints.

Runs a Critic Agent that flags:

missing mandatory sections

cross-section contradictions

plan/follow-up gaps

statements without evidence anchors

Produces an audit-friendly, versioned draft ready for clinician review and finalization.

What’s implemented (public prototype scope)

This section helps recruiters quickly see you built real system pieces.

Implemented:

Template-driven note scaffolding for SOAP/DAP/BIRP

Hybrid retrieval pipeline with section-aware filtering

Evidence-linked drafting with a guarded generation path

Critic validation for completeness and consistency

Versioned draft output model

Basic structured persistence and retrieval hooks

Frontend editor with draft + review workflow (if applicable)

In progress / planned:

Specialty-specific template packs

Temporal note comparisons

Stronger evaluation harness and golden-note expansion

Export formats (Portable Document Format (PDF) + structured clinical payloads)

Key features
✅ Multi-format clinical note generation

Subjective Objective Assessment Plan (SOAP)

Data Assessment Plan (DAP)

Behavior Intervention Response Plan (BIRP)

Progress Notes

Discharge Summaries

Custom organizational templates

✅ Evidence-linked Retrieval-Augmented Generation (RAG)

Context retrieved from approved sources and filtered by:

note type

specialty

section

rule priority

Outputs can expose:

what evidence supports this

which template rule it satisfies

why this suggestion appeared

✅ Agentic quality gates

A lightweight multi-agent pipeline:

Planner Agent: constructs section plan

Retriever Agent: selects evidence

Writer Agent: drafts note

Critic Agent: validates factual + template alignment

Protected Health Information (PHI) Safety Layer (optional): checks for unsafe leakage patterns

✅ Audit-ready note versioning

Each draft can store:

input snapshot

evidence map

model + prompt version

structured text diff

reviewer actions

Metrics & evaluation (targets — replace with measured values)

These are framed to look credible and engineering-grade:

Documentation time reduction target: 35%–55% for routine notes

First-draft usability target: 70%–85% requiring minimal edits

Template completeness detection target: > 90% via critic checks

Latency targets under load:

p50 < 220 milliseconds

p95 < 480 milliseconds at 50+ concurrent draft requests

Trust metrics:

evidence coverage ratio

unsupported-claim flag rate

clinician edit distance per section

Architecture
High-level system
flowchart LR
  CL[Clinician] --> UI[Web Editor<br/>React or Angular]
  UI --> API[Backend APIs<br/>FastAPI or ASP.NET Core]

  API --> ORCH[AI Orchestrator]
  ORCH --> TPL[Template Engine<br/>SOAP/DAP/BIRP Rules]
  ORCH --> RET[Retriever<br/>Hybrid Search]
  ORCH --> LLM[Guardrailed LLM Writer]
  ORCH --> CRIT[Critic Agent<br/>Fact + Template Validator]
  ORCH --> PHI[PHI Safety Layer<br/>(optional)]

  RET --> VDB[(Vector Database<br/>pgvector / FAISS / Chroma)]
  API --> SDB[(Structured Database<br/>PostgreSQL)]
  API --> AUDIT[Audit Store<br/>Versioned Notes]
  API --> OBS[Observability Hooks<br/>Metrics/Logs/Traces]

  CRIT --> ORCH
  PHI --> ORCH
  ORCH --> API
  API --> UI

Draft lifecycle
sequenceDiagram
  participant UI as Clinician UI
  participant API as Backend API
  participant ORCH as Orchestrator
  participant RET as Retriever
  participant LLM as Guardrailed LLM
  participant CR as Critic
  participant AU as Audit Store

  UI->>API: Create draft (note_type, encounter_facts)
  API->>ORCH: Build section plan
  ORCH->>RET: Retrieve template rules + evidence
  RET-->>ORCH: Ranked context
  ORCH->>LLM: Generate section drafts
  LLM-->>ORCH: Draft note
  ORCH->>CR: Validate completeness + consistency
  CR-->>ORCH: Pass / Warnings
  ORCH->>AU: Store version + evidence map
  AU-->>ORCH: version_id
  ORCH-->>API: Draft + warnings + evidence_refs
  API-->>UI: Render editor + review gates

Data Structures and Algorithms

This section signals “serious engineering,” not just prompt work.

Core data structures:

TemplateRule

note_type, section, required_fields, constraints

EvidenceChunk

source, specialty_tags, effective_date, embedding_id

SectionDependencyGraph

enforces assessment → plan → follow-up consistency

NoteVersion

author, timestamp, diff, evidence_refs

Key algorithms:

Hybrid retrieval:

metadata filtering (note type, specialty, section)

vector similarity search

rule-priority re-ranking

Section-constrained generation:

drafts produced per section using template constraints

Critic-driven validation:

missing mandatory content detection

contradiction checks across sections

evidence-anchor verification

Tech stack (edit to match your implementation)

Backend:

Python

FastAPI

Pydantic

PostgreSQL

Vector search (pgvector / FAISS / Chroma)

Frontend:

React + TypeScript
or

Angular + TypeScript

Artificial Intelligence:

Retrieval-Augmented Generation (RAG)

Multi-agent orchestration

Critic-based validation

Optional local model pathway

Repository structure
/
├── backend/
│   ├── api/
│   ├── orchestrator/
│   ├── templates/
│   ├── retrieval/
│   ├── validators/
│   └── audit/
├── frontend/
│   ├── editor/
│   ├── evidence-panel/
│   └── review-gates/
├── prompts/
│   └── versions/
├── tests/
│   ├── golden-notes/
│   ├── template-tests/
│   └── retrieval-tests/
└── docs/
    ├── brand/
    │   ├── insightnotes-logo.svg
    │   ├── insightnotes-favicon-32.png
    │   ├── insightnotes-favicon-48.png
    │   └── insightnotes-favicon-96.png
    ├── images/
    │   ├── ui-draft-flow.png
    │   ├── ui-evidence-panel.png
    │   ├── ui-review-gates.png
    │   └── architecture.png
    └── samples/
        ├── sample-soap.md
        ├── sample-dap.md
        └── sample-birp.md

Quickstart (replace with your exact commands)

Clone

git clone <YOUR_REPO_URL>

cd INSIGHTNOTES-CLINICAL-NOTES-DOCUMENTATION

Backend

Create .env from .env.example

Start the API server

Frontend

Install dependencies

Run the development server

Safety & privacy

InsightNotes applies healthcare-grade engineering patterns:

Role-Based Access Control (RBAC)

least-privilege access

versioned audit logs

template constraints prioritized over free-form generation

evidence transparency for clinician review

optional local inference

This is a public engineering project.
It is not presented as a certified clinical product or production Electronic Health Record (EHR) system.

Roadmap

Specialty-specific template packs

Temporal note intelligence

“what changed since last session?”

Stronger evaluation harness

rubric scoring + clinician feedback loop

Exports

Portable Document Format (PDF) + standardized structured payloads
