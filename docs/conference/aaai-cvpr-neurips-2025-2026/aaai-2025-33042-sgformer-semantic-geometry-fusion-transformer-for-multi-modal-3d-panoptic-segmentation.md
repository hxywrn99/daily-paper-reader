---
title: "SGFormer: Semantic-Geometry Fusion Transformer for Multi-modal 3D Panoptic Segmentation"
title_zh: SGFormer：面向多模态3D全景分割的语义-几何融合Transformer
authors: "Hongqi Yu, Sixian Chan, Xiaolong Zhou, Xiaoqin Zhang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33042/35197"
tags: ["query:image-fusion"]
score: 4.0
evidence: 多模态融合用于3D全景分割，非纯图像融合
tldr: 提出SGFormer模型用于自动驾驶多模态3D全景分割，通过语义-几何融合Transformer提取自适应语义上下文和几何信息，提升分割性能。该模型在nuScenes等数据集上取得最优结果，但方法聚焦于3D点云与图像融合，不直接适用于2D图像融合任务，仅作为多模态特征融合的启发。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 744, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 373, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1823, \"height\": 979, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1809, \"height\": 483, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 865, \"height\": 396, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 878, \"height\": 196, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1791, \"height\": 504, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 882, \"height\": 410, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33042/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 877, \"height\": 489, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 409, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 879, \"height\": 295, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 878, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 876, \"height\": 238, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 878, \"height\": 246, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 847, \"height\": 354, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 869, \"height\": 253, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 882, \"height\": 199, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33042/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 876, \"height\": 191, \"label\": \"Table\"}]"
motivation: 现有方法在多模态3D全景分割中语义提取不足，缺乏空间几何信息。
method: 提出语义-几何融合Transformer，分别提取语义上下文和几何信息并进行融合。
result: 在自动驾驶数据集上取得最佳分割性能。
conclusion: SGFormer有效利用多模态信息提升3D分割精度，但非图像融合专用方法。
---

## Abstract
Modern methods for autonomous driving perception widely adopt multi-modal fusion to enhance 3D scene understanding. However, existing methods suffer from inferior semantic extraction in image encoders that treat all pixels equally, ignoring contextual differences. The generated multi-modal representations also typically lack comprehensive semantic and spatial geometry information, which is crucial for the 3D panoptic segmentation task. In this paper, we propose a novel Semantic-Geometry Fusion Transformer (SGFormer) that extracts adaptive semantic contexts, aggregates geometric information and captures the semantic-geometry fusion. First,  in the Image Branch, we tailor semantic contexts for each pixel with context-guided attention and spatial context alignment to refine semantic details. Second, we transform image and voxel features into point-pixel geometry representations, simultaneously learning  semantic category priors as embeddings to better represent scene geometry and semantics. Finally, to aggregate semantic information with related geometry, we design a semantic-geometry fusion that combines the transformer, effectively capturing semantic-geometry relationships into multi-modal panoptic representations. Notably, SGFormer achieves the state-of-the-art (SOTA)  results on the nuScenes and SemanticPOSS, as well as yielding competitive performance on the SemanticKITTI. Moreover, SGFormer exhibits superior robustness  compared to leading methods, marking an improvement of 2% to 10%.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有面向自动驾驶的多模态（LiDAR+图像）3D全景分割方法中，图像编码器对所有像素一视同仁，忽视了像素间的上下文差异，导致语义提取不足。同时，生成的多模态表示缺乏综合的语义与空间几何信息，而这对于精细的3D全景分割至关重要。
- **动机**：如何为每个像素自适应地聚合上下文语义、如何有效融合语义与几何信息以生成高质量多模态全景表示，是亟待解决的关键挑战。

## 2. 方法论：核心思想与关键技术细节
论文提出 **SGFormer（Semantic-Geometry Fusion Transformer）**，整体包含两大模块：

### (1) 自适应语义上下文聚合（ASCA）
- **Context-Guided Attention (CGA)**：利用像素-上下文注意力机制，为每个像素聚合语义相关的上下文信息，通过矩阵乘法和残差结构增强语义判别性。
- **Spatial Context Alignment (SCA)**：通过特征分组、偏移预测和门控机制，自适应对齐不同层级特征的空间细节，弥补下采样带来的空间信息损失。
- **Point-Pixel Refinement (PPR)**：利用LiDAR到相机的投影建立点-像素映射，从而根据点索引精炼图像特征。

### (2) 语义-几何Transformer（SGTransformer）
- **几何聚合**：将图像特征和体素特征通过点-像素映射转换为点-像素几何表示（`Gf`）。
- **语义学习**：利用两个MLP分类器从图像和体素模态分别学习语义类别先验（概率分布），并通过阈值过滤前景类别，生成语义嵌入（`Ec, Ev`）。
- **语义-几何融合**：对拼接的语义特征进行多头自注意力（MHSA）建模跨模态语义交互，然后以几何特征作为查询、语义特征作为键值进行多头交叉注意力（MHCA），聚合与几何相关的语义信息，最后经FFN输出融合后的语义-几何特征（`Fsg`）。

整体算法：输入点云和图像 → 体素编码器 + ASCA图像编码器 → 获取点-像素几何特征和语义嵌入 → SGTransformer融合 → BEV特征 → 全景分割头（语义+实例）。

## 3. 实验设计
- **数据集**：
  - nuScenes（1000场景，大规模户外）
  - SemanticKITTI（22序列，仅前视双相机）
  - SemanticPOSS（6序列，2988帧，小且稀疏）
- **Benchmark**：3D全景分割，主要指标包括PQ（全景质量）、SQ（分割质量）、RQ（识别质量）、mIoU（语义分割IoU），并按things/stuff分别统计。
- **对比方法**：
  - LiDAR-only：GP-S3Net、EfficientLPS、Panoptic-PolarNet、SCAN、Panoptic PH-Net、CPSeg、4D-Former、PUPS、CFNet等
  - 多模态（LiDAR+相机）：LCPS
  - 鲁棒性对比：Panoptic-PolarNet†、CFNet†、LCPS†
- **实验类型**：
  - 主表对比（3个数据集上的验证集和测试集）
  - 消融实验（网络架构、ASCA各组件、SGTransformer各组件）
  - 鲁棒性实验（点云卡顿20%、相机遮挡、FoV限制）
  - 不同场景条件测试（晴天/雨天/夜晚/雨夜）
  - 泛化能力评估（Cityscapes、PASCAL VOC、CamVid语义分割；Robo3D分布外泛化）
  - 效率与精度对比（延迟/参数量 vs. PQ）
  - 语义与几何分析（类别级PQ、不同距离的实例分类精度）

## 4. 资源与算力
- **未明确说明**：论文正文及实验设置部分未提及具体使用的GPU型号、数量、训练时长。仅在实现细节中提及部分参数（如融合层数、注意力头数、通道数等），但未报告硬件与耗时信息。因此无法对计算资源进行总结。

## 5. 实验数量与充分性
- **实验数量充足**：共包含：
  - 3个数据集上的SOTA对比（含val和test）
  - 多个消融实验（表6~8，共约8组消融变体）
  - 鲁棒性实验（表5，3种噪声类型）
  - 场景适应性实验（图5，4种天气）
  - 泛化实验（表9~10，4个额外数据集 + 2个分布外基准）
  - 效率对比（图8）
  - 定性可视化（图4、图6）
- **充分性与公平性**：
  - 所有对比均采用官方或作者报告的指标，指标全面（PQ、SQ、RQ、mIoU等）。
  - 消融实验逐步验证各模块贡献，设计合理。
  - 鲁棒性实验采用与nuScenes-R一致的数据损坏方式，并与其他方法复现结果公平对比。
  - 泛化实验将ASCA作为即插即用模块嵌入其他基线，验证其通用性。
  - 整体实验覆盖了性能、鲁棒性、泛化、效率、可视化等多个维度，客观且全面。

## 6. 论文的主要结论与发现
- SGFormer在nuScenes验证集上达到PQ 80.9%（比LCPS高1.1%），在测试集上达到80.4%，在SemanticPOSS上达51.7%（超越所有对比方法），在SemanticKITTI上达64.2%（相比LCPS提高5.2%）。
- 鲁棒性方面，在三类噪声下相对领先方法提升2%~10%。
- 消融实验证明每个设计组件（CGA、SCA、PPR、几何聚合、语义学习、语义-几何融合）均有效。
- ASCA可独立作为即插即用模块提升现有语义分割基线的mIoU（+0.5~1.0%）。
- 语义-几何融合能有效捕获类别先验与空间几何关系，尤其对远处小目标感知提升明显。

## 7. 优点
- **创新性强**：首次在3D全景分割中提出显式的语义-几何融合Transformer，区别于简单的特征拼接或对齐。
- **有效解决现有问题**：针对图像编码器缺乏上下文差异、多模态表示缺少几何信息两大痛点给出了解决方案。
- **鲁棒性突出**：在相机遮挡、LiDAR卡顿、有限FoV等噪声下性能稳定，优于LCPS。
- **泛化能力好**：ASCA模块可迁移至多种语义分割任务，且在分布外数据上表现更优。
- **效率与精度平衡**：与多模态方法LCPS相比，延迟更低且精度更高；接近LiDAR-only方法的实时性（<100ms）。

## 8. 不足与局限
- **计算资源未报告**：缺少GPU型号、训练时长等信息，读者难以评估复现成本。
- **应用范围有限**：方法专为3D点云与图像融合设计，不直接适用于纯图像或纯点云的2D/3D分割任务。
- **对相机数量的依赖性**：在SemanticKITTI（仅前视双相机）上性能虽有提升，但相比多相机场景（nuScenes，6相机）仍有差距，未充分探讨少量相机时的鲁棒性边界。
- **潜在偏差风险**：实验主要基于自动驾驶室外场景，室内或低结构场景未验证；鲁棒性测试仅覆盖三类损坏，可能不足以模拟真实全部退化情况。
- **缺少失败案例分析**：论文未展示典型错误分割或失败案例，不利于全面理解模型短板。

（完）
