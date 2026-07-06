---
title: "SigFusion: Unified Signal-Level Self-Supervised Learning Paradigm for Image Fusion"
title_zh: SigFusion：面向图像融合的统一信号级自监督学习范式
authors: "Zeyu Wang, Jiawei Feng, Jiayu Wang, Pengjie Wang, Haiyu Song"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38009/41971"
tags: ["query:image-fusion"]
score: 9.0
evidence: 自监督学习用于图像融合
tldr: 该论文针对图像融合缺乏大规模真实训练数据的问题，提出SigFusion，一种统一的信号级自监督学习范式，通过可学习的信号调制器和SigFormer从无标签自然图像自动合成具有真实多源信号特征的训练集和伪标签。该方法在多个融合任务上达到有监督方法的性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38009/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 874, \"height\": 539, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38009/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1836, \"height\": 309, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38009/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1846, \"height\": 554, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38009/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1832, \"height\": 907, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38009/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 869, \"height\": 382, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38009/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 874, \"height\": 314, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38009/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1781, \"height\": 1780, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38009/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1773, \"height\": 325, \"label\": \"Table\"}]"
motivation: 图像融合缺乏大规模真实训练数据集。
method: 提出信号级伪标签生成网络，从无标签图像合成训练数据。
result: 在多种融合任务上达到与有监督方法相当的性能。
conclusion: 自监督信号级合成数据能有效缓解数据稀缺问题。
---

## Abstract
Image Fusion (IF) aims to integrate complementary features from multiple source images into a single image. However, a key challenge in this field is the lack of large-scale real-world training datasets. Existing models typically rely on either small datasets or synthetic, less realistic datasets. To address this, we propose SigFusion, a unified signal-level self-supervised learning paradigm for various IF tasks.The core idea is to use signal-level Pseudo-Label Generation Networks (PLGN) to automatically synthesize training sets and pseudo labels with real multi-source signal characteristics from vast unlabeled natural images.PLGN includes two critical components: learnable 1D Signal Modulators (SM) and SigFormer. SM learns implicit 1D signal patterns across various source images and embeds them into natural images, reducing the domain gap between synthetic and real datasets. SigFormer integrates Transformer with signal processing methods, establishing an appropriate signal representation space for SM. Its cascaded, multi-level design allows hierarchical feature learning from coarse to fine detail. Moreover, SigFormer can serve as a flexible backbone for IF, as its design adheres to the classic decomposition-reconstruction paradigm. Experimental results demonstrate that SigFusion achieves state-of-the-art performance across multiple IF tasks, including medical image fusion, infrared-visible image fusion, multi-focus image fusion, and multi-exposure image fusion.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

图像融合（Image Fusion, IF）旨在将多张源图像的互补信息整合为一张图像，但面临大规模真实训练数据集严重匮乏的瓶颈。现有方法要么依赖小规模真实数据的无监督学习（性能受限于数据量），要么使用针对特定任务合成的数据集（存在显著的域差距）。本文提出 **SigFusion**，一种**统一的信号级自监督学习范式**，通过从海量无标签自然图像自动合成具有真实多源信号特征的训练集和伪标签，有效缓解数据稀缺问题，并在多个融合任务上达到最先进性能。

## 2. 方法论

### 核心思想
- 采用**信号级伪标签生成网络（PLGN）**，将自然图像注入从真实多源图像习得的信号级特征，生成逼真的源图像对及伪标签，用于预训练融合网络。
- 整个框架遵循两阶段 SSL 范式：**预文本任务**（利用 PLGN 合成数据预训练） + **下游任务**（在真实小数据集上微调）。

### 关键技术细节

- **伪标签生成网络（PLGN）**：由两个核心组件构成：
  - **可学习 1D 信号调制器（SM）**：显式学习真实多源图像的隐式信号级模式（如红外、可见光、CT、MRI 等），并将这些模式嵌入自然图像的低频和高频分量中。SM 对低频部分使用 Transformer，对高频部分使用 MLP。
  - **SigFormer**：结合 Transformer 与经验小波变换（EWT）的级联结构，进行粗到细的层次化分解与重建。将图像分解为低频和高频 1D 信号，经 SM 调制后再重建为合成图像。SigFormer 也可直接作为融合网络的骨干（去掉 SM）。

- **训练过程**：
  - PLGN 的训练使用由多个融合模型生成的融合图像作为输入，真实源图像作为 GT，通过信号级和像素级联合损失（MSE）优化。
  - 融合网络使用 PLGN 生成的伪标签预训练，损失函数针对多模融合（MMIF）和数字摄影融合（DPIF）略有不同，均涉及结构相似性、梯度、像素最大值等约束。

- **公式表示**：
  - 预文本目标：`min θ Σ L(FN_θ(G1(X), G2(X)), G1, G2)`
  - 信号级损失：`lsignal(L_GT, δ_s(L_c)) + lsignal(H_GT, δ_s(H_c)) + lpixel(GT, G(F))`

## 3. 实验设计

### 数据集与场景
- **红外-可见光融合（VIF）**：训练集 MSRS，测试集 M³FD 和 TNO。
- **医学图像融合（MIF）**：训练集来自哈佛医疗网站（334 对，其中 300/34 分割），测试集 21 对 MRI-CT 和 42 对 MRI-PET。
- **多聚焦融合（MFIF）**：训练集 RealMFF（710 对），测试集 LYTRO 和 MFFW。
- **多曝光融合（MEF）**：训练集 SICE，测试集 SICE 和 MEFB。
- **预文本任务**：Flickr25k（25,000 张自然图像），每张合成 4 种模态图像，共生成 4×25,000 张。

### 对比方法
涉及 12 种以上方法，包括：U2Fusion, DeFusion, CDDFuse, LRRNet, MURF, EMMA, Text-Difuse, FILM, C2RF, GIFNet, DCEvo, OmniFuse, CU-Net, IFCNN, SDNet, DIFNet, FusionDiff, MGDN, ZMFF, DeepM²CDL, HoLoCo, MEF-GAN 等。

### 评估指标
QG, QM, QP, MI, SD, VIFF，均为常用参考/无参考指标。

## 4. 资源与算力

文中明确提及：
- **硬件**：i9-14900K CPU，单张 RTX 4090 GPU，128 GB RAM。
- **软件**：PyTorch, CUDA 12.1。
- **超参数**：Adam 优化器，学习率 1×10⁻⁴，训练 100 个 epoch，batch size 16。
未提供总训练时长或 GPU 使用数量，推测为单卡训练。

## 5. 实验数量与充分性

- **跨任务对比实验**：在 4 种融合任务、多个测试集上给出了完整定量（表1）和定性（图4）结果，对比方法全面。
- **消融实验**（表2）：考察了 SM（去除全部/高频/低频）、SigFormer（替换 EWT 为非 EWT、替换 Transformer 为 CNN、替换 EWT 为 DWT）、预文本任务（移除/无 PLGN/全）的影响，展示了各组件必要性。
- **合成数据集可视化**（图5、6）：证明了 PLGN 能有效将自然图像信号调制为目标模态信号。
- **PLGN 的泛化性验证**：使用合成数据预训练其他模型，提升其性能（结果在补充材料）。
- 实验设计较为充分、客观，对比方法均来自最新高水平论文，指标覆盖全面。

## 6. 主要结论与发现

- SigFusion 在 VIF、MIF、MFIF、MEF 四项任务上均达到 SOTA 性能，在多数指标上优于现有方法。
- 信号级自监督预训练能有效缩小合成数据与真实数据的域差距。
- SM 和 SigFormer 的联合设计是核心，缺失任一组件均导致性能下降。
- 预文本任务（PLGN 合成数据）是性能提升的关键，无预训练性能显著降低。

## 7. 优点

- **新颖性**：首个面向图像融合的**信号级**自监督学习范式，而非像素或特征级。
- **统一性**：用一个框架同时解决数据集合成和融合模型训练，适用于多种 IF 任务。
- **实用性强**：PLGN 可生成大规模逼真训练集，并能直接提升其他现有融合模型的性能。
- **设计精细**：SM 针对不同频率采用不同网络结构（Transformer 处理低频、MLP 处理高频）；SigFormer 结合 Transformer 与 EWT，可适配经典分解-重建范式。
- **实验全面**：涵盖主流任务、大量对比方法、多种消融验证，结论可靠。

## 8. 不足与局限

- **对预训练模型的依赖**：PLGN 的训练需要多个已训练好的融合模型生成输入图像，增加了前期计算和准备成本，且模型质量可能影响合成数据分布。
- **域差距未完全消除**：在 Stage II 仍需微调，提示合成数据仍存在残余域差。
- **计算开销未充分讨论**：未报告 PLGN 训练和推理的具体时间/显存开销，难以评估实际部署效率。
- **任务覆盖有限**：仅验证了四种常见 IF 任务，对遥感、多光谱、高动态范围等其他融合场景未测试。
- **缺少下游任务评估**：未在目标检测、医学分割等下游任务上验证融合图像的实际效用。
- **存在潜在偏差风险**：合成数据基于自然图像库（Flickr25k），可能与真实医学/红外场景存在风格偏差。

（完）
