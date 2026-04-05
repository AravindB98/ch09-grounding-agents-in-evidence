# Chapter 09: Grounding Agents in Evidence

## Magid's Newsroom RAG System

**Design of Agentic Systems with Case Studies**
*Aravind Balaji | INFO 7375: Prompt Engineering for Generative AI | Northeastern University*

**Chapter Type:** B — Production Case Study

---

## About This Chapter

This chapter is a production case study of **Magid's Collaborator Newsroom** — an AI-powered content versioning platform used by hundreds of local and national newsrooms to transform broadcast scripts into web stories, social media posts, push alerts, and translations. The chapter argues that RAG is not a search optimization but a fundamental **epistemological constraint**: it establishes a bounded reference object (the journalist's source document) against which output deviation becomes measurable. Without that measurement layer, the system produces fluent, professional output that silently misrepresents its sources.

The chapter traces Magid's architectural journey from failed single-shot prompting through a five-stage agentic pipeline (source boundary, task decomposition, generation, domain-specific evaluation, real-time observability) and demonstrates why each stage exists by showing what breaks without it. Two canonical failure cases — IBM Watson for Oncology (wrong-population evaluation corpus) and Amazon Rekognition (aggregate metrics masking disaggregated failures) — generalize the argument beyond journalism. Section 8 breaks the architecture the chapter built, identifying three limits the measurement layer cannot address: source document accuracy, editorial intent, and evaluation metric drift.

**The book's master argument, instantiated:** Magid's clients used the same powerful LLMs in single-shot mode and failed. The same models, inside a five-stage architecture with domain-specific measurement, succeeded at production scale. The model did not change. The architecture did. **Architecture is the leverage point, not the model.**

---

## Core Claim

RAG is not a search optimization — it is a fundamental epistemological constraint. First, RAG establishes a bounded reference object against which output can be compared. Second, that bounded object is what makes deviation measurable. Without measurement, the bounded object produces no trust benefit. Context adherence is a measurable property, not an aspiration.

## The Gap

The fabricated Reyes quote — where the model reversed "stopped short of endorsing" into a direct quote of support — scores **0.871 on token-overlap deviation** (FLAGGED) but only **0.12–0.25 on semantic deviation** (NOT FLAGGED). The gap is **0.62–0.75** depending on embedding model. The gap *direction* is stable across implementations: token overlap catches it, semantic similarity misses it. Generic metrics miss domain-specific failures.

## The Case Study

**Magid's Collaborator Newsroom** — a deployed RAG system processing thousands of newsroom stories daily. Five-stage architecture: source ingestion (hard knowledge boundary), agentic task decomposition (PromptLayer), nine-dimension evaluation (Analyze), domain-specific accuracy check (first hallucination detector built for journalism), and real-time observability (Galileo).

**Production metrics:** 45 min → 5 min per story, 2–6 FTEs unlocked per newsroom, 80% daily journalist adoption, 100% customer renewal.

## Failure Mode Demonstrated

**Unfaithful generation from faithful retrieval.** The retriever returns the correct source document. The generator produces fluent output that subtly deviates — paraphrased quotes presented as direct, statistics attributed to the wrong source, qualifiers dropped. The five-stage failure anatomy traces the causal chain from deployment-without-measurement through silent failure accumulation to published fabrication and misattributed root cause: "The model was blamed. The architecture was the cause."

---

## Repository Structure

```
ch09-grounding-agents-in-evidence/
│
├── README.md                                 ← This file
│
├── chapter/
│   ├── chapter_09_rag.md                     ← Full chapter (Markdown, ~680 lines, 8 figures embedded)
│   └── chapter9_grounding_agents_v3.html     ← Substack chapter (styled HTML, original publication format)
│
├── notebook/
│   ├── ch09_newsroom_demo.ipynb              ← Runnable demo notebook (10 parts, see details below)
│   └── requirements.txt                      ← pip dependencies for fresh-clone reproducibility
│
├── figures/
│   ├── fig1_knowledge_topology.svg           ← LLM vs RAG knowledge architecture comparison
│   ├── fig2_pipeline_architecture.svg        ← Magid five-stage pipeline with human decision node
│   ├── fig3_deviation_gap_chart.svg          ← Token overlap vs semantic deviation (the gap)
│   ├── fig4_failure_anatomy.svg              ← Five-stage failure causal chain (gray → red severity ramp)
│   ├── fig5_error_classification_matrix.svg  ← 2×2 error space: why the dangerous quadrant is invisible
│   ├── fig6_architectural_limits.svg         ← Three limits the architecture cannot address (Section 8)
│   ├── fig7_production_economics.svg         ← Before/after production metrics (9× faster, 100% renewal)
│   └── fig8_scoring_decision_tree.svg        ← Worst-axis gate decision tree (BLOCK/FLAG/PASS)
│
├── authors_note.md                           ← 3-page Author's Note (design choices, tool usage, self-assessment)
└── case_analysis.md                          ← Structured case analysis (Type B deliverable)
```

---

## File Descriptions

### Chapter (`chapter/`)

**`chapter_09_rag.md`** — The complete chapter in Markdown (~680 lines). Contains: learning outcomes with ABET evidence mapping, the Reyes fabrication opening, the worked quantitative example (Jaccard + cosine + gap), the four-quadrant error space, Magid's five-stage architecture with failure-annotated pipeline diagram, Watson for Oncology and Amazon Rekognition as canonical failure cases, the three-axis context adherence scorer with defect discussion, the mandatory Human Decision Node with `input()` halt and `raise RuntimeError` enforcement, the worst-axis gate scoring logic with quantitative argument, three exercises including a step-by-step "break the system" exercise and the capstone Activity 9.3 with LLM scaffolding prompts, production considerations with tiered evaluation and dollar-per-decision economics, Section 8 (where the architecture itself fails — source accuracy, editorial intent, metric drift), and cross-book connections to Chapters 5, 7, 14, 15, 21, 24, and 26–27. All 8 figures are embedded inline with captions.

**`chapter9_grounding_agents_v3.html`** — The styled HTML version of the chapter, formatted for Substack publication. This is the original chapter format with CSS styling, pull quotes, and web-optimized layout.

### Notebook (`notebook/`)

**`ch09_newsroom_demo.ipynb`** — A 10-part runnable Jupyter notebook that demonstrates the chapter's argument with live code:

| Part | What it does |
|---|---|
| 1 | Computes the token-vs-semantic gap on the Reyes fabricated quote |
| 2 | Loads the broadcast script test input (3 quotes, 2 statistics, 1 qualifier) |
| 3 | Runs Pipeline A: naive RAG with no measurement layer |
| 4 | Runs Pipeline B: scored RAG with three-axis scorer + token verification |
| 5 | **Human Decision Node** — `input()` hard halt; notebook stops until human types PASS or FLAG |
| 6 | Retroactively scores Pipeline A's unscored output — reveals invisible deviations |
| 7 | Model-swap simulation — two temperatures, measurement gap persists across both |
| 8 | Side-by-side comparison table: same model, different architecture, different trust |
| 9 | Exercise: `scorer_DISABLED` passthrough + retroactive scoring to trigger the failure |
| 10 | AI scaffold: domain-specific evaluation proposer with second `input()` halt |

**`requirements.txt`** — pip dependencies: langchain, langchain-anthropic, chromadb, sentence-transformers, numpy. Run `pip install -r requirements.txt` before opening the notebook.

### Figures (`figures/`)

All 8 figures are production SVG files generated from Figure Architect prompts. They render in any browser (drag into Chrome to view) and are embedded inline in `chapter_09_rag.md`.

| # | Figure | What it shows |
|---|---|---|
| 1 | Knowledge topology | Side-by-side: LLM without RAG (no provenance) vs RAG (measurable adherence) |
| 2 | Pipeline architecture | Magid's five stages + human decision node, with failure modes at each stage |
| 3 | Deviation gap chart | Token overlap (0.871, flagged) vs semantic similarity (0.12–0.25, not flagged) |
| 4 | Failure anatomy | Five-stage causal chain from deployment to misdiagnosis, gray-to-red severity |
| 5 | Error classification matrix | 2×2: semantic similarity × attribution accuracy; dangerous quadrant highlighted |
| 6 | Architectural limits | Three limits the system cannot address: source accuracy, editorial intent, metric drift |
| 7 | Production economics | Before/after: 9× speed, 80% adoption, 2–6 FTEs, 100% renewal |
| 8 | Scoring decision tree | Worst-axis gate: BLOCK → FLAG → FLAG+fix → PASS (priority order, first match wins) |

### Author's Note (`authors_note.md`)

Three pages documenting design choices, tool usage, and self-assessment:

- **Page 1 — Design Choices:** Why Chapter 9, connection to MediGraph AI, Type B case study rationale, scoping decisions (what was left out and why), cross-book positioning relative to Chapters 21, 26–27
- **Page 2 — Tool Usage:** Three Bookie corrections (quoted verbatim with reasoning), eight Eddy passes across 10 standards (32 issues raised, all resolved), Figure Architect (8 figures flagged and rendered), Courses (lesson sequence correction), Caze (structured case analysis)
- **Page 3 — Self-Assessment:** 100/100 with rubric-mapped justification for each criterion

### Case Analysis (`case_analysis.md`)

Structured case analysis following the Type B production case study format:

- **Corporate profile:** Magid history, product (Collaborator Newsroom), technology stack, key constraint (source-only knowledge boundary)
- **Unit economics:** Before/after metrics, dollar-per-decision justification, cost structure of the evaluation layer
- **Competitive benchmarks:** Four approaches compared (off-shelf LLM → single-shot → generic RAG → Magid architecture)
- **Three analytical questions:** Single-shot failure mechanism, token-vs-semantic gap evidence, architecture reversal consequences
- **Model answers held separately:** Full answers on a separate page from the questions
- **One-page architectural memo:** Claim, key decision, what breaks if the decision is reversed

---

## Quick Start

```bash
git clone https://github.com/abalaji-blr/ch09-grounding-agents-in-evidence.git
cd ch09-grounding-agents-in-evidence/notebook
pip install -r requirements.txt
export ANTHROPIC_API_KEY="your-key-here"
jupyter notebook ch09_newsroom_demo.ipynb
```

The notebook will:
1. Compute the gap on the Reyes fabricated quote (Part 1)
2. Run Pipeline A (naive RAG, no scorer) on a broadcast script (Part 3)
3. Run Pipeline B (scored RAG, three-axis + token verification) (Part 4)
4. **HALT at the Human Decision Node** — you must type PASS or FLAG (Part 5)
5. Retroactively score Pipeline A's output, revealing invisible deviations (Part 6)
6. Simulate a model swap showing the measurement gap persists (Part 7)
7. Present side-by-side comparison: same model, different architecture (Part 8)

---

## Tools Used

| Tool | Usage |
|---|---|
| **Bookie** | 3 corrections: rejected "RAG reduces hallucination" → epistemological constraint; rejected generic RAGAS → three-axis scorer (gap as proof); rejected "hallucination" framing → unfaithful generation from faithful retrieval (architectural failure, not model failure) |
| **Eddy** | 8 passes, 32 issues raised across 10 authoring standards, all resolved. Key fixes: Honeymoon Period (§8 breaks the architecture), Watson + Rekognition in expository prose, restored worked quantitative example, ABET mapping, priority-ordered scoring logic, four-quadrant error space, scorer defect discussion, LLM scaffolding for activities |
| **Figure Architect** | 8 high-assertion zones flagged with structural prompts, aesthetic prompts, and verification checklists. All 8 rendered as production SVGs and embedded in chapter |
| **Courses** | Lesson sequence corrected: "discuss when to implement scoring" (passive) → "disable the scorer, run the pipeline, count deviations that ship" (triggerable failure) |
| **Caze** | Structured case analysis: corporate profile, unit economics, competitive benchmarks, 3 analytical questions with model answers held separately, one-page architectural memo |

---

## Video

**YouTube (unlisted):** https://youtu.be/LzuuzXDC1Zs

10 minutes (play at 2x speed) | Explain → Show → Try

- **Explain (0:00–2:30):** Architectural claim drawn on screen. Failure mode named.
- **Show (2:30–8:00):** Notebook cells run live. Human Decision Node at ~5:00 — "The AI proposed generic RAGAS. I rejected it because the gap proves it's insufficient."
- **Try (8:00–10:00):** `scorer_DISABLED` breaks the system. Open question: who validates the scorer?

---

**Aravind Balaji** — MS Information Systems, Northeastern University
