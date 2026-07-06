---
title: "Make Foundation Models Trustworthy Again: Causal Fine-Adaptation for Medical Image Segmentation"
title_zh: 让基础模型再次可信：用于医学图像分割的因果微适应
authors: "Hongpeng Yang, Yingxin Chen, Shiqiang Ma, Fei Guo"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39970/43931"
tags: ["query:medical-seg"]
score: 10.0
evidence: 医学图像分割基础模型的因果微调适应
tldr: 针对视觉基础模型在医学图像分割中性能退化且微调易遗忘的问题，提出CausalBridgeNet因果引导校正框架，借鉴预测编码理论实现高效且可解释的微调，在脑肿瘤分割等任务上保证了可靠性和结构精度。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 视觉基础模型在医学图像分割中性能显著下降，且微调成本高、易遗忘。
method: 提出CausalBridgeNet因果引导校正框架，基于贝叶斯大脑的预测编码理论进行适应。
result: 在医学分割任务上有效提升了基础模型的精度和可靠性。
conclusion: CausalBridgeNet通过因果机制实现了基础模型在医学分割中的可信适应。
---

## Abstract
Vision foundation models (e.g., SAM2, CLIP) show strong generalization in natural image analysis but degrade significantly in specialized domains like medical imaging. This is critical for tasks such as brain tumor segmentation, where errors directly affect surgical planning and patient outcomes. In such contexts, segmentation must be highly reliable and structurally precise, underscoring the need for adaptable methods with low error tolerance. While fine-tuning is the dominant strategy, it is computationally expensive and prone to forgetting. To address this, we propose CausalBridgeNet, a causality-guided correction framework for medical image segmentation. Inspired by predictive coding theories of the Bayesian brain, our method introduces a Predictive Causal Reasoning Unit (PCRU) that estimates structured error maps and delivers targeted feedback to iteratively refine predictions. This forms a closed-loop, error-aware correction mechanism without modifying the foundation model. By keeping the backbone frozen, CausalBridgeNet preserves general visual priors while enhancing task-specific accuracy. On the BraTS 2025 benchmark, it achieves an average Dice score of 84.48 and HD95 of 5.48 across tumor subregions, demonstrating its effectiveness for high-precision medical segmentation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

视觉基础模型（如 CLIP、SAM2）在自然图像分析中展现了强大的泛化能力，但在医学图像分割等专业领域性能显著下降。脑肿瘤分割等高风险任务对分割的可靠性和结构精度要求极高，而直接使用基础模型往往产生较大误差。常见的微调策略存在两个主要问题：一是计算开销大，二是容易灾难性遗忘，损害预训练模型中的通用知识。因此，需要一种高效、可扩展且不会破坏基础模型泛化能力的方法来适应医学图像域。

论文从贝叶斯大脑的预测编码理论出发，将域适应视为因果推断问题，提出通过显式估计和补偿系统性预测误差来改进模型，而无需修改冻结的骨干网络。整体含义是：通过轻量、可解释的因果校正机制，“让基础模型再次可信”，即在保留其通用视觉先验的同时提升特定任务的精度和可靠性。

## 2. 方法论：核心思想、关键技术细节与流程

### 核心思想
- 借鉴预测编码理论：感知由最小化结构化预测误差驱动。
- 将预测与输入之间的差异视为结构化的误差信号，用于引导修正。
- 通过一个轻量、可解释的校正模块（PCRU）实现闭环因果校正，不修改基础模型。

### 关键技术细节
1. **Vision Foundation Encoder**：将冻结的 VFM 块（CLIP/SAM2）嵌入 U 形架构，构成层次化编码器。使用 Gated Spatial Convolution (GSC)、层归一化、MLP 通道适配器等轻量模块支持三维体素处理。输入图像经4个阶段逐级下采样至 1/16 分辨率，得到多尺度特征 {E1, E2, E3, E4}。
2. **Semantic Decoder**：通过 U 形上采样结构，融合编码器对应层的跳跃连接特征，逐步恢复分辨率，最终输出粗分割 logits Y。
3. **Predictive Causal Reasoning Unit (PCRU)**：
   - **Mixture of Causal Experts (MoCE)**：将中间解码器特征 F 与可学习三维提示 P 拼接，经 Router 计算全局注意力权重 w，然后将加权后的专家输出解释为假阳性（FP）和假阴性（FN）分数图：S_causal = [S_fp || S_fn]。
   - **Causal Corrector**：对每个类别 c，利用可学习标量权重 α_c、β_c 对粗 logits 进行调整：Ŷ_c = Y_c + β_c·S_c^fn - α_c·S_c^fp。实现类感知、方向敏感的修正。
4. 整个骨干网络保持冻结，仅训练适配器（Adapter）和 PCRU 模块，计算高效，易于即插即用。

### 算法流程（文字说明）
1. 输入三维医学图像 I。
2. 通过冻结的 VFM 编码器提取多尺度特征。
3. 解码器生成粗分割 logits。
4. MoCE 模块基于解码器特征和可学习提示估计 FP/FN 误差图。
5. Causal Corrector 利用误差图对粗 logits 进行加性修正，得到最终输出。
6. 整个循环模拟贝叶斯预测编码中的“感觉状态→内部状态→行为→感觉状态”闭环。

## 3. 实验设计

### 数据集与场景
- **BraTS2025 Pre-only**：仅含术前 MRI（T1, T1ce, T2, FLAIR），标注 Enhancing Tumor (ET), Tumor Core (TC), Whole Tumor (WT)。1251 训练 + 219 验证。内部按 70%/10%/20% 划分。
- **BraTS2025 Pre+Post**：含术前和术后数据，标注新增 Resection Cavity (RC)，共 2818 训练 / 407 验证。同样按 70%/10%/20% 划分内部测试。
- **BUSI**：乳腺超声图像（647 张），二值分割（良性/恶性）。80% 训练 / 20% 验证。

### Benchmark 与对比方法
- BraTS2025 上对比：UNETR、SwinUNETR、SegResNet、SegMamba（均为三维医学分割 SOTA）。
- BUSI 上对比：U-Net、Att-Unet、U-Net++、U-NeXt、Rolling-UNet、U-Mamba、U-KAN。
- 此外，在 BraTS2025 在线验证平台提交结果，与官方榜单方法对比。

## 4. 资源与算力

论文中**未明确说明使用的 GPU 型号、数量**，仅报告了训练配置：BraTS2025 上使用 SGD 优化器，学习率 0.01，多项式衰减，训练 1000 epoch；BUSI 上使用 Adam，学习率 1e-4，余弦退火，训练 400 epoch。未提及分布式训练或具体硬件信息。因此，算力资源细节未公开。

## 5. 实验数量与充分性

论文进行了多组实验，包括：
- 两个 BraTS2025 配置（Pre-only 和 Pre+Post）的完整对比实验。
- 在 BraTS2025 在线验证平台上的标准/病灶级 DSC 评估。
- 消融实验：移除 PCRU 模块后性能下降，验证其有效性。
- 迁移实验：将 PCRU 集成到 U-KAN 中在 BUSI 上测试，展示跨架构泛化能力。
- 可视化对比（多个案例）。
- 所有实验均报告 DSC 和 HD95（BraTS）或 IoU 和 F1（BUSI）。

实验覆盖了 3D 和 2D 医疗分割任务，对比了多种 SOTA 方法，消融和迁移实验设计合理。在线验证进一步增强了可靠性。总体而言，实验较充分、客观，公平性方面所有方法均沿用原作者设置，CausalBridgeNet 使用相同的骨干（冻结）而未额外调整其他方法。

## 6. 主要结论与发现

- CausalBridgeNet 在 BraTS2025 的所有肿瘤亚区上均超越现有 SOTA（如 SegMamba），尤其在增强肿瘤（ET）和切除腔（RC）上提升显著（DSC 提升 4-6 个点）。
- 使用 CLIP 或 SAM2 作为冻结骨干均能取得优异性能，表明方法不依赖特定 VFM。
- 消融实验确认 PCRU 模块带来一致性增益。
- 迁移到 U-KAN 在 BUSI 上 IoU 提升约 5.5%，F1 提升约 4.5%，证明其架构无关性和跨模态有效性。
- 可视化显示 CausalBridgeNet 能纠正假阳性/假阴性错误，保持边界清晰和结构一致性。

## 7. 优点

- **创新性**：将因果推断与预测编码理论结合，提出显式误差估计和闭环修正机制，视角新颖。
- **即插即用**：PCRU 是轻量、可解释的独立模块，可集成到任意分割框架，无需修改基础模型。
- **高效且防止灾难性遗忘**：冻结骨干，仅训练少量适配器和专家模块，计算开销小。
- **强泛化能力**：在多个数据集、多种架构（U-Net 类、U-KAN）上均有效。
- **可解释性**：FP/FN 误差图可提供模型错误的直观解释，有助于临床信任。

## 8. 不足与局限

- **实验覆盖不够全面**：仅局限于脑肿瘤和乳腺超声两个领域，未在更多医学任务（如肝脏、肺部、视网膜等）上验证泛化性。
- **资源细节缺失**：未报告 GPU 型号、数量和训练时间，难以评估实际可复现性。
- **消融实验深度有限**：仅比较了有无 PCRU，未深入分析不同专家数量 K、提示维度、路由机制等的影响。
- **偏差风险**：BraTS2025 数据集来自特定比赛，可能具有某些分布特性，其他非竞赛数据集上的表现未知。
- **应用限制**：当前仅支持 3D 和 2D 医学分割，未扩展到其他视觉任务（分类、检测）；因果校正依赖于粗预测的质量，若粗预测完全错误可能难以修正。
- **未讨论推理速度**：尽管说轻量，但未比较推理时延增量，实际部署效率不明。

（完）
