---
title: "CO²IF: Language-Bridging Hyperspectral-Multispectral Image Fusion with Coordinated and Cross-modal Optimal Transport"
title_zh: 语言桥接的高光谱与多光谱图像融合：协调跨模态最优传输
authors: "Mingjin Zhang, Zhongkai Yang, Fei Gao"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38261/42223"
tags: ["query:image-fusion"]
score: 9.0
evidence: 语言桥接的高光谱多光谱图像融合
tldr: 针对高光谱与多光谱图像融合中缺乏语义指导的问题，本文首次引入语言桥接框架CO²IF，利用跨模态最优传输将文本场景描述作为先验知识融入融合过程。该方法通过协调的跨模态最优传输实现语义对齐，显著提升了细节重建的精度。实验表明，语言先验有效地引导了融合网络生成更清晰、更真实的高分辨率高光谱图像。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法缺乏显式语义引导，细节重建精度不足。
method: 引入语言语义作为先验，通过跨模态最优传输指导融合过程。
result: 语言桥接方法显著提升了融合图像的细节质量。
conclusion: 利用文本先验增强图像融合，为多模态融合开辟新思路。
---

## Abstract
Due to the difficulties of directly obtaining high-resolution hyperspectral images (HR-HSI), the fusion of low-resolution hyperspectral images (LR-HSI) and high-resolution multispectral images (HR-MSI) has emerged as an effective approach. While existing methods leverage image-level priors from HR-MSI, they often lack explicit semantic guidance for precise detail reconstruction. Recognizing that textual scene descriptions encapsulate valuable object attributes and contextual information, we introduce the first Language-Bridging framework for Hyperspectral and Multispectral image fusion (CO²IF). CO²IF leverages language semantics as prior knowledge to explicitly guide the reconstruction process. To bridge the modality gap between textual descriptions and high-dimensional hyperspectral data, we design a Cross-modal Optimal Transport (COT) module. COT establishes precise semantic correspondences between language features and the visual cues of individual spectral bands. Building upon this semantic alignment, we develop a Multimodal Coordinated State Space Model (CoMamba). CoMamba effectively integrates the language-derived priors with spatial information from HR-MSI and spectral information from LR-HSI. This language-guided reconstruction significantly enhances the extraction of crucial spatial-spectral details, leading to superior fidelity in the generated HR-HSI. In addition, this paper adds text descriptions for three widely used datasets. Both qualitative and quantitative experimental results on the public datasets confirm the superiority of the proposed method compared to the SOTA methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

高光谱图像（HSI）包含丰富的光谱信息，广泛应用于分类、人脸识别、医疗元宇宙等领域。然而，受硬件限制，高光谱成像存在空间分辨率与光谱分辨率之间的折衷问题。现有高光谱-多光谱图像融合（HS/MS fusion）方法主要依赖图像模态的先验信息（如HR-MSI的空间信息），但缺乏明确的语义指导，导致细节重建精度不足。本文首次提出语言桥接框架CO²IF，将文本场景描述（包含物体属性和上下文信息）作为显式语义先验，引导LR-HSI和HR-MSI的融合过程，从而生成高保真的高分辨率高光谱图像（HR-HSI）。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
利用语言模态的语义知识弥补图像模态的局限性，通过跨模态对齐和融合，提升重建的空间-光谱细节质量。

### 关键技术细节
- **整体架构**：由CLIP编码器（含Adapter微调）、Cross-modal Optimal Transport（COT）模块、Mutlimodal Coordinated State Space Model（CoMamba）构成。输入为LR-HSI、HR-MSI和文本描述。文本和HR-MSI先通过CLIP进行对比学习对齐；然后文本特征经COT与LR-HSI光谱特征进行最优传输匹配；最后，对齐后的文本token和自适应语义权重矩阵W被输入到解码器中的CoMamba，指导HR-HSI生成。

- **Cross-modal Optimal Transport (COT)**
  - 将文本与HSI光谱特征的映射建模为带正则化的最优传输问题：$\min_{P_{lv}} \sum_{i=1}^{N_v}\sum_{j=1}^{N_l} C_{lv}(i,j) P_{lv}(i,j) + \beta(\nabla P_{lv}(i,j))^2$。
  - 代价矩阵$C_{lv}$结合余弦相似度和互信息$I(X,T)$：$C_{lv}(i,j) = (1 - \frac{X_i\cdot T_j}{\|X_i\|\|T_j\|}) \cdot G(X,T)$，其中$G(X,T)=1-I(X,T)$。互信息约束保证了只有满足物体类别判据的配对才被传输。
  - 由最优传输矩阵$P^*_{lv}$计算自适应语义权重矩阵$W = P^*_{lv} \cdot T + \sigma(\phi_{\text{conv}}(X)) \cdot X + X$，用于后续融合。

- **Multimodal Coordinated State Space Model (CoMamba)**
  - 在Mamba的SS2D基础上，引入HR-MSI（编码器）或文本（解码器）的附加参数来增强状态空间方程。
  - 编码器：$h_k = \bar{A}h_{k-1} + (\bar{B} + \lambda \bar{B}_y)x_k$，$y_k = (C + \gamma C_y)h_k + x_k$。
  - 解码器：$h_k = \bar{A}h_{k-1} + (\bar{B} + \lambda \bar{B}_t)x_k$，$y_k = (C + \gamma C_t)h_k \odot W + x_k$（$\odot$为逐元素乘，W过滤冗余光谱特征）。
  - 参数$\lambda, \gamma$为权重影响因子，通过实验设为0.1。

- **训练目标**：总损失$L_{\text{total}} = L_1 + L_{\text{sim}}$，包括像素级L1损失和文本-MSI对比损失（最大化相似性）。

## 3. 实验设计

### 数据集与场景
- **CAVE数据集**：31波段，512×512，22张训练，其余测试。使用Nikon D700生成HR-MSI。
- **Chikusei数据集**：128波段，512×512（从原图裁剪），12张训练，其余测试。使用Landsat-8光谱响应函数生成HR-MSI。
- **Harvard数据集**：31波段，960×960（从1392×1040裁剪），30张训练，20张测试。同样使用Nikon D700生成HR-MSI。

每个数据集均补充了由GPT-4V生成并人工审核的文本描述。

### Benchmark与对比方法
- **传统方法**：GSA、CNMF、ICCV15
- **深度学习方法**：BDT、SSTF-Unet、FusionMamba、S2CycleDiff、SRLF

评价指标：PSNR、SSIM、SAM、ERGAS。

## 4. 资源与算力

文中明确提到：模型在NVIDIA GeForce A100 GPU上使用PyTorch框架训练。未提及具体GPU数量、训练时长或参数量（除表3中Flops为106.67G外）。算力信息不够详细。

## 5. 实验数量与充分性

- **定量对比**：在三个数据集（CAVE、Chikusei、Harvard）上对比8种方法，报告了PSNR、SSIM、SAM、ERGAS平均值。
- **定性对比**：展示了各方法的重建结果及误差图。
- **消融实验**：
  - COT模块消融（移除、替换为交叉熵、移除G矩阵约束）
  - CoMamba模块消融（替换为卷积、交叉注意力、Cobra式拼接、移除权重矩阵W）
  - 模态消融（移除文本或HR-MSI）
  - 自适应语义权重矩阵可视化及光谱响应曲线分析
  - 超参数λ和γ的敏感性分析
- 实验总计约10+组，覆盖了方法核心组件和超参数，较为充分。对比方法包括多种主流方法，消融对照组设计合理，公平性较好。

## 6. 论文的主要结论与发现

- CO²IF在所有数据集上均超越SOTA方法，PSNR提升约1-2 dB，SAM和ERGAS显著降低。
- 语言模态提供了显式语义先验，有效提升了空间-光谱细节重建质量，减少了伪影和色彩偏差。
- COT模块通过互信息约束的最优传输实现了文本与光谱特征精确对齐；CoMamba模块通过结合文本和自适应权重，高效融合多模态特征。
- 自适应语义权重矩阵能够突出关键光谱波段，提升光谱保真度。

## 7. 优点

- **创新性**：首次将语言先验引入HS/MS融合任务，开辟了新方向。
- **方法设计**：COT利用了最优传输的数学理论，并创新地结合互信息，避免模糊匹配；CoMamba在Mamba基础上扩展为多模态协调状态空间模型，计算复杂度低（线性）且能建模长程依赖。
- **数据集贡献**：为三个公共数据集补充了文本描述，促进多模态研究。
- **实验结果全面**：定量、定性、消融、可视化分析充分，验证了有效性。

## 8. 不足与局限

- **计算资源信息不完整**：未提供具体GPU数量、训练时间、模型参数量等，难以复现和对比效率。
- **文本依赖**：文本描述由GPT-4V生成并人工审核，在真实应用中可能难以自动获取高质量文本，限制了部署便利性。
- **泛化性验证有限**：仅在三个特定数据集（CAVE、Chikusei、Harvard）上实验，且场景多为室内或小区域，未在更大规模或更多样化的遥感数据集（如Houston、Botswana等）上验证。
- **超参数敏感性分析范围较小**：λ和γ仅测试了0.01/0.1/0.2三个值，未探索更宽范围或联合调参。
- **潜在偏差风险**：文本描述可能引入主观语义偏差（如物体属性描述不完全准确），影响重建结果；此外，CLIP模型本身对RGB图像预训练，对高光谱数据的泛化存在局限（已通过Adapter微调缓解，但未评估跨域迁移能力）。
- **缺少对失败案例的分析**：未讨论哪些场景或光谱带下性能提升有限或出现失效，缺乏对方法局限性的诚实讨论。

（完）
