---
title: Revisiting Generative Infrared and Visible Image Fusion Based on Human Cognitive Laws
title_zh: 基于人类认知规律的生成式红外与可见光图像融合再探索
authors: "Lin Guo, Xiaoqing Luo, Wei Xie, Zhancheng Zhang, Hui Li, Rui Wang, Zhenhua Feng, Xiaoning Song"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=wvcYIEaD5X"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于人类认知规律的生成式图像融合
tldr: 现有生成式融合方法在模态信息平衡和可解释性方面存在不足。本文受人类认知规律启发，提出HCLFuse方法，通过多尺度掩码和信息映射量化理论设计无监督融合网络。实验证明该方法能更可靠地选择模态信息，生成一致性更好的融合图像。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 生成式融合方法缺乏可解释性，模态信息选择不可靠。
method: 借鉴人类认知规律，设计多尺度掩码和信息映射量化理论指导无监督融合网络。
result: 在红外与可见光融合基准上取得更优的视觉效果和定量指标。
conclusion: HCLFuse提升了生成式融合的可解释性和鲁棒性。
---

## Abstract
Existing infrared and visible image fusion methods often face the dilemma of balancing modal information. Generative fusion methods reconstruct fused images by learning from data distributions, but their generative capabilities remain limited. Moreover, the lack of interpretability in modal information selection further affects the reliability and consistency of fusion results in complex scenarios. This manuscript revisits the essence of generative image fusion under the inspiration of human cognitive laws and proposes a novel infrared and visible image fusion method, termed HCLFuse. First, HCLFuse investigates the quantification theory of information mapping in unsupervised fusion networks, which leads to the design of a multi-scale mask-regulated variational bottleneck encoder. This encoder applies posterior probability modeling and information decomposition to extract accurate and concise low-level modal information, thereby supporting the generation of high-fidelity structural details. Furthermore, the probabilistic generative capability of the diffusion model is integrated with physical laws, forming a time-varying physical guidance mechanism that adaptively regulates the generation process at different stages, thereby enhancing the ability of the model to perceive the intrinsic structure of data and reducing dependence on data quality. Experimental results show that the proposed method achieves state-of-the-art fusion performance in qualitative and quantitative evaluations across multiple datasets and significantly improves semantic segmentation metrics. This fully demonstrates the advantages of this generative image fusion method, drawing inspiration from human cognition, in enhancing structural consistency and detail quality.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有红外与可见光图像融合方法（尤其是生成式融合方法）面临模态信息平衡的困境，生成能力有限，且缺乏可解释性，导致在复杂场景下融合结果的可靠性和一致性不足。
- **研究动机**：人类认知规律（如选择性注意、信息分解等）可为生成式图像融合提供启发，从而提升模型对模态信息的选择能力和融合结果的物理一致性。
- **整体含义**：通过引入人类认知规律来指导无监督融合网络的设计，有望在保持结构细节的同时增强融合图像的真实性和鲁棒性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：受人类认知规律启发，提出 HCLFuse 方法，核心是**多尺度掩码调节的变分瓶颈编码器**与**时变物理引导机制**，以实现模态信息的解耦和生成过程的物理约束。
- **关键技术细节**：
  - **多尺度掩码调节的变分瓶颈编码器**：基于信息映射的量化理论，采用后验概率建模与信息分解，提取准确且简洁的低层模态信息，支持高保真结构细节生成。
  - **时变物理引导机制**：将扩散模型的概率生成能力与物理定律结合，形成随时间变化的引导机制，自适应调节不同生成阶段，增强模型对数据内在结构的感知，降低对数据质量的依赖。
- **算法流程**（文字说明）：
  1. 输入红外与可见光图像对；
  2. 通过变分瓶颈编码器进行多尺度掩码调节，提取各模态的互补特征；
  3. 利用扩散模型进行条件生成，同时在生成过程中施加时变物理引导；
  4. 输出融合图像，实现结构一致性与细节质量的平衡。

## 3. 实验设计
- **使用的数据集/场景**：摘要中提及“多个数据集”，但未列出具体名称。根据红外与可见光融合领域常见基准，可能包括 TNO、RoadScene、LLVIP、M3FD 等（需推断）。
- **Benchmark**：与多种现有融合方法（包括生成式和非生成式）进行定性（视觉效果）和定量（指标）比较。
- **对比方法**：文中提到“state-of-the-art fusion performance”，但未具体列举方法名称。推测对比了如 DenseFuse、FusionGAN、U2Fusion、RFN-Nest 等经典方法。
- **评价指标**：除常规融合质量指标（如 SSIM、PSNR、MI、SF 等）外，还特别评估了语义分割指标（mIoU 等），显示融合结果对下游任务的提升。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据中未提及使用的 GPU 型号、数量、训练时长、内存等计算资源信息。仅能推测为现代深度学习标准配置（如单卡或双卡 V100/A100），但无具体数据可引用。

## 5. 实验数量与充分性
- **实验数量**：摘要仅概括性描述，未列出具体实验组数。通常包含：
  - 多个公开数据集（至少3~4个）上的定性定量对比；
  - 与多种最新方法的全面比较；
  - 消融实验（验证多尺度掩码、时变物理引导等各组件的贡献）；
  - 语义分割下游任务评估。
- **充分性与客观性**：从摘要看，实验覆盖多个数据集，且包含语义分割指标，证明方法对实际应用的提升。但未提供详细统计显著性检验或误差棒信息，且无代码开源说明。整体实验设计较充分，但全面性需原文验证。

## 6. 论文的主要结论与发现
- HCLFuse 在多个数据集的定性和定量评估中达到最先进性能，显著提升语义分割指标。
- 该方法证明：融入人类认知规律（注意力、信息分解）和物理引导，能有效提升生成式图像融合的可解释性和鲁棒性，特别是在结构一致性和细节质量方面优势明显。

## 7. 优点：方法或实验设计上的亮点
- **方法论创新**：
  - 首次系统地将人类认知规律（信息映射量化、多尺度掩码）引入生成式图像融合。
  - 时变物理引导机制结合扩散模型与物理定律，自适应调控生成过程，具有理论新颖性。
- **实验设计亮点**：
  - 不仅评估常规融合指标，还评测下游语义分割任务，验证实际应用价值。
  - 采用多个数据集，避免单数据集过拟合风险。

## 8. 不足与局限
- **实验覆盖有限**：
  - 摘要中未具体列出数据集名称、对比方法、实验参数，信息透明度不足。
  - 未提及极端场景（如严重噪声、遮挡、配准误差）下的表现。
- **偏差风险**：
  - 可能只在特定数据集上优化，未见跨域泛化测试。
  - 语义分割指标的提升可能受融合与分割模型耦合影响，不一定反映通用融合质量。
- **应用限制**：
  - 依赖扩散模型推理速度较慢，可能无法满足实时要求。
  - 需要成对红外-可见光数据训练，实际场景中数据获取困难。
- **资源与可重复性**：
  - 未提供算力信息、代码或训练参数，复现有一定难度。

（完）
