---
title: Frequency-Aligned Cross-Modal Learning with Top-K Wavelet Fusion and Dynamic Expert Routing for Enhanced Retinal Disease Diagnosis
title_zh: 基于Top-K小波融合和动态专家路由的频率对齐跨模态学习用于增强视网膜疾病诊断
authors: "Yuxin Lin, Haoran Li, Haoyu Cao, Yongting Hu, Qihao Xu, Chengliang Liu, Xiaoling Luo, Zhihao Wu, Yong Xu, Wei Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37635/41597"
tags: ["query:image-fusion"]
score: 7.0
evidence: 多模态融合用于视网膜疾病诊断
tldr: 针对眼底彩照与OCT多模态融合中缺乏自适应调制的问题，提出频率对齐的跨模态学习方法，包含Top-K小波融合和动态专家路由，根据临床场景动态调节各模态贡献。在视网膜疾病诊断任务上取得更优性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多模态融合方法缺乏自适应调制，无法应对不同临床场景。
method: 提出频率对齐跨模态学习和Top-K小波融合模块，以及动态专家路由机制。
result: 在视网膜疾病诊断中提升了分类准确率。
conclusion: 该自适应融合框架有效提高了多模态诊断的鲁棒性和准确性。
---

## Abstract
Multimodal fusion of color fundus photography (CFP) and optical coherence tomography (OCT) B-scan images has demonstrated superior diagnostic potential for retinal diseases compared to single-modality approaches. However, existing fusion paradigms - whether through naive concatenation or attention mechanisms - treat cross-modal interactions indiscriminately, lacking adaptive modulation of modality-specific contributions under varying clinical scenarios. We propose an adaptive fusion framework that dynamically routes and refines multimodal signals for enhancing disease recognition. The framework comprises two key components: 1) Dynamic Cross-Modal Expert Routing (CMER), which selectively activates convolutional neural network (CNN) experts from one modality based on contextual guidance from the other, ensuring only the most relevant feature extractors contribute to fusion; and 2) Top-K Expert-Guided Wavelet Fusion (TEWF), which performs discrete wavelet transform (DWT) to decompose selected features into low- and high-frequency subbands. Cross-modal attention is then applied specifically to high-frequency components, where lesion-specific microstructures reside, enabling frequency-aware fusion. Finally, inverse DWT (IDWT) reconstructs the fused representation, weighted by CMER-derived importance scores to amplify informative modality cues while suppressing redundancy. Experimental validation on two multimodal retinal datasets demonstrates that our method achieves state-of-the-art performance, outperforming existing fusion strategies by significant margins in disease classification accuracy and robustness.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题背景**：视网膜疾病（如糖尿病视网膜病变、青光眼、AMD等）早期检测至关重要。眼底彩照（CFP）提供高分辨率表面血管信息，OCT提供横断面微观结构，两者互补。然而现有多模态融合方法（如简单拼接、统一注意力）**无差别地处理跨模态交互**，缺乏根据病变特征或临床场景**自适应调制各模态贡献**的能力，导致融合效果受限。
- **核心目标**：设计一个**自适应融合框架**，能够动态路由和细化多模态信号，提升疾病识别准确性与鲁棒性。

## 2. 提出的方法论

### 核心思想
通过**两阶段自适应融合**实现模态贡献的动态调制：
- **第一段**：动态跨模态专家路由（CMER），根据一个模态的上下文信号选择性激活另一模态的CNN专家，确保只提取最相关的特征。
- **第二段**：Top-K专家引导的小波融合（TEWF），利用离散小波变换（DWT）分解特征为低频（结构）和高频（病变细节）子带，仅在高频子带上应用交叉注意力融合，再通过逆DWT重建加权融合表示。

### 关键技术细节
- **CMER模块**（图2b）：
  - 以CFP到OCT方向为例：CFP特征`F_Csi`经过路由网络（CNN块+自适应平均池化+线性投影+Softmax）得到路由分数`R_sOsi`。
  - 根据分数排名，从OCT专家池中选出Top-K个专家，每个专家`CE_Osi,k`仅当对应分数排前K时被激活。
  - 提取的特征`Fe_Osi,k`及其重要性权重`w_Osi,k`（即对应分数）传递至TEWF模块。
  - 双向对称设计（OCT也引导CFP专家选择）。
  - 引入**负载均衡损失**`L_load`促进专家均匀使用。

- **TEWF模块**（图2c）：
  - 对CFP特征和选中的OCT特征分别进行DWT分解，得到LL（低频）、LH、HL、HH（高频）子带。
  - 仅对高频子带进行交叉注意力融合：双向注意力（OCT→CFP和CFP→OCT），结果取平均，并加残差。
  - 融合后的高频子带与原始LL子带通过IDWT重建得到`˜Fe_Osi,k`。
  - 再经过门控注意力`G`和空间注意力`Sp`增强。
  - 最终输出为所有K个专家的加权和：`˜F_Osi = Σ w_Osi,k × ˆFe_Osi,k`。

- **分类阶段**：将最后一阶段的融合输出与原始特征做残差连接，经自适应平均池化、展平、拼接后由线性层分类。
- **损失函数**：分类交叉熵损失`L_task` + 自适应加权的负载均衡损失`L_load`，权值根据`L_load`大小动态调整。

## 3. 实验设计

### 数据集
- **MMAD（多模态AMD数据集）**：1094张CFP、1289张OCT图像，四分类（正常、干性AMD、PCV、湿性AMD），采用[Wang et al. 2022]的划分协议。
- **GAMMA（青光眼数据集）**：100对CFP-OCT，三级青光眼严重等级，采用80%/20%训练/测试划分，五折交叉验证。

### 对比方法
- **单模态/基础方法**：LateFusion, Yoo, MM-CNN, MM-CNN da, smartDSP, MVCNN, MVCINN, ClipResnet, EYEMOST+T/C, B-EF, M2LC, MCDO, DE, TMC, CVSA等。
- **SOTA方法**：MM-CNN（MMAD数据集当前最优），EYEMOST（GAMMA数据集当前最优），以及GAMMA挑战赛优胜方法EyeStar、SmartDSP。

### 评估指标
- 准确率（Acc）、F1分数、精确率（Pre）、特异性（Spe）、敏感性（Sen）、Cohen’s Kappa。

### 实现细节
- 骨干网络：ResNet-18，ImageNet预训练。
- CMER：专家总数N=6，路由专家数K=2。
- 小波：Haar小波。
- 交叉注意力：8头，4层，嵌入维度1024。

## 4. 资源与算力

文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及采用ResNet-18作为骨干，在ImageNet上预训练，实验在标准深度学习框架下进行。

## 5. 实验数量与充分性

- **主实验**：在两个数据集（MMAD和GAMMA）上分别进行了全面的性能对比，表中列出了多个指标（表1、表2、表3）。
- **消融实验**：表4展示了移除CMER、移除小波变换（w/o WT）、移除交叉注意力（w/o CA）、同时移除CMER和TEWF后的性能下降，验证每个模块的贡献。
- **超参数分析**：图3分析了专家总数N、路由专家数K的影响，找出最优配置（N=6, K=2）。
- **公平性**：采用与其他方法相同的训练/测试划分，五折交叉验证，指标全面。对比的方法广泛且均为近年SOTA。
- **充分性评价**：实验设计较为充分，覆盖了多数据集、多指标、多对比方法，消融和超参数分析也较完整。但数据集规模较小（MMAD约1000+对，GAMMA仅100对），泛化性验证有限。

## 6. 主要结论与发现

- 提出的方法在**MMAD**上达到最优：F1=0.925, Acc=0.881, Pre=0.911, Spe=0.959，超越所有对比方法，包括强基线MM-CNN da和CLIP-based ClipResnet。
- 在**GAMMA**上取得最高Acc=0.870和Kappa=0.841，优于EYEMOST等SOTA方法。
- 消融实验表明，**CMER和TEWF均为核心组件**，同时移除时性能下降显著（F1下降3.1%，Acc下降4.9%）。
- 最佳超参数为总专家数N=6、路由专家K=2。
- 方法可推广至其他需要互补模态信息的医学诊断任务。

## 7. 优点

- **创新性**：首次将**动态专家路由**与**频率对齐小波融合**结合，实现自适应模态贡献调制，克服了传统融合无差别处理的局限。
- **设计合理性**：高频子带聚焦病变微观结构，双向交叉注意力实现子区域级别对齐，专家路由引入负载均衡防止过拟合。
- **实验全面性**：在两个不同疾病、不同规模的数据集上验证，对比方法涵盖经典和最新SOTA，消融实验设计清晰。
- **结果优势显著**：在多个指标上均超过SOTA，尤其对难分类类别（PCV、湿性AMD）提升明显。

## 8. 不足与局限

- **计算资源未披露**：未提供训练时长、GPU型号等，影响可复现性评估。
- **数据规模较小**：GAMMA仅100对样本，统计显著性可能不足；MMAD也属中小规模数据集，在更大数据集（如数十万对）上表现未知。
- **泛化性待验证**：仅针对视网膜疾病（AMD、青光眼），未在其他多模态医学任务（如肺癌CT-PET、脑MRI-PET）上实验，可推广性存疑。
- **超参数敏感性**：N=6, K=2为最优，但未系统分析在其他数据集上是否需要调整，可能存在过调。
- **临床实用性未涉及**：未讨论模型在真实筛查场景中的部署、推理速度、与医生协作等实际问题。
- **仅使用两个模态**：方法可扩展到更多模态，但本文未展示。

（完）
