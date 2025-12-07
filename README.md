# InsightNotes — Clinical Notes Documentation
**AI-assisted, privacy-first clinical note drafting with evidence-linked generation and multi-agent quality checks**

![License](https://img.shields.io/badge/License-MIT-success)
![Status](https://img.shields.io/badge/Status-Public%20Prototype-blue)
![Backend](https://img.shields.io/badge/Backend-FastAPI%20%7C%20ASP.NET%20Core-009688)
![Frontend](https://img.shields.io/badge/Frontend-React%20%7C%20Angular-3178C6)
![AI](https://img.shields.io/badge/AI-Retrieval--Augmented%20Generation%20(RAG)%20%7C%20Agentic%20Quality-blueviolet)
![Privacy](https://img.shields.io/badge/Privacy-Protected%20Health%20Information%20(PHI)%20Aware-critical)

InsightNotes is a **clinical documentation assistant** that converts structured encounter inputs into **high-quality, template-aligned note drafts** (for example: **Subjective Objective Assessment Plan (SOAP)**, **Data Assessment Plan (DAP)**, **Behavior Intervention Response Plan (BIRP)**, progress notes, and discharge summaries).  
It combines **deterministic template rules** with **Retrieval-Augmented Generation (RAG)** and **multi-agent validation** so clinicians get faster drafts **without sacrificing trust, consistency, or auditability**.

---

## Why this project stands out

Most documentation tools optimize for speed of text generation.  
InsightNotes is designed for **high-integrity clinical environments**, where **structure, evidence, and traceability** matter as much as speed.

**Core philosophy**  
**Structure first → Retrieval second → Generation last → Critic always**

---

## What it does

Given clinician-entered encounter facts and selected note type, InsightNotes:

1. Builds a **section plan** based on validated templates.
2. Retrieves **approved evidence** (organizational guidance, note rules, optional clinical references).
3. Generates section-by-section drafts using a **guardrailed Large Language Model (LLM)**.
4. Runs a **Critic Agent** to catch:
   - missing mandatory sections  
   - cross-section contradictions  
   - plan/follow-up inconsistencies  
   - statements without evidence anchors
5. Produces an **audit-friendly, versioned draft** ready for clinician review.

---

## Key features

### ✅ Multi-format note generation
- Subjective Objective Assessment Plan (SOAP)
- Data Assessment Plan (DAP)
- Behavior Intervention Response Plan (BIRP)
- Progress Notes
- Discharge Summaries
- Custom organizational templates

### ✅ Evidence-linked Retrieval-Augmented Generation (RAG)
- Context is retrieved from **approved sources** and filtered by:
  - note type
  - specialty
  - section
  - rule priority
- Outputs can expose:
  - **what evidence supports this**
  - **which template rule it satisfies**
  - **why this suggestion appeared**

### ✅ Agentic quality gates
A lightweight multi-agent pipeline:
- **Planner Agent**: constructs section plan
- **Retriever Agent**: selects evidence
- **Writer Agent**: drafts note
- **Critic Agent**: validates factual + template alignment
- **Protected Health Information (PHI) Safety Layer** (optional): checks for unsafe leakage patterns

### ✅ Audit-ready note versioning
Each draft can store:
- input snapshot  
- evidence map  
- model + prompt version  
- structured text diff  
- reviewer actions

---

## Impact (replace with your measured values)

These are **reference targets** you can swap with your real benchmarks:

- **Documentation time reduction target:** [35%–55%] for routine notes  
- **First-draft usability target:** [70%–85%] with minimal edits  
- **Template completeness detection target:** [>90%] via critic checks  
- **Latency targets under load:**
  - p50: [<220 milliseconds]
  - p95: [<480 milliseconds] at [50+] concurrent draft requests  
- **Trust metrics:**
  - evidence coverage ratio
  - unsupported-claim flag rate
  - clinician edit distance per section

---

## Architecture

### High-level system

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
  API --> OBS[Observability<br/>Metrics/Logs/Traces]

  CRIT --> ORCH
  PHI --> ORCH
  ORCH --> API
  API --> UI
