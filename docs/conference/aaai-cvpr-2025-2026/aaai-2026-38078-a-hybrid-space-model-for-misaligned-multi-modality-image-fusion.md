---
title: A Hybrid Space Model for Misaligned Multi-modality Image Fusion
title_zh: 用于未对齐多模态图像融合的混合空间模型
authors: "Yi Xiao, Jia Wang, Zhu Liu, Di Wang, Jinyuan Liu, Risheng Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38078/42040"
tags: ["query:image-fusion"]
score: 9.0
evidence: 混合空间模型用于未对齐多模态图像融合
tldr: 针对现实多模态图像融合中未对齐和几何变形导致伪影的问题，提出联合欧几里得与双曲空间的融合框架。欧几里得空间保留局部细节，双曲空间建模层次结构，通过联合优化处理未对齐。实验表明该方法在多种未对齐场景下融合质量显著优于仅使用欧几里得空间的方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法仅在欧几里得空间提取特征，忽略了多模态表征中的层次结构，且难以处理未对齐。
method: 提出联合优化欧几里得和双曲空间的框架，分别处理局部细节和层次结构，并引入对齐模块。
result: 在红外-可见光融合等任务上，该方法有效降低未对齐伪影，提升融合质量。
conclusion: 混合空间建模能更鲁棒地处理多模态融合中的未对齐问题。
---

## Abstract
Infrared and visible image fusion aims to integrate complementary information, such as thermal saliency from infrared imagery and fine-grained texture details from visible imagery. However, real-world multi-modal misalignment and geometric deformation often introduce severe artifacts. Most existing methods focus on feature extraction within Euclidean space, thereby neglecting the inherent hierarchical structures embedded in multimodal representations. While Euclidean space excels at preserving local structural details and supporting efficient computation, hyperbolic space is naturally suited for modeling hierarchical relationships due to its geometric properties. Building upon these observations, this paper proposes a unified framework that jointly optimizes image registration and fusion through a dual-space architecture. This architecture synergistically combines the local fidelity of Euclidean geometry with the hierarchical modeling capability of hyperbolic geometry to enhance multimodal representation learning. Specifically, this paper introduces Hyperbolic Coupled Contrastive Learning Optimization (HCCLO), which aligns and optimizes the hierarchical structures of infrared and visible embeddings in hyperbolic space. Moreover, this paper designs a task-adaptive dual-space features fusion mechanism, which dynamically balances and fuses Euclidean local features with hyperbolic hierarchical representations, thereby improving adaptability for downstream tasks. Extensive experiments on misaligned multimodal datasets demonstrate that our method achieves state-of-the-art performance, while effectively capturing both spatial dependencies and hierarchical semantics.

---

## 论文详细总结（自动生成）

# 用于未对齐多模态图像融合的混合空间模型：详细总结

## 1. 论文的核心问题与整体含义

- **研究背景**：红外与可见光图像融合旨在结合红外图像的热显著性信息和可见光图像的精细纹理细节。然而，真实场景中多模态图像常存在未对齐（misalignment）和几何变形问题，导致融合结果出现严重伪影（如边缘重影、结构扭曲）。
- **现有局限**：大多数现有方法仅在欧几里得空间提取特征，忽视多模态表征中固有的层次结构。欧几里得空间擅长保留局部细节，但无法有效建模层次关系；而双曲空间因负曲率性质，天然适合表示树状层次结构。
- **核心动机**：结合欧几里得和双曲空间各自的优势，通过联合优化配准与融合，解决未对齐多模态图像融合中的伪影和结构失真问题。

## 2. 论文提出的方法论

### 核心思想
提出一个统一的**双空间架构（HMMF）**，将欧几里得空间的局部保真性与双曲空间的层次建模能力协同起来，同时优化图像配准与融合。

### 关键技术细节

1. **整体流程**（图2）：
   - 共享编码器：提取模态不变特征，降低跨模态差异。
   - 全局变换矩阵预测：对未对齐图像进行粗配准。
   - 双空间特征提取：
     - 欧几里得编码器：使用Transformer-CNN结构+多窗口注意力机制，捕捉细粒度全局特征。
     - 双曲编码器：通过HCCLO模块，在双曲空间提取层次特征并进行跨模态对齐。
   - 任务自适应双空间特征融合（TDFF）：动态学习权重，融合欧几里得局部特征和双曲层次特征。
   - 两个解码器：配准解码器（多尺度密集子网络）和融合解码器（双通道注意力），分别输出配准后的红外图像和最终融合图像。

2. **双曲空间基础操作**：
   - **Poincaré球模型**：具有负曲率 \(c\)，可训练。
   - **Möbius加法**：保持双曲距离的非欧运算。
   - **指数映射/对数映射**：实现欧几里得空间与双曲空间的特征转换。

3. **HCCLO（双曲耦合对比学习优化）**：
   - 利用SAM(分割任意模型)生成的显著性掩码 \(M\) 及其补集，将特征划分为前景和背景。
   - 红外分支对比损失 \(L_{ir}\)：拉近红外前景特征与其自身，推远与可见光前景特征（因可见光前景信息不足）。
   - 可见光分支对比损失 \(L_{vis}\)：拉近可见光背景特征与其自身，推远与红外背景特征（因红外背景细节缺失）。
   - 损失函数形式：\(\max(d_p(\cdot,\cdot) - d_p(\cdot,\cdot) + \varepsilon, 0)\)。

4. **TDFF（任务自适应双空间特征融合）**：
   - 通道压缩层：通过卷积+BN+LeakyReLU将双曲和欧几里得特征降维。
   - 自适应权重生成：拼接后经Softmax生成两个空间权重图（和为1）。
   - 特征融合：加权求和得到最终配准/融合特征。

## 3. 实验设计

- **数据集**：
  - 训练集：RoadScene、M3FD、MSIFT（随机采样128×128 patches）。
  - 测试集：TNO（24张）、M3FD、RoadScene。
  - 模拟未对齐：对红外图像施加仿射变换和弹性变换（平移0.0~0.01 px；弹性σ=32, kernel 97~101）。

- **基准与对比方法**：
  - 配准方法：CrossRAFT (AAAI'22)。
  - 融合方法：UMFusion (IJCAI'22)、SuperFusion (IEEE JAS'22)、MURF (TPAMI'23)、CDDFuse (CVPR'23)、IMF (TCSVT'24)、CoCoNet (IJCV'24)、MulFS-CAP (TPAMI'25)。
  - 对于CDDFuse和CoCoNet（无内置配准模块），使用CrossRAFT作为预配准基线，确保公平比较。

- **评价指标**：
  - 配准：NCC、MI。
  - 融合：QNCIE、MI、VIF、Qabf（指标越高越好）。

## 4. 资源与算力

- 论文明确说明：在**NVIDIA V100 GPU**上训练，使用Adam优化器（初始学习率1e-4，每300 epoch衰减），总训练**1200 epochs**。
- **未明确说明**：批大小（batch size）、显存消耗、每次epoch时长、是否有多个GPU等详细信息。

## 5. 实验数量与充分性

- **定量实验**：Table 1（融合结果）涵盖三个测试集（TNO、M3FD、RoadScene）上4项指标；Table 2（配准结果）覆盖两个数据集（TNO、RoadScene）上2项指标。
- **定性实验**：图3为配准误差图（RoadScene），图4为融合结果可视化（RoadScene），直观对比伪影和细节保留。
- **消融实验**：
  - Table 3：在RoadScene上验证HCCLO的有效性（比较有无HCCLO的指标提升）。
  - 比较单空间欧几里得(E)、单空间双曲(H)和双空间(E+H+TDFF)，证明双空间优势。
  - 图5可视化HCCLO消除重影效果，图6展示TDFF的融合效果。
- **充分性**：实验覆盖多个主流数据集，对比方法全面（8种），且通过统一配准基线保证了公平性。消融实验针对核心模块均进行验证。不足是消融实验仅在RoadScene上量化，未在其他数据集上重复。

## 6. 论文的主要结论与发现

- 提出的双空间框架（HMMF）在未对齐多模态图像融合中达到**SOTA性能**：在TNO、M3FD、RoadScene数据集上，所有四类融合指标（QNCIE、MI、VIF、Qabf）均显著优于现有方法，配准指标NCC也最高。
- **HCCLO**有效增强了跨模态层次结构对齐，减少了重影伪影。
- **TDFF**动态平衡欧几里得局部细节与双曲层次特征，提升了下游任务适应性。
- 验证了双曲空间建模层次结构对未对齐多模态融合的鲁棒性。

## 7. 优点

- **创新性**：首次将双曲空间引入未对齐多模态图像融合，结合欧几里得和双曲空间优势，提供新视角。
- **方法设计严谨**：HCCLO利用SAM掩码自动构建正负样本，对比损失在双曲空间中进行，符合层次结构特性。
- **端到端联合优化**：配准与融合互促，避免了传统"先配准后融合"的级联误差。
- **任务自适应**：TDFF动态权重机制使模型灵活适应不同下游视觉任务。
- **实验充分客观**：使用统一配准基线，多种指标和数据集比较，并进行了可视化与消融分析。

## 8. 不足与局限

- **计算开销**：双曲空间中的指数/对数映射、Möbius运算等可能带来额外计算负担，文中未分析推理速度或参数量。
- **依赖SAM**：HCCLO依赖SAM生成显著性掩码，SAM的性能可能影响模型鲁棒性，且SAM本身需要一定计算资源。
- **泛化性验证不足**：仅测试红外-可见光融合场景，未在其他多模态对（如RGB-深度、医学图像）上验证；此外未探讨极端的未对齐程度（如大幅旋转或遮挡）。
- **消融实验覆盖有限**：Table 3仅基于RoadScene数据集进行量化消融，未在TNO或M3FD上重复，可能影响结论普适性。
- **指标局限性**：虽使用多种评价指标，但这些指标与人类视觉感知的关联性仍有争议，缺乏用户或下游任务（如检测、分割）的直接验证。
- **代码与可复现性**：提供了GitHub仓库，但未提及是否公开发布预训练模型或训练细节（如批大小、随机种子）。

（完）
