# 模型原生计算体系结构

**[English](README.md)**

> 面向模型原生计算系统的分层架构框架——在经典计算机体系结构与新兴 AI/LLM 生态之间建立系统性类比。

## 概述

大语言模型（LLM）正在从一种"模型技术"演变为一种"系统技术"。本文提出了**智能计算体系结构（ICA）**——一个六层模型，为将智能组织成稳定、可扩展、可审计的系统提供架构基础。

### 核心贡献

1. **ICA 六层模型** — 统一框架（L1–L6），每层有明确的接口契约、设计公理和度量指标
2. **双平面架构** — 将概率式执行（模型推理）与确定性控制（Agent 运行时治理）分离
3. **三条量化定律** — 语义局部性定律、上下文预算定律、Agent 加速比定律，类比传统体系结构中的 Amdahl 定律
4. **范式映射** — 将 CPU 五十年演进的经典教训系统性映射到 LLM/Agent 系统的演进路径
5. **社会技术分析** — 通过半导体和 PC 产业的解耦历史分析 AI 栈的产业结构

## 架构

### ICA 六层模型

| 层级 | 名称 | 经典类比 |
|:----:|------|---------|
| L6 | 工作流与应用 | 用户应用 |
| L5 | Agent 编排 | 操作系统 |
| L4 | 工具与协议接口 | 系统调用 / ABI |
| L3 | 上下文与记忆管理 | 虚拟内存 |
| L2 | 推理与 KV 缓存服务 | 指令流水线 |
| L1 | 模型权重与硬件 | CPU / 硅片 |

### 双平面架构

框架将系统分解为两个正交平面：

- **概率执行平面**（L1–L3）：模型推理、KV 缓存、上下文管理——系统"能做什么"
- **确定性控制平面**（L4–L5）：工具协议、Agent 编排、治理——系统"应该做什么"

### 三条量化定律

| 定律 | 公式 | 类比 |
|------|------|------|
| **定律一：语义局部性** | $S = \frac{1}{(1-H) + H \cdot \alpha^{-1}}$ | 缓存命中率版本的 Amdahl 定律 |
| **定律二：上下文预算** | $W_{\text{eff}} \leq C \cdot \beta(L)$ | 工作集估计 |
| **定律三：Agent 加速比** | $S_{\text{agent}} = \frac{1}{(1-F) + \frac{F}{N \cdot E}}$ | 多 Agent 版本的 Amdahl 定律 |

## 图表

论文包含 27 幅 TikZ 生成的图表。核心图表如下：

### 核心架构

<p align="center">
  <img src="paper/figures/fig_analogy_map.png" width="90%" alt="计算机体系结构与模型原生计算系统的类比映射">
  <br><em>经典计算 ↔ 模型原生计算的类比映射</em>
</p>

<p align="center">
  <img src="paper/figures/fig_kv_hierarchy.png" width="90%" alt="CPU 缓存层级与 LLM KV 缓存层级的结构对应">
  <br><em>CPU 缓存层级 ↔ LLM KV 缓存层级的结构对应</em>
</p>

### Agent 演进

<p align="center">
  <img src="paper/figures/fig_agent_timeline_gemini.png" width="90%" alt="Agent 框架五代演进时间线">
  <br><em>Agent 框架演进时间线（第一代 → 第五代）</em>
</p>

<p align="center">
  <img src="paper/figures/fig_impl_map_gemini.png" width="90%" alt="开源生态系统映射到 ICA 六层架构">
  <br><em>开源生态系统映射到 ICA 六层架构</em>
</p>

### 其他关键图表（LaTeX/TikZ 生成）

| 图表 | 说明 |
|------|------|
| `fig_arch` | ICA 六层架构图 |
| `fig_dual_plane` | 双平面架构：概率执行平面 vs 确定性控制平面 |
| `fig_icam_crossmap` | ICA 各层与双平面架构的交叉映射 |
| `fig_law_curves` | 三条量化定律的特征曲线 |
| `fig_gen_matrix` | 五代 Agent 框架的能力成熟度矩阵 |
| `fig_gen_trajectory` | 五代框架中 $F$、$E$、$S_{\text{agent}}$ 的量化轨迹 |
| `fig_trilemma` | LLM 服务中的延迟–吞吐量–成本三难困境 |
| `fig_beta_decay` | 注意力保持率 $\beta(L)$：经验 U 形曲线 vs 指数衰减模型 |
| `fig_biglittle` | 三阶段演进：ARM big.LITTLE ↔ LLM 模型编排 |
| `fig_paradigm_timeline` | 从硅基到基底无关计算的范式时间线 |
| `fig_interface_contracts` | ICA 层间接口契约 |
| `fig_axioms_overview` | 六条设计公理在各层的适用性热力图 |
| `fig_classical_stack` | 经典计算栈的六层抽象层次结构 |
| `fig_context_tiers` | 热/温/冷分层上下文管理层次结构 |
| `fig_state_lifecycle` | 三层 Agent 状态生命周期 |
| `fig_impl_recommendations` | 五条实现建议映射到 ICA 各层 |
| `fig_roadmap_swimlane` | 按 ICA 层级和时间跨度组织的研究路线图 |
| `fig_dual_stack` | 经典栈 vs 模型原生栈的层级对比 |
| `fig_flow` | 请求在架构中的双平面泳道流图 |
| `fig_coverage_map` | 现有工作对 ICA 各层的覆盖情况 |
| `fig_security_layers` | 安全层次图 |

## 仓库结构

```
paper/
├── paper.tex              # 中文版
├── paper_en.tex           # 英文版
├── paper.pdf              # 编译后的中文 PDF
├── paper_en.pdf           # 编译后的英文 PDF
├── refs.bib               # 共享参考文献
├── cn_sections/           # 中文章节（15 个文件）
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
├── en_sections/           # 英文章节（15 个文件，平行结构）
│   └── ...
└── figures/               # TikZ 图表源文件 + 渲染后的 PNG
    ├── fig_arch.tex
    ├── fig_dual_plane.tex
    ├── ...
    └── *.png              # 预渲染图表
```

## 编译

```bash
cd paper

# 中文版
pdflatex paper.tex && bibtex paper && pdflatex paper.tex && pdflatex paper.tex

# 英文版
pdflatex paper_en.tex && bibtex paper_en && pdflatex paper_en.tex && pdflatex paper_en.tex
```

> **说明：** 图表通过 TikZ 在编译时渲染。部分复杂布局的图表使用 `\includegraphics` 引入预渲染的 PNG 文件。

## 引用

```bibtex
@article{ica2026modelnative,
  title={模型原生计算体系结构：面向智能计算系统的分层框架},
  author={林, 詹, 郑},
  year={2026}
}
```

## 许可

本作品仅供研究与学术用途。
