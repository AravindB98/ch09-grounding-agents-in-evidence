# Author's Note

**Aravind Balaji — INFO 7375: Prompt Engineering for Generative AI**
**Chapter 09: Grounding Agents in Evidence — Magid's Newsroom RAG System**

---

## Page 1 — Design Choices

I chose Chapter 9 because I have built and broken the exact architecture this chapter describes. My MediGraph AI project — Neo4j + Snowflake + RAG for healthcare — indexed clinical guidelines from multiple years. When a user asked about a drug interaction updated in the 2023 guidelines, the RAG pipeline retrieved chunks from both the 2021 and 2023 versions. The generator paraphrased the 2023 dosage restriction in a way that altered its clinical meaning. The retrieval was correct. The generation was unfaithful. No generic metric caught it — because the paraphrase was semantically similar to the source. That experience is the origin of my conviction that context adherence must be measured with domain-specific metrics, not estimated with generic semantic similarity.

This is a **Type B chapter: Production Case Study.** The case is Magid's Collaborator Newsroom — a deployed RAG system processing thousands of newsroom stories daily. The chapter teaches through the architectural choices Magid made and the failure modes those choices prevent. The structured case analysis (`case_analysis.md`) provides the corporate profile, unit economics, competitive benchmarks, three analytical questions with model answers held separately, and the one-page architectural memo.

The book's master argument — **architecture is the leverage point, not the model** — is demonstrated by Magid's journey. Their clients used the same powerful LLMs in single-shot mode and failed. The same models, embedded in an architecture with source-bounded RAG, agentic decomposition, domain-specific evaluation, and real-time observability, succeeded at production scale. The model did not change. The architecture did.

The core claim is epistemological: RAG is not a search optimization. It is a constraint that makes agent outputs *trustable* by making them *traceable*. But traceability is necessary, not sufficient. Whether the measurement is performed, and whether it catches domain-specific failures, are separate architectural decisions — and both can fail independently.

I left out multi-hop RAG because Magid's system is source-bounded — it operates only on the journalist's uploaded document, not an open knowledge base. I left out RAG pipeline mechanics (chunking strategies, embedding models, hybrid search, re-ranking) because those are covered in Chapters 21, 26, and 27 — this chapter's scope is the measurement and observability layers that sit *after* retrieval, not the retrieval pipeline itself. I left out fine-tuned embeddings because the chapter's argument is about evaluation architecture, not component optimization. The IBM Watson for Oncology and Amazon Rekognition references (in the expository prose, not just activities) demonstrate that the evaluation-layer argument generalizes beyond journalism.

A key scoping decision: Chapter 26 argues teams should start simple — large context window before RAG pipeline. Magid's source documents often fit in a context window. The chapter addresses this directly: RAG's value in this case is not better retrieval but the bounded reference object that makes deviation measurable. "Capacity is not architecture." This distinction prevents the chapter from contradicting Chapter 26 while advancing its own epistemological argument.

---

## Page 2 — Tool Usage

### Bookie the Bookmaker — Three Corrections

**Correction 1 — Core Claim Framing**

Bookie proposed:
> *"After reading this chapter, a student will understand RAG well enough to build a retrieval pipeline that provides grounded context to a generator, reducing hallucination rates."*

I rejected this. It treats RAG as a search optimization. "Reducing hallucination rates" frames RAG as a model-quality improvement. The chapter's argument is that RAG changes the epistemological status of the output: without RAG, outputs are unverifiable; with RAG, deviation from source becomes measurable. Corrected claim: *"RAG is not a search optimization — it is a necessary but insufficient epistemological constraint."*

**Correction 2 — Evaluation Metric Design**

Bookie proposed:
> *"Use RAGAS context adherence scoring (embedding cosine similarity between generated output and retrieved context) to measure whether the RAG system is producing grounded outputs."*

I rejected this after building the worked example. The fabricated Reyes quote scores high on semantic similarity (cosine ≈ 0.81, deviation ≈ 0.19) because it discusses the same topic, person, and vote. Token overlap catches it (Jaccard ≈ 0.129, deviation ≈ 0.871) because the hedging words are absent. The **0.681 gap** proves generic semantic metrics miss domain-specific failures. Corrected: three-axis scorer (quote fidelity, attribution accuracy, semantic fidelity).

**Correction 3 — Failure Mode Attribution**

Bookie proposed:
> *"The primary failure mode of RAG systems is hallucination — when the model generates claims not present in the retrieved context."*

I rejected "hallucination" because it attributes the failure to the model. The chapter's failure mode — unfaithful generation from faithful retrieval — is an architectural failure. The model is replaced (Stage 5); error rate improves from 3% to 2.1%; the architecture deficiency is preserved. "The model was blamed. The architecture was the cause."

### Eddy the Editor — Full Audit (10 Standards, 3 Passes)

**Pass 1 fixes:**
- Added §3.3 Watson for Oncology in expository prose (not just activity prompt)
- Added §8 "Where the Architecture Itself Fails" — three limits: source accuracy, editorial intent, metric drift (resolved the Honeymoon Period problem)
- Annotated pipeline diagram with failure mode at every stage
- Added §7.3 evaluation cost at production scale with tiered strategy

**Pass 2 fixes:**
- Rewrote §1 opening — phenomenon first, statistics moved later, "obvious" removed
- Restored full Jaccard/cosine worked example in §2.1 (was missing from Markdown draft)
- Filled empty §5.3 with actual Human Decision Node code and `input()` halt
- Added §5.4 multi-dimensional aggregate scoring worked example (worst-axis gate)
- Added ABET evidence mapping table after learning outcomes
- Moved Tetrahedron table to after architecture diagram, renamed columns to five pipeline positions

**Pass 3 fixes:**
- Added §3.4 Amazon Rekognition disaggregation analysis in expository prose
- Removed duplicate Human Decision Node comment block from §5.4
- Compressed §1 failure list to one sentence (let fabricated quote breathe)
- Added topic classification (crime, legal, political) to Evaluation row in pipeline table
- Pulled cosine caveat from code block into surrounding prose
- Named alternatives Magid chose not to build in §8.1 and why
- Fixed duplicate §7.3 numbering → §7.4
- Removed meta-commentary from §8 transition

**Documented but deferred** (noted for HTML chapter revision):
- Activities 9.1 and 9.3 lack LLM scaffolding guidance (Standard 8)
- Activity 9.2 Human Decision Node needs format specification (Standard 9)

### Figure Architect — 8 Figures Flagged and Rendered

Figure Architect ran the full protocol on the expanded chapter and flagged 8 high-assertion zones. All 8 were rendered as production SVG figures and embedded in the Markdown chapter with inline references:

| # | Figure | Priority | Section |
|---|---|---|---|
| 1 | Knowledge topology (LLM vs RAG) | Critical | §2.1 |
| 2 | Five-stage pipeline architecture | Critical | §5.1 |
| 3 | Token vs semantic deviation gap chart | Critical | §2.1 |
| 4 | Five-stage failure anatomy chain | Important | §4.2 |
| 5 | Quote error classification matrix | Important | §2.1 |
| 6 | Three architectural limits | Critical | §8 |
| 7 | Production economics before/after | Supplementary | §3.5 |
| 8 | Scoring logic decision tree | Important | §5.4 |

### Courses — Lesson Sequence Correction

Courses proposed the "Try" segment as: "Have viewers discuss when they would implement context adherence scoring." I rejected this — discussion is not an exercise. Corrected to: "Disable the adherence scorer. Run the pipeline. Then retroactively score the output. Count the deviations that would have shipped." This produces an observable, triggerable failure.

### Caze — Structured Case Analysis

Caze generated the structured case analysis (`case_analysis.md`) following the Type B production case study format: corporate profile (Magid, 70+ years, Marion Iowa, Collaborator Newsroom product), unit economics (45 min → 5 min, 2-6 FTEs, 80% adoption, 100% renewal, dollar-per-decision justification), competitive benchmarks (four approaches compared: off-shelf LLM → single-shot → generic RAG → Magid architecture), three analytical questions (single-shot failure mechanism, 0.681 gap quantitative evidence, architecture reversal consequences), model answers held separately, and the one-page architectural memo stating the claim, the key decision, and what breaks if the decision is reversed.

---

## Page 3 — Self-Assessment

**Architectural Rigor (35 pts)**: The chapter identifies RAG as an epistemological constraint (not a search optimization), traces the five-stage failure anatomy from deployment-without-measurement to model-gets-blamed, and demonstrates the 0.681 gap between token-overlap and semantic deviation on the Reyes fabricated quote. The failure is triggered in the notebook in three ways: (1) retroactive scoring reveals deviations in Pipeline A that were invisible without the measurement layer; (2) per-quote Jaccard-vs-cosine comparison shows the gap on the reader's own generated data; (3) the model-swap simulation demonstrates that the architectural deficiency persists across model changes (Stages 4-5 of the failure anatomy). Cross-book sync verified against Chapters 7, 14, 15, 21, 24, and 26-27 with explicit scoping to prevent overlap. Section 8 breaks the architecture the chapter built — three limits the system cannot address (source accuracy, editorial intent, metric drift). The honeymoon period is set and sprung. *Self-score: 35/35.*

**Technical Implementation (25 pts)**: The notebook implements: (a) the worked example computed programmatically; (b) Pipeline A (naive) and Pipeline B (scored, three-axis + token verification with per-quote gap analysis); (c) retroactive scoring of Pipeline A's output; (d) a programmatic `input()` halt at the Human Decision Node; (e) the model-swap simulation; (f) the AI scaffold with a second `input()` halt. The `requirements.txt` enables fresh-clone reproducibility. Eight production-quality SVG figures are embedded in the chapter, all derived from Figure Architect's full protocol. The structured case analysis (`case_analysis.md`) provides the Type B deliverable: corporate profile, unit economics, competitive benchmarks, three analytical questions with model answers held separately, and architectural memo. *Self-score: 25/25.*

**Pedagogical Clarity (20 pts)**: The chapter opens with the fabricated Reyes quote — the journalist catches it by chance, the institutional cost is traced (9 errors/week at 3% deviation). The Jaccard/cosine worked example follows immediately after the core claim (Feynman Standard). Watson for Oncology (§3.3) and Amazon Rekognition (§3.4) are in expository prose, not just activities — both canonical failure cases receive full forensic analysis. The Activities section includes four exercises with Human Decision Nodes. The ABET evidence mapping table connects each learning outcome to a specific activity and student deliverable. *Self-score: 20/20.*

**Relative Quality (20 pts)**: The Human Decision Node is: (1) a programmatic `input()` halt in the notebook; (2) documented in the Author's Note with three Bookie proposals quoted verbatim and corrected; (3) scripted for on-camera delivery at the 5:40 mark citing the 0.681 gap as evidence; (4) embedded in the chapter's Activities section. All 8 Figure Architect figures are rendered as production SVGs and embedded in the chapter. Three Eddy passes documented with 19+ specific fixes across 10 standards. Cross-book connections to 7 chapters with explicit scoping. Caze structured case analysis complete with all required components. *Self-score: 20/20.*

**Total self-assessment: 100/100.**
