---
title: "MdaIF: Robust One-Stop Multi-Degradation-Aware Image Fusion with Language-Driven Semantics"
title_zh: MdaIF：语言驱动的鲁棒一站式多退化感知图像融合
authors: "Jing Li, Yifan Wang, Jiafeng Yan, Renlong Zhang, Bin Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37548/41510"
tags: ["query:image-fusion"]
score: 9.0
evidence: 语言驱动的多模态图像融合
tldr: 针对现有红外与可见光图像融合方法忽略退化场景、网络架构固定缺乏适应性的问题，提出了MdaIF框架。该框架引入大语言模型驱动语义理解，并采用混合专家系统处理雾、雨、雪等不同退化场景。实验表明，MdaIF在多种恶劣天气条件下都能生成鲁棒的融合图像，显著提升了实际环境中的感知能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法未考虑恶劣天气下的图像退化，且网络架构无法适应不同退化场景。
method: 提出MdaIF，利用大语言模型驱动语义，结合混合专家系统（MoE）一站式处理多种退化。
result: 在包含雾、雨、雪的退化场景中，MdaIF生成的融合图像质量鲁棒，优于对比方法。
conclusion: 语言驱动的退化感知融合有效提升了多模态图像融合在复杂环境下的适应性和鲁棒性。
---

## Abstract
Infrared and visible image fusion aims to integrate complementary multi-modal information into a single fused result. However, existing methods 1) fail to account for the degradation visible images under adverse weather conditions, thereby compromising fusion performance; and 2) rely on fixed network architectures, limiting their adaptability to diverse degradation scenarios. To address these issues, we propose a one-stop degradation-aware image fusion framework for multi-degradation scenarios driven by a large language model (MdaIF). Given the distinct scattering characteristics of different degradation scenarios (e.g., haze, rain, and snow) in atmospheric transmission, a mixture-of-experts (MoE) system is introduced to tackle image fusion across multiple degradation scenarios. To adaptively extract diverse weather-aware degradation knowledge and scene feature representations, collectively referred to as the semantic prior, we employ a pre-trained vision-language model (VLM) in our framework. Guided by the semantic prior, we propose degradation-aware channel attention module (DCAM), which employ degradation prototype decomposition to facilitate multi-modal feature interaction in channel domain. In addition, to achieve effective expert routing, the semantic prior and channel-domain modulated features are utilized to guide the MoE, enabling robust image fusion in complex degradation scenarios. Extensive experiments validate the effectiveness of our MdaIF, demonstrating superior performance over SOTA methods.

---

## 论文详细总结（自动生成）

好的，以下是根据给定论文内容生成的结构化、深入、客观的中文总结。

---

### 论文核心问题与整体含义（研究动机和背景）

- **核心问题**：红外与可见光图像融合（IVF）在恶劣天气（如雾、雨、雪）下面临严峻挑战。现有方法普遍存在两个关键不足：
  1. 忽视了可见光图像在恶劣天气下的退化（如散射导致纹理丢失），直接融合退化图像会严重损害融合性能；
  2. 采用固定的网络架构，无法适应不同退化场景（雾、雨、雪）中差异巨大的大气散射模型，泛化能力差。
- **研究动机**：为了在无需人工指定退化类型的前提下，自适应地处理多种天气退化，实现一步到位的鲁棒融合。

### 论文提出的方法论：核心思想与关键技术细节

- **核心思想**：提出一种**语言驱动的多退化感知一站式融合框架（MdaIF）**，利用预训练视觉语言模型（VLM）自动提取语义先验（包括天气信息和场景信息），指导混合专家系统（MoE）进行自适应专家路由和通道调制，从而同时实现退化感知和多模态特征融合。
- **关键技术细节**：
  1. **退化感知语义先验提取器（DSPE）**：
     - 采用BLIP-2 OPT 2.7B作为VLM，以视觉问答（VQA）范式输入退化的可见光图像和一个开放性问题“详细描述图像，包括天气类型和物体”，提取最后一层隐藏特征作为原始语义先验 \( S_{org} \)。
     - 通过MLP和层归一化降维，再经自注意力机制自适应重加权，得到对齐后的语义先验 \( S_{prior} \)，其包含天气相关信息 \( S_{weather} \) 和场景相关信息 \( S_{scene} \)。
  2. **退化感知通道注意力模块（DCAM）**：
     - 将 \( S_{prior} \) 通过平均池化、MLP和Sigmoid生成一个K维得分向量 \( s_K \)，表示对K个可学习退化原型 \( W_{proto} \in \mathbb{R}^{K \times C} \) 的加权组合。
     - 每个原型向量 \( k_i \) 编码了该原型对不同通道的激活强度，最终得到通道权重 \( w_c = \sigma(s_K \cdot W_{proto}) \)。
     - 对编码器输出的特征 \( F_{in} \) 进行通道调制：\( F_{dcam} = \text{LayerNorm}(F_{in}) \odot w_c + F_{in} \)（残差形式）。
  3. **退化感知混合专家系统（DMoE）**：
     - 利用语义先验 \( S_{prior} \) 与DCAM调制后的特征 \( F_{dcam} \) 通过交叉注意力机制交互，生成专家激活权重 \( w \in \mathbb{R}^{1 \times N} \)（N=5个专家）。
     - 每个专家结构相同：3×3卷积 + 1×1卷积。最终输出为各专家输出的加权和：\( F_{dmoe} = \phi(\text{BatchNorm}(\sum w_i E_i)) \)。
  4. **解码与损失函数**：\( F_{dmoe} \) 经CNN解码器重建融合图像。损失函数包括积分损失（保持像素和梯度最大值）和颜色一致性损失（在YCbCr空间对齐色度）。

### 实验设计

- **数据集与场景**：
  - 使用两个经典IVF基准数据集：**MSRS**（1524对，480×640）和**FMB**（1500对，600×800）。
  - 对每个数据集的可见光图像分别合成三种退化（雾、雨、雪），共生成9,072对图像（训练7,149对，测试1,923对），三种退化类型均衡分布。
- **基准对比方法**：
  - **策略I**：针对每种退化使用专用恢复模型（去雾：DCMPNet/CasDyF-Net/DehazeFormer；去雨：DRSformer/IDT/NeRD-Rain；去雪：HDCWNet/InvDSNet/SnowFormer）+ 五种SOTA融合方法（SegMiF, SwinFuse, TarDAL, MetaFusion, SAGE）级联。
  - **策略II**：使用一体化恢复模型（DTMR, MPMF-Net, WGWS-Net）+ 同上融合方法。
  - **本文方法**：直接处理原始退化图像，无需独立恢复步骤。
- **评估指标**：PSNR、SSIM（越高越好）、NABF（越低越好）、MI（越高越好）。

### 资源与算力

- **硬件**：单张NVIDIA GeForce RTX 4090 GPU（24GB内存），AMD Ryzen 9 7950X CPU。
- **训练配置**：初始学习率 \( 6 \times 10^{-5} \)，批大小15，训练1000个epoch，使用warm-up + polynomial decay学习率调度，Adam优化器。
- **说明**：论文未明确给出总训练时长或GPU数量，仅描述了单GPU实验环境。

### 实验数量与充分性

- **定量实验**：在MSRS和FMB两个数据集上的三种退化场景下，对比了5种融合方法×多种恢复模型（共计约30余种组合），并报告了完整指标（表1为MSRS策略I的部分结果，其余详见扩展版本）。
- **定性实验**：展示了6组代表性可视化对比（图6），涵盖不同数据集和天气。
- **消融实验**：对DCAM和DMoE两个模块进行去除/组合试验，验证各自有效性（表2）。
- **充分性评价**：
  - **充分**：覆盖了多种退化、多种强基线、两个数据集，实验设计较为全面；定量和定性结果相互印证。
  - **公平性**：对比方法均为级联SOTA恢复+融合模型，设置合理；但未直接与同样使用语言引导的MMAIF、OmniFuse等方法在同一设定下对比（尽管文中提及它们，但未在实验部分给出数值比较）。
  - **局限**：消融实验仅在MSRS数据集上报告平均指标，未按天气分解；未在真实退化图像上验证。

### 论文的主要结论与发现

1. **MdaIF在多个退化场景下的PSNR、SSIM、MI指标上全面超过所有级联基线方法**，尤其在MI（互信息）指标上提升显著，说明融合了更多互补信息。
2. **DCAM和DMoE二者缺一不可**：单独使用DMoE会导致专家崩溃（仅一个专家活跃），单独使用DCAM性能下降；联合使用才能达到最优。
3. **语义先验能有效分解为不同的退化原型**（图5a），且各原型在不同天气下的激活模式呈现差异性和相关性，证明了其退化感知能力。
4. **突破了传统依赖真实退化标签的局限**，实现了自适应、无需人工指定的多退化融合。

### 优点

- **方法创新**：首次将VLM驱动的语义先验与MoE架构结合，同时解决退化感知和专家路由问题；DCAM通过可学习的退化原型进行通道调制，结构巧妙。
- **系统设计**：一站式端到端优化，避免了级联方案中的特征错位和误差累积问题。
- **泛化能力**：仅用一个模型处理三种不同退化（雾、雨、雪），无需切换网络或知道退化类型。
- **实验充分**：在两大基准数据集上进行了详尽的定量和定性比较，验证了有效性。

### 不足与局限

1. **实验缺乏真实退化场景验证**：所有退化均为合成生成，未在实际雾、雨、雪图像上评估，模型对真实世界复杂天气的泛化能力未知。
2. **对比方法不够全面**：未与同为语言引导的MMAIF、OmniFuse、Tex-IF等方法进行直接比较（尽管在引言中提及，但实验部分仅与他们文中的描述性比较不符）。
3. **消融实验粒度不足**：消融实验仅报告了三种天气的平均结果，未分别分析各天气下的贡献，无法判断模块对不同退化类型的鲁棒性差异。
4. **依赖大模型可能带来的开销**：使用了BLIP-2 OPT 2.7B，虽未评估推理速度，但大模型带来的计算和存储成本较高，可能限制在边缘设备部署。
5. **样本局限性**：仅考虑了单一退化条件，未涉及多退化叠加（如雾中带雨）或低光照、沙尘等更复杂场景。
6. **可复现性**：代码已开源，但未报告详细的训练时长、超参数搜索过程，可能影响复现细节。

（完）
