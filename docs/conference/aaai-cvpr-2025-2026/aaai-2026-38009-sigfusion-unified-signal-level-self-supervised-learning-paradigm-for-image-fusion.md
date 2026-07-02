---
title: "SigFusion: Unified Signal-Level Self-Supervised Learning Paradigm for Image Fusion"
title_zh: SigFusion：统一的信号级自监督学习范式用于图像融合
authors: "Zeyu Wang, Jiawei Feng, Jiayu Wang, Pengjie Wang, Haiyu Song"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38009/41971"
tags: ["query:image-fusion"]
score: 9.0
evidence: 自监督深度学习图像融合方法
tldr: 针对图像融合缺乏大规模真实训练数据的问题，提出SigFusion统一信号级自监督学习范式。核心是信号级伪标签生成网络（PLGN），包含可学习1D信号调制器和SigFormer，能从大量未标记自然图像自动合成具有真实多源信号特性的训练数据。实验表明，该方法在各种融合任务上取得了与有监督方法相当甚至更优的性能，解决了数据瓶颈问题。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 图像融合缺乏大规模真实训练数据，现有模型依赖小规模或合成数据集。
method: 提出SigFusion，通过信号级伪标签生成网络（PLGN）自动合成训练数据，包括信号调制器和SigFormer。
result: 在多种图像融合任务上，SigFusion无需真实配对数据即可达到优于现有方法的性能。
conclusion: 信号级自监督学习为图像融合提供了有效的数据生成范式，降低了对标注数据的依赖。
---

## Abstract
Image Fusion (IF) aims to integrate complementary features from multiple source images into a single image. However, a key challenge in this field is the lack of large-scale real-world training datasets. Existing models typically rely on either small datasets or synthetic, less realistic datasets. To address this, we propose SigFusion, a unified signal-level self-supervised learning paradigm for various IF tasks.The core idea is to use signal-level Pseudo-Label Generation Networks (PLGN) to automatically synthesize training sets and pseudo labels with real multi-source signal characteristics from vast unlabeled natural images.PLGN includes two critical components: learnable 1D Signal Modulators (SM) and SigFormer. SM learns implicit 1D signal patterns across various source images and embeds them into natural images, reducing the domain gap between synthetic and real datasets. SigFormer integrates Transformer with signal processing methods, establishing an appropriate signal representation space for SM. Its cascaded, multi-level design allows hierarchical feature learning from coarse to fine detail. Moreover, SigFormer can serve as a flexible backbone for IF, as its design adheres to the classic decomposition-reconstruction paradigm. Experimental results demonstrate that SigFusion achieves state-of-the-art performance across multiple IF tasks, including medical image fusion, infrared-visible image fusion, multi-focus image fusion, and multi-exposure image fusion.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：图像融合（Image Fusion, IF）领域长期面临**大规模真实训练数据严重匮乏**的瓶颈。现有方法主要采取两条路径：① 无监督学习（依赖小规模真实数据集，如 MMIF），受限于数据量，模型容易过拟合，泛化能力差；② 合成数据集（如 DPIF 通过亮度调整、局部模糊模拟多曝光/多聚焦），但合成与真实图像之间存在显著**域间隙**，且方法高度任务相关。
- **研究动机**：能否设计一种**统一的、跨任务的数据合成范式**，既能够利用大量无标签自然图像，又能生成与真实多源图像信号特性接近的合成数据，从而从根本上缓解数据稀缺问题？
- **整体含义**：论文提出 SigFusion，首个面向 IF 的**信号级自监督学习**框架，通过自动合成训练集和伪标签，在多个 IF 任务上取得 SOTA 结果，验证了信号级学习在解决数据瓶颈上的巨大潜力。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用**两阶段自监督学习（SSL）范式**：
  - **预文本任务（Pretext Task）**：使用大量无标签自然图像，通过**信号级伪标签生成网络（PLGN）** 自动生成带有多源信号特性的合成图像对及其伪标签，以此预训练融合网络。
  - **下游任务（Downstream Task）**：在真实 IF 数据集上微调预训练模型，适应实际分布。

- **关键技术细节**：
  - **伪标签生成网络（PLGN）**：
    - 由两个核心模块组成：**可学习 1D 信号调制器（SM）** 和 **SigFormer**。
    - **SM**：学习真实多源图像（如红外、可见光、CT、MRI 等）的隐式 **1D 信号模式（幅度、频率、相位）**，然后将这些模式注入自然图像的对应频带信号中。对低频分量使用 Transformer 捕获长程依赖，对高频分量使用 MLP 高效调制。
    - **SigFormer**：将 **Transformer** 与 **经验小波变换（EWT）** 相结合，形成级联的多级分解-重建结构。分解器逐级将图像解耦为低频和一个或多个高频子带；重建器逆向恢复。这种设计为 SM 提供了合适的信号表示空间，同时 SigFormer 本身也可作为融合网络的骨干。
  - **PLGN 训练**：以各种融合模型生成的融合图像为输入，以真实源图像为 GT，通过**像素级和信号级双重损失**（式 (7)）优化，使网络学会从融合图像逆映射回源图像，从而习得信号特征注入能力。
  - **融合网络**：与 PLGN 共享 SigFormer 骨干，仅在预训后使用真实 IF 数据微调，损失函数针对 MMIF 和 DPIF 略有不同（式 (2)、(3)）。

- **核心公式**（文字说明）：
  - PLGN 训练损失：信号级 MSE（低频与高频子带） + 像素级 MSE。
  - 预文本目标：最小化融合网络输出与伪标签之间的损失。
  - 微调目标：在真实数据上最小化融合结果与源图像之间的损失。

### 3. 实验设计：数据集、基准、对比方法

- **任务与数据集**：
  - **红外-可见光融合（VIF）**：训练集 MSRS，测试集 M³FD 和 TNO。
  - **医学图像融合（MIF）**：训练集 334 对哈佛医学网站图像（300 训练+34 验证），测试集 21 对 MRI-CT 和 42 对 MRI-PET。
  - **多聚焦融合（MFIF）**：训练集 RealMFF（710 对），测试集 LYTRO 和 MFFW。
  - **多曝光融合（MEF）**：训练集 SICE，测试集 SICE 和 MEFB。
  - **预文本自然图像**：Flickr25k（25,000 张），每张生成 4 种变体（对应四种任务），共 100,000 张合成图像。

- **基准（Benchmark）**：评估指标包括 **QG↑、QM↑、QP↑、MI↑、SD↑、VIFF↑**（除 SD 为无参考外，其余均以源图像作为参考，符合 IF 领域惯例）。

- **对比方法**：
  - **VIF/MIF**：U2Fusion、DeFusion、CDDFuse、LRRNet、MURF、EMMA、Text-Difuse、FILM、C2RF、GIF-Net、DCEvo、OmniFuse 等。
  - **MFIF**：CU-Net、IFCNN、SDNet、FusionDiff、MGDN、ZMFF、DeepM²CDL 等。
  - **MEF**：MEF-GAN、SwinFusion、HoLoCo 等。
  - 每种任务均对比 **12 种以上** 近期 SOTA 方法。

### 4. 资源与算力

- 论文明确说明：训练使用 **1 块 RTX 4090 GPU**（24 GB VRAM），搭配 **i9-14900k CPU、128 GB RAM**，PyTorch，CUDA 12.1。
- 优化器：Adam，学习率 1×10⁻⁴，Epochs=100，Batch Size=16。
- **但未报告具体训练时长**（如每阶段需要的天数或小时数），因此无法精确评估计算成本。

### 5. 实验数量与充分性

- **实验数量充分**：共覆盖 **4 大类 IF 任务**，每个任务在至少 2 个测试集上定量评估（表 1），同时提供定性可视化（图 4）。
- **消融实验完整**：包括：
  - SM 组件消融（移除全部 SM、仅移除高频 SM、仅移除低频 SM）；
  - SigFormer 组件消融（去掉 EWT、Transformer 替换为 CNN、EWT 替换为 DWT）；
  - 预文本任务消融（无预训练、无 PLGN 的预训练、完整预训练）。
  - 结果见表 2，验证了每个组件的必要性。
- **公平性**：对比方法均采用原文默认配置，且论文声称 PLGN 合成的数据集可提升其他融合模型性能（于补充材料中验证），说明其通用性。
- **结论**：实验设计较为完整、客观，消融实验和跨任务对比足以支撑论文主要结论。

### 6. 论文的主要结论与发现

- **主要发现**：
  - 信号级自监督学习范式（SigFusion）在**所有四个 IF 任务上均达到或超越当前 SOTA**（尤其 VIF、MIF、MFIF 的多数指标）。
  - **预文本任务**显著提升下游性能（对比 w/o-Pretrained 变体），证明了大规模信号级数据集合成的价值。
  - **SM** 是缩小域间隙的关键：同时调制高/低频信号比单调制或完全不调制效果更好。
  - **SigFormer**（Transformer + EWT）优于纯 Transformer、纯 CNN 或使用 DWT 的变体，表明信号分解与长程依赖结合的有效性。
- **结论**：SigFusion 为 IF 提供了一种统一、高效的数据生成与训练框架，有效解决了真实数据匮乏问题，且框架可扩展至多种任务。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - **首个信号级自监督 IF 范式**，突破了传统像素级或特征级生成的局限，从根本上缩小域间隙。
  - **PLGN 的通用性**：仅需少量真实多源图像对作为训练信号模式的参考，即可从大量自然图像生成逼真的多模态/多聚焦/多曝光数据集。
  - **SigFormer 的双重身份**：既可作为 PLGN 的组成部分，也可直接作为融合网络骨干，简化了整体设计。
  - **可学习 1D 信号调制器（SM）** 的引入，将图像信号模式显式建模为低频（全局）和高频（局部）两个分量，并分别用不同网络结构处理，设计合理。
- **实验设计亮点**：
  - **消融实验覆盖全面**，验证了每个创新组件的贡献。
  - **跨任务、多数据集对比**，充分展示了方法在不同场景下的鲁棒性。
  - 合成数据集的可视化（图 5、图 6）直观展示了信号注入效果，增强了说服力。

### 8. 不足与局限

- **实验局限**：
  - **训练时长未报告**：尽管给出硬件配置，但读者无法估计训练成本，对可复现性造成障碍。
  - **PLGN 训练依赖多种融合模型的输出**：论文未明确说明具体使用了哪些融合模型来生成训练 PLGN 的输入图像。若模型选择不当或多样性不足，可能引入偏差，影响 PLGN 的泛化能力。
  - **未在真实极端场景下测试**：例如极端光照、遮挡、大位移等情况，论文主要针对标准基准集。
  - **计算复杂度与推理速度缺乏分析**：SigFormer 采用级联 Transformer 和 EWT，可能带来较大计算开销，但未与对比方法进行效率比较。
- **应用限制**：
  - 目前仅验证了四种常见 IF 任务，是否可无缝扩展到遥感融合、全景融合等其他子领域尚不明确。
  - **假设自然图像与目标多源图像之间存在可对齐的信号模式**，对于某些信号结构差异极大的 modality（如深度图与可见光），信号级调制可能失效。
  - 预文本阶段需要 Flickr25k 规模的通用自然图像，但在知识产权或隐私敏感场景下可能受限。

（完）
