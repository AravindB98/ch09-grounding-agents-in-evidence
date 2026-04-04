# Chapter 09: Grounding Agents in Evidence

## Magid's Newsroom RAG System

**Design of Agentic Systems with Case Studies**
*Aravind Balaji | INFO 7375: Prompt Engineering for Generative AI | Northeastern University*

**Chapter Type:** B — Production Case Study

---

## Core Claim

RAG is not a search optimization — it is a fundamental epistemological constraint that determines whether an agent's outputs can be trusted as evidence-grounded. Without RAG, LLM agents confabulate with high confidence. With RAG, agent outputs are traceable to sources and deviations are detectable. Context adherence is a measurable property, not an aspiration.

## The 0.681 Gap

The fabricated Reyes quote — where the model reversed "stopped short of endorsing" into a direct quote of support — scores **0.871 on token-overlap deviation** (FLAGGED) but only **0.190 on semantic deviation** (NOT FLAGGED). The delta is **0.681**. Generic metrics miss domain-specific failures.

## The Case Study

**Magid's Collaborator Newsroom** — a deployed RAG system processing thousands of newsroom stories daily. Five-stage architecture: source ingestion (hard knowledge boundary), agentic task decomposition (PromptLayer), nine-dimension evaluation (Analyze), domain-specific accuracy check, and real-time observability (Galileo).

**Production metrics:** 45 min → 5 min per story, 2–6 FTEs per newsroom, 80% daily adoption, 100% renewal.

---

## Repository Structure

```
ch09-grounding-agents-in-evidence/
├── README.md
├── authors_note.md                           ← 3-page Author's Note
├── case_analysis.md                          ← Structured case analysis (Type B)
├── chapter/
│   ├── chapter_09_rag.md                     ← Full chapter (~590 lines, 8 figures)
│   └── chapter9_grounding_agents_v3.html     ← Substack chapter (styled HTML)
├── figures/
│   ├── fig1_knowledge_topology.svg
│   ├── fig2_pipeline_architecture.svg
│   ├── fig3_deviation_gap_chart.svg
│   ├── fig4_failure_anatomy.svg
│   ├── fig5_error_classification_matrix.svg
│   ├── fig6_architectural_limits.svg
│   ├── fig7_production_economics.svg
│   └── fig8_scoring_decision_tree.svg
└── notebook/
    ├── ch09_newsroom_demo.ipynb              ← Runnable demo (10 parts)
    └── requirements.txt
```

## Quick Start

```bash
git clone https://github.com/YOUR_USERNAME/ch09-grounding-agents-in-evidence.git
cd ch09-grounding-agents-in-evidence/notebook
pip install -r requirements.txt
export ANTHROPIC_API_KEY="your-key-here"
jupyter notebook ch09_newsroom_demo.ipynb
```

## Figures

| # | Figure | Section | Priority |
|---|---|---|---|
| 1 | Knowledge topology (LLM vs RAG) | §2.1 | Critical |
| 2 | Five-stage pipeline architecture | §5.1 | Critical |
| 3 | Token vs semantic deviation gap | §2.1 | Critical |
| 4 | Five-stage failure anatomy | §4.2 | Important |
| 5 | Quote error classification matrix | §2.1 | Important |
| 6 | Three architectural limits | §8 | Critical |
| 7 | Production economics | §3.5 | Supplementary |
| 8 | Scoring logic decision tree | §5.4 | Important |

## Tools Used

| Tool | Key corrections |
|---|---|
| **Bookie** | 3 corrections: epistemological constraint framing, three-axis scorer, architectural failure attribution |
| **Eddy** | 3 passes, 19+ fixes across 10 standards including Honeymoon Period, Watson/Rekognition in prose, ABET mapping |
| **Figure Architect** | 8 figures flagged and rendered as production SVGs |
| **Courses** | Lesson sequence correction: discussion → triggerable exercise |
| **Caze** | Structured case analysis: corporate profile, unit economics, competitive benchmarks, 3 analytical questions with model answers, architectural memo |

## Video

**YouTube (unlisted):** https://youtu.be/2f0i4Lfqwqo
10 minutes played at 2x speed or more · Explain → Show → Try · Human Decision Node on camera

---

**Aravind Balaji** — MS Information Systems, Northeastern University
