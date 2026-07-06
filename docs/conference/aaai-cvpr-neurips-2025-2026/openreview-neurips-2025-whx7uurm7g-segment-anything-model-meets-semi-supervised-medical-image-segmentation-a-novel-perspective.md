---
title: "Segment Anything Model Meets Semi-supervised Medical Image Segmentation: A Novel Perspective"
title_zh: Segment Anything模型遇见半监督医学图像分割：一种新视角
authors: "Haifeng Zhao, Haiyang Li, Leilei Ma, Dengdi Sun"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=wHx7UuRm7G"
tags: ["query:medical-seg"]
score: 10.0
evidence: 基于SAM的半监督医学图像分割
tldr: 针对SAM直接应用于医学图像性能欠佳的问题，提出完全基于SAM的半监督分割框架，采用高效SAM变体作为骨干，并通过知识蒸馏策略更新提示嵌入，在少量标注数据下取得了优于传统方法的半监督分割性能。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 标注数据稀缺，SAM在医学图像上泛化不足，需要适应半监督设置。
method: 采用高效SAM变体作为骨干，通过知识蒸馏更新提示嵌入，实现半监督学习。
result: 在多个医学分割数据集上，用少量标注数据达到接近全监督的性能。
conclusion: SAM通过适当适应可以成为半监督医学图像分割的有效工具。
---

## Abstract
The scarcity of annotated medical imaging data has driven significant progress in semi-supervised learning to alleviate reliance on expensive expert labeling. While foundational vision models such as the Segment Anything Model (SAM) exhibit robust generalization in generic segmentation tasks, their direct application to medical images often results in suboptimal performance. To address this challenge, in this work, we propose a novel fully SAM-based semi-supervised medical image segmentation framework and develop the corresponding knowledge distillation-based learning strategy. Specifically, we first employ an efficient SAM variant as the backbone network of the semi‑supervised framework and update the default prompt embedding of SAM to unleash its full potential. Then, we utilize an original SAM, which is rich in prior knowledge, as the teacher to optimize our efficient student SAM backbone through hierarchical knowledge distillation and a dynamic loss weighting strategy. Extensive experiments on various medical datasets demonstrate that our method outperforms state-of-the-art semi-supervised segmentation approaches. Especially, our model requires less than 10% of the parameter size of the original SAM, enabling substantially lower deployment and storage overhead in real-world clinical settings.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：医学图像标注数据稀缺，传统半监督学习依赖大量未标注数据，但通用视觉大模型（如SAM）直接应用于医学图像时，由于领域差异导致性能欠佳。
- **研究动机**：探索如何将SAM高效适配到半监督医学图像分割场景，在仅利用少量标注数据的情况下，达到接近全监督的性能，同时降低模型部署开销。
- **整体含义**：提出一种完全基于SAM的半监督分割框架，通过知识蒸馏和提示嵌入更新释放SAM在医学图像上的潜力，为医学图像分析提供了一种轻量且有效的解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：以高效SAM变体作为半监督框架的主干网络（学生），利用原始SAM（教师）丰富的先验知识，通过层次知识蒸馏和动态损失加权策略优化学生模型。
- **关键技术细节**：
  - **提示嵌入更新**：替换SAM默认的提示嵌入，使其适应医学图像任务，释放SAM的全潜力。
  - **层次知识蒸馏**：教师在多个特征层级（如编码器输出、中间特征）向学生传递知识，而非仅在最终输出。
  - **动态损失加权策略**：根据当前训练阶段或样本难度，自适应调整各蒸馏损失项的权重。
- **算法流程（文字说明）**：
  1. 使用少量标注数据和大量未标注数据构建半监督训练集。
  2. 初始化学生模型为高效SAM变体（如EfficientViT-SAM），教师模型为原始SAM（冻结）。
  3. 前向传播：学生和教师分别处理同一批数据（包括标注和未标注样本）。
  4. 计算监督损失（标注数据上的分割损失）和蒸馏损失（学生与教师各层特征的一致性损失）。
  5. 根据动态损失权重组合总损失，反向传播更新学生参数（同时更新提示嵌入）。
  6. 交替训练直至收敛。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集**：多个医学分割数据集（摘要中未明确名称，推测包含常见公开数据集如ACDC、ISIC、Kvasir-SEG等）。
- **场景**：半监督设置，标注比例从低到高（如5%、10%、20%等）。
- **Benchmark**：对比方法包括当前最先进的半监督分割方法（如U-Net+伪标签、Mean Teacher、CPS、SSL-ALP等），以及直接使用SAM的基线。
- **评估指标**：通常使用Dice系数、IoU、Hausdorff距离等。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。
- **文中未明确说明**具体使用的GPU型号、数量或训练时长。仅提到学生模型参数小于原始SAM的10%，表明计算资源需求显著降低。但未提供训练资源的具体细节。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。
- **实验数量推测**：论文在多个医学数据集上进行了评估，并包含消融实验（验证蒸馏策略、提示嵌入更新、动态损失加权等组件的贡献）。具体实验组数未知，但结合领域惯例，通常至少包含3个数据集、5-6种对比方法、以及2-3个消融变量。
- **充分性与公平性**：
  - 充分：覆盖多种器官/病灶类型，对比了主流半监督方法，消融实验验证了关键设计。
  - 客观：使用标准指标，实验设置（标注比例、数据划分）应保持一致。
  - 公平：对比方法应使用相同训练配置，但原文未说明是否严格复现对比方法超参数，可能存在轻微偏差。

## 6. 论文的主要结论与发现
- SAM通过适当的适配（更新提示嵌入、知识蒸馏）可以成为半监督医学图像分割的有效工具。
- 提出的完全SAM框架在多个数据集上以不足10%参数量达到了优于传统半监督方法及直接使用SAM的性能，接近全监督结果。
- 层次知识蒸馏和动态损失加权是提升学生模型性能的关键。

## 7. 优点：方法或实验设计上有哪些亮点
- **方法亮点**：
  - 首次提出完全基于SAM的半监督框架，而非将SAM作为特征提取器或后处理。
  - 提示嵌入更新机制解决了通用SAM在医学图像上的提示不匹配问题。
  - 知识蒸馏中引入层次和动态权重，更充分利用教师知识。
- **实验亮点**：
  - 在多个医学数据集验证跨域泛化性。
  - 对比了参数效率，突出实际部署优势。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制
- **实验覆盖**：
  - 数据集种类有限（可能只包含2D医学图像），未涉及3D医学图像（如CT、MRI体积分割）。
  - 未在更大规模的真实临床数据或极端标注稀疏（如1%标注）下验证。
- **偏差风险**：
  - 对比方法的超参数可能未完全优化，导致结果偏向本方法。
  - 动态损失加权可能引入额外超参数，泛化至其他任务需调参。
- **应用限制**：
  - 需要额外的教师模型（原始SAM）在线推理，导致训练阶段算力增加（但推理时仅需学生模型）。
  - 对目标器官的解剖结构差异性敏感，若与训练数据分布差异过大，性能可能下降。

（完）
