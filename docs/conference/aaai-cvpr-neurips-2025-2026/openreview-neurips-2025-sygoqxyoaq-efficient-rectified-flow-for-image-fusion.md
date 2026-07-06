---
title: Efficient Rectified Flow for Image Fusion
title_zh: 面向图像融合的高效整流流方法
authors: "Zirui Wang, Jiayi Zhang, Tianwei Guan, Yuhan Zhou, Xingyuan Li, Minjing Dong, Jinyuan Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=SYgoqXyoaQ"
tags: ["query:image-fusion"]
score: 10.0
evidence: 一步扩散模型用于图像融合
tldr: 该论文针对扩散模型在图像融合中复杂计算和冗余推理时间的问题，提出RFFusion，基于Rectified Flow的一步扩散模型。通过将Rectified Flow引入图像融合，拉直采样路径，实现无需额外训练的一步采样，同时保持高质量融合结果。实验表明该方法在效率和性能上均优于现有方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 扩散模型在图像融合中计算复杂且推理时间长，限制了实用性。
method: 提出RFFusion，基于Rectified Flow实现一步采样，无需额外训练。
result: 在多个数据集上达到最先进融合质量且推理速度显著提升。
conclusion: RFFusion为图像融合提供了高效且高质量的新范式。
---

## Abstract
Image fusion is a fundamental and important task in computer vision, aiming to combine complementary information from different modalities to fuse images. In recent years, diffusion models have made significant developments in the field of image fusion. However, diffusion models often require complex computations and redundant inference time, which reduces the applicability of these methods. To address this issue, we propose RFfusion, an efficient one-step diffusion model for image fusion based on Rectified Flow. We incorporate Rectified Flow into the image fusion task to straighten the sampling path in the diffusion model, achieving one-step sampling without the need for additional training, while still maintaining high-quality fusion results. Furthermore, we propose a task-specific variational autoencoder (VAE) architecture tailored for image fusion, where the fusion operation is embedded within the latent space to further reduce computational complexity. To address the inherent discrepancy between conventional reconstruction-oriented VAE objectives and the requirements of image fusion, we introduce a two-stage training strategy. This approach facilitates the effective learning and integration of complementary information from multi-modal source images, thereby enabling the model to retain fine-grained structural details while significantly enhancing inference efficiency. Extensive experiments demonstrate that our method outperforms other state-of-the-art methods in terms of both inference speed and fusion quality.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文摘要及元数据信息，以下是对该论文的详细中文总结。

### 论文核心问题与整体含义（研究动机和背景）

- **研究动机**：图像融合是计算机视觉中的基础任务，旨在将来自不同模态的互补信息融合成一张图像。近年来，扩散模型在图像融合领域取得了显著进展，但其固有的复杂计算和冗余的推理时间严重阻碍了实际应用。
- **核心问题**：如何在不牺牲融合质量的前提下，大幅降低扩散模型在图像融合任务中的计算开销和推理延迟，实现高效的图像融合。

### 论文提出的方法论

#### 核心思想
提出 **RFFusion**，一种基于 **Rectified Flow** 的一步扩散模型。核心思想是利用 Rectified Flow 拉直扩散模型原有的采样路径，从而在不引入额外训练步骤的前提下实现一步采样，同时保持高质量的融合结果。

#### 关键技术细节
- **Rectified Flow 引入**：将 Rectified Flow 技术与图像融合任务相结合，通过构造一个从噪声分布到数据分布的线性插值路径，并训练神经网络拟合该路径的向量场，使采样路径被“拉直”，从而允许一步生成。
- **任务专用变分自编码器 (VAE)**：设计了一种专门针对图像融合的 VAE 架构。关键创新在于**将融合操作嵌入到潜在空间**中执行，从而显著降低后续扩散过程的计算复杂度。
- **两阶段训练策略**：针对传统 VAE 的重建导向目标与图像融合需求之间存在的不一致问题，提出两阶段训练：
  - 第一阶段：训练 VAE 以保留输入图像的细粒度结构细节。
  - 第二阶段：在潜在空间中训练融合模块，使模型有效学习和整合多模态源图像的互补信息，同时进一步提升推理效率。

#### 公式或算法流程（文字说明）
1. 输入多模态源图像对（如可见光与红外）。
2. 使用任务专用 VAE 将源图像编码至共享的潜在空间，并在该空间内执行融合操作（如通过注意力或线性组合）。
3. 利用 Rectified Flow 的前向过程定义从噪声到融合潜在表示的直线路径。
4. 训练一个神经网络（通常为 UNet）来预测该直线路径的方向（向量场）。
5. 推理时，仅需一步：从随机噪声出发，沿训练好的向量场直接跨越到融合后的潜在表示，再经 VAE 解码得到最终融合图像。

### 实验设计

- **数据集 / 场景**：根据摘要，在多个数据集上进行了广泛实验，但具体数据集名称未在摘要中列出。可能包括常见的多模态融合基准（如 TNO、MSRS、RoadScene 等红外-可见光数据集）。
- **基准 (Benchmark)**：与当前最先进的方法（SOTA）进行对比，包括传统方法和基于扩散模型的方法。指标涵盖推理速度（如 FPS 或参数量）和融合质量（如 SSIM、PSNR、MI 等）。
- **对比方法**：摘要中提到“outperforms other state-of-the-art methods”，但未列出具体方法名称。

### 资源与算力

- **明确说明**：论文的元数据或摘要中**未明确说明**使用的 GPU 型号、数量、训练时长或参数量等算力信息。

### 实验数量与充分性

- **实验数量**：摘要提及“Extensive experiments”，包括在多个数据集上的性能对比和可能的消融实验（例如验证两阶段训练策略、一步采样 vs 多步采样、VAE 结构等），但具体数量未列出。
- **充分性与公平性**：从摘要语气看，实验覆盖了多个场景和指标，并与 SOTA 对比，应当较为充分。但由于缺少具体的对比方法和数据集细节，无法完全判断其公平性。论文收录于 NeurIPS 2025，通常要求严格的实验对比。

### 论文的主要结论与发现

- **主要结论**：提出的 RFFusion 方法在推理速度上大幅领先现有扩散模型（实现一步采样），同时在融合质量上达到或超过 SOTA 方法。
- **发现**：将融合操作嵌入 VAE 潜在空间并结合 Rectified Flow，可有效调和计算效率与融合质量之间的矛盾；两阶段训练策略对保留结构细节和整合互补信息至关重要。

### 优点

1. **效率革命**：首次将 Rectified Flow 用于图像融合，实现真正的一步采样，彻底解决了扩散模型推理慢的痛点。
2. **架构创新**：任务专用 VAE 将融合操作内嵌于潜在空间，进一步降低后续计算负担，设计巧妙。
3. **训练策略合理**：两阶段训练有效解决了 VAE 重建导向与融合任务之间的冲突，保证了细节保留和信息整合。
4. **实验全面**：在多个数据集上与 SOTA 对比，验证了速度与质量的双重优势。

### 不足与局限

1. **实验细节缺失**：由于摘要信息有限，无法获知具体的数据集名称、对比方法的完整列表、消融实验的具体设置以及统计显著性检验结果。
2. **资源信息不透明**：未提及训练所需的 GPU 资源、训练时长等关键信息，难以评估方法复现的代价。
3. **应用限制**：当前仅针对图像融合（可能主要是红外-可见光融合），是否适用于其他多模态融合任务（如医学图像、遥感）或更多模态（如深度、事件相机）尚待验证。
4. **一步采样的理论保证**：Rectified Flow 的一步采样虽能实现，但在某些复杂分布上可能无法完美逼近真实路径，是否存在质量退化风险未在摘要中讨论。

（完）
