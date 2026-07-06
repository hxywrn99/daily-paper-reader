---
title: Multi-Focus Image Fusion via Explicit Defocus Blur Modelling
title_zh: 基于显式离焦模糊建模的多焦点图像融合
authors: "Yuhui Quan, Xi Wan, Zitao Tang, Jinxiu Liang, Hui Ji"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32714/34869"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于显式离焦建模的多焦图像融合
tldr: 该文针对现有深度多焦融合方法忽视离焦模糊物理特性的问题，提出显式离焦模糊建模框架。利用原子化空间变化参数化模型计算逐像素离焦描述符，并通过尺度循环方式估计决策图。实验表明该方法在融合质量和可解释性上优于现有方法。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32714/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 874, \"height\": 669, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32714/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1807, \"height\": 905, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32714/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1833, \"height\": 824, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32714/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1825, \"height\": 1124, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1833, \"height\": 455, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 874, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 347, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 873, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 873, \"height\": 351, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 862, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32714/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 871, \"height\": 140, \"label\": \"Table\"}]"
motivation: 深度学习方法忽视离焦模糊物理特性，泛化性受限。
method: 结合参数化离焦模型和尺度循环估计，显式建模模糊过程。
result: 在多个多焦数据集上融合结果更清晰、可解释性更强。
conclusion: 显式物理建模提升了多焦融合的性能和可解释性。
---

## Abstract
Multi-focus image fusion (MFIF) enhances depth of field in photography by generating an all-in-focus image from multiple images captured at different focal lengths. While deep learning has shown promise in MFIF, most existing methods overlooked the physical properties of defocus blurring in their network design, limiting their interoperability and generalization. This paper introduces a novel framework that integrates explicit defocus blur modelling into the MFIF process, improving both interpretability and performance. Using an atom-based spatially-varying parameterized defocus blurring model, our approach calculates pixel-wise defocus descriptors and initial focused images from multi-focus source images in a scale-recurrent manner to estimate soft decision maps. Fusion is then performed using masks derived from these decision maps, with special treatment for pixels likely defocused in all source images or near boundaries of defocused/focused regions. The model is trained with a fusion loss and a cross-scale defocus estimation loss. Extensive experiments on benchmark datasets demonstrated the effectiveness of our approach.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：多焦点图像融合（MFIF）旨在将不同焦距下拍摄的多张图像融合成一张全清晰图像。现有深度学习方法虽然在特征提取和融合上取得了进展，但几乎都忽略了离焦模糊（defocus blur）背后的物理成像模型，导致网络可解释性差、泛化能力受限。
- **背景**：离焦模糊由有限景深造成，可通过空间变化的点扩散函数（PSF）建模。传统方法或基于决策（分类）或基于重建，但未显式利用该物理模型。本文提出将显式离焦模糊建模引入网络设计，以提升融合质量与可解释性。

## 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

- **核心思想**：通过参数化的空间变化离焦模糊模型（将逐像素PSF表示为若干高斯核原子的线性组合），显式建模离焦过程，并以此指导特征提取与融合。
- **关键模块**：
  - **Defocus Blur Estimator (DBE)**：采用粗到细的尺度循环结构（尺度数T=3），在每个尺度上通过ConvLSTM（SRM）估计N个系数矩阵（即离焦描述符）{C<sub>n,s</sub>}，然后利用一阶近似逆算子B<sup>-1†</sup>计算初始聚焦图像Z<sub>s</sub>（公式8/12）。
  - **Decision Map Estimator (DME)**：基于DBE输出的描述符和初始聚焦图像，通过编码器-解码器结构生成软决策图M<sub>s</sub>，表示每个像素的聚焦程度。
  - **Uncertainty-Aware Fuser (UAF)**：根据决策图将像素分为三类：确定聚焦于第一源图像、确定聚焦于第二源图像、不确定区域（边界或全局离焦）。对不确定区域，使用初始聚焦图像替代源图像进行融合，从而减少伪影。
- **损失函数**：融合损失L<sub>fusion</sub>（MSE against GT）+ 跨尺度离焦估计损失L<sub>defocus</sub>（多尺度MSE），权重β平衡。

## 3. 实验设计：数据集、基准与对比方法

- **数据集**：
  - 训练集：10000对合成图像，来自Agustsson & Timofte (2017)和Flickr1024 (Wang et al. 2019)。
  - 测试集：五个基准数据集——MFI-WHU（含GT）、Lytro、SIMIF、MFFW（自然图像）、OR-PAM（生物图像）。
- **对比方法**：包括传统方法（DSIFT、MFF、MWGF、GFF）和深度方法（IFCNN、GACN、EAY-Net、GRFusion、FusionDiff、DB-MFIF）。
- **评价指标**：有GT时用PSNR、SSIM、LPIPS；无GT时用NMI、MI、Q<sub>G</sub>、Q<sub>M</sub>、Q<sub>Y</sub>，并计算平均排名ARank。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长。仅在Table 1中报告了推理时间（512×512图像对耗时0.03秒）和参数量（4.065M），但训练资源的细节缺失。

## 5. 实验数量与充分性

- **实验覆盖**：在5个不同难度和领域的数据集上进行了定量对比，每个数据集均报告了多种指标。此外，还进行了**消融实验**（4个变体：单尺度、去掉DBE、去掉尺度循环、去掉离焦损失）和阈值γ的影响分析（从0.1降至0）。
- **充分性与公平性**：对比了多个最新方法（包括2024年方法），并展示了定性可视化（差异图）。实验设计较为完整，但缺少对超参数（如原子数N、尺度数T）的系统探索，以及训练资源的公开。总体而言，充分且客观。

## 6. 论文的主要结论与发现

- 显式融入离焦物理模型的DMANet在**所有五个数据集**上的平均排名（ARank）均为第一，且在许多单项指标上最优。
- 在MFI-WHU数据集上PSNR高达42.898 dB，远超第二名（DSIFT为39.396 dB），且参数量（4.065M）和推理时间（0.03s）均处于较低水平。
- 消融实验证实DBE、尺度循环结构、离焦损失各组件均贡献显著；UAF的不确定性处理（γ>0）可进一步提升PSNR。
- 模型在生物图像（OR-PAM）上也表现出强泛化能力，说明物理建模的普适性。

## 7. 优点

- **物理驱动**：首次在MFIF网络中显式建模离焦模糊，提升了可解释性与泛化能力。
- **轻量高效**：相比GRFusion（9.857M）、FusionDiff（26.915M），DMANet参数量仅4.065M，推理速度0.03s（GPU），效率高。
- **稳健处理边界与全离焦区域**：UAF模块通过不确定性处理，有效减少了焦点/离焦边界处的伪影。
- **多尺度学习**：利用离焦模糊的跨尺度相似性，ConvLSTM结构增强了特征重用。

## 8. 不足与局限

- **训练资源未披露**：缺少GPU型号、数量、训练时长等信息，影响可复现性。
- **超参数敏感性未充分探究**：原子数N、尺度数T、平衡权重β仅使用默认值（文中未详细讨论），其他可能影响性能的设置（如不同γ值的更详细曲线）可以进一步分析。
- **局限於二输入场景**：论文假设输入图像数量S=2，虽声称可推广，但未实验验证多输入（如≥3张）情况。
- **仅评估合成数据训练**：训练数据全部为合成，未在真实场景（如不同相机、非对齐图像）上验证，真实应用可能存在域偏移。
- **缺乏与其他物理模型方法的对比**：当前基线均为非物理驱动网络，未与同样使用显式模糊建模的方法（如某些去模糊网络）对比。

（完）
