---
title: "SpaCRD: Multimodal Deep Fusion of Histology and Spatial Transcriptomics for Cancer Region Detection"
title_zh: SpaCRD：用于癌症区域检测的组织学与空间转录组学多模态深度融合
authors: "Shuailin Xue, Jun Wan, Lihua Zhang, Wenwen Min"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38135/42097"
tags: ["query:medical-seg"]
score: 9.0
evidence: 通过多模态深度融合进行癌症区域检测
tldr: 该论文针对传统组织学假阳性率高的问题，提出SpaCRD，通过深度融合组织学图像和空间转录组学数据，实现准确的癌症组织区域检测。该方法有效整合多模态信息，在跨样本和跨平台场景中表现出高鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 868, \"height\": 478, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1834, \"height\": 959, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 829, \"height\": 393, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 662, \"height\": 429, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 875, \"height\": 412, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 794, \"height\": 419, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38135/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 787, \"height\": 430, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38135/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1829, \"height\": 763, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38135/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1832, \"height\": 820, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38135/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 706, \"height\": 284, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38135/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 669, \"height\": 327, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38135/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1476, \"height\": 228, \"label\": \"Table\"}]"
motivation: 传统组织学方法假阳性率高，现有方法无法有效整合空间转录组数据。
method: 设计多模态深度融合网络，联合组织学和ST数据进行癌症检测。
result: 在多个数据集上检测精度显著优于现有方法。
conclusion: 多模态融合为癌症区域检测提供了更可靠的解决方案。
---

## Abstract
Accurate detection of cancer tissue regions (CTR) enables deeper analysis of the tumor microenvironment and offers crucial insights into treatment response. Traditional CTR detection methods, which typically rely on the rich cellular morphology in histology images, are susceptible to a high rate of false positives due to morphological similarities across different tissue regions. The groundbreaking advances in spatial transcriptomics (ST) provide detailed cellular phenotypes and spatial localization information, offering new opportunities for more accurate cancer region detection. However, current methods are unable to effectively integrate histology images with ST data, especially in the context of cross-sample and cross-platform/batch settings for accomplishing the CTR detection. To address this challenge, we propose SpaCRD, a transfer learning-based method that deeply integrates histology images and ST data to enable reliable CTR detection across diverse samples, platforms, and batches. Once trained on source data, SpaCRD can be readily generalized to accurately detect cancerous regions across samples from different platforms and batches. The core of SpaCRD is a category-regularized variational reconstruction-guided bidirectional cross-attention fusion network, which enables the model to adaptively capture latent co-expression patterns between histological features and gene expression from multiple perspectives. Extensive benchmark analysis on 23 matched histology-ST datasets spanning various disease types, platforms, and batches demonstrates that SpaCRD consistently outperforms existing eight state-of-the-art methods in CTR detection.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）

癌症组织区域（CTR）检测对于肿瘤微环境分析、治疗策略制定至关重要。传统方法主要依赖于组织学图像的细胞形态学特征，但由于不同组织区域间的形态相似性以及染色质量不一致，导致高假阳性率。近年来，空间转录组学（ST）能够提供详细的细胞表型和空间定位信息，为更准确的癌症检测带来新机遇。然而，现有方法无法有效整合组织学图像与ST数据，尤其在跨样本、跨平台/批次的场景下。因此，本文提出**SpaCRD**，一种基于迁移学习、深度融合组织学图像与ST数据的框架，以实现鲁棒且通用的CTR检测。

### 2. 方法论：核心思想、关键技术细节

**核心思想**：通过多模态深度融合与迁移学习，将组织学图像的形态信息与ST的基因表达信息在共享嵌入空间中对齐，并利用变分重建和双向交叉注意力机制过滤噪声、生成紧凑且类别一致的嵌入，最终实现准确预测。

**关键技术细节**（三个阶段）：

- **阶段一：模态对齐表示学习**  
  使用病理基础模型**UNI**提取组织学图像特征（无需微调）。设计两个轻量MLP编码器（图像编码器`fc1`和基因编码器`fc2`），通过**CLIP对比学习**（InfoNCE损失）将配对图像与基因表达在潜在空间中拉近，非配对推远，减小模态差异。

- **阶段二：VRBCA融合网络**  
  包括双向交叉注意力（BCA）和类别正则化变分自编码器（RVAE）。  
  - **BCA**：包含基因引导和H&E引导两个交叉注意力模块，每个模块使用m个并行注意力头，分别以图像/基因嵌入作为查询，对方模态作为键和值，从而从多视角捕获特征交互；同时引入相邻点信息，增强空间上下文建模。  
  - **RVAE**：对融合后的多模态表示进行编码，得到均值μ和方差σ²，通过重参数化采样得到潜在变量z，再解码重构。训练目标包括重构损失和**类别正则化KL散度**——引入可学习的类别特定先验（均值μ_y），迫使潜在空间按类别（肿瘤/健康）分离，过滤噪声并生成紧凑嵌入。

- **阶段三：癌症似然估计**  
  将RVAE编码器输出的μ和logσ²拼接后送入两层MLP分类器，预测每个点是否为癌症。损失函数为BCE + γ·L_fused。推理时使用两成分高斯混合模型（GMM）自动确定分类阈值。

### 3. 实验设计：数据集、Benchmark、对比方法

- **数据集**：包含23个匹配的组织学-ST数据集，覆盖乳腺癌（11个）和结直肠癌（12个），来自多个平台和批次。  
  - 跨样本评估：STHBC（8个乳腺癌组织切片）和CRC（12个结直肠癌切片），采用留一法交叉验证。  
  - 跨平台/批次评估：训练在ST平台（STHBC）上，测试在Visium（ViHBC、IDC）和Xenium（XeHBC）平台。

- **对比方法**：8种SOTA方法，分为四类：  
  - 多模态：MEATRD、STANDS、SpaCell-Plus、iStar、TESLA  
  - ST-based：STAGE、Spatial-ID  
  - 图像-based：SimpleNet

- **评估指标**：AUC、AP（平均精度）、F1-score、KS距离。

### 4. 资源与算力

文中“Implementation Details”明确说明：  
- 单块NVIDIA RTX 3090 GPU（24GB）  
- 开发环境：PyTorch 2.1.1、Python 3.11.5  
- **未明确**训练时长或总计算量。

### 5. 实验数量与充分性

- **跨样本实验**：20个数据集（12 CRC + 8 STHBC），每组运行5次取均值标准差，统计充分。  
- **跨平台/批次实验**：3个测试集（ViHBC、XeHBC、IDC），同样5次重复。  
- **消融实验**：  
  - 特征提取器：UNI vs ResNet50、Swin-Tiny、HIPT  
  - 模态与模块：仅图像、仅基因、移除BCA、移除RVAE、移除VRBCA、移除对比学习  
  - 超参数敏感性：α、β、γ、邻居点数量  
  - 小样本训练分析（<10%训练点）  
- **下游分析**：检测潜在病变区域、基因表达验证。  

实验设计全面，覆盖多种场景和消融，与多种SOTA公平比较，结论可靠。

### 6. 主要结论与发现

- **性能优越**：SpaCRD在所有23个数据集上均取得最佳AUC、AP和F1，平均提升分别为13.5%、14.1%、14.0%（vs 第二优方法）。  
- **跨平台泛化**：在ViHBC、XeHBC、IDC上依然显著领先，证明能缓解技术批次效应。  
- **区分癌症严重程度**：在XeHBC上，SpaCRD预测分数可区分浸润癌（0.91）、原位癌（0.64）和正常组织（0.17），而基线方法区分度弱。  
- **发现潜在病变**：在IDC中，高分但非标注癌症的点表现出乳腺癌标志基因（ERBB2、CCND1、ACTB）高表达，提示可能为早期病变。

### 7. 优点

- **创新性**：首次将多模态深度融合与迁移学习结合用于CTR检测，提出VRBCA模块，兼顾跨模态交互、噪声过滤和类别一致性。  
- **鲁棒性**：在跨样本、跨平台/批次场景下保持高性能，泛化能力强。  
- **实验严谨**：在23个数据集、8个基线、多角度消融下验证，结果统计稳定（5次重复）。  
- **实用价值**：不仅检测癌症区域，还能分层疾病严重性、发现潜在病变，辅助临床研究。  
- **开源代码**：提供完整代码和补充材料，可复现。

### 8. 不足与局限

- **计算资源**：虽仅用单张3090，但依赖UNI大模型（预训练参数大），对资源有限团队可能成本高。  
- **训练时长未报告**：文中未给出具体训练时间，难以评估实际效率。  
- **泛化范围有限**：仅验证乳腺癌和结直肠癌，对其他癌症类型（如肺癌、前列腺癌）的效果未知。  
- **小样本分析不够深入**：仅提及<10%训练点仍稳定，但未系统分析不同比例下的性能曲线。  
- **可解释性不足**：多模态融合和潜在空间建模的黑箱特性，难以直观解释预测依据。  
- **阈值设定依赖GMM**：若两类概率分布重叠严重，GMM交点可能不准，需进一步验证。

（完）
