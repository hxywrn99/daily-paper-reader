---
title: "DFormerv2: Geometry Self-Attention for RGBD Semantic Segmentation"
title_zh: DFormerv2：用于RGBD语义分割的几何自注意力
authors: "Yin, Bo-Wen, Cao, Jiao-Long, Cheng, Ming-Ming, Hou, Qibin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Yin_DFormerv2_Geometry_Self-Attention_for_RGBD_Semantic_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 7.0
evidence: RGB与深度特征融合
tldr: 针对RGBD语义分割中深度信息编码方式的问题，提出DFormerv2编码器，将深度图作为几何先验，通过几何自注意力机制与RGB特征融合，避免了显式深度编码。在复杂条件下（如低光照）提升了分割性能。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 729, \"height\": 770, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1784, \"height\": 324, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1789, \"height\": 369, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1781, \"height\": 754, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 865, \"height\": 683, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 859, \"height\": 443, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 859, \"height\": 493, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 857, \"height\": 214, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 851, \"height\": 473, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1792, \"height\": 1179, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 480, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 826, \"height\": 273, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 862, \"height\": 244, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 870, \"height\": 280, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 895, \"height\": 542, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-yin-dformerv2-geometry-self-attention-for-rgbd-semantic-segmentation-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 727, \"height\": 253, \"label\": \"Table\"}]"
motivation: 探索深度图是否需显式编码，将其作为几何先验与RGB融合。
method: 提出DFormerv2，使用几何自注意力将深度作为先验，而非直接编码。
result: 在RGBD语义分割上取得先进性能，尤其复杂光照条件。
conclusion: 深度作为几何先验可有效提升RGBD特征表示。
---

## Abstract
Recent advances in scene understanding benefit a lot from depth maps because of the 3D geometry information, especially in complex conditions (e.g., low light and overexposed). Existing approaches encode depth maps along with RGB images and perform feature fusion between them to enable more robust predictions. Taking into account that depth can be regarded as a geometry supplement for RGB images, a straightforward question arises: Do we really need to explicitly encode depth information with neural networks as done for RGB images? Based on this insight, in this paper, we investigate a new way to learn RGBD feature representations and present DFormerv2, a strong RGBD encoder that explicitly uses depth maps as geometry priors rather than encoding depth information with neural networks. Our goal is to extract the geometry clues from the depth and spatial distances among all the image patch tokens, which will then be used as geometry priors to allocate attention weights in self-attention. Extensive experiments demonstrate that DFormerv2 exhibits exceptional performance in various RGBD semantic segmentation benchmarks. Code is available at: https://github.com/VCIP-RGBD/DFormer.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：在RGBD语义分割中，现有方法普遍使用双编码器（双骨干网络）分别编码RGB和深度图，再进行特征融合。这带来了高计算开销和预训练不一致问题（骨干网络通常在RGB ImageNet上预训练，却同时输入RGB和深度）。作者提出一个根本性问题：**是否真的需要用神经网络显式编码深度信息？**
- **整体含义**：作者认为深度图本质上是场景的几何补充，因此可以**将深度直接作为几何先验**，而非作为另一个模态需要神经网络编码。由此提出的DFormerv2首次将深度信息与空间信息结合形成几何先验，并嵌入自注意力机制，实现了更高效准确的RGBD特征学习。

## 2. 方法论

### 核心思想
- **不显式编码深度图**：仅利用深度值计算patch间的几何关系，作为注意力权重分配的指导。
- **几何先验生成**：融合深度先验和空间先验。
  - **深度先验**：对深度图每个patch做平均池化得到深度值\(z_{ij}\)，计算任意两patch之间的深度距离\(D_{ij,i'j'} = |z_{ij} - z_{i'j'}|\)，得到深度关系矩阵\(D \in \mathbb{R}^{HW \times HW}\)。
  - **空间先验**：计算任意两patch之间的曼哈顿距离\(S_{ij,i'j'} = |i-i'| + |j-j'|\)，得到空间关系矩阵\(S \in \mathbb{R}^{HW \times HW}\)。
  - **融合操作**：使用可学习的记忆权重对\(D\)和\(S\)进行加权求和，得到几何先验矩阵\(G \in \mathbb{R}^{HW \times HW}\)。

### 几何自注意力 (Geometry Self-Attention, GSA)
- 将几何先验\(G\)通过衰减方式引入自注意力：
  \[
  \text{GeoAttn}(Q, K, V, G) = \left( \text{Softmax}(QK^T) \odot \beta^G \right) V
  \]
  其中\(\beta \in (0,1)\)为衰减率，\(\beta^G\)使远距离token的注意力权重被抑制，近距离token被增强。
- **轴分解**：为降低计算复杂度，将注意力沿水平和垂直方向分解（先沿行计算，再沿列计算），并对几何先验也做对应分解。
- **多尺度几何先验**：在不同阶段对深度图使用不同尺度的平均池化，生成对应特征分辨率的几何先验。

### 网络架构 (DFormerv2)
- 金字塔编码器，四个阶段分别输出原图1/4、1/8、1/16、1/32分辨率特征。
- 每个阶段由多个GSA块组成：前三阶段使用分解的GSA，最后阶段使用完整GSA。
- 解码器采用轻量级头部，接收最后三个阶段特征进行预测。

## 3. 实验设计

### 数据集与Benchmark
- **NYU DepthV2** (官方40类，480×640输入)
- **SUN-RGBD** (37类，530×730输入)
- **Deliver** (多模态语义分割数据集，1024×1024输入)
- 评价指标：mIoU（多尺度翻转推理：0.5, 0.75, 1.0, 1.25, 1.5倍）

### 对比方法
涵盖17种近期SOTA方法，包括：ESANet、TokenFusion、Omnivore、DFormer、AsymFormer、SGNet、ShapeConv、FRNet、EMSANet、CMX、CMNext、GeminiFusion、MultiMAE等。所有方法按参数量分为小、中、大三个规模组对比。

### 消融实验
- **从Vanilla Attention到完整GSA的四步渐进**（表3）：仅加深度先验、仅加空间先验、两者融合、再分解，验证各组件贡献。
- **融合操作对比**（表4）：加法、Hadamard积、卷积、记忆权重，记忆权重最佳。
- **衰减率策略**（表5）：固定值与线性采样，发现[0.75,1.0]线性采样表现最好。
- **RGB vs 深度贡献分析**（表7）：在LUSS数据集上做分类和前背景分割实验，发现深度主要提升分割质量，对分类提升有限。

### 推理延迟对比（表6）
在单张3090 RTX GPU上统一测试，DFormerv2在速度和精度间取得最佳平衡。

## 4. 资源与算力

- **预训练**：在ImageNet-1K（通过深度估计方法生成深度图）上训练300 epoch，batch size 1024，使用AdamW优化器，学习率1e-3，权重衰减5e-2。
- **微调**：使用AdamW，初始学习率6e-5，poly衰减策略。
- **未明确说明预训练和微调使用的GPU型号与数量**。仅在推理延迟测试中提及使用单张NVIDIA 3090 RTX GPU。推测大规模预训练使用多卡（如8卡或更多），但论文未具体说明。

## 5. 实验数量与充分性

- **三大多数据集**（NYU DepthV2、SUN-RGBD、Deliver）均进行对比，覆盖室内和复杂场景。
- **完整消融实验**：逐步验证先验重要性、融合方式、衰减策略、分解影响、模态贡献。
- **多规模模型**（Small/Base/Large），对比不同量级的SOTA方法，公平性较好。
- **推理延迟**：与主流方法在同一硬件下对比。
- **定性可视化**：包括几何先验图、注意力聚焦图、特征图对比、分割结果图。
- **综合评估**：实验数量充分，覆盖性能、计算量、速度、消融、可视化多个维度，结果客观公平。

## 6. 主要结论与发现

- DFormerv2在三个数据集上均取得**新的SOTA**，且计算成本显著低于之前最优方法。例如：
  - DFormerv2-L在NYU DepthV2上以95.5M参数、124.1G FLOPs达到58.4% mIoU，优于GeminiFusion-B5（137.2M参数、256.1G FLOPs、57.7% mIoU）。
  - 在Deliver数据集上，DFormerv2-L以114.5G FLOPs达到67.1% mIoU，同样超过GeminiFusion-B5（218.4G FLOPs、66.9% mIoU）。
- **几何先验是提升RGBD分割的关键**：深度信息通过几何关系帮助模型更好地分割物体形状，而语义分类主要依赖RGB。
- **无需显式编码深度**的设计不仅减少了参数量和计算量，还避免了预训练不一致问题。

## 7. 优点

- **创新性强**：首次将深度图作为几何先验而非可学习特征，颠覆了传统的RGBD双编码范式。
- **高效性**：在更少计算量下达到更高精度，向实际部署迈进一步。
- **设计简洁**：几何先验生成仅依赖深度与空间距离的融合，无需额外网络层。
- **消融完整**：从基线逐步构建，清晰展示每个组件的贡献。
- **多任务友好**：深度主要增强分割能力，不会干扰分类，思路清晰。

## 8. 不足与局限

- **未讨论深度噪声的影响**：几何先验高度依赖深度图的准确性，而实际场景中的深度传感器可能产生噪声和空洞，可能影响性能。论文未对此进行鲁棒性评估。
- **仅针对室内/可控场景**：数据集（NYU, SUN-RGBD, Deliver）多为室内环境，室外复杂场景（如自动驾驶）未验证。室外深度可能更稀疏，几何先验效果有待考证。
- **缺少与其他更近期的RGBD方法的比较**：论文发布于2025年，但对比方法截至2024年，可能遗漏了更近期的先进方法（如大模型蒸馏类）。
- **分解操作可能牺牲全局建模**：虽然实验显示基本无损，但在某些需要强全局上下文的场景（如大物体）可能受限。
- **预训练依赖生成的深度图**：ImageNet-1K无真值深度，使用估计方法生成，存在误差，可能影响下游效果（但对SOTA依然有效）。
- **未提供多模态融合的理论分析**：为何几何先验能提升性能，缺乏更直观的理论解释（如信息论角度）。

（完）
