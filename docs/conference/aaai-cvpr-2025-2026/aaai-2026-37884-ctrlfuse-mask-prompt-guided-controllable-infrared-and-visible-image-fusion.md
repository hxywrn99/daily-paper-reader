---
title: "CtrlFuse: Mask-Prompt Guided Controllable Infrared and Visible Image Fusion"
title_zh: CtrlFuse：掩码提示引导的可控红外与可见光图像融合
authors: "Yiming Sun, Yuan Ruan, Qinghua Hu, Pengfei Zhu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37884/41846"
tags: ["query:image-fusion"]
score: 9.0
evidence: 红外与可见光图像融合的深度学习方法
tldr: 针对现有红外与可见光图像融合方法忽略下游任务适应性、无法交互式处理多样语义目标感知需求的问题，提出了CtrlFuse可控融合框架。该框架集成多模态特征提取器、参考提示编码器和提示语义融合模块，通过掩码提示实现交互式动态融合。实验表明该方法能根据任务提示自适应调整融合策略，提升了融合结果在下游任务中的实用性，为智能无人系统提供了更灵活的环境感知能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多模态图像融合方法缺乏对下游任务的适应性，无法根据语义目标动态调整融合策略。
method: 提出CtrlFuse框架，包含多模态特征提取器、参考提示编码器和提示语义融合模块，通过掩码提示引导可控融合。
result: 在红外与可见光融合任务上，该方法能根据任务提示生成适应下游感知需求的融合图像，性能优于现有方法。
conclusion: 掩码提示引导的可控融合为多模态图像融合提供了更灵活、任务适应的解决方案。
---

## Abstract
Infrared and visible image fusion generates all-weather perception-capable images by combining complementary modalities, enhancing environmental awareness for intelligent unmanned systems. Existing methods either focus on pixel-level fusion while overlooking downstream task adaptability or implicitly learn rigid semantics through cascaded detection/segmentation models, unable to interactively address diverse semantic target perception needs. We propose CtrlFuse, a controllable image fusion framework that enables interactive dynamic fusion guided by mask prompts. The model integrates a multi-modal feature extractor, a reference prompt encoder (RPE), and a prompt-semantic fusion module (PSFM). The RPE dynamically encodes task-specific semantic prompts by fine-tuning pre-trained segmentation models with input mask guidance, while the PSFM explicitly injects these semantics into fusion features. Through synergistic optimization of parallel segmentation and fusion branches, our method achieves mutual enhancement between task performance and fusion quality. Experiments demonstrate state-of-the-art results in both fusion controllability and segmentation accuracy, with the adapted task branch even outperforming the original segmentation model.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：红外与可见光图像融合旨在结合两种模态的互补信息，提升智能无人系统的全天候环境感知能力。现有方法主要分为两类：一类仅关注像素级融合质量，忽略下游任务（如检测、分割）的适应性；另一类通过级联检测/分割网络隐式学习语义，但局限于固定类别，无法根据实际需求交互式地动态调整对不同语义目标的关注。
- **整体含义**：本文提出一种**掩码提示（Mask Prompt）引导的可控融合框架 CtrlFuse**，首次将交互式语义控制引入多模态图像融合，使得融合过程能够根据用户指定的目标（通过掩码提示）动态增强特定区域，从而桥接低级融合与高级感知任务。

## 2. 方法论
- **核心思想**：利用联邦学习范式，将预训练的语义分割基础模型（SAM）的语义能力注入融合网络，通过**参考提示编码器（RPE）** 和**提示语义融合模块（PSFM）** 实现掩码提示驱动的动态语义注入，并联合优化融合损失与分割损失，实现融合质量与任务性能的相互促进。
- **关键流程**（文字描述）：
  - 输入红外图像 I_ir 和可见光图像 I_vis，分别通过红外和可见光编码器提取特征 F_ir 和 F_vis，拼接后经图像解码器得到初步融合特征 F_ref 及参考图像 I_ref。
  - **参考提示编码器（RPE）**：以掩码提示（Prompt m）指导，将 F_ir（或 F_vis）作为支持特征、F_ref 作为查询特征，通过交叉注意力与自注意力机制生成任务相关的提示嵌入 P。具体地，先计算掩码与特征的点积并进行平均池化得到全局特征 F_t，再拼接后经过卷积得到 F_supp 和 F_qry，然后通过一组可学习查询 Q 经过交叉/自注意力生成类别特定的参考提示 P。
  - **提示语义融合模块（PSFM）**：将编码器特征 F、提示嵌入 P 和 SAM 生成的分割掩码 M 作为输入，先下采样并展平 F 为序列，与 P 做交叉注意力，然后恢复空间维度并上采样，最后与对应掩码 M 做逐元素乘法，得到增强后的提示特征 F_ir^p（或 F_vis^p）。
  - 最终融合特征 = F_ref + F_ir^p + F_vis^p，再送入图像解码器生成最终融合图像 IF。
  - 损失函数：融合损失 L_fusion（包括像素级约束和感知损失）与分割损失 L_seg（SAM微调损失）联合优化。

## 3. 实验设计
- **数据集**：
  - **FMB**：1500对车载红外-可见光图像，含语义分割标签。训练1020/验证280/测试200对。
  - **MSRS**：1444对图像，训练1072/验证150/测试200对。
  - **DroneVehicle**：无人机视角数据集，无分割标签，使用其他模型生成掩码，测试200对。
- **Benchmark**：与8种方法对比——LDFusion（CLIP-based）、SwinFuse（Transformer）、NestFuse（autoencoder）、CDDFuse（分解）、DIDFuse（分解）、SeAFusion（分割驱动）、PSFusion（高级视觉任务驱动）、SDCFusion（分割驱动）。
- **评估指标**：融合质量指标（MSE、PSNR、Qabf、Nabf、SSIM、SCD）和下游任务指标（语义分割mIoU、目标检测AP@[0.5:0.95]）。

## 4. 资源与算力
- 文中明确说明：使用**4块 NVIDIA GeForce RTX 3090 GPU**，优化器为Adam，学习率1×10⁻⁴，训练150个epoch，batch size为4。训练数据量约1000-1500对图像，总训练时长未提及，但可推测在合理范围内。

## 5. 实验数量与充分性
- **实验组数**：
  - 融合质量定量对比：在三个数据集上各报告6个指标（共18个数值对比）。
  - 定性可视化：给出FMB和DroneVehicle的视觉对比图。
  - 高级视觉任务评估：语义分割（MSRS数据集，9个类别mIoU + 总体mIoU）、目标检测（DroneVehicle数据集，4类+总体AP）。
  - 消融实验：在MSRS上进行了5组消融（w/o Prompt、w/o Seg、w/o Vis、w/o Ir、Exchange SQ），对比6个指标。
  - 附加分析：掩码提示微调对SAM性能的影响（图6）、可控性验证（图7）、掩码质量敏感度分析（图8）。
- **充分性**：实验设计较为全面，覆盖了融合质量、下游任务、消融、可控制性和鲁棒性。对比方法选择SOTA，公平性良好。消融实验设计合理，分别验证了掩码提示、分割分支、双模态、支持/查询交换的影响。
- **潜在不足**：未在更多复杂场景（如恶劣天气、低光）上验证；未与最新的扩散模型方法（如DRMF）直接对比（文中提及但未在对比表中列出）；缺少运行时间或模型复杂度对比。

## 6. 主要结论与发现
- CtrlFuse在三个数据集上的融合质量指标（尤其PSNR、Qabf）和下游任务（分割mIoU、检测AP）均达到或超过SOTA。
- 掩码提示的可控融合能显著提升指定类别的分割性能（如“Car”类），甚至使整体分割精度超过原始SAM模型。
- 消融实验表明，提示分支、分割损失、双模态均对性能有正向贡献；分割任务有助于提升融合质量，融合任务也有助于提升分割精度，形成互促进关系。
- 模型对掩码质量具有鲁棒性，即使掩码不完整或粗略，仍能有效增强目标区域。

## 7. 优点
- **创新性强**：首次将SAM的交互式提示概念引入多模态图像融合，实现用户指定的动态语义控制，比传统级联方法更灵活。
- **显式语义注入**：通过PSFM模块显式地将语义提示与多模态特征融合，而非隐式学习，提升了可控性和可解释性。
- **互促进学习**：联合优化融合和分割分支，实现两个任务的协同增强，而非单向引导。
- **鲁棒性好**：对提示掩码的完整度不敏感，在粗/不完整掩码下仍能有效工作，实用性强。
- **实验全面**：涵盖三个不同来源数据集（车载、无人机）、多类指标、多视角分析。

## 8. 不足与局限
- **计算复杂度**：使用了冻结的SAM和多个交叉注意力模块，推理速度可能较慢，未报告运行时间或参数量，不利于实时应用部署。
- **数据集覆盖有限**：仅使用了白天/正常光照条件下的数据集，未在夜间、大雾等极端场景验证，泛化性需进一步评估。
- **掩码生成依赖**：对于无标签数据集（如DroneVehicle、LLVIP），需要额外模型（Grounded-SAM）生成掩码，引入级联误差；且掩码的类别由用户输入文本决定，灵活性受限于文本到掩码的质量。
- **控制粒度**：目前只支持掩码提示（区域级别），不支持更精细的像素级或实例级控制。
- **对比方法遗漏**：未与最新扩散模型驱动的方法（如DRMF、Text-IF）等直接比较，可能导致基准不够完整。
- **偏差风险**：实验主要在车载和无人机视角进行，可能对行人、车辆等类别有偏，其他场景（如遥感、医学）未验证。

（完）
