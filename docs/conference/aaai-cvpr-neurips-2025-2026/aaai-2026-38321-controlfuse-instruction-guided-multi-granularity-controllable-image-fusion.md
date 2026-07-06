---
title: "ControlFuse: Instruction-guided Multi-Granularity Controllable Image Fusion"
title_zh: ControlFuse：指令引导的多粒度可控图像融合
authors: "Libo Zhao, Xiaoli Zhang, Zeyu Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38321/42283"
tags: ["query:image-fusion"]
score: 9.0
evidence: 指令引导的可控图像融合
tldr: 该论文针对现有图像融合方法输出固定、无法满足用户特定需求的问题，提出ControlFuse，一种指令引导的多粒度可控红外-可见光图像融合框架。通过引入文本指令与空间标注的关联数据集和精细跨模态对齐方法，实现实例级控制，赋予融合过程灵活性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38321/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 876, \"height\": 354, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38321/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1813, \"height\": 806, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38321/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1799, \"height\": 579, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38321/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 882, \"height\": 355, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38321/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 881, \"height\": 685, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38321/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 881, \"height\": 355, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38321/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1835, \"height\": 361, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38321/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1835, \"height\": 472, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38321/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 862, \"height\": 404, \"label\": \"Table\"}]"
motivation: 现有方法输出固定，缺乏对用户特定需求的适应能力。
method: 构建文本指令与空间标注对齐的数据集和精细跨模态对齐网络。
result: 在可控融合任务中实现高质量的实例级操控。
conclusion: 指令引导为图像融合提供了灵活且实用的控制方式。
---

## Abstract
Infrared and Visible Image Fusion (IVIF) produces enhanced images by fusing complementary visual information. However, most existing methods generate fixed outputs and cannot flexibly adapt to user-specific requirements. Recent text-guided approaches offer partial control but are limited to global or semantic levels, lacking instance-level control. This limitation arises from two challenges: first, the lack of datasets that directly link textual instructions with corresponding spatial annotations, and second, the use of coarse cross-modal alignment methods that struggle to precisely match textual instructions with visual features. To overcome these challenges, we propose ControlFuse, a controllable IVIF framework enabling multi-granularity fusion across global, semantic, and instance levels, guided by user instructions. First, we construct an automated multi-granularity dataset that provides explicit textual-mask correspondences at these three levels. Second, inspired by manifold geometry, we design a Multimodal Feature Interaction Module (MFIM) comprising Feature Manifold Converter (FMC) and Curvature-Guided Interaction (CGI). FMC projects textual and visual features into a unified manifold space, while CGI leverages manifold curvature as a geometric cue to refine cross-modal alignment. Extensive experiments validate ControlFuse, outperforming state-of-the-art methods in robustness and flexibility.

---

## 论文详细总结（自动生成）

# 论文总结：ControlFuse：指令引导的多粒度可控图像融合

## 1. 核心问题与整体含义（研究动机与背景）

现有红外-可见光图像融合（IVIF）方法大多生成固定输出，缺乏对用户特定需求的灵活性。近年来的文本引导方法虽提供了部分控制能力，但仅局限于全局或语义层面，无法实现实例级控制。这一局限源于两个挑战：一是缺乏直接将文本指令与对应空间标注关联的数据集；二是现有跨模态对齐方法（如全局交叉注意力、元素级乘积等）较粗糙，难以精确匹配文本指令与视觉特征。为此，作者提出 ControlFuse，首个支持全局、语义、实例三粒度指令引导的可控 IVIF 框架，旨在实现灵活、精准的融合控制。

## 2. 方法论

### 核心思想
- 通过构建多粒度文本-掩码对数据集，为不同粒度控制提供监督信号。
- 设计多模态特征交互模块（MFIM），利用流形几何特性实现精细跨模态对齐。

### 关键技术细节
- **多粒度数据集自动构建**：利用 GroundingDINO 和 SAM 自动生成全局、语义、实例级别的掩码，并配对应自然语言指令。
- **多模态特征交互模块（MFIM）**：
  - **特征流形转换器（FMC）**：将视觉与文本特征投影到统一流形空间，通过图注意力网络（GAT）保持几何与语义关系。
  - **曲率引导交互（CGI）**：利用流形曲率作为几何线索，计算视觉曲率（图像中边缘、轮廓）和文本曲率（语义过渡点），调制交叉注意力权重，实现细粒度对齐。
- **损失函数**：
  - 对象定位损失（Lm）：二元交叉熵监督掩码预测。
  - 内容保真度损失（Lf）：L1 + SSIM 损失。
  - 曲率对齐损失（La）：对比学习，正负三元组加权，强调结构显著区域与语义区分性 token。
  - 总损失采用不确定性自适应加权，平衡三项损失。

### 算法流程
1. 多模态特征编码：Restormer 提取可见光/红外视觉特征，冻结 BLIP-2 提取文本嵌入。
2. FMC 投影至统一流形；CGI 生成位置图。
3. 位置图调制视觉特征，进行交叉注意力融合。
4. 多尺度解码器重建融合图像。

## 3. 实验设计

- **数据集**：
  - 训练集：RoadScene（171对）、M3FD（3780对）、MSRS（1083对）共5034对。
  - 测试集：RoadScene（50对）、M3FD（424对）、MSRS（361对）。
- **评测指标**：EN、SD、SF、AG、VIF、Qabf 六个客观指标；下游任务使用 mAP@.5（检测）和 mIoU（分割）。
- **对比方法**：
  - 传统/深度学习方法：EMMA、Text-IF、TextFusion、MTG-Fusion、OmniFuse、DCEvo、SAGE 等 8 种 SOTA。
- **下游任务**：目标检测（YOLOv12 在 M3FD 上评估）、语义分割（RTFNet 在 MSRS 上评估）。
- **消融实验**：
  - MFIM 模块消融：FMC-only、CGI-only、baseline。
  - 损失项消融：去掉 Lm、Lf、La 之一或全部。
  - 负提示（negative prompt）消融：分析其对对齐性能的影响。

## 4. 资源与算力

论文明确说明：实验在 **NVIDIA GeForce RTX 4090 GPU** 上进行，使用 **PyTorch** 框架。训练 **100 个 epoch**，初始学习率 1e⁻⁴，每 20 epoch 衰减 0.5 倍，batch size 为 8。未提及使用 GPU 数量、显存大小或训练时长，也未说明是否使用了多卡并行。

## 5. 实验数量与充分性

- **数量**：在 3 个独立数据集上进行了定量与定性对比，每组包含 6 个指标；下游任务在两个数据集上分别做了检测和分割评测。消融实验包括模块消融、损失项消融、负提示消融，共 3 组主要消融。
- **充分性**：
  - 对比方法覆盖了近年主流文本引导和传统融合方法，指标全面。
  - 消融实验验证了每个设计组件的必要性，并展示了负提示的贡献。
  - 下游任务评估证明了实际应用价值。
- **公平性**：在全局指令下与基线方法公平对比；对于可控性对比，也展示了语义/实例级指令下的定性结果。但在定量对比中，其他方法无法直接支持实例级指令，因此仅定性展示，这在一定程度上限制了公平性。

## 6. 主要结论与发现

- ControlFuse 在三个标准数据集上，全局指令下在多数指标（EN、SD、SF、AG、VIF、Qabf）上达到最优或次优，显著优于先前方法。
- 在目标检测任务中，多粒度指令（尤其是语义和实例级）能提升检测精度（mAP@.5 达 0.666），优于所有对比方法。
- 在语义分割任务中，多粒度指令也带来更高 mIoU（0.496）。
- 消融实验证实 FMC 与 CGI 互补，三者损失项缺一不可，负提示有助于提升跨模态对齐。

## 7. 优点

- **新颖性**：首次在 IVIF 中实现全局-语义-实例三粒度指令控制，填补了实例级精细控制的空白。
- **数据贡献**：自动构建了多粒度文本-掩码数据集，解决了标注稀缺问题，可复用。
- **方法创新**：将流形几何（曲率）引入跨模态对齐，设计精巧的 FMC 和 CGI 模块，理论上合理且实验有效。
- **实验全面**：覆盖多个数据集、多种指标、下游任务验证，消融实验充分，结果可信。

## 8. 不足与局限

- **数据集规模**：虽然构建了多粒度标注，但训练集总量约 5000 对，相对于真实场景的多样性可能仍有不足，尤其是复杂遮挡、多目标场景下的泛化能力未充分验证。
- **负提示构造**：论文提到“存储而不配 ground-truth 掩码”，但未详细说明负提示的具体生成策略对性能波动的敏感度。
- **对比公平性**：在实例级指令下，基线方法无法直接支持，因此仅定性对比；在指标上未提供其他方法在实例级控制下的量化结果。
- **计算开销**：未报告推理速度或模型参数量，实时性未知。
- **应用风险**：依赖 BLIP-2 等预训练视觉语言模型，可能继承其偏见或对不常见指令（如隐喻、复杂空间关系）的鲁棒性不足。
- **仅评估特定下游任务**：未测试如跟踪、识别等其他高级任务，实际应用范围有限。

（完）
