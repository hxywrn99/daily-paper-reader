---
title: "ControlFuse: Instruction-guided Multi-Granularity Controllable Image Fusion"
title_zh: ControlFuse：指令引导的多粒度可控图像融合
authors: "Libo Zhao, Xiaoli Zhang, Zeyu Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38321/42283"
tags: ["query:image-fusion"]
score: 9.0
evidence: 指令引导的可控图像融合
tldr: 针对现有红外-可见光融合方法输出固定、缺乏用户控制的问题，提出ControlFuse框架。通过引入文本指令和实例级空间标注，实现多粒度可控融合。设计细粒度跨模态对齐方法，精确匹配指令与视觉特征。实验表明ControlFuse在多种控制粒度下均保持高质量融合，并支持灵活的用户交互。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有红外-可见光融合方法无法根据用户需求灵活调节融合结果，缺少实例级控制。
method: 提出ControlFuse，利用文本指令和细粒度跨模态对齐实现多粒度可控融合。
result: 在多种控制任务下，ControlFuse能在保持融合质量的同时实现精准的实例级控制。
conclusion: 引入指令引导可显著增强融合方法的灵活性和用户交互能力。
---

## Abstract
Infrared and Visible Image Fusion (IVIF) produces enhanced images by fusing complementary visual information. However, most existing methods generate fixed outputs and cannot flexibly adapt to user-specific requirements. Recent text-guided approaches offer partial control but are limited to global or semantic levels, lacking instance-level control. This limitation arises from two challenges: first, the lack of datasets that directly link textual instructions with corresponding spatial annotations, and second, the use of coarse cross-modal alignment methods that struggle to precisely match textual instructions with visual features. To overcome these challenges, we propose ControlFuse, a controllable IVIF framework enabling multi-granularity fusion across global, semantic, and instance levels, guided by user instructions. First, we construct an automated multi-granularity dataset that provides explicit textual-mask correspondences at these three levels. Second, inspired by manifold geometry, we design a Multimodal Feature Interaction Module (MFIM) comprising Feature Manifold Converter (FMC) and Curvature-Guided Interaction (CGI). FMC projects textual and visual features into a unified manifold space, while CGI leverages manifold curvature as a geometric cue to refine cross-modal alignment. Extensive experiments validate ControlFuse, outperforming state-of-the-art methods in robustness and flexibility.

---

## 论文详细总结（自动生成）

# ControlFuse 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题背景**：红外与可见光图像融合（IVIF）旨在融合两种模态的互补信息，但现有大多数方法输出固定，无法根据用户具体需求灵活调整。近期的文本引导方法虽提供了部分控制，但仅限于全局或语义级别，缺乏实例级控制能力。
- **两个关键挑战**：
  1. 缺乏直接链接文本指令与对应空间标注的数据集；
  2. 现有跨模态对齐方法（如全局交叉注意力、元素级乘积等）过于粗糙，难以精确匹配文本指令与视觉特征。
- **研究目标**：提出 ControlFuse 框架，首次实现全局、语义、实例三个粒度的用户指令可控融合。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 构建多粒度数据集，提供全局、语义、实例级别的文本-掩码对应关系。
- 设计多模态特征交互模块（MFIM），通过流形几何中的曲率信息实现细粒度跨模态对齐。

### 关键技术细节
1. **多粒度数据集自动构建**（Algorithm 1）：
   - 使用 GroundingDINO 和红外检测器生成候选框，融合后经 SAM 得到实例掩码。
   - 按类别分组得到语义掩码，全图像域作为全局掩码。
   - 为每个级别生成自然语言指令（全局、语义、实例），并合成负样本指令用于对比学习。
2. **融合模型结构**（Fig. 2）：
   - 多模态特征编码：Restormer 提取可见光和红外特征；BLIP-2（冻结）提取文本特征。
   - 多模态特征交互模块（MFIM）：
     - **特征流形转换器（FMC）**：将视觉和文本特征通过图注意力网络（GAT）投影到统一流形空间。
     - **曲率引导交互（CGI）**：分别计算视觉特征的空间曲率（二阶空间导数）和文本特征的序列曲率；利用曲率值调制交叉注意力权重，生成对象定位掩码。
   - 重建：由 Restormer 块和卷积组成的多尺度解码器。
3. **损失函数**：
   - 目标定位损失（Lm）：预测掩码与 GT 掩码的二元交叉熵。
   - 内容保真损失（Lf）：L1 损失 + SSIM 损失。
   - 曲率对齐损失（La）：基于曲率加权的三模态嵌入距离对比损失（正负样本对）。
   - 总损失：采用不确定性自适应加权动态平衡三项损失。

## 3. 实验设计

- **数据集与划分**：
  - RoadScene：221 对（171 训练，50 测试）
  - M3FD：4204 对（3780 训练，424 测试）
  - MSRS：1444 对（1083 训练，361 测试）
- **评价指标**：EN（信息熵）、SD（标准差）、SF（空间频率）、AG（平均梯度）、VIF（视觉保真度）、Qabf（基于梯度的融合质量）。
- **对比方法**：EMMA、Text-IF、TextFusion、MTG-Fusion、OmniFuse、DCEvo、SAGE（均为 2024-2025 年顶会/顶刊方法）。
- **下游任务评估**：
  - 目标检测：M3FD 数据集上使用 YOLOv12，评价 mAP@.5。
  - 语义分割：MSRS 数据集上使用 RTFNet，评价 IoU 和 mIoU。

## 4. 资源与算力

- **明确信息**：使用 NVIDIA GeForce RTX 4090 GPU，PyTorch 框架，AdamW 优化器，训练 100 个 epoch，初始学习率 1×10⁻⁴，批量大小 8，学习率每 20 epoch 减半。
- **未明确信息**：未说明使用的 GPU 数量、总训练时间、显存占用等细节。

## 5. 实验数量与充分性

- **实验组数**：
  - 定性比较（Fig.3）：三个场景（低光、低质、高亮）的可视化。
  - 定量比较（Table 1）：三个数据集 × 六个指标 × 八种方法，共 144 个数据点。
  - 可控性比较（Fig.4）：语义/实例级指令与 TextFusion、OmniFuse 对比。
  - 下游任务（Table 2, Fig.5）：目标检测和语义分割的多粒度指令效果。
  - 消融实验（Table 3）：
    - 多模态交互模块（FMC/CGI 的三种变体 + 基线）
    - 损失项（去掉 Lm、La、Lf 及全量）
  - 负样本消融（Fig.6）：训练过程中 IoU/F1 曲线的对比。
- **充分性评价**：
  - 覆盖主流数据集和 SOTA 方法，指标全面；消融实验验证了各组件和损失的必要性；下游任务验证了实际应用收益。
  - 不足：未在更多复杂场景（如动态背景、遮挡严重场景）下测试，也未进行跨数据集泛化实验。

## 6. 论文的主要结论与发现

- **主要结论**：ControlFuse 在融合质量和可控性上全面超越现有传统融合和文本引导方法，尤其在下游任务（检测、分割）中由于实例级控制带来显著提升。
- **发现**：
  - 流形曲率作为几何线索能有效引导细粒度跨模态对齐。
  - 多粒度指令可实现从全局到实例的灵活控制，且不同粒度指令彼此不冲突。
  - 负样本对比学习对提升指令-区域匹配精度至关重要。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 首次在 IVIF 中实现实例级文本可控融合，填补了领域空白。
  - 自动构建多粒度数据集流程高效，无需人工标注，可扩展。
  - MFIM 中的流形曲率引导机制新颖，几何驱动对齐优于传统粗糙对齐。
  - 损失函数设计兼顾定位、内容保真和跨模态对齐，并采用不确定性自适应加权。
- **实验亮点**：
  - 下游任务验证了可控融合的实际价值，增强说服力。
  - 消融实验设计完整，证实各组件贡献。
  - 负样本消融以动态曲线展示训练过程，直观清晰。

## 8. 不足与局限

- **数据集局限**：总计约 5000+ 对训练数据，规模有限，且自动标注可能引入噪声（如 SAM 和检测器的误差）；未在更大规模的如 LLVIP、FLIR 等数据集上验证。
- **方法局限**：
  - BLIP-2 冻结，可能在特定场景语义理解上受限。
  - 仅支持单句指令，未探索多指令或交互式对话融合。
  - 融合速度未报告，可能影响实时应用。
- **实验不足**：
  - 缺少跨数据集泛化实验（如用 RoadScene 训练在 M3FD 测试）。
  - 未进行真实用户调研或主观评价。
  - 未讨论失败案例（如小目标、遮挡目标时的可控性下降）。
- **公平性**：对比方法中的某些（如 OmniFuse）在论文中评价指标计算可能存在微小差异，但定性比较基本合理。

（完）
