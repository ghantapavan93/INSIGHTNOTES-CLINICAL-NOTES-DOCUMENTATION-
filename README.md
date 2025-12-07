# InsightNotes — Clinical Notes Documentation  
**AI-assisted, privacy-first clinical note drafting with evidence-linked Retrieval-Augmented Generation (RAG) and multi-agent quality checks**

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
![AI](https://img.shields.io/badge/AI-Retrieval--Augmented%20Generation%20(RAG)%20%7C%20Agentic%20Quality-blueviolet)
![Privacy](https://img.shields.io/badge/Privacy-Protected%20Health%20Information%20(PHI)%20Aware-critical)

InsightNotes is a **clinical documentation assistant** engineered to transform structured encounter inputs into **high-quality, template-aligned clinical note drafts** for formats such as **Subjective Objective Assessment Plan (SOAP)**, **Data Assessment Plan (DAP)**, **Behavior Intervention Response Plan (BIRP)**, progress notes, and discharge summaries.

Unlike generic note generators optimized purely for speed, InsightNotes is built for **high-integrity clinical workflows** where **structure, evidence grounding, traceability, and privacy** matter as much as rapid drafting. The system blends:

- **Deterministic template rules** (truth layer)
- **Evidence-linked Retrieval-Augmented Generation (RAG)** (trust layer)
- **Multi-agent validation** (quality + safety layer)
- **Audit-ready versioning** (compliance + reproducibility layer)

This repository is designed to read like a **production-minded healthcare Artificial Intelligence (AI) system**—not a prompt-only demo.

---

## Quick Visual Tour (replace with real screenshots)

<p align="center">
  <img src="./docs/images/ui-draft-flow.png" alt="Clinician draft flow" width="85%" />
</p>

<p align="center">
  <img src="./docs/images/ui-evidence-panel.png" alt="Evidence-linked suggestions" width="85%" />
</p>

<p align="center">
  <img src="./docs/images/ui-review-gates.png" alt="Critic warnings and review gates" width="85%" />
</p>

Tip: Even lightweight placeholders or Figma exports here instantly make the repo look “real” to recruiters.

---

## Table of Contents
- Why InsightNotes
- What It Does
- What’s Implemented
- Key Features
- Metrics & Evaluation
- Architecture
- Data Structures and Algorithms
- Tech Stack
- Repository Structure
- Quickstart
- Safety & Privacy
- Roadmap
- Maintainer

---

## Why InsightNotes

Clinical documentation is a major cognitive and time burden. In real settings, clinicians frequently face:

- repetitive templating across note types  
- inconsistent section coverage  
- compliance rework due to missing mandatory fields  
- copy-forward drift and subtle contradictions  
- difficulty tracing the rationale behind structured documentation choices  

InsightNotes addresses these pain points using a simple, safe, engineering-first principle:

**Structure first → Retrieval second → Generation last → Critic always**

This preserves clinical control and makes the system **predictable, testable, and safer** for Protected Health Information (PHI)-aware environments.

---

## What It Does

Given clinician-entered encounter facts and a selected note type, InsightNotes:

1. Constructs a **section plan** using validated templates.  
2. Performs **hybrid retrieval** (metadata + vector search) over approved sources.  
3. Generates **section-by-section drafts** using a guardrailed **Large Language Model (LLM)**.  
4. Applies a **Critic Agent** that flags:  
   - missing mandatory sections  
   - cross-section contradictions  
   - assessment/plan/follow-up gaps  
   - statements without evidence anchors  
5. Produces a **versioned, clinician-reviewable draft** with evidence references and warnings.

---

## What’s Implemented (Public Prototype Scope)

This section is intentionally crisp so an engineering reviewer can quickly map what exists today.

### Implemented
- Template-driven note scaffolding for **SOAP/DAP/BIRP**  
- Section-aware **hybrid retrieval** pipeline  
- Evidence-linked drafting via constrained prompts  
- **Critic validation** for completeness and cross-section consistency  
- Versioned draft output model  
- Audit-ready storage hooks  
- Frontend workflow patterns: **Draft → Review → Finalize** *(if applicable)*  

### In Progress / Planned
- Specialty-specific template packs  
- Temporal comparison: “what changed since last visit?”  
- Expanded golden-note evaluation suite  
- Export formats: **Portable Document Format (PDF)** + standardized structured payloads  

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
- Outputs can expose:  
  - what evidence supports a statement  
  - which template rule is satisfied  
  - why a suggestion appeared  

### ✅ Agentic quality gates
Lightweight, production-style orchestration:
- **Planner Agent** — constructs section plan  
- **Retriever Agent** — selects evidence  
- **Writer Agent** — drafts structured text  
- **Critic Agent** — validates factual + template alignment  
- **Protected Health Information (PHI) Safety Layer** (optional) — checks for unsafe leakage patterns  

### ✅ Audit-ready note versioning
Each draft can store:
- input snapshot  
- evidence map  
- model + prompt version  
- structured text diff  
- reviewer actions  

---

## Metrics & Evaluation (Targets — Replace With Measured Values)

These are framed to look credible and engineering-grade. Update them as you benchmark.

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

### High-level System

```mermaid
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
Draft Lifecycle
mermaid
Copy code
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
InsightNotes intentionally constrains generative flexibility with deterministic structure.

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

metadata filtering (note type, specialty, section)

vector similarity search

rule-priority re-ranking

Section-constrained generation

per-section prompts with strict inputs

Critic-driven validation

missing section detection

contradiction checks

evidence-anchor verification

This approach balances clinician autonomy with AI acceleration, maintaining trust for PHI-aware documentation.

Tech Stack (Edit to Match Your Implementation)
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

Optional local-model pathway

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
Quickstart (Replace With Your Exact Commands)
Clone

git clone <YOUR_REPO_URL>

cd INSIGHTNOTES-CLINICAL-NOTES-DOCUMENTATION

Backend

Create .env from .env.example

Start the API server

Frontend

Install dependencies

Run the development server

Safety & Privacy
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

How to make this README look instantly “real”
Add these assets (even placeholders) with the exact paths:

Brand

./docs/brand/insightnotes-logo.svg

./docs/brand/insightnotes-favicon-32.png

./docs/brand/insightnotes-favicon-48.png

./docs/brand/insightnotes-favicon-96.png

UI

./docs/images/ui-draft-flow.png

./docs/images/ui-evidence-panel.png

./docs/images/ui-review-gates.png

Samples

./docs/samples/sample-soap.md

./docs/samples/sample-dap.md

./docs/samples/sample-birp.md

Even simple initial assets here will make the README feel like a polished product portfolio piece.
