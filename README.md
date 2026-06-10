# Model-Native Computing Architecture

**[中文版 / 中文](README_CN.md)**

> A layered architecture framework for model-native computing systems — drawing systematic parallels between classical computer architecture and the emerging AI/LLM ecosystem.

## Overview

Large language models (LLMs) are transitioning from a *model technology* to a *system technology*. This paper proposes the **Intelligent Computing Architecture (ICA)** — a six-layer model that provides the architectural foundation for organizing intelligence into stable, scalable, and auditable systems.

### Key Contributions

1. **ICA Six-Layer Model** — A unified framework (L1–L6) with explicit interface contracts, design axioms, and metrics for each layer
2. **Dual-Plane Architecture** — Separating probabilistic execution (model inference) from deterministic control (agent runtime governance)
3. **Three Quantitative Laws** — Semantic Locality, Context Budget, and Agent Speedup, analogous to Amdahl's Law for traditional architecture
4. **Paradigm Mapping** — Systematic mapping of 50 years of CPU evolution lessons onto LLM/Agent system trajectories
5. **Socio-Technical Analysis** — Industry structure analysis through the lens of semiconductor and PC industry unbundling history

## Architecture

### ICA Six-Layer Model

| Layer | Name | Classical Analogy |
|:-----:|------|-------------------|
| L6 | Workflow & Applications | User Applications |
| L5 | Agent Orchestration | Operating System |
| L4 | Tool & Protocol Interface | System Call / ABI |
| L3 | Context & Memory Management | Virtual Memory |
| L2 | Inference & KV Cache Serving | Instruction Pipeline |
| L1 | Model Weights & Hardware | CPU / Silicon |

### Dual-Plane Architecture

The framework decomposes the system into two orthogonal planes:

- **Probabilistic Execution Plane** (L1–L3): Model inference, KV cache, context management — *what the system CAN do*
- **Deterministic Control Plane** (L4–L5): Tool protocols, agent orchestration, governance — *what the system SHOULD do*

### Three Quantitative Laws

| Law | Formula | Analogy |
|-----|---------|---------|
| **Law I: Semantic Locality** | $S = \frac{1}{(1-H) + H \cdot \alpha^{-1}}$ | Amdahl's Law for cache hit ratios |
| **Law II: Context Budget** | $W_{\text{eff}} \leq C \cdot \beta(L)$ | Working set estimation |
| **Law III: Agent Speedup** | $S_{\text{agent}} = \frac{1}{(1-F) + \frac{F}{N \cdot E}}$ | Amdahl's Law for multi-agent |

## Figures

The paper includes 27 TikZ-generated figures and diagrams. Key figures:

### Core Architecture

<p align="center">
  <img src="paper/figures/fig_analogy_map.png" width="90%" alt="Analogy mapping between classical computing and model-native computing">
  <br><em>Analogy mapping: classical computing ↔ model-native computing</em>
</p>

<p align="center">
  <img src="paper/figures/fig_kv_hierarchy.png" width="90%" alt="CPU cache hierarchy vs LLM KV cache hierarchy">
  <br><em>Structural correspondence: CPU cache hierarchy ↔ LLM KV cache hierarchy</em>
</p>

### Agent Evolution

<p align="center">
  <img src="paper/figures/fig_agent_timeline_gemini.png" width="90%" alt="Five generations of agent framework evolution">
  <br><em>Agent framework evolution timeline (Gen I → Gen V)</em>
</p>

<p align="center">
  <img src="paper/figures/fig_impl_map_gemini.png" width="90%" alt="Open-source ecosystem mapped to ICA layers">
  <br><em>Open-source ecosystem mapped to the six-layer ICA architecture</em>
</p>

### Additional Key Figures (LaTeX/TikZ)

| Figure | Description |
|--------|-------------|
| `fig_arch` | ICA six-layer architecture diagram |
| `fig_dual_plane` | Dual-plane architecture: probabilistic execution vs deterministic control |
| `fig_icam_crossmap` | Cross-mapping of ICA layers against the dual-plane architecture |
| `fig_law_curves` | Characteristic curves of the three quantitative laws |
| `fig_gen_matrix` | Capability maturity matrix across five agent generations |
| `fig_gen_trajectory` | Quantitative trajectory of $F$, $E$, and $S_{\text{agent}}$ across generations |
| `fig_trilemma` | Latency–throughput–cost trilemma in LLM serving |
| `fig_beta_decay` | Attention retention rate $\beta(L)$: empirical U-shape vs exponential decay model |
| `fig_biglittle` | Three-stage evolution: ARM big.LITTLE ↔ LLM model orchestration |
| `fig_paradigm_timeline` | Paradigm timeline from silicon to substrate-agnostic computing |
| `fig_interface_contracts` | ICA inter-layer interface contracts |
| `fig_axioms_overview` | Applicability heatmap of six design axioms across layers |
| `fig_classical_stack` | Classical computing stack as six-layer abstraction hierarchy |
| `fig_context_tiers` | Hot/warm/cold tiered context management hierarchy |
| `fig_state_lifecycle` | Three-tier agent state lifecycle |
| `fig_impl_recommendations` | Five implementation recommendations mapped to ICA layers |
| `fig_roadmap_swimlane` | Research roadmap by ICA layer and time horizon |
| `fig_dual_stack` | Layer comparison: classical stack vs model-native stack |
| `fig_flow` | Request flow through the architecture as dual-plane swim-lane |
| `fig_coverage_map` | Coverage of ICA layers by existing works |
| `fig_security_layers` | Security layers diagram |

## Repository Structure

```
paper/
├── paper.tex              # Chinese version (中文版)
├── paper_en.tex           # English version
├── paper.pdf              # Compiled Chinese PDF
├── paper_en.pdf           # Compiled English PDF
├── refs.bib               # Shared bibliography
├── cn_sections/           # Chinese sections (15 files)
│   ├── section_intro.tex
│   ├── section_bg.tex
│   ├── section_framework.tex
│   ├── section_components.tex
│   ├── section_icam.tex
│   ├── section_laws.tex
│   ├── section_agent_evolution.tex
│   ├── section_challenges.tex
│   ├── section_paradigm.tex
│   ├── section_implementation.tex
│   ├── section_roadmap.tex
│   ├── section_ethics.tex
│   ├── section_social.tex
│   ├── section_related.tex
│   └── section_conclusion.tex
├── en_sections/           # English sections (15 files, parallel structure)
│   └── ...
└── figures/               # TikZ figure sources + rendered PNGs
    ├── fig_arch.tex
    ├── fig_dual_plane.tex
    ├── ...
    └── *.png              # Pre-rendered figures
```

## Building

```bash
cd paper

# Chinese version
pdflatex paper.tex && bibtex paper && pdflatex paper.tex && pdflatex paper.tex

# English version
pdflatex paper_en.tex && bibtex paper_en && pdflatex paper_en.tex && pdflatex paper_en.tex
```

> **Note:** Figures are rendered via TikZ at compile time. Some figures use `\includegraphics` with pre-rendered PNGs for complex layouts.

## Citation

```bibtex
@article{ica2026modelnative,
  title={Model-Native Computing Architecture: A Layered Framework for Intelligent Computing Systems},
  author={Lin, Zhan, Zheng},
  year={2026}
}
```

## License

This work is provided for research and academic purposes.
