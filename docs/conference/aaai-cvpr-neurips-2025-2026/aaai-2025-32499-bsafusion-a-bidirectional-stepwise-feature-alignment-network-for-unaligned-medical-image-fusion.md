---
title: "BSAFusion: A Bidirectional Stepwise Feature Alignment Network for Unaligned Medical Image Fusion"
title_zh: BSAFusion：面向未对齐医学图像融合的双向逐步特征对齐网络
authors: "Huafeng Li, Dayong Su, Qing Cai, Yafei Zhang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32499/34654"
tags: ["query:image-fusion"]
score: 10.0
evidence: 医学图像融合，双向逐步特征对齐
tldr: 未对齐多模态医学图像的同时对齐与融合面临兼容性挑战。本文提出BSAFusion，采用双向逐步特征对齐与融合策略，逐步减少模态差异，并引入模态差异无关特征表示以提升匹配鲁棒性。在多种未对齐医学图像数据集上，该方法实现了精确对齐和高质量融合，优于分步处理方案，为医学图像融合提供了高效端到端解决方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32499/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 792, \"height\": 689, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32499/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1840, \"height\": 785, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32499/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 883, \"height\": 273, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32499/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1661, \"height\": 1126, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32499/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 802, \"height\": 784, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32499/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 801, \"height\": 708, \"label\": \"Figure\"}]"
motivation: 解决未对齐医学图像融合中特征对齐与融合的不兼容问题。
method: 提出双向逐步特征对齐与融合策略，结合模态差异无关特征表示。
result: 在医学图像融合任务上有效对齐并提升融合质量。
conclusion: BSAFusion实现了未对齐医学图像的高效同步对齐与融合。
---

## Abstract
If unaligned multimodal medical images can be simultaneously aligned and fused using a single-stage approach within a unified processing framework, it will not only achieve mutual promotion of dual tasks but also help reduce the complexity of the model. However, the design of this model faces the challenge of incompatible requirements for feature fusion and alignment. To address this challenge, this paper proposes an unaligned medical image fusion method called Bidirectional Stepwise Feature Alignment and Fusion (BSFA-F) strategy. To reduce the negative impact of modality differences on cross-modal feature matching, we incorporate the Modal Discrepancy-Free Feature Representation (MDF-FR) method into BSFA-F. MDF-FR utilizes a Modality Feature Representation Head (MFRH) to integrate the global information of the input image. By injecting the information contained in MFRH of the current image into other modality images, it effectively reduces the impact of modality differences on feature alignment while preserving the complementary information carried by different images. In terms of feature alignment, BSFA-F employs a bidirectional stepwise alignment deformation field prediction strategy based on the path independence of vector displacement between two points. This strategy solves the problem of large spans and inaccurate deformation field prediction in single-step alignment. Finally, Multi-Modal Feature Fusion block achieves the fusion of aligned features. The experimental results across multiple datasets demonstrate the effectiveness of our method.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：多模态医学图像融合（CT、MRI、PET等）在临床中具有重要意义，但传统方法假设源图像已严格像素对齐。实际应用中图像采集存在刚性和非刚性变形，导致对齐假设失效。
- **现有方法局限**：两阶段方法（先配准后融合）需要独立、完整的配准模型，增加了模型复杂度，且配准性能直接影响融合效果；联合配准与融合方法（如U2Fusion、MURF等）虽尝试一体化处理，但未针对医学图像设计，且常采用“配准在前、融合在后”的两阶段处理模式，特征提取需求不兼容。
- **本文目标**：提出一个**单阶段**、统一的未对齐医学图像配准与融合框架，使两个任务在同一处理流程中互相促进，同时避免引入额外编码器带来的复杂度增加。

## 2. 方法论
### 核心思想
- 设计**模态差异无关特征表示（MDF-FR）** 减小模态差异；采用**双向逐步特征对齐（BSFA）** 策略预测变形场；最后通过**多模态特征融合（MMFF）** 得到融合图像。

### 关键技术细节
- **编码器**：使用Restormer + Transformer层提取特征，得到浅层特征 \(F_s^A, F_s^B\)（用于保留细节）和深层特征 \(\hat{F}_A, \hat{F}_B\)（用于对齐与融合）。
- **MDF-FR**：
  - 为每个输入图像附加一个**模态特征表示头（MFRH）**（\(\hat{f}_A, \hat{f}_B\)），通过MLP和交叉熵损失 \(L_{ce1}\) 确保其包含模态信息。
  - 通过公式 (2) 将 \(\hat{f}_B\) 注入 \(\hat{F}_A\)，\(\hat{f}_A\) 注入 \(\hat{F}_B\)，消除模态差异。
  - 再经过两个独立的Transfer块（TransferA/B）提取特征 \(\bar{F}_A, \bar{F}_B\)，并用交叉熵损失 \(L_{ce2}\) 约束其不再具有模态特异性。
- **BSFA（双向逐步特征对齐）**：
  - 理论基础：两点间矢量位移具有**路径独立性**（公式4），可通过多步累积。
  - 设计**前向配准层（FRL）** 和**反向配准层（RRL）**，各含K层（本文K=5），逐步预测变形场 \(\phi_A^i\) 和 \(\phi_B^i\)。
  - 每一步使用concat、warp、上采样操作，最终通过公式 (7) 合成直接对齐的变形场 \(\phi_{\overrightarrow{AB}} = \phi_A - \phi_B\)。
  - 引入平滑损失 \(L_{smooth}\) 和一致性损失 \(L_{consis}\) 优化变形场。
- **MMFF**：将 \(\phi_{\overrightarrow{AB}}\) 作用于浅层和深层特征，通过多个FusionBLK逐步融合，最后经Restormer+卷积+Sigmoid得到融合图像。
- **总损失函数**（公式14）：\(L_{total} = L_{ce1} + L_{ce2} + L_{consis} + L_{smooth} + L_{struct} + L_{grad} + \lambda L_{inten}\)，其中结构损失、梯度损失、强度损失共同约束融合图像质量。

## 3. 实验设计
- **数据集**：来自哈佛大学公共数据库，共三个模态组合：
  - CT-MRI：144对训练，20对测试
  - PET-MRI：194对训练，55对测试
  - SPECT-MRI：261对训练，77对测试
  - 所有图像原始大小为256×256，且已严格配准。
- **模拟未对齐**：以MRI为参考，对非MRI图像施加**刚性和非刚性混合变形**，每个epoch随机生成不同的变形以增加多样性，同时采用随机旋转和翻转进行数据增强。
- **对比方法**（联合配准与融合范畴）：UMF-CMGR, SuperFusion, MURF, IMF, PAMRFuse。此外，与“Registration+Fusion”范式方法的比较在补充材料中给出。
- **评价指标**：QAB/F（越高越好）、QCV（越低越好）、QVIF（越高越好）、QS（越高越好）、QSSIM（越高越好）。
- **消融实验**：
  - MDF-FR消融：设置A（无MFRH交换且无Lce2）、设置B（有Lce2但无交换），对比完整方法。
  - BSFA消融：去除BSFA、仅保留RRL、仅保留FRL，对比对齐效果。

## 4. 资源与算力
- **硬件**：单张NVIDIA GeForce RTX 4090 GPU。
- **框架**：PyTorch。
- **训练配置**：
  - 每个数据集训练3000个epoch。
  - Batch size = 32。
  - 初始学习率 \(5\times10^{-5}\)，采用余弦退火调度降至 \(5\times10^{-7}\)。
  - 优化器：Adam。
- **时间**：原文未明确给出总训练时长，但单卡4090处理约200-260对训练样本、3000 epoch，实际时间可估算为数小时至十余小时。缺乏精确时间说明。

## 5. 实验数量与充分性
- **数量**：在3个不同模态数据集上进行了实验，与5种SOTA联合方法对比。提供了可视化图像（图4）和箱线图（图5）展示分布。
- **消融实验**：两组关键消融（MDF-FR和BSFA），每组含2-3种变体，使用对齐效果可视化（图6）证明有效性。
- **充分性**：
  - 实验覆盖了主要医学模态组合，指标多样，结果具有统计意义。
  - 消融实验设计合理，逐步验证各组件作用。
  - 但**未与最优的两阶段配准+融合方法进行公开对比**（仅提到见补充材料），且未在真实临床未对齐数据上验证（全为仿真变形）。
  - 对比方法仅限联合配准与融合类，未包括纯配准+纯融合的最佳组合（如VoxelMorph + DDFM等），可能高估单阶段优势。

## 6. 主要结论与发现
- BSAFusion在三个数据集上均取得优于现有联合配准与融合方法的性能，在所有五个指标上平均值最高（图5箱线图）。
- 单阶段设计有效降低了模型复杂度（共享编码器），同时实现了对齐与融合的相互促进。
- MDF-FR通过模态头部信息注入能显著减少模态差异对对齐的干扰，保留互补信息。
- 双向逐步对齐策略（BSFA）相比单向或单步对齐，在大位移场景下对准精度更高且误差累积更小。

## 7. 优点
- **创新性**：首次在医学图像融合中提出“双向逐步对齐”策略，利用矢量位移路径独立性理论，有效解决大跨度位移问题。
- **实用性强**：单阶段端到端框架，无需单独配准模型，便于临床部署。
- **特征保留良好**：通过保留浅层细节特征并利用MFRH消除模态差异，融合结果同时保留边缘细节和全局结构。
- **实验设计规范**：采用模拟变形构建未对齐数据，控制变量合理；消融实验针对动机明确。
- **开源复现**：提供代码链接（GitHub），利于验证和推广。

## 8. 不足与局限
- **数据局限性**：所有实验均基于**仿真变形**的严格配准医学图像，未在真实临床未对齐图像上验证，可能低估真实场景中配准难度的差异（如呼吸运动、设备噪声等）。
- **对比范围有限**：仅与联合配准与融合方法对比，未系统比较“最优配准+最优融合”的两阶段组合，结论的通用性需谨慎。
- **模态覆盖不全**：仅涉及CT、MRI、PET、SPECT，未包括超声、X光等常见模态，泛化性未验证。
- **训练细节缺失**：未报告训练时间、收敛曲线、调参过程，难以复现算力需求。
- **超参数设置**：损失函数中的权重 \(\mu\) 需根据训练结果动态计算，增加了调参负担；\(\lambda\) 固定为0.5，缺乏灵敏度分析。
- **理论局限性**：BSFA的K层数（5层）未给出选择依据；双向对齐在弹性变形极大时可能仍存在误差累积，文中未分析极限情况。
- **偏差风险**：模拟变形参数（刚性与非刚性比例）未公开，可能导致对其他场景的适应性不足。

（完）
