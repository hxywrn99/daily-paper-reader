---
title: U-KAN Makes Strong Backbone for Medical Image Segmentation and Generation
title_zh: U-KAN：为医学图像分割与生成打造强大骨干网络
authors: "Chenxin Li, Xinyu Liu, Wuyang Li, Cheng Wang, Hengyu Liu, Yifan Liu, Zhen Chen, Yixuan Yuan"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32491/34646"
tags: ["query:medical-seg"]
score: 10.0
evidence: 基于KAN的医学图像分割骨干网络
tldr: 针对U-Net线性建模能力有限和可解释性不足，受KAN网络启发提出U-KAN，将非线性可学习激活函数堆叠引入骨干网络，在医学图像分割和生成任务上展现出更优的精度和可解释性，为医学视觉骨干提供了新思路。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: U-Net及其变种线性建模能力有限且可解释性差。
method: 利用KAN的非线性可学习激活函数堆叠重构U-Net骨干，提出U-KAN。
result: 在多个医学分割和生成任务上，U-KAN优于传统U-Net和基于Transformer的变体。
conclusion: KAN的引入为医学图像分析骨干网络带来了精度与可解释性的双重提升。
---

## Abstract
U-Net has become a cornerstone in various visual applications such as image segmentation and diffusion probability models. While numerous innovative designs and improvements have been introduced by incorporating transformers or MLPs, the networks are still limited to linearly modeling patterns as well as the deficient interpretability. To address these challenges, our intuition is inspired by the impressive results of the Kolmogorov-Arnold Networks (KANs) in terms of accuracy and interpretability, which reshape the neural network learning via the stack of non-linear learnable activation functions derived from the Kolmogorov-Anold representation theorem. Specifically, in this paper, we explore the untapped potential of KANs in improving backbones for vision tasks. We investigate, modify and re-design the established U-Net pipeline by integrating the dedicated KAN layers on the tokenized intermediate representation, termed U-KAN. Rigorous medical image segmentation benchmarks verify the superiority of UKAN by higher accuracy even with less computation cost. We further delved into the potential of U-KAN as an alternative U-Net noise predictor in diffusion models, demonstrating its applicability in generating task-oriented model architectures.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：U-Net及其变体（如U-Net++、Transformer-U-Net等）在医学图像分割与生成中已成为基石，但其核心结构（卷积、MLP、Transformer）本质上是线性模式建模，且缺乏可解释性。这限制了模型对医学图像中复杂非线性关系（如不同通道对应不同临床/解剖/病理特征）的捕获能力。
- **背景**：KAN（Kolmogorov-Arnold Networks）基于Kolmogorov-Arnold表示定理，通过堆叠可学习的非线性激活函数（而非固定激活函数+线性权重矩阵），在精度和可解释性上展现出潜力。受此启发，论文首次尝试将KAN融入U-Net架构，以提升医学图像骨干网络的性能与可解释性。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：设计U-KAN，采用“卷积阶段 + Tokenized KAN阶段”的混合架构，在U-Net的浅层（高分辨率）用传统卷积保持局部细节，在深层（低分辨率）用KAN层增强非线性建模能力与可解释性。
- **关键技术细节**：
  - **Convolution Phase**：三个卷积块（Conv+BN+ReLU+MaxPool），逐步降低分辨率、增加通道数。
  - **Tokenized KAN Phase**：两个Tokenized KAN块。首先将卷积阶段的输出特征图token化（reshape为patch序列），通过线性投影映射到D维嵌入空间；然后经过三个KAN层，每个KAN层后接深度可分离卷积（DwConv）、BN、ReLU以及残差连接；最后层归一化。时间嵌入仅在扩散版本中注入。
  - **KAN层定义**：KAN( Z ) = ( Φ_K-1 ◦ ... ◦ Φ_0 ) Z，其中每个Φ矩阵由可学习的一维非线性激活函数ϕ组成，代替传统MLP的线性权重+固定激活。
  - **解码器**：采用U形对称结构，通过跳跃连接融合编码器和解码器特征，最终输出分割图（或生成任务中的噪声预测）。
  - **扩散扩展**：将U-KAN作为噪声预测器（Diffusion U-KAN），输入含噪图像和时间步，输出预测噪声，使用MSE损失优化。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：三个医学分割/生成公开数据集：
  - **BUSI**（乳腺癌超声）：647张图像，256×256。
  - **GlaS**（腺体分割）：165张图像（来自31个序列），512×512。
  - **CVC-ClinicDB**（结肠镜息肉）：612张图像，256×256。
- **基准任务**：医学图像分割（IoU、F1）、无条件图像生成（FID、IS）。
- **对比方法**：
  - 分割：U-Net、Att-UNet、U-Net++、U-NeXt、Rolling-UNet、U-Mamba。
  - 生成：Diffusion U-Net（ResBlock+Attn、Identity、MLP）变体。
- **训练细节**：PyTorch，NVIDIA RTX 4090 GPU，batch size 8，学习率1e-4（Adam，余弦退火至1e-5），损失函数BCE+Dice，训练400 epoch，80%/20%分割，随机旋转/翻转增强。

### 4. 资源与算力

- 论文明确提到使用**单块NVIDIA RTX 4090 GPU**，基于PyTorch实现。
- **未说明**：具体训练时长（总GPU小时数）、多卡并行与否、内存占用等细节。

### 5. 实验数量与充分性

- **分割实验**：在3个数据集上对比**6种基线**（共7种方法），报告3次随机运行的均值和标准差，含定量表格与定性可视化。
- **生成实验**：在同样的3个数据集上对比**3种扩散U-Net变体**，报告FID和IS。
- **消融实验**：
  - KAN层数（1~5层）：确定3层最优。
  - KAN vs MLP（替换不同位置）：验证KAN的必要性。
  - 模型缩放（Small/Base/Large）：分析通道数影响。
  - 可解释性分析：通过激活图与GT的Plausibility IoU对比KAN与MLP。
- **充分性**：实验覆盖了多种医学模态（乳腺超声、腺体病理、内镜息肉），消融系统全面（结构、替代、规模、可解释性），且报告了随机种子下的统计数据，具有较好的客观性与公平性。但未在3D医学数据或更大分辨率上验证，泛化性受限。

### 6. 论文的主要结论与发现

- U-KAN在所有三个分割数据集上均取得最佳IoU和F1，且计算量（GFLOPs）显著低于Transformer/Mamba类方法，仅略高于U-NeXt但精度更高。
- 在无条件图像生成任务中，Diffusion U-KAN在三个数据集上均实现最低FID和最高IS，优于传统U-Net扩散变体。
- KAN层替代MLP后，模型可解释性显著提升（激活图更贴近GT区域）。
- 3层KAN为最优配置；模型规模增大性能提升，符合缩放定律。

### 7. 优点

- **方法创新**：首次将KAN成功嵌入U-Net，提出Tokenized KAN Block，巧妙利用KAN的非线性建模与可解释性优势，同时保持与卷积的兼容性。
- **性能优异**：在多个医学分割任务上超越包括Transformer、Mamba等在内的SOTA，且计算开销更低（GFLOPs 14.02 vs U-Mamba 2087）。
- **可解释性增强**：通过激活图可视化证明KAN层有助于定位病灶边界，增强临床可信度。
- **通用性**：既可用于分割，也可扩展为扩散模型的噪声预测器，展示了骨干网络的通用潜力。

### 8. 不足与局限

- **实验覆盖有限**：仅测试了2D医学图像（超声、病理、内镜），未涉及3D医学数据（如CT、MRI）或更大分辨率的自然图像，泛化能力待验证。
- **计算细节不充分**：未提供具体训练时长、显存消耗或推理速度对比，不利于复现和实际部署评估。
- **参数效率**：虽然GFLOPs低，但参数量（6.35M）仍高于U-NeXt（1.47M），在资源极度受限场景下可能不是最优。
- **KAN本身局限性**：论文未讨论KAN层在训练稳定性、收敛速度或大尺度模型上的潜在问题（如激活函数过拟合）。
- **可解释性量化**：仅用Plausibility IoU一个指标，且未与Grad-CAM等形式化可解释性方法对比，解释性分析较初步。

（完）
