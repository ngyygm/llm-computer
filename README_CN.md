# 模型原生计算体系结构
#### 借计算机体系结构之镜，展望未来系统架构

> 📄 **arXiv：** <https://arxiv.org/abs/2606.00288>
>
> 📖 **在线阅读（PDF）：**
> [English](https://github.com/ngyygm/llm-computer/raw/master/paper/paper_en_compressed.pdf) ·
> [中文](https://github.com/ngyygm/llm-computer/raw/master/paper/paper_zh_compressed.pdf)

**[English README](README.md)**

> 面向模型原生计算系统的分层架构框架——在八十年经典计算机体系结构与新兴 AI/LLM 生态之间建立系统性类比。

---

## 概述

大语言模型正在从一种"模型技术"演变为一种"系统技术"。当开发者用 Codex 写代码、用 Claude Code 管理项目时，浮现出的工程挑战——缓存复用、上下文容量、Agent 调度、权限控制——与经典计算机系统的问题惊人地相似。这引出一个自然的问题：如果把 LLM 当作 CPU、KV 缓存当作处理器缓存、上下文窗口当作主存、Agent 框架当作操作系统，那么八十年计算机体系结构的智慧，能否用来指导下一代模型原生计算系统的构建？

本文以**技术畅想型综述**的方式展开这一类比，提出**智能计算体系结构（ICA）**——一个统一的分层框架，为模型原生系统提供共享词汇、明确的接口契约、设计公理，以及量级估算式的设计启发式。

### 核心贡献

1. **类比框架** —— 经典计算机体系结构与模型原生栈之间的系统性映射，明确界定类比在何处成立、在何处失效。
2. **双平面架构** —— 解决"LLM 究竟是 CPU 还是操作系统"这一长期争论：将*概率执行平面*（系统"能做什么"）与*确定性控制平面*（系统"应该做什么"）分离。
3. **ICA 六层模型** —— L1–L6，每层有明确的层间接口契约与六条设计公理。
4. **三条 Amdahl 式设计启发式** —— 语义局部性、上下文预算、Agent 加速比，把 Amdahl 定律的分析形式移植到模型原生场景，为以往只能定性描述的设计空间提供可计算的量级直觉（明确声明为启发式，而非已验证的标度律）。
5. **综述、范式映射与路线图** —— 将 CPU 五十年演进的教训映射到 LLM/Agent 的轨迹，分析新兴的"无晶圆厂 AI"产业结构，并给出十年尺度的研究路线图。

## 架构

### ICA 六层模型

| 层级 | 名称 | 经典类比 |
|:----:|------|---------|
| L6 | 工作流与应用 | 用户应用 |
| L5 | Agent 编排 | 操作系统 |
| L4 | 工具与协议接口 | 系统调用 / ABI |
| L3 | 上下文与记忆管理 | 虚拟内存 |
| L2 | 推理与 KV 缓存服务 | 指令流水线 / 缓存 |
| L1 | 模型权重与硬件 | CPU / 硅片 |

### 双平面架构

框架将系统分解为两个正交平面，二者在 **L3–L4 附近形成渐变交叉**，而非硬性切分：

- **概率执行平面**（侧重 L1–L3）：模型推理、KV 缓存、上下文管理——系统"能做什么"
- **确定性控制平面**（侧重 L4–L5）：工具协议、Agent 编排、治理——系统"应该做什么"

这一划分调和了以往的单平面提法（ArbiterOS 的"概率 CPU"、AIOS 的"内核"、AgentOS 的"推理内核"）——它们并非相互矛盾，而是同一双平面系统的不同视角。

### 三条设计启发式

这些被刻意定义为量级估算式的*启发式*（组织性模型），而非已验证的标度律：

| 启发式 | 公式 | 经典类比 |
|------|------|------|
| **一、语义局部性** | $S = \dfrac{1}{(1-H) + H \cdot \alpha^{-1}}$ | 缓存命中率版本的 Amdahl 定律 |
| **二、上下文预算** | $W_{\text{eff}} = C \cdot \bar{\beta} \le C$ | 工作集估计 |
| **三、Agent 加速比** | $S_{\text{agent}} = \dfrac{1}{(1-F) + \dfrac{F}{N \cdot E}}$ | 多 Agent 版本的 Amdahl 定律 |

## 图表

论文配有大量 TikZ 生成的图表。核心图表如下：

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

### 其他关键图表（LaTeX/TikZ 生成）

| 图表 | 说明 |
|------|------|
| `fig_arch` | ICA 六层架构图 |
| `fig_dual_plane` | 双平面架构：概率执行平面 vs 确定性控制平面 |
| `fig_icam_crossmap` | ICA 各层与双平面架构的交叉映射 |
| `fig_law_curves` | 三条设计启发式的特征曲线 |
| `fig_gen_matrix` | 五代 Agent 框架的能力成熟度矩阵 |
| `fig_gen_trajectory` | 五代框架中 $F$、$E$、$S_{\text{agent}}$ 的轨迹 |
| `fig_trilemma` | LLM 服务中的延迟–吞吐量–成本三难困境 |
| `fig_beta_decay` | 注意力保持率 $\beta(L)$：经验 U 形曲线 vs 指数衰减包络 |
| `fig_biglittle` | ARM big.LITTLE ↔ LLM 模型编排 |
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
├── paper_en.tex                 # 英文版（主文件）
├── paper.tex                    # 中文版（主文件）
├── paper_en.pdf / paper.pdf     # 编译后的全分辨率 PDF（英文版经 Git LFS 存储）
├── paper_en_compressed.pdf      # 英文压缩版 PDF（约 12 MB，打印质量）
├── paper_zh_compressed.pdf      # 中文压缩版 PDF（约 6 MB）
├── refs.bib                     # 共享参考文献
├── en_sections/  cn_sections/   # 分章节源文件（平行结构，各 15 个）
└── figures/                     # TikZ 图表源文件 + 渲染图
    ├── fig_*.tex                # TikZ 源文件
    ├── *.png                    # 预渲染栅格图
    └── generated/               # 生成的插图 PNG（Git LFS 存储）
```

## 编译

```bash
cd paper

# 英文版
pdflatex paper_en.tex && bibtex paper_en && pdflatex paper_en.tex && pdflatex paper_en.tex

# 中文版
pdflatex paper.tex && bibtex paper && pdflatex paper.tex && pdflatex paper.tex
```

> **说明：** 图表通过 TikZ 在编译时渲染。`figures/generated/` 下的栅格插图经 Git LFS 存储，克隆后若缺失请运行 `git lfs pull`。

## 引用

```bibtex
@article{lin2026modelnative,
  title   = {Model-Native Computing Architecture: Envisioning Future System Architecture
             Through the Lens of Computer Architecture},
  author  = {Lin, Hai and Pao, Hoilam and Zhan, Shaoxiong and Zheng, Hai-Tao},
  year    = {2026},
  eprint  = {2606.00288},
  archivePrefix = {arXiv},
  url     = {https://arxiv.org/abs/2606.00288}
}
```

## 许可

本作品仅供研究与学术用途。
