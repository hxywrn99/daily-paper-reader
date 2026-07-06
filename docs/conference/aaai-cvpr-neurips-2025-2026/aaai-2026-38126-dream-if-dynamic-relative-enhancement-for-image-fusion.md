---
title: "Dream-IF: Dynamic Relative EnhAnceMent for Image Fusion"
title_zh: Dream-IF：面向图像融合的动态相对增强框架
authors: "Xingxin Xu, Bing Cao, Dongdong Li, Qinghua Hu, Pengfei Zhu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38126/42088"
tags: ["query:image-fusion"]
score: 8.0
evidence: 动态增强用于图像融合
tldr: 该论文发现融合图像中一个模态的显著区域提示另一模态需要增强，提出Dream-IF框架，量化各模态相对优势并动态增强，实现融合与增强的协同优化。实验表明该方法在多种退化场景下显著提升融合质量。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38126/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 868, \"height\": 343, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38126/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1846, \"height\": 551, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38126/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1831, \"height\": 766, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38126/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1833, \"height\": 275, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38126/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 872, \"height\": 451, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38126/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1829, \"height\": 423, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38126/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 841, \"height\": 421, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38126/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 882, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38126/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 879, \"height\": 300, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38126/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 419, \"label\": \"Table\"}]"
motivation: 传统方法将增强和融合分离，忽略了二者之间的内在关联。
method: 引入显著区域概念，动态增强弱模态以提升融合质量。
result: 在多个数据集上融合结果质量优于分离方法。
conclusion: 联合增强与融合是提升图像融合质量的有效策略。
---

## Abstract
Image fusion aims to integrate comprehensive information from images acquired through multiple sources. However, images captured by diverse sensors often encounter various degradations that can negatively affect fusion quality. Traditional fusion methods generally treat image enhancement and fusion as separate processes, overlooking the inherent correlation between them; notably, the dominant regions in one modality of a fused image often indicate areas where the other modality might benefit from enhancement. Inspired by this observation, we introduce the concept of dominant regions for image enhancement and present a Dynamic Relative EnhAnceMent framework for Image Fusion (Dream-IF). This framework quantifies the relative dominance of each modality across different layers and leverages this information to facilitate reciprocal cross-modal enhancement. By integrating the relative dominance derived from image fusion, our approach supports not only image restoration but also a broader range of image enhancement applications. Furthermore, we employ prompt-based encoding to capture degradation-specific details, which dynamically steer the restoration process and promote coordinated enhancement in both multi-modal image fusion and image enhancement scenarios. Extensive experimental results demonstrate that Dream-IF consistently outperforms its counterparts.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **研究动机**：传统图像融合方法通常将图像增强（退化恢复）和融合视为两个独立任务，或者简单级联，忽略了二者之间的内在关联。在多模态图像（如可见光与红外）中，一个模态的显著区域往往对应另一个模态的弱区域，融合后可以指导增强。
- **核心问题**：如何在融合过程中动态利用模态间的互补性，同时实现鲁棒的退化恢复和高质量融合，提升模型在多种退化场景下的泛化能力。
- **整体含义**：本文首次提出“相对优势”概念，利用融合过程中各模态的互补性来指导增强，将增强与融合有机协同，实现“以融合促增强，以增强助融合”，从而在退化条件下仍能保持融合图像的质量。

## 2. 论文提出的方法论
- **核心思想**：引入“相对优势（Relative Dominance, RD）”量化每个模态在融合中的主导程度，并利用RD指导弱模态的交叉增强和自增强，同时通过提示编码（prompt-based encoding）动态捕捉退化特征，无先验地完成盲恢复与融合。
- **关键技术细节**：
  - 整体框架：采用U-Net结构，编码器由Restormer块组成，提取可见光和红外特征；解码器逐层重建，并在每层引入相对增强（RE）模块。
  - 相对优势（RD）计算：对每个模态的特征图 \( F^m \) 通过卷积和激活函数得到权重图 \( RD^m = \sigma(\text{Conv}(F^m)) \)，并通过损失函数 \( L_{RD} = \sum_{h,w} |\sum_{m} RD^m_{h,w} - 1| \) 约束其和为1。
  - 交叉增强（CE）模块：利用另一模态的RD图增强当前模态的弱区域：\( \dot{F}^m = F^m + \text{CE}(F^m, RD) \)。
  - 自增强（SE）模块：结合RD提示和可学习参数z进一步恢复自身：\( \hat{F}^m = F^m + \text{SE}(\text{RDP}(F^m, RD, z), F^m) \)，其中RDP为相对优势提示模块，动态生成恢复提示。
  - 融合与损失：增强后的特征经卷积拼接后送入融合模块；总损失 \( L = L_f + L_{RD} \)，\( L_f \) 包括像素损失、梯度损失、SSIM损失和颜色损失。

## 3. 实验设计
- **数据集**：LLVIP（训练12025对，测试70对，随机退化处理）、MSRS和M3FD（仅用于测试泛化能力，无微调）。
- **退化场景**：模拟高斯噪声（σ=35）、泊松噪声、散斑噪声、模糊、缩放、雨天等，用于评估鲁棒性。
- **对比方法**：TarDAL, CDDFuse, EMMA, Text-IF, TTD, Text-DiFuse, FusionBooster（共7种SOTA融合方法）。
- **评价指标**：AG（平均梯度）、SF（空间频率）、Qabf（梯度相似性）、SSIM（结构相似性）、VIFF（视觉信息保真度）、LPIPS（感知相似性，用于单阶段融合对比）、Recall和mAP（用于下游目标检测）。
- **对比形式**：
  - 无退化对比：在三个数据集上直接比较。
  - 有退化对比：
    - 两阶段融合：先使用SCUNet恢复再融合，Dream-IF为端到端盲恢复融合。
    - 单阶段融合：与Text-IF和Text-DiFuse直接对比（需退化类型描述，Dream-IF无先验）。
  - 下游任务：用YOLOv11在LLVIP融合结果上训练并测试检测性能。
  - 消融实验：移除CE、SE模块，评估各组件贡献。

## 4. 资源与算力
- 文中在“Implementation Details”中说明：使用PyTorch实现，在NVIDIA 3090 GPU（单卡）上训练，未报告训练时长或具体迭代次数。论文未提及多卡并行或总计算量，资源开销信息有限。

## 5. 实验数量与充分性
- **实验数量**：共涵盖6组实验（无退化对比、有退化两阶段对比、有退化单阶段对比、下游检测、消融实验（含5个变体）），每个实验均在多个数据集上测试，指标全面。
- **充分性**：
  - 数据集覆盖三个公开数据集（LLVIP、MSRS、M3FD），其中两个作为零样本泛化测试，分布广泛。
  - 退化类型多样（4种噪声+模糊+缩放+雨天），模拟真实场景。
  - 消融实验系统验证CE、SE、RD各模块的效果，并报告了定量指标。
  - 下游任务实验提供了高层视觉任务验证，增加说服力。
- **客观性与公平性**：对比方法均采用公开代码或官方结果，两阶段融合使用统一的SCUNet恢复模型，文本引导方法给予额外退化描述（公平偏向弱化），整体对比设置较为公正。但未与更近期方法（如2025年后）对比（论文为AAAI-26，可视为最新方法）。

## 6. 论文的主要结论与发现
- Dream-IF在无退化、多种退化及下游检测任务中均优于现有SOTA方法，尤其在AG、SF、Qabf、VIFF等指标上表现突出。
- 提出的相对优势机制有效捕捉模态间的互补性，利用RD引导交叉增强和自增强，能够显著提升退化图像的融合质量。
- 盲恢复能力：无需知道退化类型即可同时完成恢复与融合，优于需要退化先验的Text-IF等方法。
- 消融实验证明CE、SE和RD损失均对性能有正向贡献，且协同效果更佳。

## 7. 优点
- **创新性**：首次明确将图像融合中的模态互补性（相对优势）用于指导增强，突破了传统“先恢复后融合”或单独处理两任务的范式。
- **技术亮点**：
  - 相对优势度（RD）的可微设计，可端到端学习。
  - 交叉增强（CE）和自增强（SE）模块设计简洁有效，可嵌入任意融合骨干。
  - 利用提示编码实现盲恢复，无需退化类型先验。
- **实验全面性**：多数据集、多退化、多指标、下游任务验证，且消融充分。
- **代码开源**，便于复现和应用。

## 8. 不足与局限
- **计算资源报告不足**：未提供训练时间、参数数量、推理速度等，难以评估实际部署成本。
- **退化模拟有限**：仅使用合成噪声，未在真实退化图像（如低光、雾霾、运动模糊）上验证，现实应用效果未知。
- **下游任务单一**：仅测试了目标检测（YOLOv11），未覆盖语义分割、行人重识别等其他任务。
- **对比方法时效性**：虽为2026年论文，但对比方法多为2023-2025年，未与同期更新的方法（如2025年下半年的方法）比较。
- **架构依赖**：基于Restormer块，计算复杂度可能较高，在资源受限设备上的效果需进一步验证。
- **局限性讨论缺失**：论文未明确讨论潜在的失败案例或场景（如极端光照、严重遮挡等）。

（完）
