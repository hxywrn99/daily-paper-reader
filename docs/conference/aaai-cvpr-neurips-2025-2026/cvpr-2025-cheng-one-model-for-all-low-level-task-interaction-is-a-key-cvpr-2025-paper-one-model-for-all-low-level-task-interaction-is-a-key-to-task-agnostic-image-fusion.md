---
title: "One Model for ALL: Low-Level Task Interaction Is a Key to Task-Agnostic Image Fusion"
title_zh: 一个模型解决所有：低层任务交互是实现任务无关图像融合的关键
authors: "Cheng, Chunyang, Xu, Tianyang, Feng, Zhenhua, Wu, Xiaojun, Tang, Zhangyong, Li, Hui, Zhang, Zeyang, Atito, Sara, Awais, Muhammad, Kittler, Josef"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Cheng_One_Model_for_ALL_Low-Level_Task_Interaction_Is_a_Key_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 通过低层任务交互实现任务无关的图像融合
tldr: 现有图像融合方法优先考虑高层任务，导致任务间语义鸿沟。本文提出GIFNet，利用低层视觉任务进行像素级监督，实现跨不同融合任务的有效特征交互。该单一模型在多种融合任务上均取得高性能，包括未见过的场景。实验证明了低层任务交互在通用融合中的关键作用。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 440}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 520}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 868, \"height\": 236}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1808, \"height\": 806}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 794, \"height\": 397}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 865, \"height\": 222}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 865, \"height\": 845}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 869, \"height\": 307}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 832, \"height\": 440}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1810, \"height\": 493}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 866, \"height\": 423}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 867, \"height\": 256}]"
motivation: 现有方法专注于特定高层任务，难以泛化到多种融合场景。
method: 利用低层视觉任务进行像素级监督，学习任务共享特征，实现通用融合。
result: 单个模型在多种融合任务上达到甚至超过专用模型性能。
conclusion: 低层任务交互范式为通用图像融合提供了新思路。
---

## Abstract
Advanced image fusion methods mostly prioritise high-level missions, where task interaction struggles with semantic gaps, requiring complex bridging mechanisms. In contrast, we propose to leverage low-level vision tasks from digital photography fusion, allowing for effective feature interaction through pixel-level supervision. This new paradigm provides strong guidance for unsupervised multimodal fusion without relying on abstract semantics, enhancing task-shared feature learning for broader applicability. Owning to the hybrid image features and enhanced universal representations, the proposed GIFNet supports diverse fusion tasks, achieving high performance across both seen and unseen scenarios with a single model. Uniquely, experimental results reveal that our framework also supports single-modality enhancement, offering superior flexibility for practical applications. Our code will be available at https://github.com/AWCXV/GIFNet.

---

## 论文详细总结（自动生成）

# 论文结构化总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：现有先进的图像融合方法大多依赖高层视觉任务（如目标检测、语义分割）作为监督信号，导致任务间存在语义鸿沟，需要复杂的桥接模块，且泛化能力差，不同融合任务需单独训练模型，资源开销大。
- **背景**：多模态融合（如红外-可见光）和数字摄影融合（如多聚焦、多曝光）虽同属低层视觉任务，但以往未充分利用其共性。高层任务提供的语义信息与像素级融合目标不匹配，易损失细节质量。
- **核心含义**：提出利用低层数字摄影融合任务（如多聚焦）的像素级监督来引导无监督多模态融合，通过任务间交互学习任务共享特征，实现一个模型处理多种融合任务，甚至支持单模态图像增强。

## 2. 方法论

### 核心思想
- 引入**低层任务交互**范式：将多模态融合（IVIF）作为主任务，数字摄影融合（MFIF）作为辅助任务，通过交替训练和交叉融合机制实现特征互增强。
- 通过**重建任务（REC）** 和**RGB联合数据集**对齐不同任务的域差异，学习通用表示。
- 提出的**GIFNet**（Generalized Image Fusion Network）具有三分支架构：主任务分支、辅助任务分支、重建分支，以及交叉融合门控机制（CFGM）。

### 关键技术细节
- **三分支架构**：
  - 共享编码器（S-Enc）提取公共特征。
  - 多模态分支（MM）和数字摄影分支（DP）分别提取任务特定特征。
  - 重建分支（REC）通过自编码器重建RGB图像，提供像素级监督，促进特征对齐。
- **交叉融合门控机制（CFGM）**：基于SwinTransformer块，使用自注意力与跨注意力，交替训练主/辅助分支（冻结一个，优化另一个），通过可学习参数λ控制辅助分支影响。
- **损失函数**：
  - 公共损失 \(L_{pub}\)：重建分支输出与可见光图像的SSIM+MSE损失。
  - 私有损失：
    - MM分支：基于梯度加权（DenseNet121计算权重）的信息加权MSE损失。
    - DP分支：直接与可见光GT的MSE损失。
- **训练策略**：交替训练，仅使用IVIF数据集（LLVIP）及其增强的多聚焦数据。

## 3. 实验设计

- **数据集与场景**：
  - **训练集**：LLVIP（红外-可见光）及由其生成的RGB联合数据（含近聚焦/远聚焦）。
  - **测试集**：
    - 已见任务：IVIF（LLVIP、TNO）、MFIF（Lytro、MFI-WHU）。
    - 未见任务：MEIF（SCIE）、NIR-VIS（VIS-NIR Scene）、遥感（QuickBird）、医学（Harvard）。
    - 单模态增强：CIFAR100分类任务。
- **Benchmark**：与专用方法（Text-IF、CDDFuse、DDFM、LRRNet、ZMFF等）和通用方法（MURF、MUFusion、U2Fusion、SDNet等）对比。
- **评估指标**：VIF、SCD、EI、AG（非参考质量指标），以及分类准确率（Top-1/Top-5）。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长，仅提到模型大小3.30 MB、GFLOPs 9.96，计算效率极高（较同类方法降低>96%）。

## 5. 实验数量与充分性

- **实验数量**：
  - 消融实验：11组（表1），分析MTL、CFGM、REC、任务组合（MF、ME）的影响。
  - 主实验：6个融合任务（表2），每组均与5-10种方法对比定量指标，并配有定性可视化（图7）。
  - 分类增强实验：表3，与7种方法对比在CIFAR100上的分类性能。
  - 效率对比：表4，与6种多任务方法对比模型大小和GFLOPs。
- **充分性**：
  - 消融覆盖了关键组件和任务选择，验证了低层任务交互的有效性。
  - 测试场景多样，包括已见/未见任务、单模态增强，客观指标和主观视觉并存。
  - 对比方法包括近年顶会/期刊方法，公平性较高（均使用公开模型或代码）。
  - 不足之处：仅选择MFIF作为辅助任务（文中实验表明MF优于ME），但未测试其他低层任务（如去雨、去噪）的泛化潜力。

## 6. 主要结论与发现

- 低层任务交互（IVIF+MFIF）显著提升无监督多模态融合性能，优于依赖高层任务的范式。
- 单个GIFNet模型在多个已见和未见融合任务上均达到或超越专用方法，展现了强大的泛化能力。
- 首次将图像融合模型扩展到单模态增强任务，在CIFAR100分类中超越原始数据及其他融合方法。
- 模型极轻量（3.3 MB，9.96 GFLOPs），适合移动端部署。
- 重建分支和RGB联合数据集有效减小了任务域差异。

## 7. 优点

- **范式创新**：首次系统论证低层任务交互对通用融合的关键作用，避免高层语义鸿沟。
- **跨任务泛化**：单一模型处理多种融合场景，无需针对任务微调。
- **多功能性**：支持单模态增强，拓展了融合模型的应用范围。
- **高效轻量**：比现有多任务方法降低99%以上参数和FLOPs，具备实用价值。
- **实验充分**：覆盖多种数据集和对比方法，定量定性结合，消融设计合理。

## 8. 不足与局限

- **辅助任务选择**：仅采用MFIF作为辅助，其他数字摄影任务（如MEIF）效果较差，缺乏对更多低层任务（如去模糊、去噪）的探索，未来可扩展。
- **训练数据依赖**：需要额外的RGB联合数据生成，数据增强过程可能引入偏差。
- **客观指标局限性**：主要使用非参考指标（VIF、EI等），未采用基于学习的主观质量评估或下游任务性能（除分类外）的直接验证。
- **未见任务性能**：虽然全面，但在某些任务（如遥感全色锐化）上个别指标略逊于专用方法，仍有改进空间。
- **资源信息缺失**：未报告训练硬件和耗时，影响可复现性评估。

（完）
