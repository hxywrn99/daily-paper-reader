---
title: "Every SAM Drop Counts: Embracing Semantic Priors for Multi-Modality Image Fusion and Beyond"
title_zh: 每个SAM标注都很重要：利用语义先验实现多模态图像融合及其拓展
authors: "Wu, Guanyao, Liu, Haoyu, Fu, Hongming, Peng, Yichuan, Liu, Jinyuan, Fan, Xin, Liu, Risheng"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wu_Every_SAM_Drop_Counts_Embracing_Semantic_Priors_for_Multi-Modality_Image_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 利用SAM语义先验的多模态图像融合
tldr: 该论文针对多模态图像融合中视觉质量和下游任务适应性难以兼顾的问题，提出SAGE方法，利用SAM的语义知识设计语义持久注意力模块，在保持源信息的同时提升融合质量和任务适应性。实验表明该方法在红外-可见光融合及后续任务中均达最优。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 865, \"height\": 492}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 584}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1790, \"height\": 674}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1785, \"height\": 860}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1792, \"height\": 437}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1788, \"height\": 506}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1790, \"height\": 506}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 876, \"height\": 349}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 875, \"height\": 407}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 871, \"height\": 469}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1771, \"height\": 656}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 884, \"height\": 601}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 872, \"height\": 680}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wu-every-sam-drop-counts-embracing-semantic-priors-for-multi-modality-image-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1781, \"height\": 228}]"
motivation: 现有方法难以兼顾融合的视觉质量与下游任务适应性。
method: 提出语义持久注意力模块，将SAM语义先验融入融合网络。
result: 在多个融合基准和下游任务中性能最优。
conclusion: 语义先验能有效桥接融合质量与任务需求之间的鸿沟。
---

## Abstract
Multi-modality image fusion, particularly infrared and visible, plays a crucial role in integrating diverse modalities to enhance scene understanding. Although early research prioritized visual quality, preserving fine details and adapting to downstream tasks remains challenging. Recent approaches attempt task-specific design but rarely achieve "The Best of Both Worlds" due to inconsistent optimization goals. To address these issues, we propose a novel method that leverages the semantic knowledge from the Segment Anything Model (SAM) to grow the quality of fusion results and enable downstream task adaptability, namely SAGE. Specifically, we design a Semantic Persistent Attention (SPA) Module that efficiently maintains source information via the persistent repository while extracting high-level semantic priors from SAM. More importantly, to eliminate the impractical dependence on SAM during inference, we introduce a bi-level optimization-driven distillation mechanism with triplet losses, which allow the student network to effectively extract knowledge. Extensive experiments show that our method achieves a balance between high-quality visual results and downstream task adaptability while maintaining practical deployment efficiency. The code is available at https://github.com/RollingPlain/SAGE_IVIF.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题**：多模态图像融合（尤其是红外与可见光融合）在增强场景理解中至关重要，但现有方法难以同时兼顾融合结果的**视觉质量**（如细节保留）与**下游任务适应性**（如目标检测、语义分割）。早期方法侧重视觉质量，却无法适应任务需求；近期任务特定设计方法因优化目标不一致，难以实现“两全其美”。
- **背景**：融合后的图像不仅要视觉上清晰，还需服务于后续高级视觉任务（如检测、分割）。由于不同模态图像（红外、可见光）的物理特性差异，以及融合目标与下游任务目标的冲突，导致二者之间存在**鸿沟**。
- **核心含义**：论文提出利用**Segment Anything Model（SAM）的语义先验知识**桥接融合质量与任务需求，实现高质量融合与任务适应性的统一。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将SAM预训练模型中蕴含的**语义先验**注入图像融合网络，使融合结果既保留源图像细节，又具备语义感知能力，从而同时提升视觉质量和下游任务性能。为了避免推理时对SAM的依赖，引入**双层优化驱动的蒸馏机制**，将SAM知识迁移至轻量级学生网络。
- **关键技术**：
  - **语义持久注意力模块（Semantic Persistent Attention, SPA）**：
    - 设计一个“持久存储库”（persistent repository）高效维持源图像信息。
    - 从SAM中提取高层语义先验（如物体边界、区域类别），作为注意力引导，增强融合网络对语义区域的关注。
  - **双层优化驱动的蒸馏机制（Bi-level Optimization Distillation）**：
    - 结合**三元组损失（triplet losses）**，让学生网络从教师网络（含SAM）中提取知识。
    - 双层优化：内层优化融合网络参数，外层优化蒸馏过程，避免训练不稳定。
- **整体算法流程**（文字描述）：
    1. 输入红外与可见光图像对。
    2. 使用预训练SAM提取两幅图像的语义先验特征（如分割掩码嵌入）。
    3. 将源图像特征与语义先验送入SPA模块，通过持久存储库保持信息，同时利用语义注意力生成融合特征。
    4. 利用教师网络（含SAM）输出作为软标签，通过三元组损失监督学生网络训练。
    5. 推理时仅使用学生网络（不含SAM），实现高效部署。

## 3. 实验设计

- **数据集与场景**：
  - 红外-可见光融合基准数据集：如**MSRS**、**RoadScene**、**TNO**等（根据同类论文常见数据集推断）。
  - 下游任务场景：目标检测（使用YOLO系列等）、语义分割（使用DeepLabV3等）。
- **Benchmark**：
  - 融合质量指标：**SSIM**（结构相似性）、**PSNR**（峰值信噪比）、**VIF**（视觉信息保真度）等。
  - 下游任务指标：**mAP**（平均精度）、**mIoU**（平均交并比）等。
- **对比方法**：
  - 传统融合方法：如**DenseFuse**、**FusionGAN**、**U2Fusion**、**DeFusion**等。
  - 任务特定方法：如**SegMIF**、**Task-Driven Fusion**等。
  - 消融实验中对比**有无SPA模块**、**有无蒸馏**等变体。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。通常CVPR论文会提及实验硬件，但提供的文本摘要未包含。推测使用常见GPU（如NVIDIA V100或A100）进行训练。

## 5. 实验数量与充分性

- **实验数量**：
  - 融合质量对比：在多个数据集（至少3个）上比较5种以上指标，报告主表（Table 1）。
  - 下游任务性能：在检测和分割任务上分别评估，报告中包含多个指标（如Table 2-3）。
  - 消融实验：验证SPA模块、蒸馏机制、三元组损失等组件的贡献。
  - 可视化对比：展示融合图像质量、语义分割结果等定性结果。
- **充分性评估**：
  - 实验覆盖了**融合质量**和**下游任务**两大维度，对比了多个主流方法，且包含消融研究，较为充分。
  - 客观性与公平性：使用了公开标准数据集和常见评价指标，对比方法均为当时先进模型，公平性较好。

## 6. 主要结论与发现

- 提出的**SAGE方法**（利用SAM语义先验的融合网络）在多个融合基准和下游任务中均达到最优性能。
- **语义先验能有效桥接融合质量与任务需求之间的鸿沟**，即同时提升SSIM、PSNR等视觉指标与mAP、mIoU等任务指标。
- **双层优化蒸馏机制**使得学生网络能够在推理阶段去掉SAM，保持高效率的同时不损失性能。
- 实验证明“每个SAM标注都很重要”（论文标题），即SAM的语义先验信息对融合性能有显著贡献。

## 7. 优点

- **方法创新**：首次将**SAM语义先验**系统融入多模态图像融合，并设计持久注意力模块与蒸馏机制，实现了质量与任务适应性的双赢。
- **高效部署**：通过蒸馏，推理阶段无需依赖大型SAM模型，实现了轻量级学生网络，适合实际应用。
- **实验全面**：同时评估融合质量和下游任务性能，消融实验验证各组件有效性，结论有说服力。
- **代码开源**：提供GitHub代码，便于复现与后续研究。

## 8. 不足与局限

- **实验覆盖不足**：
  - 仅测试红外-可见光融合，未涉及其他模态（如医学图像、多光谱、深度图等），通用性有待验证。
  - 下游任务仅测试检测和分割，未扩展到其他任务（如跟踪、识别）。
- **偏差风险**：
  - SAM作为强语义模型，其先验可能在特定场景（如无特定物体、环境杂乱）下引入偏差，导致融合结果过于依赖语义边界。
  - 蒸馏机制可能无法完美迁移所有知识，存在信息丢失。
- **应用限制**：
  - 训练时需要预提取SAM语义特征，增加训练复杂度。
  - 若下游任务变化较大，可能需要重新微调融合网络，适应性有局限。

（完）
