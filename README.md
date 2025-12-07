# InsightNotes — Clinical Notes Documentation  
**Artificial Intelligence-assisted, privacy-first clinical note drafting with evidence-linked Retrieval-Augmented Generation (RAG) and multi-agent quality checks**

<p align="center">
  <img src="./docs/brand/insightnotes-logo.svg" alt="InsightNotes logo" width="140" />
</p>

<p align="center">
  <img src="./docs/brand/insightnotes-favicon-32.png" alt="InsightNotes favicon" width="18" />
  <img src="./docs/brand/insightnotes-favicon-48.png" alt="InsightNotes favicon" width="18" />
  <img src="./docs/brand/insightnotes-favicon-96.png" alt="InsightNotes favicon" width="18" />
</p>

![License](https://img.shields.io/badge/License-MIT-success)
![Status](https://img.shields.io/badge/Status-Public%20Prototype-blue)
![Backend](https://img.shields.io/badge/Backend-FastAPI%20%7C%20ASP.NET%20Core-009688)
![Frontend](https://img.shields.io/badge/Frontend-React%20%7C%20Angular-3178C6)
![AI](https://img.shields.io/badge/AI-RAG%20%7C%20Agentic%20Quality-blueviolet)
![Privacy](https://img.shields.io/badge/Privacy-Protected%20Health%20Information%20(PHI)%20Aware-critical)

InsightNotes is a **clinical documentation assistant** engineered to transform structured encounter inputs into **high-quality, template-aligned clinical note drafts** for formats such as **Subjective Objective Assessment Plan (SOAP)**, **Data Assessment Plan (DAP)**, **Behavior Intervention Response Plan (BIRP)**, progress notes, and discharge summaries.

Unlike generic note generators optimized purely for speed, InsightNotes is designed for **high-integrity clinical workflows** where **structure, evidence grounding, traceability, and privacy** matter as much as rapid drafting. The system combines:

- **Deterministic template rules** (truth layer)
- **Evidence-linked Retrieval-Augmented Generation (RAG)** (trust layer)
- **Multi-agent validation** (quality and safety layer)
- **Audit-ready versioning** (compliance and reproducibility layer)

**Core philosophy**  
**Structure first → Retrieval second → Generation last → Critic always**

---

## Table of Contents
- Overview
- Why InsightNotes
- What It Does
- What’s Implemented
- Key Features
- Impact and Metrics
- Architecture
- Draft Lifecycle
- Data Structures and Algorithms
- Technology Stack
- Repository Structure
- Quickstart
- Safety and Privacy
- Roadmap
- Maintainer

---

## Overview

Clinical documentation often consumes significant clinician time and introduces risk through:
- inconsistent template coverage  
- copy-forward drift  
- missing sections  
- subtle cross-section contradictions  
- low visibility into the rationale behind generated statements  

InsightNotes addresses these issues by using **template-governed generation** with **evidence traceability** and **critic-driven validation** to ensure drafts are both fast and clinically structured.

---

## Why InsightNotes

Documentation systems in healthcare must balance two constraints:

1. **Time pressure**
2. **Clinical integrity**

InsightNotes is intentionally built to *constrain* generative flexibility with deterministic structure to support:
- safer drafting in Protected Health Information (PHI)-aware settings  
- consistent section completeness  
- auditable, explainable clinical text  
- testable quality gates  

---

## What It Does

Given clinician-entered encounter facts and a selected note type, InsightNotes:

1. Constructs a **section plan** using validated templates.
2. Performs **hybrid retrieval** (metadata filtering + vector similarity + rule-priority re-ranking) over approved sources.
3. Generates **section-by-section drafts** using a guardrailed **Large Language Model (LLM)** aligned to template constraints.
4. Applies a **Critic Agent** to flag:
   - missing mandatory sections  
   - cross-section contradictions  
   - assessment → plan → follow-up gaps  
   - statements without evidence anchors  
5. Produces a **versioned, clinician-reviewable draft** with warnings and evidence references.

---

## What’s Implemented (Public Prototype Scope)

### Implemented
- Template-driven scaffolding for **SOAP**, **DAP**, and **BIRP**
- Section-aware hybrid retrieval pipeline
- Evidence-linked drafting with constrained prompts
- Critic validation for completeness and cross-section consistency
- Versioned draft output model with audit hooks
- Foundational frontend workflow (Draft → Review → Finalize) where applicable

### In Progress
- Specialty-specific template packs
- Expanded golden-note evaluation suite
- Temporal note comparison signals

---

## Key Features

### ✅ Multi-format note generation
- Subjective Objective Assessment Plan (SOAP)  
- Data Assessment Plan (DAP)  
- Behavior Intervention Response Plan (BIRP)  
- Progress Notes  
- Discharge Summaries  
- Custom organizational templates  

### ✅ Evidence-linked Retrieval-Augmented Generation (RAG)
- Retrieval filtered by:
  - note type  
  - specialty  
  - section  
  - rule priority  
- Outputs designed to support:
  - evidence visibility  
  - template rule coverage  
  - explainable suggestions  

### ✅ Agentic quality gates
- **Planner Agent** — constructs section plan  
- **Retriever Agent** — selects evidence  
- **Writer Agent** — drafts structured text  
- **Critic Agent** — validates factual and template alignment  
- **Protected Health Information (PHI) Safety Layer** (optional) — checks for unsafe leakage patterns  

### ✅ Audit-ready note versioning
Each draft can record:
- input snapshot  
- evidence map  
- model and prompt version  
- structured text diff  
- reviewer actions  

---

## Impact and Metrics (Targets)

- **Documentation time reduction target:** 35%–55% for routine notes  
- **First-draft usability target:** 70%–85% requiring minimal edits  
- **Template completeness detection target:** > 90% via critic checks  
- **Latency targets under load:**  
  - p50 < 220 milliseconds  
  - p95 < 480 milliseconds at 50+ concurrent draft requests  
- **Trust metrics:**  
  - evidence coverage ratio  
  - unsupported-claim flag rate  
  - clinician edit distance per section  

---

## Architecture

```mermaid
flowchart LR
  CL[Clinician] --> UI[Web Editor<br/>React or Angular]
  UI --> API[Backend APIs<br/>FastAPI or ASP.NET Core]

  API --> ORCH[Artificial Intelligence Orchestrator]
  ORCH --> TPL[Template Engine<br/>SOAP/DAP/BIRP Rules]
  ORCH --> RET[Retriever<br/>Hybrid Search]
  ORCH --> LLM[Guardrailed LLM Writer]
  ORCH --> CRIT[Critic Agent<br/>Fact + Template Validator]
  ORCH --> PHI[PHI Safety Layer<br/>(optional)]

  RET --> VDB[(Vector Database<br/>pgvector / FAISS / Chroma)]
  API --> SDB[(Structured Database<br/>PostgreSQL)]
  API --> AUDIT[Audit Store<br/>Versioned Notes]
  API --> OBS[Observability Hooks<br/>Metrics / Logs / Traces]

  CRIT --> ORCH
  PHI --> ORCH
  ORCH --> API
  API --> UI
## Draft Lifecycle
mermaid
Copy code
sequenceDiagram
  participant UI as Clinician User Interface
  participant API as Backend API
  participant ORCH as Orchestrator
  participant RET as Retriever
  participant LLM as Guardrailed Large Language Model
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
## Data Structures and Algorithms
InsightNotes intentionally constrains generative flexibility using deterministic structure to improve reliability.

Core data structures
TemplateRule

note_type, section, required_fields, constraints

EvidenceChunk

source, specialty_tags, effective_date, embedding_id

SectionDependencyGraph

enforces assessment → plan → follow-up consistency

NoteVersion

author, timestamp, diff, evidence_refs

Key algorithms
Hybrid retrieval

metadata filtering by note type, specialty, and section

vector similarity search

rule-priority and section-relevance re-ranking

Section-constrained generation

per-section prompts with strict template and evidence inputs

Critic-driven validation

missing mandatory content detection

cross-section contradiction checks

evidence-anchor verification

Technology Stack (Edit to match your implementation)
Backend
Python

FastAPI

Pydantic

PostgreSQL

Vector search: pgvector / FAISS / Chroma

Frontend
React + TypeScript
or

Angular + TypeScript

Artificial Intelligence
Retrieval-Augmented Generation (RAG)

Multi-agent orchestration

Critic-based validation

Optional local model pathway

Repository Structure
text
Copy code
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
Quickstart
Clone

git clone <YOUR_REPO_URL>

cd INSIGHTNOTES-CLINICAL-NOTES-DOCUMENTATION

Backend

Create .env from .env.example

Start the API server

Frontend

Install dependencies

Run the development server

Safety and Privacy
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

Temporal drafting intelligence

Expanded evaluation harness

Export-ready outputs (Portable Document Format (PDF) + structured formats)

Maintainer
Pavankalyan Ghanta
GitHub: <your link>
Portfolio: <your link>
LinkedIn: <your link>
