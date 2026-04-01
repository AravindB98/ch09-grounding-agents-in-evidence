# Chapter 09: Grounding Agents in Evidence — Magid's Newsroom RAG System

**Design of Agentic Systems with Case Studies**  
*Aravind Balaji | INFO 7375: Prompt Engineering for Generative AI | Northeastern University*

**Chapter Type:** B — Production Case Study

---

## Core Claim

RAG is not a search optimization — it is a fundamental epistemological constraint that determines whether an agent's outputs can be trusted as evidence-grounded. Without RAG, LLM agents confabulate with high confidence. With RAG, agent outputs are traceable to sources and deviations are detectable. Context adherence is a measurable property, not an aspiration.

## The Case Study

**Magid's Collaborator Newsroom** — a deployed RAG system processing thousands of newsroom stories daily. Re-versions broadcast scripts into web, social, push alerts, and translations. Production metrics: 45-minute stories in 5 minutes, 2-6 FTEs per newsroom, 100% customer renewal.

## Failure Mode Demonstrated

**Unfaithful generation from faithful retrieval** — the retriever returns the correct source, the generator produces fluent output that subtly deviates (paraphrased quotes, shifted attributions, dropped qualifiers). The **0.681 gap** between token-overlap deviation (0.871, FLAGGED) and semantic deviation (0.190, NOT FLAGGED) on the same fabricated quote proves generic metrics miss domain-specific failures.

---

## Repository Structure

```
ch09-grounding-agents-in-evidence/
│
├── README.md                                    ← This file
│
├── chapter/
│   ├── chapter_09_rag.md                        ← Full chapter (Markdown, 568 lines)
│   └── chapter9_grounding_agents_v3.html        ← Substack chapter (styled HTML)
│
├── notebook/
│   ├── ch09_newsroom_demo.ipynb                 ← Runnable demo (10 parts)
│   └── requirements.txt                         ← pip dependencies
│
├── authors_note.md                              ← 3-page Author's Note
├── video_script.md                              ← 10-min video script
│
└── figures/                                     ← (rendered inline in conversation;
    └── figure_architect_output.md                  Figure Architect prompts documented)
```

## Quick Start

```bash
git clone https://github.com/YOUR_USERNAME/ch09-grounding-agents-in-evidence.git
cd ch09-grounding-agents-in-evidence/notebook
pip install -r requirements.txt
export ANTHROPIC_API_KEY="your-key-here"
jupyter notebook ch09_newsroom_demo.ipynb
```

The notebook will:
1. Compute the 0.681 gap on the Reyes fabricated quote (Part 1)
2. Run Pipeline A (naive RAG, no scorer) on a broadcast script (Part 3)
3. Run Pipeline B (scored RAG, three-axis + token verification) (Part 4)
4. **HALT at the Human Decision Node** — you must type PASS or FLAG (Part 5)
5. Retroactively score Pipeline A's output, revealing invisible deviations (Part 6)
6. Simulate a model swap showing the measurement gap persists (Part 7)

## Tools Used

| Tool | Key Correction |
|------|---------------|
| **Bookie** | Rejected "RAG reduces hallucination" → "RAG is epistemological constraint" |
| **Bookie** | Rejected generic RAGAS → three-axis domain-specific scorer (0.681 gap as proof) |
| **Bookie** | Rejected "hallucination" framing → "unfaithful generation from faithful retrieval" |
| **Eddy** (3 passes) | 19 fixes across 10 standards; honeymoon period resolved; Watson + Rekognition in prose |
| **Figure Architect** | 6 figures flagged; 4 rendered (pipeline, gap chart, failure anatomy, error matrix) |
| **Courses** | Corrected "Try" from discussion → triggerable gate-disabling exercise |

## Video

**YouTube (unlisted):** [INSERT LINK AFTER RECORDING]

10 minutes · Explain → Show → Try  
Human Decision Node on camera at 5:40

---

## Author

**Aravind Balaji** — MS Information Systems, Northeastern University  
VP of Research & Development, AI Skunkworks  
[aravindbalaji.com](https://aravindbalaji.com) · [Substack](https://aravindbalaji1.substack.com)
