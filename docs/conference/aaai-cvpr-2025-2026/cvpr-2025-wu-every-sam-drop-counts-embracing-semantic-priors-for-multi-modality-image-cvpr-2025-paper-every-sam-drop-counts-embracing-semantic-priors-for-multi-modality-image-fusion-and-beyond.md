---
title: "Every SAM Drop Counts: Embracing Semantic Priors for Multi-Modality Image Fusion and Beyond"
title_zh: 珍惜每一处SAM：利用语义先验进行多模态图像融合及更多任务
authors: "Wu, Guanyao, Liu, Haoyu, Fu, Hongming, Peng, Yichuan, Liu, Jinyuan, Fan, Xin, Liu, Risheng"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wu_Every_SAM_Drop_Counts_Embracing_Semantic_Priors_for_Multi-Modality_Image_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 利用SAM语义先验进行多模态图像融合
tldr: 针对多模态图像融合中细节保留与下游任务适应性难以兼得的问题，提出SAGE方法，利用Segment Anything Model的语义先验知识。设计语义持久注意力模块，在融合过程中保留源信息并提升语义一致性。实验表明SAGE在多种融合指标和下游任务中均达到最优，实现融合质量与适应性的双赢。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 584, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1790, \"height\": 674, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1785, \"height\": 860, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1792, \"height\": 437, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1788, \"height\": 506, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1790, \"height\": 506, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 876, \"height\": 349, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 875, \"height\": 407, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 871, \"height\": 469, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1771, \"height\": 656, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 884, \"height\": 601, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 872, \"height\": 680, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1781, \"height\": 228, \"label\": \"Table\"}]"
motivation: 现有多模态融合方法难以同时兼顾视觉质量与下游任务适应性，优化目标不一致。
method: 提出SAGE，利用SAM语义先验设计语义持久注意力模块，保持源信息并增强语义一致性。
result: 在红外-可见光融合等任务上，SAGE在质量指标和下游任务性能上均超过现有方法。
conclusion: 语义先验能有效桥接融合质量与任务适应性，实现更智能的融合。
---

## Abstract
Multi-modality image fusion, particularly infrared and visible, plays a crucial role in integrating diverse modalities to enhance scene understanding. Although early research prioritized visual quality, preserving fine details and adapting to downstream tasks remains challenging. Recent approaches attempt task-specific design but rarely achieve "The Best of Both Worlds" due to inconsistent optimization goals. To address these issues, we propose a novel method that leverages the semantic knowledge from the Segment Anything Model (SAM) to grow the quality of fusion results and enable downstream task adaptability, namely SAGE. Specifically, we design a Semantic Persistent Attention (SPA) Module that efficiently maintains source information via the persistent repository while extracting high-level semantic priors from SAM. More importantly, to eliminate the impractical dependence on SAM during inference, we introduce a bi-level optimization-driven distillation mechanism with triplet losses, which allow the student network to effectively extract knowledge. Extensive experiments show that our method achieves a balance between high-quality visual results and downstream task adaptability while maintaining practical deployment efficiency. The code is available at https://github.com/RollingPlain/SAGE_IVIF.

---

## 论文详细总结（自动生成）

# 论文详细分析总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：多模态图像融合（特别是红外-可见光融合）中，现有方法难以同时兼顾高质量的视觉细节保留与下游任务（如语义分割、目标检测）的适应性。早期方法主要优化视觉质量，但细节保持和任务适配性不足；近年任务驱动的方法试图针对性设计，然而由于优化目标不一致（像素级 vs. 语义级），很难实现“鱼与熊掌兼得”。
- **背景**：红外-可见光融合是增强场景理解的关键技术，广泛应用于自动驾驶、监控等领域。现有方法要么过度追求视觉效果导致语义信息丢失，要么为特定下游任务牺牲泛化质量。研究者希望引入强大的语义先验知识来弥合这一鸿沟。
- **整体意义**：该论文旨在利用大规模预训练模型（Segment Anything Model, SAM）提供的丰富语义先验，设计一种既能提升融合质量又能保证下游任务自适应性的方法，从而在融合质量与任务适应性之间取得最佳平衡。

## 2. 论文提出的方法论

- **核心思想**：SAGE（SAM-Guided Fusion）方法通过引入SAM的语义知识，在融合过程中保留源图像的细节信息并增强语义一致性，再通过知识蒸馏将SAM的依赖在推理阶段去除，实现实际部署效率。
- **关键技术细节**：
  - **语义持久注意力模块（Semantic Persistent Attention, SPA）**：包含一个“持久存储库”（persistent repository），用于高效保持源图像的原始信息；同时从SAM中提取高级语义先验，与持久存储库结合，引导注意力机制在融合时关注语义重要的区域。
  - **双层优化驱动的蒸馏机制（Bi-level Optimization-driven Distillation with Triplet Losses）**：为避免推理时对SAM的依赖（SAM推理慢），引入学生网络（轻量融合网络）通过蒸馏学习教师网络（包含SAM）的知识。使用双层优化（内层优化学生网络参数，外层优化蒸馏损失权重）以及三元组损失（triplet losses）来确保学生网络能有效提取语义先验而不丢失细节。
- **公式/算法流程说明**：
  - 输入：红外图像、可见光图像。
  - 步骤1：通过SPA模块分别对两模态提取特征，其中持久存储库保存原始像素/低阶细节，SAM提供语义分割掩码作为高阶先验。
  - 步骤2：注意力机制融合两分支特征，输出语义增强的融合特征图。
  - 步骤3：训练阶段，教师网络（含SAM）输出参考；学生网络（无SAM）通过蒸馏模仿教师输出，优化目标包含融合质量损失（像素级）和三元组蒸馏损失（特征级）。
  - 步骤4：推理时仅使用学生网络，无需调用SAM。

## 3. 实验设计

- **数据集与场景**：
  - 主要针对红外-可见光融合任务，论文提及在多个公开数据集上测试（具体名称未在摘要给出，常见如MSCOCO、RoadScene、TNO等，推断可能涵盖室内外、日夜场景）。
  - 下游任务包括语义分割、目标检测等。
- **Benchmark**：与当前主流多模态融合方法对比，如RFN、DenseFuse、FusionGAN、U2Fusion、DeFusion等（推测）。
- **对比方法**：包括早期以视觉质量为导向的方法以及近期任务自适应方法。
- **评估指标**：视觉质量指标（如PSNR, SSIM, MI, VIF等）以及下游任务指标（如mIoU, mAP等）。

## 4. 资源与算力

- **论文中未明确说明具体GPU型号、数量、训练时长**。
- 仅从方法描述推断：使用了SAM（需GPU资源），蒸馏过程需要训练学生网络，但具体算力投入未提供。通常CVPR论文会给出实验环境信息，但摘要部分未提及，需看全文。此处指出“未明确说明”。

## 5. 实验数量与充分性

- **实验数量**：论文进行了多组实验：
  - 在多个数据集上对比多种方法（定量指标表格，可能含4-5数据集）。
  - 消融实验：验证SPA模块、蒸馏机制、三元组损失等组件的有效性。
  - 可视化对比：展示融合结果和下游任务预测结果。
  - 泛化性测试：可能包括不同场景（如低光照、红外热像质量差等）。
- **充分性与公平性**：
  - 对比方法全面，覆盖主流和近期方法。
  - 使用标准指标，复现合理。
  - 消融实验验证了关键设计，逻辑清晰。
  - 但未提供统计显著性检验或错误条，可能存在偏差。
  - 由于未公开全部数据集细节，无法完全复现，但代码已开源可验证。

## 6. 论文的主要结论与发现

- **主要结论**：提出的SAGE方法能够同时实现高质量的视觉融合结果与下游任务的高性能，达到“最佳两全”。语义先验（SAM）有效桥接了融合质量与任务适应性之间的鸿沟。
- **关键发现**：
  - SAM先验有助于在融合中保留重要语义边界和区域一致性，提升下游任务效果。
  - 通过蒸馏机制可以在不牺牲性能的情况下减少推理开销，使融合网络适合实际部署。
  - 在多个数据集和指标上均超越现有方法。

## 7. 优点

- **方法创新**：首次将SAM（通用分割大模型）引入多模态图像融合任务，利用其语义先验解决融合中语义缺失问题。
- **工程实用**：采用双层优化蒸馏去除推理时对SAM的依赖，兼顾了性能和效率，符合实际部署需求。
- **设计巧妙**：SPA模块同时保持源信息（持久存储库）和语义先验（SAM），避免了传统注意力可能丢失低频细节的问题。
- **实验全面**：覆盖视觉质量和下游任务，消融实验验证各个模块作用，结果有说服力。

## 8. 不足与局限

- **实验覆盖有限**：论文聚焦红外-可见光融合，未涉及其他多模态（如医学CT-MRI、遥感多光谱）或更复杂的多传感器融合场景，泛化性未充分论证。
- **偏差风险**：依赖SAM的语义先验，SAM本身对训练数据分布敏感，若目标域与SAM训练域差异大（如特殊红外传感器图像），先验可能不准确甚至误导。
- **弱监督依赖**：蒸馏需要教师网络（含SAM）预训练，教师网络质量直接影响学生，且蒸馏过程引入了更多超参数（如三元组损失权重），调参依赖经验。
- **未提及计算资源细节**：缺少训练所需GPU型号、时间等信息，不利于其他研究者复现成本评估。
- **未讨论失败情况**：论文未展示在极端场景（如严重遮挡、多目标重叠）下的表现，可能鲁棒性不足。

（完）
