# 模型原生计算体系结构
#### 借计算机体系结构之镜，展望未来系统架构

> 📄 **arXiv：** <https://arxiv.org/abs/2606.00288>
>
> 📖 **在线阅读（PDF）：**
> [English](https://github.com/ngyygm/llm-computer/raw/master/paper/paper_en_compressed.pdf) ·
> [中文](https://github.com/ngyygm/llm-computer/raw/master/paper/paper_zh_compressed.pdf)

**[English README](README.md)** · ✍️ 林 / 包 / 詹 / 郑海涛（清华大学深圳国际研究生院 / 鹏城实验室）

---

## 一句话概览

大语言模型正从"模型技术"变为"系统技术"。当前主导工程实践的瓶颈——缓存复用、上下文容量、Agent 调度、权限控制——本质都是*系统*问题，而且与经典计算机体系结构八十年解决的问题惊人地相似。**本文把 LLM 栈当成一台计算机来设计**，提出**智能计算体系结构（ICA）**——六层模型 + 接口契约 + 设计公理 + Amdahl 式启发式，并以**双平面**视角统一了"LLM 究竟是 CPU 还是 OS"的旧争论。

## 1 · 核心类比

把模型原生栈映射到经典计算机，每一层都有对应物：

| 经典计算 | 模型原生计算 | 共同的问题 |
|----------|--------------|------------|
| CPU | LLM 推理核心 | 通用计算 |
| 缓存（L1/L2/L3） | KV 缓存 | 热数据复用、访存优化 |
| 虚拟内存 | 上下文窗口 + 外部记忆 | 有限容量的地址空间管理 |
| 操作系统 | Agent 运行时 | 调度、权限、资源管理 |
| I/O 总线（PCIe/USB） | MCP / A2A 协议 | 标准化的外设/工具接入 |
| 应用与用户 | Agent 应用与领域专家 | 把计算变成解决方案 |

<p align="center">
  <img src="assets/fig_analogy_map.jpg" width="92%" alt="类比映射">
  <br><em>类比不止是形似：PagedAttention 直接借用了操作系统的分页，MemGPT 借用了虚拟内存，MCP/A2A 扮演着 I/O 总线的角色。</em>
</p>

但类比不是理论——"像"不等于"是"。本文第一项贡献正是明确界定**类比在何处成立、在何处失效**（例如 KV 缓存随语义序列增长而非按地址索引；上下文"换页"是有损摘要而非无损分页）。

## 2 · 双平面架构

已有工作分成相互冲突的阵营——ArbiterOS 把 LLM 称为*"概率 CPU"*、AIOS 称为*"内核"*、AgentOS 称为*"推理内核"*。本文通过识别**两个正交平面**予以统一：

- **概率执行平面** —— 模型推理与生成：系统*能做什么*。
- **确定性控制平面** —— 调度器、审批、审计日志：系统*应该做什么*。

每一层都穿过这两个平面，在 **L3–L4 附近形成渐变交叉**（而非硬切）。于是各派主张只是同一双平面系统的不同截面。

<p align="center">
  <img src="assets/fig_dual_plane.jpg" width="80%" alt="双平面架构">
  <br><em>双平面架构映射到 ICA 六层。</em>
</p>

## 3 · ICA 六层模型

| 层级 | 名称 | 经典类比 |
|:----:|------|---------|
| L1 | 模型权重与硬件 | CPU / 硅片 |
| L2 | 推理与 KV 缓存服务 | 指令流水线 / 缓存 |
| L3 | 上下文与记忆管理 | 虚拟内存 |
| L4 | 工具与协议接口 | 系统调用 / ABI |
| L5 | Agent 编排 | 操作系统 |
| L6 | 工作流与应用 | 用户应用 |

每对相邻层都有明确的**接口契约**（操作、不变量、度量），模型还带有**六条设计公理**（局部性、分层抽象、概率执行、虚拟化、最小权限、可观测性）。

<p align="center">
  <img src="assets/fig_interface_contracts.jpg" width="95%" alt="层间接口契约">
  <br><em>ICA 的五个层间接口契约。</em>
</p>

## 4 · 三条 Amdahl 式设计启发式

这些被刻意定义为量级估算式的*启发式*（而非已验证的标度律），把 Amdahl 定律的分析形式移植到模型原生场景：

| 启发式 | 公式 | 经典类比 |
|------|------|------|
| **一、语义局部性** | $S = \dfrac{1}{(1-H) + H \cdot \alpha^{-1}}$ | KV 缓存命中率 ↔ Amdahl 定律 |
| **二、上下文预算** | $W_{\text{eff}} = C \cdot \bar{\beta} \le C$ | 工作集模型（Denning） |
| **三、Agent 加速比** | $S_{\text{agent}} = \dfrac{1}{(1-F) + \dfrac{F}{N \cdot E}}$ | Amdahl 定律 + 编排开销 |

它们为以往只能定性描述的设计空间提供可计算的量级直觉（如"KV 命中率从 0.5→0.8 约带来 2× 加速"；"最优 Agent 数 $N^\* \approx \sqrt{F/(E\rho c(1-F)^2)}$"）。

## 5 · Agent 框架五代演进

Agent 框架的历史映射了操作系统的历史，共五代：

| 代次 | 时期 | 标志 | OS 类比 |
|:----:|------|------|---------|
| 一 | 2022–23 | ReAct：思考–行动–观察循环 | 单任务批处理 |
| 二 | 2023 | AutoGPT / Voyager：持久化与目标分解 | 多道程序 |
| 三 | 2023–24 | AIOS / Agents SDK：调度与隔离 | 分时系统 |
| 四 | 2024–25 | Codex / Claude Code：真实软件工程、沙箱 | 现代操作系统 |
| 五 | 2025– | ArbiterOS / CaMeL：治理优先、能力安全 | 可信操作系统 |

<p align="center">
  <img src="assets/fig_agent_timeline_gemini.jpg" width="95%" alt="Agent 框架演进时间线">
  <br><em>五代 Agent 框架，映射了操作系统五十年的成熟之路。</em>
</p>

## 6 · 关键组件深入

本文逐个组件用"经典↔模型原生"的视角分析。两个代表性案例：

<p align="center">
  <img src="assets/fig_comp_51_model.jpg" width="78%" alt="基础模型作为计算核心">
  <br><em>L1 —— 基础模型作为一个通用但<b>概率性</b>的执行核心。</em>
</p>

<p align="center">
  <img src="assets/fig_kv_hierarchy.jpg" width="92%" alt="KV 缓存层级">
  <br><em>L2 —— 正在浮现的 KV 缓存层级（会话级 → 前缀/共享 → 语义级），在结构上平行于 CPU 的 L1/L2/L3 缓存。</em>
</p>

## 7 · 横切设计挑战

五个设计挑战横跨整个栈（延迟–吞吐–成本三难、状态管理、接口漂移、安全与隐私、治理）。双平面视角揭示，例如：**安全首先是运行时结构问题，其次才是提示词问题**：

<p align="center">
  <img src="assets/fig_ch84_security.jpg" width="80%" alt="安全挑战映射到 ICA 各层">
  <br><em>安全与隐私映射到 ICA 各层。</em>
</p>

## 8 · 范式：从硅到基底

CPU 五十年的教训映射到 LLM/Agent 的轨迹——Dennard 缩放与功耗墙 ↔ 推理"能耗墙"；big.LITTLE 异构核 ↔ 异构模型编排；ISA ↔ 基底无关性（"智能 ISA"应让同一 Agent 框架能跑在硅 GPU、神经形态芯片或生物基底上）。分析提炼出两条架构原则：**基底无关性**与**最弱环节原理**（系统的智能受限于其最弱的能力维度）。

<p align="center">
  <img src="assets/fig_paradigm_mhz_analogy.jpg" width="78%" alt="MHz 营销 vs 真实性能">
  <br><em>"MHz 营销"谬误（主频 ≠ 性能）在单指标 LLM 基准测试中找到了回响。</em>
</p>

## 9 · 社会技术：无晶圆厂 AI 产业

把 ICA 各层投射到新兴 AI 产业，可以看出价值在哪里集中、在哪里被商品化：

| 层级 | 经典类比 | 新兴 AI 产业所有者 | 价值捕获 |
|:----:|----------|--------------------|----------|
| L1 | 晶圆厂 + 芯片设计 | 加速器与晶圆厂（GPU/ASIC） | **高**（资本壁垒） |
| L2 | 微架构 / OEM | 推理服务运行时 + 云 | 被商品化 |
| L3 | 内存 OEM | 记忆与检索中间件 | 靠集成差异化 |
| L4 | 总线 / 接口标准 | 开放协议（MCP、A2A） | 赢者通吃多数 |
| L5 | OS 厂商 | Agent-OS 网关（Codex、Claude Code） | **最高** —— 流量入口 |
| L6 | 应用 ISV | 垂直 Agent 应用 | 长尾 |

<p align="center">
  <img src="assets/fig_social_fabless_ai.jpg" width="88%" alt="无晶圆厂 AI 产业结构">
  <br><em>预测中走向"无晶圆厂 AI"的解耦：模型设计者、AI 晶圆厂、OS 平台。</em>
</p>

## 10 · 与已有工作的差异

| 架构构件 | 最近的已有工作 | ICA（本文） |
|----------|----------------|-------------|
| 全栈分层模型 | Mi et al.：模块化映射，无契约 | 六层 + 接口契约 |
| 双平面分离 | ArbiterOS：在"概率 CPU"上加治理层 | 概率 + 确定双平面 |
| 接口契约 | 无（层间仅非正式描述） | 五个形式化契约 |
| 设计公理 | 无 | 六条移植而来的公理 |
| 定量启发式 | 无 | 三条 Amdahl 式启发式 |

<p align="center">
  <img src="assets/fig_coverage_map.jpg" width="90%" alt="现有工作对 ICA 各层的覆盖">
  <br><em>没有任何已有工作覆盖超过栈的一个切片；ICA 整合了全部六层。</em>
</p>

---

## 仓库结构

```
paper/
├── paper_en.tex / paper.tex        # 英文 / 中文主文件
├── paper_en_compressed.pdf         # 英文压缩版 PDF（约 12 MB）
├── paper_zh_compressed.pdf         # 中文压缩版 PDF（约 6 MB）
├── refs.bib                        # 共享参考文献
├── en_sections/  cn_sections/      # 分章节源文件（各 15 个，平行结构）
└── figures/                        # TikZ 源文件 + 渲染图
    ├── fig_*.tex                   # TikZ 源
    ├── *.png                       # 预渲染栅格图
    └── generated/                  # 生成的插图 PNG
assets/                             # 本 README 用的缩略图
```

## 编译

```bash
cd paper
# 英文
pdflatex paper_en.tex && bibtex paper_en && pdflatex paper_en.tex && pdflatex paper_en.tex
# 中文
pdflatex paper.tex && bibtex paper && pdflatex paper.tex && pdflatex paper.tex
```

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

仅供研究与学术用途。
