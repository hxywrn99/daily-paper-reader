---
title: "Revisiting Network Inertia: Dynamic Inertia Inhibition Coupled Multidimensional Periodicity for Infrared and Visible Image Fusion"
title_zh: 重新审视网络惯性：动态惯性抑制耦合多维周期性的红外与可见光图像融合
authors: "Yufeng Chen, Yuan Sun, Hao Pan, Xujian Zhao, Jian Dai, Zhenwen Ren, Xingfeng Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37309/41271"
tags: ["query:image-fusion"]
score: 9.0
evidence: 红外与可见光图像融合
tldr: 针对深度网络在红外与可见光图像融合中出现的网络懒惰现象（权重更新变慢），提出轻量级融合方法AIDFusion，通过动态惯性抑制学习策略逐步调节多级预测的协作学习，充分释放网络各层潜力。实验表明该方法在融合质量和效率上均优于现有方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 红外与可见光融合中网络懒惰现象限制特征表征潜力。
method: 提出AIDFusion，包含动态惯性抑制学习策略，逐步调节多级预测协作。
result: 在多个数据集上取得更好的融合质量和更低的计算开销。
conclusion: AIDFusion有效克服网络懒惰，提升融合性能。
---

## Abstract
Infrared and visible image fusion (IVIF) technology has become a frontier of great interest due to the ability to integrate information from multiple sources. However, the progressive slowdown of weight updates in deep networks (i.e., “network laziness” phenomenon), makes existing methods far from realizing the full characterization potential. To this end, we propose a lightweight fusion method for IVIF, Anti-Inert Dynamic Fusion (AIDFusion), to fully utilize the potential of the network at all levels. Specifically, by progressively regulating the collaborative Learning process of multi-level prediction in the network, Dynamic Inertia Inhibition Learning Strategy (DIILS) is proposed to adaptively and efficiently inhibit inertia accumulation. Subsequently, to deeply explore the representation potential while breaking through the performance threshold, lightweight Multi-dimensional modulation fusion module (MMFM) is specifically proposed to capture comprehensive multi-view and multi-scale features efficiently. Finally, considering the semantic bias between the prediction maps of DIILS and the fusion feature of MMFM, Fourier Analysis Convolution (FAConv) is designed in feature recovery as a bridge between prediction and fusion to accomplish the implicit periodic modeling. Based on the above study, extensive experiments on three public IVIF datasets demonstrate the dual advantages of AIDFusion in terms of fusion performance and computational overhead compared to state-of-the-art baseline methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **研究问题**：红外与可见光图像融合（IVIF）中，深度网络在训练中后期会出现“网络懒惰”（network laziness）现象——权重更新幅度急剧减小，导致网络潜力无法充分发挥。现有方法多通过堆叠更复杂的模型来提升性能，但加重了计算负担和训练停滞。
- **核心含义**：作者提出一种新的视角——从“网络利用率”出发，通过轻量级架构配合精心设计的训练策略和模块，在保持低计算开销的同时充分释放网络各层潜力，突破性能瓶颈。

## 2. 方法论：核心思想与关键技术细节
### 核心思想
- 提出 **AIDFusion**（Anti-Inert Dynamic Fusion），由三个关键组件组成：
  - **DIILS**：动态惯性抑制学习策略，通过渐进式分层优化和自适应协作损失防止中后期训练停滞。
  - **MMFM**：轻量级多维调制融合模块，高效捕获多视角、多尺度特征。
  - **FAConv**：傅里叶分析卷积，在特征恢复中嵌入周期性先验，弥合融合特征与预测图之间的语义偏差。

### 关键技术细节
1. **动态惯性抑制学习策略（DIILS）**
   - **渐进分层优化（PHO）**：根据训练epoch逐步添加预测层的监督信号。前半阶段正常训练，后半阶段每增加 T/10 epoch 就多加入一个预测分支的损失，最终利用所有层的输出进行监督。
   - **自适应协作融合损失（LACF）**：对不同层的预测损失进行自适应加权，权重基于各层损失值动态调整。损失函数包含相关性损失、梯度损失、强度损失，并使用反向传播促进梯度活跃。
2. **多维调制融合模块（MMFM）**
   - **多视角稀疏注意力（MPSA）**：从 HW、HC、WC 三个视角提取特征，通过信息熵计算稀疏权重，融合关键特征。
   - **多尺度循环Transformer（MSRT）**：沿通道维度分割特征，对不同尺度进行自注意力计算，并复用特征实现多尺度捕获，同时将注意力计算复杂度从 [HW×HW] 降低到 [C/4×C/4]。
3. **傅里叶分析卷积（FAConv）**
   - 将输入分别映射到 C/4 和 C/2 通道，对 C/4 部分使用余弦和正弦函数进行周期建模，C/2 部分使用 GELU 激活，最后拼接输出。该卷积替代传统卷积用于解码器，提供明确的先验学习方向，加速特征恢复。

### 整体框架流程
- 输入VI和IR，经过ResNet编码器得到各层特征 → 将编码特征、上一解码层输出、上一预测输出送入MMFM得到融合特征 → 经解码器（含FAConv）恢复 → 经预测模块得到最终融合图像 → 各层预测图与下采样原图计算LACF损失，同时重构VI、IR计算重构损失。

## 3. 实验设计
- **数据集**：MSRS、M3FD、FMB 三个公开IVIF数据集。训练在MSRS上进行，测试在三个数据集上。
- **评价指标**：EN（熵）、SD（标准差）、CC（相关系数）、SCD（结构相似度差异）、SSIM、MS-SSIM。
- **对比方法**：10种基线方法：SwinFusion、LRRNet、CDDFuse、DATFuse、DSFusion、CrossFuse、MRFS、EMMA、SAGE、GIFNet。均使用作者发布的预训练权重。

## 4. 资源与算力
- **GPU**：RTX 4090 单卡。
- **训练设置**：SGD优化器，初始学习率0.001，batch size=12，图像尺寸352×352，训练100个epoch。
- 未说明使用了多少张GPU或具体训练时长。

## 5. 实验数量与充分性
- **定量对比**：在MSRS上报告了6个指标及Params、GFLOPs、Memory；在M3FD和FMB上分别报告了6个指标。
- **定性对比**：在MSRS上展示了一组融合图像视觉效果对比。
- **消融实验**：系统性地去除了DIILS、MMFM、FAConv三个组件的单个、两两、全部组合，共7组+全模型=8组，定量结果（EN、SD、SCD、SSIM）和一组可视化。
- **泛化性实验**：直接在M3FD和FMB上测试MSRS训练的模型，验证跨数据集泛化能力。
- **充分性评价**：实验覆盖了多种数据集、多种指标、足量对比方法、全面的消融和泛化测试，设计较充分、客观、公平。

## 6. 主要结论与发现
- AIDFusion在MSRS上取得了CC、SCD、MS-SSIM最优，SSIM次优，且参数量、FLOPs、内存占用均处于较低水平（GFLOPs排名1/11）。
- 在M3FD和FMB上同样在CC、SCD、MS-SSIM上最优，EN在M3FD最优、FMB次优，整体优于多数方法。
- 定性结果显示AIDFusion能同时处理三种关键情况（双模态信息丰富、可见光缺失红外补充、红外噪声可见光丰富），而其他方法无法兼顾。
- 消融实验证明每个组件不可或缺，特别是同时去除DIILS和MMFM时SSIM从1.002骤降至0.701。
- 轻量设计（0.247M参数、1.85 GFLOPs）在保持高性能的同时具有显著的计算效率优势。

## 7. 优点
- **创新视角**：首次明确关注并定义IVIF中的“网络懒惰”问题，从利用率角度而非单纯堆叠复杂度解决。
- **方法完整性**：训练策略（DIILS）、特征融合（MMFM）、解码恢复（FAConv）三者环环相扣，相互促进。
- **高效性**：在极低计算开销下达到SOTA或接近SOTA的融合质量，尤其GFLOPs在所有方法中最低。
- **泛化性**：跨数据集测试证明方法具有良好泛化能力。
- **消融充分**：系统验证了每个组件贡献，并揭示了组件间耦合作用的重要性。

## 8. 不足与局限
- **训练资源信息不完整**：未报告GPU数量、训练总时长、功耗等，难以完全复现和比较训练成本。
- **数据集规模较小**：MSRS、M3FD、FMB均为中等规模遥感/监控场景数据集，在更大规模、更多样化场景（如城市场景、低光照、恶劣天气）上的表现未知。
- **评价指标有限**：仅使用了6个传统非参考指标，缺少与下游任务（如目标检测、语义分割）结合的评估，也缺乏用户感知评价。
- **模型泛化风险**：虽然跨数据集测试良好，但训练仅在MSRS上进行，若目标域与MSRS差异巨大（如航空影像、医学图像），可能仍需微调。
- **未讨论失败案例**：文中未给出任何融合效果不佳的样例分析，可能过于乐观。
- **与最新大模型方法的对比**：虽对比了SAGE、GIFNet等与视觉语言模型结合的方法，但未全面对比最近采用Diffusion、Foundation Model的方法，时效性受限。

（完）
