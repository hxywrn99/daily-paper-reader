---
title: "DCEvo: Discriminative Cross-Dimensional Evolutionary Learning for Infrared and Visible Image Fusion"
title_zh: DCEvo：用于红外与可见光图像融合的判别性跨维度进化学习
authors: "Liu, Jinyuan, Zhang, Bowei, Mei, Qingyun, Li, Xingyuan, Zou, Yang, Jiang, Zhiying, Ma, Long, Liu, Risheng, Fan, Xin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liu_DCEvo_Discriminative_Cross-Dimensional_Evolutionary_Learning_for_Infrared_and_Visible_Image_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 进化学习联合优化的红外与可见光图像融合
tldr: 该文针对红外与可见光融合中图像融合与高层任务分离导致融合图像任务增益有限的问题，提出DCEvo。利用进化学习的鲁棒搜索能力，交叉维度优化融合和感知精度，实现融合质量与任务性能的协同提升。实验表明在多个基准上融合图像既视觉清晰又对下游任务有益。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1796, \"height\": 495}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1791, \"height\": 888}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1788, \"height\": 430}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 861, \"height\": 488}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1799, \"height\": 537}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1793, \"height\": 495}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1807, \"height\": 445}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 875, \"height\": 401}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 872, \"height\": 499}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1813, \"height\": 610}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1813, \"height\": 594}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 869, \"height\": 248}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 873, \"height\": 294}]"
motivation: 现有方法将融合与高层任务分离，融合图像对任务性能提升有限。
method: 提出判别性跨维度进化学习框架，联合优化融合质量和感知精度。
result: 在视觉质量和下游任务性能上均显著提升。
conclusion: 进化学习可有效协同融合与高层任务。
---

## Abstract
Infrared and visible image fusion integrates information from distinct spectral bands to enhance image quality by leveraging the strengths and mitigating the limitations of each modality. Existing approaches typically treat image fusion and subsequent high-level tasks as separate processes, resulting in fused images that offer only marginal gains in task performance and fail to provide constructive feedback for optimizing the fusion process. To overcome these limitations, we propose a Discriminative Cross-Dimension Evolutionary Learning Framework, termed DCEvo, which simultaneously enhances visual quality and perception accuracy. Leveraging the robust search capabilities of Evolutionary Learning, our approach formulates the optimization of dual tasks as a multi-objective problem by employing an Evolutionary Algorithm (EA) to dynamically balance loss function parameters. Inspired by visual neuroscience, we integrate a Discriminative Enhancer (DE) within both the encoder and decoder, enabling the effective learning of complementary features from different modalities. Additionally, our Cross-Dimensional Embedding (CDE) block facilitates mutual enhancement between high-dimensional task features and low-dimensional fusion features, ensuring a cohesive and efficient feature integration process. Experimental results on three benchmarks demonstrate that our method significantly outperforms state-of-the-art approaches, achieving an average improvement of 9.32% in visual quality while also enhancing subsequent high-level tasks. The code is available at https://github.com/Beate-Suy-Zhang/DCEvo.

---

## 论文详细总结（自动生成）

# DCEvo：用于红外与可见光图像融合的判别性跨维度进化学习——详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：红外与可见光图像融合（IVIF）中，现有方法通常将图像融合与后续高层视觉任务（如目标检测、语义分割）视为独立过程，导致融合图像对任务性能提升有限，且任务结果无法为融合优化提供有效反馈。此外，多目标优化中损失函数超参数的手动调节难以平衡融合质量和任务精度，特征在不同维度（像素级与任务级）之间也缺乏协调。
- **研究动机**：利用进化学习的强大搜索能力，将融合与高层任务的协同优化建模为多目标优化问题，通过进化算法自动搜索最优损失函数权重，同时设计判别性增强器和跨维度特征嵌入，实现视觉质量与感知精度的同步提升。
- **整体含义**：首次将进化学习引入IVIF领域，提出一种端到端的判别性跨维度进化学习框架（DCEvo），在多个基准上显著超越现有方法，为图像融合与下游任务的联合优化提供了新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 2.1 核心思想
DCEvo框架由三个核心模块构成：
- **进化学习策略**：将融合损失与检测损失的权重系数作为进化个体，通过遗传算法迭代优化，自动寻找双目标任务的最佳平衡点。
- **判别性增强器（DE）**：受视觉神经科学启发，通过特征图的自适应调制增强目标与背景的差异，突出显著物体，用于编码器和解码器。
- **跨维度嵌入块（CDE）**：利用交叉注意力机制，将高层任务特征（如检测特征）嵌入到低层融合特征中，实现跨维度特征相互增强。

### 2.2 关键技术细节
- **进化算法（EA）超参数优化**：
  - 初始种群随机生成，每个个体为损失函数系数向量（包含融合损失和检测损失的各子项权重）。
  - 适应度函数由融合损失和检测损失共同构成（越低越好）。
  - 每代通过选择、交叉、变异产生新个体，采用轮盘赌选择，迭代更新种群，最终输出全局最优解。
  - 算法伪代码（Algorithm 1）给出：种群大小N=5，迭代次数In=5，变异概率Pm=10%。
- **判别性增强器（DE）**：
  - 基于特征图均值μ和Sigmoid激活函数，对每个像素计算增强权重：\(\tilde{x}_i = x_i \cdot S\left(\frac{y_i^2}{\mu_y}\right)\)，其中\(y_i = S(x_i)\)，\(\mu_y\)为Sigmoid输出的均值。
  - 放大目标区域（像素值较大）与背景区域的差异，提升物体显著性。
- **跨维度嵌入块（CDE）**：
  - 检测网络输出的高层特征F_det通过Patch Align（PA）切片，与红外、可见光特征（F_ir, F_vis）分别经过CNN块后，组成Q（来自检测特征）、K（红外特征）、V（可见光特征），进行交叉注意力计算。
  - 输出融合后的特征图\(\hat{F}_{ca}\)，再送入自注意力模块，最终由解码器生成融合图像。
- **总体损失函数**：
  - 融合损失：\(L_{Fu} = L_{SSIM} + L_{deco} + L_{grad} + L_{int}\)
  - 检测损失：\(L_{Det} = L_{cls} + L_{box} + L_{dfl}\)（分类、CIoU、分布聚焦损失）
  - 进化算法动态平衡两部分损失的系数。

## 3. 实验设计

### 3.1 数据集与场景
- **融合任务**：M3FD、RoadScene、TNO、FMB四个数据集，分别包含300、221、57、280对红外/可见光图像，场景涵盖低光照、高亮度、低质量等。
- **下游任务**：
  - 目标检测：M3FD数据集（80/20划分），使用YOLOv8s作为检测器。
  - 语义分割：FMB数据集，使用SegFormer-b1作为分割模型。

### 3.2 Benchmark与对比方法
- 对比了12种最新方法：DDFM、YDTR、ReCoNet、TarDAL、LRRNet、MetaFusion、SwinFusion、TIM、CDDFuse、SegMiF、EMMA、Text-IF（均在CVPR/ICCV/ECCV等顶会发表）。
- 评测指标：MI（互信息）、SSIM、VIF、Qabf（融合质量）；检测用mAP（各类别平均精度）；分割用mIoU。

## 4. 资源与算力

- **原文未明确说明GPU型号、数量、训练时长等具体算力信息**。仅提及“所有实验使用PyTorch实现”，进化学习在每epoch后更新模型，但未提供训练时间或硬件配置。
- 缺失算力细节，是论文的一个信息不足点。

## 5. 实验数量与充分性

- **实验数量充足**：
  - 融合性能：在4个数据集上对比12种方法，定性（图4）和定量（表1）均完整。
  - 下游任务：检测（M3FD）和分割（FMB）均给出详细定量结果（表2）和定性示例（图5、图6）。
  - 消融实验：包括对进化算法（表3：对比经验值、均匀系数、分离训练）、模型架构（表4：DE和CDE逐步添加）、特征可视化（图9：Grad-CAM）进行系统分析。
  - 归一化混淆矩阵（图7）展示各模块贡献。
- **公平性**：对比方法均选自近年顶会，且使用相同下游模型（YOLOv8s、SegFormer-b1）评估，控制变量合理；消融实验逐步验证各组件有效性。
- **充分性评价**：实验覆盖面广，定量定性结合，消融设计严谨，能较好支撑结论。

## 6. 论文的主要结论与发现

- DCEvo在视觉质量（MI、SSIM、VIF、Qabf）上平均提升9.32%，在目标检测mAP（62.13% vs 次优61.71%）和语义分割mIoU（52.52% vs 次优51.55%）上均达到最优。
- 进化学习策略能有效自动平衡融合与检测损失，避免手动调参的次优问题。
- 判别性增强器显著提升网络对目标区域的关注（图9特征激活图），跨维度嵌入实现任务级与像素级特征的互补。
- 证明IVIF中融合与高层任务协同优化是可行且有益的，进化学习是解决多目标优化的有效工具。

## 7. 优点

- **方法创新性**：首次将进化学习引入IVIF，解决多任务损失平衡难题，思想新颖且实用。
- **模块设计合理**：DE基于视觉神经科学原理，数学推导清晰（从特征均值差异引出调制公式）；CDE利用交叉注意力使不同维度特征双向增强，设计精巧。
- **实验全面且结果显著**：在多个数据集和下游任务上均大幅超越SOTA，消融实验验证每个组件的必要性，可视化结果直观。
- **代码开源**（GitHub链接已提供），便于复现和后续研究。

## 8. 不足与局限

- **算力信息缺失**：未报告训练所需的GPU型号、数量、总训练时长，不利于评估方法实际效率与可复现性。
- **进化算法的收敛性分析不足**：仅给出种群大小和迭代次数（5,5），未展示适应度曲线或不同参数对性能的影响，可能存在局部最优风险。
- **跨数据集泛化验证有限**：虽然用了4个融合数据集，但下游检测仅在一个数据集（M3FD）上评估，分割仅用FMB，跨域泛化能力未充分验证（如在不同场景/传感器下）。
- **应用限制**：方法依赖任务级特征（如检测网络），对于无标签或新任务场景需要额外标注，且进化算法的在线搜索会增加训练复杂度，可能不适用于轻量级部署。
- **对比方法时效性**：尽管对比了2024年方法（如Text-IF、EMMA），但未与最新（2025年）方法比较，由于论文发表于CVPR2025，可能缺乏同台竞争的最新模型。

（完）
