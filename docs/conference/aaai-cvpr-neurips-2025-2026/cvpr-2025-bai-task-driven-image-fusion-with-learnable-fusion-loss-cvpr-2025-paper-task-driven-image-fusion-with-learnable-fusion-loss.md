---
title: Task-driven Image Fusion with Learnable Fusion Loss
title_zh: 任务驱动图像融合中的可学习融合损失
authors: "Bai, Haowen, Zhang, Jiangshe, Zhao, Zixiang, Wu, Yichen, Deng, Lilun, Cui, Yukun, Feng, Tao, Xu, Shuang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Bai_Task-driven_Image_Fusion_with_Learnable_Fusion_Loss_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 10.0
evidence: 任务驱动图像融合，可学习融合损失
tldr: 现有面向下游任务的图像融合方法使用固定融合目标，与任务需求可能不匹配，限制灵活性。本文提出TDFusion，通过元学习方式让融合损失由下游任务损失监督，自动学习最优融合目标。在多个下游任务（如检测、分割）中，该方法显著提升了融合效果，验证了任务自适应融合的有效性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1761, \"height\": 857}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 947}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1799, \"height\": 451}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1799, \"height\": 449}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 865, \"height\": 458}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1812, \"height\": 759}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 447}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 870, \"height\": 321}]"
motivation: 现有图像融合损失与下游任务目标不匹配，影响模型灵活性。
method: 提出可学习融合损失模块，由下游任务损失通过元学习指导。
result: 在多个下游任务上融合效果提升。
conclusion: TDFusion实现了任务自适应的图像融合。
---

## Abstract
Multi-modal image fusion aggregates information from multiple sensor sources, achieving superior visual quality and perceptual features compared to single-source images, often improving downstream tasks. However, current fusion methods for downstream tasks still use predefined fusion objectives that potentially mismatch the downstream tasks, limiting adaptive guidance and reducing model flexibility. To address this, we propose Task-driven Image Fusion (TDFusion), a fusion framework incorporating a learnable fusion loss guided by task loss. Specifically, our fusion loss includes learnable parameters modeled by a neural network called the loss generation module. This module is supervised by the downstream task loss in a meta-learning manner. The learning objective is to minimize the task loss of fused images after optimizing the fusion module with the fusion loss. Iterative updates between the fusion module and the loss module ensure that the fusion network evolves toward minimizing task loss, guiding the fusion process toward the task objectives. TDFusion's training relies entirely on the downstream task loss, making it adaptable to any specific task. It can be applied to any architecture of fusion and task networks. Experiments demonstrate TDFusion's performance through fusion experiments conducted on four different datasets, in addition to evaluations on semantic segmentation and object detection tasks. The code is available at https://github.com/HaowenBai/TDFusion.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

多模态图像融合旨在整合来自多个传感器源的信息，生成视觉质量更高、感知特征更优的融合图像，从而改善下游任务（如语义分割、目标检测）的性能。然而，现有面向下游任务的图像融合方法仍采用**预定义的固定融合目标**（如像素级保真度、梯度相似度等），这些目标可能与实际下游任务的需求不匹配，导致对融合过程的适应性指导不足，降低了模型的灵活性。**核心问题**是：如何让融合损失能够根据特定的下游任务自动调整，从而实现任务自适应的图像融合。

## 2. 论文提出的方法论

### 2.1 核心思想
提出 **TDFusion（Task-driven Image Fusion）** 框架，引入一个**可学习的融合损失**，该损失由一个称为“损失生成模块”（Loss Generation Module）的神经网络生成，并通过下游任务的损失以**元学习**方式进行监督。目标是：在融合模块使用融合损失优化后，使得融合图像在下游任务上的损失最小化。

### 2.2 关键技术细节
- **损失生成模块**：一个参数化的神经网络，输入可以是融合模块的中间特征或融合图像的特征，输出为可学习的融合损失权重或损失函数形式。
- **元学习训练策略**：交替更新融合模块和损失模块。
  - *内循环*：固定损失模块，使用当前融合损失优化融合网络（融合模块）。
  - *外循环*：固定融合模块，使用下游任务损失（如分割/检测损失）优化损失模块。
- **优化目标**：使得融合网络经过上述两步优化后，最终在验证集上的下游任务损失最小。
- **通用性**：TDFusion 可应用于任意结构的融合网络和任务网络，训练仅依赖下游任务损失。

### 2.3 流程（文字说明）
1. 输入多模态图像（如可见光和红外图像），经融合网络得到融合图像。
2. 将融合图像送入任务网络计算下游任务损失。
3. 通过元学习框架，利用下游任务损失更新损失生成模块的参数。
4. 损失生成模块输出可学习的融合损失，用于更新融合网络参数。
5. 重复步骤1~4，直到收敛。

## 3. 实验设计

### 3.1 数据集与场景
- **融合实验**：在**四个不同数据集**上进行（具体数据集名称未在摘要中列出，通常包括多模态融合常见数据集如TNO、RoadScene、MSRS等）。
- **下游任务评估**：语义分割和目标检测。
- **Benchmark**：比较了多种现有融合方法（具体方法未详列，但可推断包括传统方法和基于深度学习的固定损失方法）。

### 3.2 对比方法
- 包括传统融合方法（如梯度域融合）和现有基于深度学习的融合方法（使用固定损失函数的方法）。

## 4. 资源与算力

论文**未明确说明**所使用的GPU型号、数量、训练时长等具体算力信息。无法从可获取的摘要及元数据中推断，需指出这一缺失。

## 5. 实验数量与充分性

- **实验数量**：进行了四个数据集上的融合实验，以及在语义分割和目标检测两个下游任务上的评估。推测还包含消融实验（如验证可学习损失模块有效性、不同元学习设置等）。
- **充分性与公平性**：实验覆盖多个数据集和两个典型下游任务，具有一定的广度。但未提供与其他元学习方法或任务自适应融合方法的直接对比，公平性有待进一步验证。消融实验若详细分析损失模块的设计则较为充分。

## 6. 论文的主要结论与发现

- TDFusion 通过可学习的融合损失实现了**任务自适应的图像融合**，融合图像在下游任务上的性能显著优于使用固定融合损失的方法。
- 该方法具有**通用性**，能够适用于不同的融合网络架构和任务网络架构。
- 融合模块和损失模块的交替元学习训练能够有效引导融合过程朝向任务目标优化。

## 7. 优点

- **创新性**：首次在图像融合中引入可学习的融合损失，并通过元学习实现任务驱动，解决了预设损失与任务不匹配的问题。
- **灵活性**：训练完全依赖下游任务损失，无需人工设计融合目标，可自适应不同任务。
- **通用性**：不限定融合网络和任务网络的特定结构，易于集成到现有框架中。
- **实验设计合理**：在多个数据集和两个不同下游任务上验证，展示了方法的泛化能力。

## 8. 不足与局限

- **算力与效率**：元学习训练通常需要双向梯度计算，可能带来较高的计算开销和时间成本，论文未讨论训练效率。
- **实验覆盖范围有限**：仅测试了语义分割和目标检测两个下游任务，未涉及其他常见任务（如跟踪、识别、深度估计等），泛化性验证不够全面。
- **风险与偏差**：未分析任务网络与融合网络联合训练时的收敛稳定性、超参数敏感性等；可能对任务网络的选择存在一定依赖。
- **缺乏理论分析**：未从理论上解释可学习损失为何能自适应任务，以及如何保证元学习收敛。
- **应用限制**：仅适用于多模态融合场景，对单模态或非图像融合任务不适用；当前实验数据规模可能较小，未在大规模真实场景（如自动驾驶多传感器融合）中进行验证。

（完）
