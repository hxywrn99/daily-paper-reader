---
title: "MedSAMix: A Training-Free Model Merging Approach for Medical Image Segmentation"
title_zh: MedSAMix：一种无需训练的模型合并方法用于医学图像分割
authors: "Yanwu Yang, Guinan Su, Jiesi Hu, Francesco Sammarco, Jonas Geiping, Thomas Wolfers"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38161/42123"
tags: ["query:medical-seg"]
score: 10.0
evidence: 无需训练的模型合并用于医学图像分割
tldr: 针对现有医学SAM微调变体因训练数据有限而泛化不足的问题，提出MedSAMix无需训练即可合并多个微调模型，通过模型合并增强对多样医学分割任务的泛化能力，实验证明在多个任务上超过单一微调模型。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 微调SAM变体受限于有限医学数据，泛化能力不足。
method: 提出MedSAMix模型合并方法，无需训练即可融合多个微调SAM变体。
result: 在多种医学分割任务中超越了单一微调模型。
conclusion: MedSAMix为提升医学分割模型的通用性提供了一种轻量级解决方案。
---

## Abstract
Universal medical image segmentation models have emerged as a promising paradigm due to their strong generalizability across diverse tasks, showing great potential for a wide range of clinical applications. This potential has been partly driven by the success of general-purpose vision models such as the Segment Anything Model (SAM), which has inspired the development of various fine-tuned variants for medical segmentation tasks. However, fine-tuned variants like MedSAM are trained on comparatively limited medical imaging data that often suffers from heterogeneity, scarce annotations, and distributional shifts. These challenges limit their ability to generalize across a wide range of medical segmentation tasks. In this regard, we propose MedSAMix, a training-free model merging method that integrates the strengths of both generalist models (e.g., SAM) and specialist models (e.g., MedSAM) for medical image segmentation. In contrast to traditional model merging approaches that rely on manual configuration and often result in suboptimal outcomes, we propose a zero-order optimization method to automatically discover optimal layer-wise merging solutions. Furthermore, for clinical applications, we develop two regimes to meet the demand of domain-specificity and generalizability in different scenarios by single-task optimization and multi-objective optimization respectively. Extensive evaluations on 25 medical segmentation tasks demonstrate that MedSAMix effectively mitigates model bias and consistently improves performance in both domain-specific accuracy and generalization, achieving improvements of 6.67% on specialized tasks and 4.37% on multi-task evaluations.

---

## 论文详细总结（自动生成）

# MedSAMix：一种无需训练的模型合并方法用于医学图像分割——详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：当前通用医学图像分割模型（如基于SAM的微调变体MedSAM、MedicoSAM）虽在部分任务上表现优异，但由于医学影像数据存在异质性、标注稀缺、分布偏移等问题，微调模型易收敛到次优局部最小值，导致泛化能力不足。甚至在某些任务上，微调模型的表现还不如原始的SAM模型（通用视觉模型），暴露出领域特化与通用性之间的权衡。
- **动机**：如何在不重新训练的前提下，融合通用模型（如SAM）的强泛化能力和专家模型（如MedSAM）的领域专精能力，以提升整体分割性能。
- **含义**：提出一种无需训练的模型合并方法（MedSAMix），通过自动搜索最优层级别合并配置，在多个任务上同时提升领域专精和通用泛化能力，为医学图像分割的模型融合提供新范式。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将多个从同一基座（SAM）微调得到的模型进行后训练合并，采用零阶优化自动发现最优层级别合并超参数，无需额外的训练数据或计算开销。
- **搜索空间**：
  - 针对SAM架构（含ViT图像编码器、提示编码器、掩码解码器）设计层级别合并空间。
  - 通过层粒度（granularity）控制各组层共享相同的合并策略（线性组合、任务算术、TIES、SLERP等），平衡搜索规模与细粒度控制。
- **优化目标**：
  - **单任务优化**：针对特定任务，使用校准集上的Dice损失作为目标函数，寻找最优合并配置。
  - **多任务优化**：采用ParEGO方法（基于帕累托效率），对多个任务的目标函数进行加权标量化，搜索帕累托前沿上的解，以平衡不同任务的性能。
- **搜索算法**：采用基于随机森林代理模型的贝叶斯优化（SMAC），通过期望改进（EI）函数引导搜索，迭代进行模型合并与评估，直至收敛（算法1）。
- **关键公式**：
  - 单任务目标：\( f_{single}(M_\omega) = L(M_\omega, D_T) \)
  - 多任务目标：\( f_{multi}(M_\omega, \lambda) = \max_i \{\lambda_i f_{single,i}(M_\omega)\} + \alpha \sum_i \lambda_i f_{single,i}(M_\omega) \)
  - 搜索过程通过贝叶斯优化选择最优 \(\omega^*\)。

## 3. 实验设计

- **数据集**：25个公开医学图像分割数据集，涵盖多种器官（脑肿瘤、血管、肝脏、肾脏、脾脏、胰腺、前列腺、视盘、视杯、心脏、小鼠肺/胰腺、鼻腔/鼻窦等）和模态（CT、MRI、MRA、眼底图像等）。80%用于测试，20%作为校准集用于合并搜索和上下文学习模型的提示。
- **基准对比方法**：
  - ICL-based: UniverSeg, SegGPT, Neuroverse3D
  - SAM-based: SAM, MedSAM, MedicoSAM, SAM-Med2D
  - 全监督上限：nnU-Net（作为参考）
- **评价指标**：Dice系数
- **模型池**：SAM（基座）、MedSAM、MedicoSAM（三个模型），合并候选技术包括TIES、任务算术、线性组合、SLERP。

## 4. 资源与算力

- 文中明确提及：
  - 单任务优化：120次迭代（trials），使用2个GPU
  - 多任务优化：200次迭代，使用4个GPU
- 未提及GPU具体型号、训练时长以及搜索过程的额外耗时（除迭代次数外）。整体属于轻量级搜索，无需大规模训练资源。

## 5. 实验数量与充分性

- **实验数量**：大量实验覆盖25个任务，每个任务在单任务和多任务设置下均进行了验证。
- **消融与敏感性分析**：
  - 对比不同合并方法（TIES, TA, Linear, SLERP, MedSAMix-S, MedSAMix-M）的平均Dice。
  - 层粒度敏感性：1~4，分析对单/多任务性能的影响。
  - 多任务优化所用任务数量影响（2~10个）实验。
  - 不同模型组合（MedSAM+SAM, MedicoSAM+SAM, 三者合并）比较。
- **充分性评价**：实验设计较为全面，覆盖核心变量，对比了多种当前主流方法，且包含统计显著性分析需求（文中未明确给出p值，但展示平均提升）。总体上，实验客观、公平（使用相同评估协议、零样本设置），但缺乏统计显著性检验（如配对t检验）的明确报告。

## 6. 主要结论与发现

- MedSAMix在单任务（MedSAMix-S）和多任务（MedSAMix-M）设置下均显著优于所有基线方法：
  - 单任务平均提升6.67%（相对MedicoSAM）
  - 多任务平均提升4.37%，在18/25个任务上超越第二佳模型
- 模型合并可以有效缓解微调模型的泛化不足，同时不损害其在擅长任务上的性能。
- 多任务搜索中，使用8个任务进行搜索可获得最佳平衡；层粒度为2时多任务性能最优。
- 合并三个模型（SAM、MedSAM、MedicoSAM）比任何两两组合效果更好，证明了互补知识的价值。

## 7. 优点

- **无需训练**：方法完全基于模型级合并，无需额外数据或重训练，计算开销极小。
- **自动搜索**：通过零阶贝叶斯优化自动寻找最优合并配置，避免手动调参的次优性。
- **灵活适应不同场景**：单任务优化可用于领域专精，多任务优化用于通用泛化，满足临床多元化需求。
- **跨模型兼容**：方法适用于从相同基座微调的任意模型，可扩展到更多候选模型。
- **实验覆盖广泛**：25个任务覆盖多种器官和模态，验证了方法的普适性。
- **可复现性**：提供代码和模型权重（GitHub, Hugging Face）。

## 8. 不足与局限

- **任务覆盖有限**：虽然25个任务，仍不能代表所有医学图像分割场景（如病理学、手术规划等），泛化广度需进一步验证。
- **缺乏统计检验**：未报告置信区间或p值，难以判断性能提升的统计显著性。
- **模型池限制**：仅合并了三个SAM基模型，其他架构如Medical SAM Adapter因架构差异无法兼容。未来需要研究跨架构合并。
- **多任务搜索任务数量影响**：当搜索任务超过8个时性能下降，表明帕累托前沿搜索在高维时面临挑战，如何自动选择最佳搜索任务集未解决。
- **搜索成本**：虽无需训练，但120~200次迭代仍需多个GPU小时（具体时长未报），且每次迭代需加载完整模型并进行前向计算，可能存在中等算力需求。
- **校准集依赖性**：单任务优化依赖校准集，若校准集分布偏移可能导致次优合并；多任务优化则面临任务间权衡的困难。

（完）
