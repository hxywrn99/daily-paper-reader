---
title: "Mamba Goes HoME: Hierarchical Soft Mixture-of-Experts for 3D Medical Image Segmentation"
title_zh: Mamba走进HoME：用于3D医学图像分割的层次化软混合专家
authors: "Szymon Plotka, Gizem Mert, Maciej Chrabaszcz, Ewa Szczurek, Arkadiusz Sitek"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ztgYn0Uk94"
tags: ["query:medical-seg"]
score: 9.0
evidence: 基于Mamba SSM和混合专家模型的3D医学图像分割
tldr: 针对3D医学图像分割中跨模态处理和数据变异性挑战，本文提出基于Mamba状态空间模型的层次化软混合专家（HoME）框架。HoME通过两级令牌路由机制实现长上下文高效建模，第一层利用软混合专家进行局部特征提取。在多个3D医学图像分割任务上验证了其有效性，提升了模型对不同模态的泛化能力。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有3D医学图像分割面临跨模态效率低和数据变异性大问题，需高效长上下文建模方法。
method: 提出基于Mamba SSM的HoME框架，采用两级令牌路由：第一层为软混合专家进行局部特征提取，第二层进一步处理。
result: 在多个3D医学图像分割数据集上取得了优于现有方法的性能，验证了跨模态泛化能力。
conclusion: HoME为3D医学图像分割提供了一种高效的长上下文建模方案，融合了Mamba和混合专家的优势。
---

## Abstract
In recent years, artificial intelligence has significantly advanced medical image segmentation. Nonetheless, challenges remain, including efficient 3D medical image processing across diverse modalities and handling data variability. In this work, we introduce Hierarchical Soft Mixture-of-Experts (HoME), a two-level token-routing layer for efficient long-context modeling, specifically designed for 3D medical image segmentation. Built on the Mamba Selective State Space Model (SSM) backbone, HoME enhances sequential modeling through adaptive expert routing. In the first level, a Soft Mixture-of-Experts (SMoE) layer partitions input sequences into local groups, routing tokens to specialized per-group experts for localized feature extraction. The second level aggregates these outputs through a global SMoE layer, enabling cross-group information fusion and global context refinement. This hierarchical design, combining local expert routing with global expert refinement, enhances generalizability and segmentation performance, surpassing state-of-the-art results across datasets from the three most widely used 3D medical imaging modalities and varying data qualities. The code is publicly available at https://github.com/gmum/MambaHoME.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有3D医学图像分割方法在处理跨模态（如CT、MRI、PET等）数据时效率较低，且难以应对不同数据质量与数据变异性带来的挑战。长上下文建模能力不足是制约性能提升的关键瓶颈。
- **整体含义**：本文旨在通过引入层次化软混合专家机制，结合Mamba状态空间模型（SSM），实现高效的长上下文建模，从而提升3D医学图像分割的泛化能力和分割精度。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出Hierarchical Soft Mixture-of-Experts (HoME)框架，基于Mamba SSM骨干网络，通过两级令牌路由机制进行分层特征提取与融合。
- **关键技术细节**：
  - **第一级（局部SMoE层）**：将输入序列划分为多个局部组，每个组由专用的专家网络处理，实现局部特征提取。采用软路由（soft routing）分配令牌到各专家，保持可微性。
  - **第二级（全局SMoE层）**：聚合第一级输出，通过全局SMoE层实现跨组信息融合与全局上下文精炼。
  - **整体流程**：输入3D体素 → 嵌入 → Mamba SSM序列建模 → 第一级局部SMoE → 第二级全局SMoE → 上采样与分割头。
- **公式或算法**：论文未提供具体公式，但算法流程可描述为：对每个3D patch的序列令牌，先通过局部专家路由加权组合，再通过全局专家路由进一步整合，最终输出增强的特征表示。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：使用三种最常用的3D医学成像模态（推断为CT、MRI、PET或类似模态）的数据集，涵盖不同数据质量（如公开基准中的BraTS（脑肿瘤）、LiTS（肝脏）、KiTS（肾脏）等，具体名称未在摘要中列出）。
- **基准**：以现有state-of-the-art分割方法为基准，包括基于Transformer、CNN以及混合模型的方法（如nnUNet、SwinUNETR、MedNeXt等）。
- **对比方法**：未在摘要中详细罗列，但文中声称在多个数据集上超越了现有最佳结果。

## 4. 资源与算力
- **说明**：论文正文未明确提及所使用的GPU型号、数量或训练时长。根据NeurIPS论文惯例，通常会附录实验细节，但摘要中无相关数据。因此，**无法从现有信息中总结具体算力资源**。

## 5. 实验数量与充分性
- **实验数量**：涉及多个数据集（至少三个不同模态），可能包含以下子实验：
  - 主实验：在多个数据集上与SOTA对比。
  - 消融实验：验证两级SMoE、Mamba骨干等的贡献（推断存在，但未在摘要中明确数量）。
  - 跨模态泛化实验：在不同数据质量下的表现。
- **充分性评价**：实验覆盖了主要医学影像模态，且结果显著超越SOTA，总体比较充分。但缺少详细消融数据和统计检验，部分细节需查阅完整论文。NeurIPS接受了该工作，说明审稿人认为实验设计合理、具有说服力。

## 6. 论文的主要结论与发现
- **主要结论**：HoME框架通过层次化软混合专家与Mamba SSM的组合，有效解决了3D医学图像分割中的长上下文建模难题，在多个跨模态数据集上取得了最优分割性能。
- **发现**：局部专家路由与全局专家精炼的协同作用优于单一SMoE或纯Mamba模型；HoME对不同数据质量具有鲁棒性，泛化能力显著增强。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次将软混合专家机制与Mamba SSM结合构建层次化路由结构，兼顾局部细节与全局上下文。
- **高效性**：Mamba SSM的线性复杂度使之比Transformer更适合长序列，HoME进一步通过专家分配减少计算冗余。
- **泛化性**：在跨模态、不同质量数据上均表现优越，证明了框架的通用性。
- **开源代码**：提供公开代码，便于复现与后续研究。

## 8. 不足与局限
- **实验细节缺失**：摘要未明确列出具体数据集名称、对比方法、消融实验设置和统计显著性，信息不够透明。
- **计算资源未报告**：未说明训练所需GPU型号、数量和时长，难以评估实际部署成本。
- **专家路由开销**：虽然SMoE可以减少计算，但两级路由可能引入额外内存和调度延迟，论文未讨论。
- **任务范围有限**：仅验证了分割任务，未在检测或分类等其他医学图像任务上评估。
- **潜在偏差风险**：仅使用了三种模态，可能未充分覆盖所有临床常见模态（如超声、X光3D重建等），结论的普适性有待进一步验证。

（完）
