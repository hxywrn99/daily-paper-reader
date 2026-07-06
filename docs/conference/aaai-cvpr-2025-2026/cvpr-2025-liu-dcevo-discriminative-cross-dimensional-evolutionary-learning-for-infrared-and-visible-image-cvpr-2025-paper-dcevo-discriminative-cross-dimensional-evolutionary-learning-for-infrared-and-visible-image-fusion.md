---
title: "DCEvo: Discriminative Cross-Dimensional Evolutionary Learning for Infrared and Visible Image Fusion"
title_zh: DCEvo：面向红外与可见光图像融合的判别式跨维度进化学习
authors: "Liu, Jinyuan, Zhang, Bowei, Mei, Qingyun, Li, Xingyuan, Zou, Yang, Jiang, Zhiying, Ma, Long, Liu, Risheng, Fan, Xin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liu_DCEvo_Discriminative_Cross-Dimensional_Evolutionary_Learning_for_Infrared_and_Visible_Image_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 红外与可见光图像融合的进化学习
tldr: 针对红外与可见光图像融合中融合与下游任务分离的问题，本文提出判别式跨维度进化学习框架DCEvo。该方法利用进化学习的搜索能力，同时优化融合图像的视觉质量和感知精度，实现融合与任务的协同提升。实验表明，DCEvo在多个数据集上均取得了更优的融合效果和目标检测性能。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1796, \"height\": 495, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1791, \"height\": 888, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1788, \"height\": 430, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 861, \"height\": 488, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1799, \"height\": 537, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1793, \"height\": 495, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1807, \"height\": 445, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 875, \"height\": 401, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 872, \"height\": 499, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1813, \"height\": 610, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1813, \"height\": 594, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 869, \"height\": 248, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liu-dcevo-discriminative-cross-dimensional-evolutionary-learning-for-infrared-and-visible-image-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 873, \"height\": 294, \"label\": \"Table\"}]"
motivation: 现有方法将融合与高层任务分离，限制了融合效果。
method: 提出判别式跨维度进化学习框架，协同优化视觉质量与感知精度。
result: 在融合质量和下游任务性能上均取得提升。
conclusion: 融合与任务的联合优化是提升性能的有效途径。
---

## Abstract
Infrared and visible image fusion integrates information from distinct spectral bands to enhance image quality by leveraging the strengths and mitigating the limitations of each modality. Existing approaches typically treat image fusion and subsequent high-level tasks as separate processes, resulting in fused images that offer only marginal gains in task performance and fail to provide constructive feedback for optimizing the fusion process. To overcome these limitations, we propose a Discriminative Cross-Dimension Evolutionary Learning Framework, termed DCEvo, which simultaneously enhances visual quality and perception accuracy. Leveraging the robust search capabilities of Evolutionary Learning, our approach formulates the optimization of dual tasks as a multi-objective problem by employing an Evolutionary Algorithm (EA) to dynamically balance loss function parameters. Inspired by visual neuroscience, we integrate a Discriminative Enhancer (DE) within both the encoder and decoder, enabling the effective learning of complementary features from different modalities. Additionally, our Cross-Dimensional Embedding (CDE) block facilitates mutual enhancement between high-dimensional task features and low-dimensional fusion features, ensuring a cohesive and efficient feature integration process. Experimental results on three benchmarks demonstrate that our method significantly outperforms state-of-the-art approaches, achieving an average improvement of 9.32% in visual quality while also enhancing subsequent high-level tasks. The code is available at https://github.com/Beate-Suy-Zhang/DCEvo.

---

## 论文详细总结（自动生成）

# 论文详细中文总结：DCEvo: 判别式跨维度进化学习用于红外与可见光图像融合

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：现有红外与可见光图像融合（IVIF）方法通常将图像融合与后续高层任务（如目标检测、语义分割）视为独立过程，导致融合图像仅能获得微弱的任务性能提升，且无法为融合优化提供有效反馈。
- **背景**：红外图像在黑暗、烟雾等环境下表现好但纹理少；可见光图像分辨率高、色彩丰富但在低光条件下性能差。融合旨在结合两者优势，提升复杂环境下的信息获取能力。但现有方法在处理多任务协同优化时面临两大挑战：多维度特征协调困难（融合与任务所需特征不一致）和损失函数调节困难（手动调参易顾此失彼）。
- **整体含义**：提出一种判别式跨维度进化学习框架（DCEvo），首次将进化学习引入IVIF领域，实现融合视觉质量与下游任务精度的协同优化。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 将融合与目标检测的双任务优化建模为多目标优化问题，利用进化算法（EA）动态搜索最优损失函数超参数，而非手动设定。
- 结合视觉神经科学原理设计判别式增强器（DE），增强特征表示；设计跨维度嵌入模块（CDE）促进高维任务特征与低维融合特征相互增强。

### 关键技术细节

#### （1）进化学习策略（EA）
- 采用遗传算法（GA），种群大小5，迭代次数5，变异率10%。
- 每个个体的适应度基于损失值计算（损失越低适应度越高），通过选择、交叉、变异迭代优化。
- 损失函数包括融合损失 \( L_{Fu} = L_{SSIM} + L_{deco} + L_{grad} + L_{int} \) 和检测损失 \( L_{Det} = L_{cls} + L_{box} + L_{dfl} \)。
- EA自动平衡二者权重，替代传统手动调参。

#### （2）判别式增强器（DE）
- 基于特征图均值 \(\mu\) 与像素值的关系，通过 Sigmoid 映射计算重要性权重，放大目标与背景差异。
- 公式：\(\tilde{x}_i = x_i \times S(y_i^2 / \mu_y)\)，其中 \(y_i = S(x_i)\)。
- 在融合网络的编码器和解码器中均集成DE，增强目标区域特征。

#### （3）跨维度嵌入模块（CDE）
- 将检测网络提取的高维任务特征（如特征金字塔）与融合网络的低维特征对齐，通过 Patch Align 切片匹配。
- 使用 QKV 注意力机制：Q = 检测特征补丁，K = 红外特征，V = 可见光特征，输出融合后的跨维度特征。
- 实现任务特征对融合特征的监督，使融合图像包含目标感知信息。

#### （4）整体训练流程
- 先在 MSRS 数据集上预训练融合模型，再在 M3FD 数据集上使用 EA 进行协同训练（每 epoch 更新超参数）。

## 3. 实验设计

### 数据集与场景
- **IVIF 融合评估**：M3FD（300张，多场景）、RoadScene（221张，道路场景）、TNO（57张，低光/军事）、FMB（280张，全时段）。
- **下游任务**：目标检测用 M3FD（80:20划分，YOLOv8s）；语义分割用 FMB（Segformer-b1）。
- **Benchmark**：对比12种SOTA方法：YDTR、SwinFusion、ReCoNet、TarDAL、SegMiF、DDFM、LRRNet、CDDFuse、MetaFusion、TIM、EMMA、Text-IF。

### 对比方法与指标
- **融合指标**：MI、SSIM、VIF、Qabf（共4项）。
- **检测指标**：mAP（含人物、汽车、公交车、灯、摩托车、卡车6类）。
- **分割指标**：mIoU（含7类：车、人、天空、自行车、摩托、杆、建筑等）。

### 消融实验
- 对EA、DE、CDE分别进行消融，包括：
  - EA vs 经验/均匀系数 vs 独立优化。
  - DE vs 无DE，以及编码器/解码器不同组合。
  - CDE vs 基础融合、拼接、交叉注意力等变体。

## 4. 资源与算力

论文中未明确说明使用的GPU型号、数量和训练时长。仅提到所有实验使用 PyTorch 实现，进化算法每 epoch 更新一次超参数。未提供详细的算力评估。

## 5. 实验数量与充分性

- **实验数量**：较为充分。
  - 融合性能：在4个数据集上对比12种方法，每组报告4个指标。
  - 下游任务：在M3FD上做检测（6类mAP），在FMB上做分割（7类mIoU）。
  - 消融实验：至少3组（EA、DE、CDE），各有多个子对比。
  - 可视化分析：融合图对比（图4）、检测结果对比（图5）、分割结果对比（图6）、特征激活图（图9）、混淆矩阵（图7）等。
- **充分性与公平性**：
  - 对比方法为近年发表的高水平论文（CVPR/ICCV/ECCV等），具有代表性。
  - 下游任务采用统一检测/分割模型（YOLOv8s、Segformer-b1），控制了变量。
  - 但未报告实验标准差或统计显著性测试，无法完全排除随机性影响。

## 6. 主要结论与发现

1. **融合性能**：DCEvo在MI、SSIM、VIF、Qabf上全面领先，平均视觉质量提升9.32%。
2. **下游任务**：检测mAP达62.13%，分割mIoU达52.52%，均高于所有对比方法（比第二名高1-2%）。
3. **进化学习的有效性**：EA自动搜索的超参数优于手动和均匀设置，避免陷入局部最优。
4. **DE与CDE的贡献**：DE增强了目标区域特征表达；CDE实现了任务特征对融合特征的有效监督，使融合图像更利于检测和分割。

## 7. 优点

- **方法创新**：首次将进化学习引入IVIF，实现融合与任务的联合优化；DE基于视觉神经科学的物理先验设计，可解释性强。
- **性能优势**：在多个数据集和多指标上显著超越SOTA，尤其在低光、烟雾等挑战场景下表现突出。
- **消融完备**：对三个核心组件进行了系统性的消融，验证了各自贡献。
- **下游任务验证**：不仅报告融合指标，还在检测和分割任务上验证实际应用价值，符合任务驱动融合的趋势。

## 8. 不足与局限

- **算力信息缺失**：未报告训练资源、耗时、模型参数量或FLOPs，不利于其他研究者复现和公平比较效率。
- **代码与复现**：虽提供了代码链接，但论文未包含关键超参数的具体数值（如EA的交叉概率、变异概率的精确设置，除变异率10%外）。
- **实验统计**：未报告多次实验的均值和方差，缺乏统计显著性分析，结果可能受单次运行影响。
- **场景覆盖**：实验数据集以道路、军事、通用场景为主，未涉及水下、遥感等更极端环境。
- **可扩展性**：当前仅结合目标检测和语义分割，对其他高层任务（如实例分割、深度估计）未经测试。

（完）
