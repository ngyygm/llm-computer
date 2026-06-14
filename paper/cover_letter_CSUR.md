# Cover Letter — ACM Computing Surveys

**To:** Editor-in-Chief, *ACM Computing Surveys*
**From:** Hai-Tao Zheng (corresponding), Hai Lin, Hoilam Pao, Shaoxiong Zhan
**Affiliation:** Shenzhen International Graduate School, Tsinghua University; Pengcheng Laboratory
**Date:** [submission date]
**Subject:** Submission of "Model-Native Computing Architecture: Envisioning Future System Architecture Through the Lens of Computer Architecture"

---

Dear Editor-in-Chief,

We are pleased to submit our manuscript **"Model-Native Computing Architecture"** for consideration by *ACM Computing Surveys*. We request that it be reviewed for the **Survey / Tutorial** track as a comprehensive survey and perspective.

## 1. What the paper is

The paper maps eight decades of computer-architecture wisdom onto the emerging stack of model-native (LLM/agent) computing systems. It is a **conceptual survey and perspective**: it surveys a rapidly growing literature across LLM-as-OS, memory management, agent frameworks, tool protocols, multi-agent coordination, cognitive architectures, and safety governance, and organizes that literature under a unifying architectural lens. It does **not** present new algorithms or experimental results; its contribution is integrative and organizational.

## 2. Why it fits CSUR

- **Scope and comprehensiveness.** The survey covers ~188 references across seven functional themes, with an explicit, reproducible literature-search methodology (Section 2.2: five databases, a 2022–2026 window, documented query strings, and PRISMA-style counts from ~450 records to ~188 retained). A coverage map (Figure 3) makes the scope and its gaps explicit.
- **Unifying thesis.** A field that has fragmented into dozens of overlapping "LLM as OS / kernel / CPU" proposals lacks a shared vocabulary. We provide one.
- **Length.** ~108 pages is within CSUR norms for an integrative full-stack survey.

## 3. The contributions (and how they differ from prior surveys)

1. **An analogy framework** that explicitly delineates where the classical-architecture analogy holds and where it breaks.
2. **A dual-plane architecture** that resolves the enduring "Is an LLM a CPU or an OS?" metaphor conflict by recognizing two orthogonal planes — a *probabilistic execution plane* and a *deterministic control plane* — through which every layer passes as a graded crossover around L3–L4. This reconciles ArbiterOS, AIOS, and AgentOS as complementary views of the two planes rather than contradictory claims about one component.
3. **The Intelligent Computing Architecture (ICA):** a six-layer model with five formal inter-layer interface contracts and six design axioms.
4. **Three Amdahl-style design heuristics** (Semantic Locality, Context Budget, Agent Speedup) — explicitly framed as *back-of-envelope organizing models*, not validated scaling laws, with a derived N\* closed form.
5. **A paradigm mapping** (silicon → substrate) and a **socio-technical analysis** (fabless AI), plus a research roadmap.

**Differentiation.** The closest prior work is Mi et al. (arXiv:2504.04485, "Building LLM Agents by Incorporating Insights from Computer Systems"), a von-Neumann-inspired survey that maps agent modules but provides **no** layered interface contracts, design axioms, quantitative heuristics, or a dual-plane separation. Table 2 consolidates the distinction: across the five architectural artifacts that distinguish a *discipline* from a collection of metaphors, no single prior work combines more than one, whereas ICA integrates all five.

## 4. Anticipating likely reviewer concerns

We have tried to pre-empt the objections a CSUR reviewer is most likely to raise:

- **"Is the dual-plane just relabeling the deterministic-shell / probabilistic-core pattern?"** It is more: it *resolves* a metaphor conflict that the prior single-plane proposals created, and it yields a concrete derived invariant — every irreversible, side-effecting operation is mediated through the deterministic control plane, giving a single auditable choke point (Axiom 6). We position it honestly as a *unifying organizational lens*, not a uniquely forced structure.
- **"Where is the evidence / where are the experiments?"** The paper is a perspective/survey and contains no experiments by design. We are explicit (Abstract, Introduction contribution 4, Section 8, Conclusion) that the three heuristics are *illustrative, not validated*, and we identify systematic predict-then-measure validation as the principal open task. We consider this honest scoping a feature, not a gap.
- **"Is the six-layer count arbitrary or falsifiable?"** We do not claim the decomposition is unique. It is a deliberate, pragmatic one-to-one mapping onto classical strata, in which each boundary is independently measurable via its interface contract; we acknowledge the boundaries will be revised as the field matures (Conclusion).
- **"Single-vendor empirical spine."** Several quantitative industry figures are drawn from a single Anthropic report. We have (i) quarantined them in prose as self-reported, single-vendor data; (ii) co-cited independent corroboration of the headline "high-use / low-delegation" pattern (Dohmke et al. on ~30% code-suggestion acceptance; Klemmer et al. on use "despite deep mistrust"); and (iii) flagged independently audited economic studies as needed.
- **"Figure/notation consistency."** The dual-plane boundary, plane terminology, and color convention are unified across all figures and body text (graded crossover at L3–L4; probabilistic = orange, deterministic = blue).

## 5. Declarations

- This manuscript is original, has not been published, and is not under consideration elsewhere.
- All cited works were verified; bibliography integrity was checked (no undefined citations, no orphan entries in the compiled bibliography).
- There are no conflicts of interest to declare.

We believe a comprehensive, architecture-grounded survey of model-native computing is timely for the CSUR readership, and we would be glad to revise according to the editors' and reviewers' guidance. Thank you for your consideration.

Sincerely,

**Hai-Tao Zheng** (corresponding author)
Professor, Shenzhen International Graduate School, Tsinghua University
Email: zheng.haitao@sz.tsinghua.edu.cn
