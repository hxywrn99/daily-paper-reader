---
title: "CO²IF: Language-Bridging Hyperspectral-Multispectral Image Fusion with Coordinated and Cross-modal Optimal Transport"
title_zh: CO2IF：基于语言桥接与协调跨模态最优传输的高光谱-多光谱图像融合
authors: "Mingjin Zhang, Zhongkai Yang, Fei Gao"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38261/42223"
tags: ["query:image-fusion"]
score: 9.0
evidence: 语言桥接的高光谱-多光谱图像融合
tldr: 高光谱-多光谱融合缺乏明确语义指导。本文首次引入语言桥接框架CO2IF，利用文本场景描述作为先验知识，结合协调跨模态最优传输，引导细节重建。实验表明语言语义显著提升了融合图像的光谱保真度和空间分辨率。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38261/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 831, \"height\": 684, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38261/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1811, \"height\": 887, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38261/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 855, \"height\": 792, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38261/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1744, \"height\": 1179, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38261/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 859, \"height\": 384, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38261/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 878, \"height\": 511, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38261/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1793, \"height\": 478, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38261/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 877, \"height\": 244, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38261/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 858, \"height\": 280, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38261/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 876, \"height\": 169, \"label\": \"Table\"}]"
motivation: 现有方法缺乏语义指导，细节重建不精确。
method: 利用语言语义作为先验，结合跨模态最优传输实现语义引导融合。
result: 在多个数据集上提升了光谱保真度和空间细节。
conclusion: 语言桥接为多光谱融合提供了新的语义增强途径。
---

## Abstract
Due to the difficulties of directly obtaining high-resolution hyperspectral images (HR-HSI), the fusion of low-resolution hyperspectral images (LR-HSI) and high-resolution multispectral images (HR-MSI) has emerged as an effective approach. While existing methods leverage image-level priors from HR-MSI, they often lack explicit semantic guidance for precise detail reconstruction. Recognizing that textual scene descriptions encapsulate valuable object attributes and contextual information, we introduce the first Language-Bridging framework for Hyperspectral and Multispectral image fusion (CO²IF). CO²IF leverages language semantics as prior knowledge to explicitly guide the reconstruction process. To bridge the modality gap between textual descriptions and high-dimensional hyperspectral data, we design a Cross-modal Optimal Transport (COT) module. COT establishes precise semantic correspondences between language features and the visual cues of individual spectral bands. Building upon this semantic alignment, we develop a Multimodal Coordinated State Space Model (CoMamba). CoMamba effectively integrates the language-derived priors with spatial information from HR-MSI and spectral information from LR-HSI. This language-guided reconstruction significantly enhances the extraction of crucial spatial-spectral details, leading to superior fidelity in the generated HR-HSI. In addition, this paper adds text descriptions for three widely used datasets. Both qualitative and quantitative experimental results on the public datasets confirm the superiority of the proposed method compared to the SOTA methods.

---

## 论文详细总结（自动生成）

## 详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

高光谱-多光谱图像融合（HS/MS fusion）旨在通过融合低分辨率高光谱图像（LR-HSI）的高光谱信息与高分辨率多光谱图像（HR-MSI）的高空间分辨率信息，生成高分辨率高光谱图像（HR-HSI）。现有方法主要依赖图像层面的先验（如HR-MSI的空间结构），但缺乏**明确的语义指导**，导致细节重建不精确（例如物体边界模糊、光谱失真）。作者认识到，文本场景描述蕴含丰富的物体属性和上下文信息，因此首次提出**语言桥接框架CO²IF**，利用自然语言语义作为先验知识，显式引导重建过程，从而提升融合图像的光谱保真度和空间细节。

### 2. 方法论

- **核心思想**：通过跨模态对齐将文本语义与高光谱数据中的视觉线索关联，并利用对齐后的语义信息指导空间-光谱融合。
- **关键技术组成**：
  - **CLIP编码器+适配器（Adapter）**：对文本-图像对进行微调，使文本特征与HR-MSI特征在共享空间中获得最大相似度（通过对比学习）。
  - **跨模态最优传输（COT）**：将文本特征与HSI特征的对齐建模为**带正则项的最优传输问题**。构建包含余弦相似度和互信息（Mutual Information）的成本矩阵，学习最优传输矩阵 \(P_{lv}\)，实现文本与各光谱波段视觉线索的精确对应。进一步利用该矩阵生成**自适应语义权重矩阵 \(W\)**，用于后续融合。
  - **多模态协调状态空间模型（CoMamba）**：改进Mamba的2D选择性扫描（SS2D），在编码器侧利用HR-MSI图像导出额外参数（\(\bar{B}_y, \Delta_y, C_y\)），在解码器侧利用文本特征导出参数（\(\bar{B}_t, \Delta_t, C_t\)）以及自适应权重\(W\)更新状态方程和输出方程，从而实现文本先验、HR-MSI空间信息与LR-HSI光谱信息的**多模态协调融合**。
- **损失函数**：像素级L1重构损失 + 文本-MSI对比损失（CLIP风格）。

### 3. 实验设计

- **数据集**：
  - **CAVE**：31波段，512×512，32张日常物体图像（22训练/10测试）。
  - **Chikusei**：128波段，源自航空遥感，裁剪为16个512×512块（12训练/4测试）。
  - **Harvard**：31波段，960×960，50张室内外图像（30训练/20测试）。
  - 以上数据集均扩展了对应文本描述（通过GPT-4V生成并人工审核）。
- **评价指标**：PSNR、SSIM、SAM（光谱角映射器）、ERGAS（相对全局综合误差）。
- **对比方法**：
  - 传统方法：GSA、CNMF、ICCV15。
  - 深度学习方法：BDT、SSTF-Unet、FusionMamba、S₂CycleDiff、SRLF。

### 4. 资源与算力

论文明确提到：**训练在NVIDIA GeForce A100 GPU上**，使用PyTorch框架。但**未说明GPU数量、训练时长等具体信息**。

### 5. 实验数量与充分性

- **数量**：共进行了以下几组实验：
  - 在三个数据集上的定量和定性对比（表1、图4）。
  - COT模块消融：去除COT、替换为交叉熵损失、去除互信息约束（表2）。
  - CoMamba模块消融：替换为卷积、交叉注意力、Cobra结构、去除自适应权重\(W\)（表3）。
  - 模态消融：移除文本模态、移除HR-MSI模态（图5）。
  - 自适应权重矩阵可视化与光谱曲线分析（图6）。
  - 超参数λ和γ的影响（表4）。
- **充分性评估**：实验设计**较为充分**，涵盖了主要组件验证、模态贡献分析、参数敏感性以及多种SOTA对比。方法间的比较基于公开数据集和公认指标，结果客观。但未在更多遥感或医学数据集上验证，泛化性有待进一步检验。

### 6. 论文的主要结论与发现

- CO²IF在所有三个数据集上均显著优于所有对比方法，**PSNR提升可达1-2dB**，SAM和ERGAS更低，说明语言先验能有效提升光谱和空间重建质量。
- COT模块通过最优传输和互信息实现了文本与高光谱特征间的**精确语义对齐**，优于交叉熵和普通余弦相似度。
- CoMamba通过多模态协调参数融合及自适应权重，**显著减少伪影和光谱失真**。
- 文本和HR-MSI两个模态信息**互补**：缺少任一模态均导致性能下降（图5）。
- 自适应权重矩阵能够根据光谱特征**动态增强关键波段**，使光谱响应曲线更接近真值（图6）。

### 7. 优点

- **创新性**：首次将语言语义先验引入HS/MS融合任务，填补了该方向空白。
- **方法精巧**：COT模块将对齐问题转化为带互信息约束的最优传输，理论清晰且与任务高度贴合；CoMamba巧妙地将文本和图像信息融入状态空间模型，实现线性复杂度下的高效融合。
- **数据贡献**：为三个常用数据集补充了经过人工审核的文本描述，便于后续研究。
- **实验充分**：消融实验覆盖所有核心组件，定性定量结合，结果可信度高。
- **性能领先**：在多个指标上达到SOTA，视觉质量显著提升。

### 8. 不足与局限

- **依赖高质量文本描述**：文本生成依赖GPT-4V并需人工审核，成本较高，且文本质量直接影响对齐效果。
- **CLIP域偏移**：CLIP基于RGB训练，对高光谱数据的丰富波段存在域差异，可能限制对齐精度。
- **数据集规模有限**：仅验证了三个较小规模的数据集（CAVE、Chikusei、Harvard），未在更大尺度（如遥感宽幅图像）或更复杂场景（如动态视频）中测试。
- **算力成本未明确**：未报告训练耗时或推理速度，实际应用中的效率评估不足。
- **未来方向**：论文自身指出可探索半监督/无监督方法以降低对文本标注的依赖，以及将语言引导拓展到更广泛的数据集。

（完）
