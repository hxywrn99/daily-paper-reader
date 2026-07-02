---
title: Text-Guided Channel Perturbation and Pre-Trained Knowledge Integration for Unified Multi-Modality Image Fusion
title_zh: 文本引导的通道扰动与预训练知识集成用于统一多模态图像融合
authors: "Xilai Li, Xiaosong Li, Weijun Jiang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37581/41543"
tags: ["query:image-fusion"]
score: 9.0
evidence: 统一多模态图像融合框架
tldr: 针对统一多模态图像融合中模态差异导致的梯度冲突和泛化性不足问题，提出UP-Fusion框架，通过通道扰动抑制冗余信息，并集成预训练知识增强关键特征。实验表明该方法在多模态融合任务上取得优异性能，提升了模型的泛化能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 统一多模态图像融合中模态差异大导致梯度冲突，限制性能。
method: 提出基于通道扰动和预训练知识集成的UP-Fusion框架，包括语义感知通道扰动模块。
result: 在多个融合任务上实现更优的融合质量和泛化性能。
conclusion: UP-Fusion有效解决了梯度冲突，提高了统一融合模型的泛化能力。
---

## Abstract
Multi-modality image fusion enhances scene perception by combining complementary information. Unified models aim to share parameters across modalities for multi-modality image fusion, but large modality differences often cause gradient conflicts, limiting performance. Some methods introduce modality-specific encoders to enhance feature perception and improve fusion quality. However, this strategy reduces generalisation across different fusion tasks. To overcome this limitation, we propose a unified multi-modality image fusion framework based on channel perturbation and pre-trained knowledge integration (UP-Fusion). To suppress redundant modal information and emphasize key features, we propose the Semantic-Aware Channel Pruning Module (SCPM), which leverages the semantic perception capability of a pre-trained model to filter and enhance multi-modality feature channels. Furthermore, we proposed the Geometric Affine Modulation Module (GAM), which uses original modal features to apply affine transformations on initial fusion features to maintain the feature encoder modal discriminability. Finally, we apply a Text-Guided Channel Perturbation Module (TCPM) during decoding to reshape the channel distribution, reducing the dependence on modality-specific channels. Extensive experiments demonstrate that the proposed algorithm outperforms existing methods on both multi-modality image fusion and downstream tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
多模态图像融合（MMIF）旨在整合不同成像模态的互补信息，增强场景感知。现有的统一模型（如单自动编码器结构）共享参数，具有跨任务泛化能力，但因模态差异大导致梯度冲突，融合质量受限。而采用多自动编码器（模态专用编码器）虽能提升融合质量，但泛化性差，当迁移到未见模态组合或不同分布的数据集时性能严重下降。论文旨在平衡泛化能力与融合质量，提出基于文本引导的通道扰动与预训练知识集成的统一框架（UP-Fusion）。

## 2. 方法论
### 核心思想
通过三阶段模块设计：编码阶段抑制冗余、保留显著通道；特征交互阶段用几何仿射调制保持模态可辨别性；解码阶段用文本引导通道扰动减少模态依赖，实现统一参数跨任务泛化。

### 关键技术细节
- **语义感知通道剪枝模块（SCPM）**：
  - 利用通道注意力计算重要性权重 ωC，同时用预训练 ConvNeXt 提取语义特征，经线性映射得语义权重 ωS，加权融合得最终通道权重 ωF = ωC + α·σ(ωS)（α 为可学习参数）。
  - 采用 Top-k 策略保留前 70% 显著通道，再通过 1×1 卷积扩展回原维度。
- **几何仿射调制模块（GAM）**：
  - 对原始多模态特征做全局平均池化，经两层 1×1 卷积映射得到缩放因子 γ 和偏移量 β。
  - 对 SCPM 输出的融合特征进行仿射调制：F_M^O = F_M^use · (1 + γ) + β。
- **文本引导通道扰动模块（TCPM）**：
  - 先将多模态特征沿通道拼接，用通道注意力评估重要性，Top-k 保留前 50% 通道，再用 1×1 卷积扩至两倍。
  - 利用 CLIP 编码的文本特征产生引导权重，生成通道重排索引，使融合特征进行语义相关重排。
  - 用自注意力机制（重排特征为 Q，原始特征为 K/V）进一步交互，最后通过前馈网络。
- **网络架构**：基于 Transformer 块，4 层编码器（注意力头数递增 [1,2,4,8]）和 4 层解码器，每层集成 TCPM。
- **损失函数**：梯度损失 L_grad 和 L1 损失 L_l1 之和。

## 3. 实验设计
### 数据集与场景
- **红外-可见光融合**：MSRS、LLVIP、M3FD 三个主流数据集。
- **医学图像融合**：哈佛医学院的 CT-MRI、PET-MRI、SPECT-MRI，以及 GFP-PC 荧光/相差图像。
- **下游任务**：语义分割（MSRS 上用 BANet）、目标检测（M3FD 上用 YOLOv7）。

### Benchmark 与对比方法
- **IVIF 专用方法**：CrossFuse、SAGE、TDFusion。
- **MEIF 专用方法**：ALMFNet、MMIF-INet、GeSeNet。
- **通用 MMIF 方法**：GIFNet、EMMA、TIMFusion、CoCoNet。
- **评价指标**：Q_NCIE、Q_P、VIF、SSIM、Q_AB/F。

## 4. 资源与算力
文中明确说明：所有实验在 PyTorch 2.1.1 环境下，配备 4 块 GeForce RTX 3090 GPU 的服务器上运行。训练 100 个 epoch，批大小 2，图像随机裁剪至 192×192，初始学习率 0.0001 余弦衰减至 0.00001，使用 Adam 优化器。

## 5. 实验数量与充分性
- **定量对比**：在红外-可见光（3 个数据集）和医学图像（4 个场景）上进行了 5 个指标的全面比较，涵盖 10 种 SOTA 方法。
- **定性对比**：给出多组视觉结果图，展示了跨任务泛化效果。
- **消融实验**：对 SCPM、GAM、TCPM、通道注意力（CA）、预训练模型（ConvNeXt）分别进行消融，在医学图像上给出了定量表格（表3）和可视化对比（图5）。
- **下游任务实验**：语义分割（表4）和目标检测（表5）各一组。
- **充分性判断**：实验覆盖多模态、多任务、多评价维度，消融全面，对比方法数量充分，结论客观公平。不足在于消融实验仅基于医学图像任务，未覆盖红外-可见光场景，但整体仍具有说服力。

## 6. 主要结论与发现
- UP-Fusion 在红外-可见光融合和医学图像融合任务中，大部分指标达到最优或次优，尤其在跨任务泛化上显著优于现有统一模型和模态专用模型。
- 在医学图像上，尽管仅用 LLVIP 数据集训练，仍能超越医学专用融合方法。
- 下游任务中，UP-Fusion 在语义分割和目标检测上均取得最佳 mIoU 和 mAP。
- 各模块（SCPM、GAM、TCPM、CA、预训练模型）均对性能有实质性贡献，移除后指标和视觉效果下降明显。

## 7. 优点
- **创新性**：首次将文本引导通道扰动引入统一多模态融合，结合预训练知识（ConvNeXt、CLIP）提升语义感知，有效解决梯度冲突和泛化性问题。
- **模块设计合理**：SCPM 筛选显著通道，GAM 保持模态可辨别性，TCPM 消除模态标记，三者协同实现统一参数下的高质量融合。
- **实验充分**：跨任务、跨数据集、多指标、多对比方法，并验证了下游任务可用性。
- **代码开源**：提供 GitHub 链接，可复现。

## 8. 不足与局限
- **计算资源依赖**：使用了 ConvNeXt 和 CLIP 两个大型预训练模型，训练需要 4 块 3090 GPU，对算力要求较高，可能限制实际部署。
- **消融范围局限**：消融实验仅基于医学图像任务，未在红外-可见光任务上验证各模块的普适性。
- **模态覆盖**：仅测试了红外-可见光和四种医学模态，未涉及更多模态（如多光谱、SAR 等）或真实低照度/恶劣天气场景。
- **训练数据单一**：仅用 LLVIP 单个红外-可见光数据集训练，虽然在医学上泛化良好，但未分析训练数据多样性对泛化边界的影响。
- **实时性未讨论**：未报告模型推理速度或参数量，可能不适用于实时应用。

（完）
