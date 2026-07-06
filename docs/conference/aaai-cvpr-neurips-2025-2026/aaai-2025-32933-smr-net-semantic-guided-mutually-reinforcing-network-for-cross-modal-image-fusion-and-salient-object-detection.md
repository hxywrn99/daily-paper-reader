---
title: "SMR-Net: Semantic-Guided Mutually Reinforcing Network for Cross-Modal Image Fusion and Salient Object Detection"
title_zh: SMR-Net：语义引导的相互增强网络用于跨模态图像融合和显著目标检测
authors: "Guobao Xiao, Xinyu Liu, Zebin Lin, Rui Ming"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32933/35088"
tags: ["query:image-fusion"]
score: 9.0
evidence: 跨模态图像融合与语义引导相互增强
tldr: 该文针对跨模态图像融合与显著目标检测协同不足的问题，提出SMR-Net。核心是语义引导的相互增强机制：渐进式跨模态交互子网络利用卷积和注意力进行局部-全局交互；位平面切片SOD子网络将融合图像作为第三模态。实验表明该方法在融合质量和检测精度上均超越现有方法，且模型轻量。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 471, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 866, \"height\": 637, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1655, \"height\": 285, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1839, \"height\": 390, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1823, \"height\": 644, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 762, \"height\": 628, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32933/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 799, \"height\": 246, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32933/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1791, \"height\": 511, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32933/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1706, \"height\": 545, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32933/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 581, \"height\": 299, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32933/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 784, \"height\": 354, \"label\": \"Table\"}]"
motivation: 现有方法忽略语义对融合和检测的相互促进，导致性能受限。
method: 设计渐进式跨模态交互子网络和位平面切片SOD子网络，利用语义引导相互增强。
result: 在多个数据集上融合质量和检测精度显著优于基线。
conclusion: 语义引导相互增强机制可有效提升跨模态融合和检测性能。
---

## Abstract
This paper introduces a lightweight Semantic-guided Mutually Reinforcing network (SMR-Net) for the tasks of cross-modal image fusion and salient object detection (SOD). The core concept of SMR-Net is to leverage semantics for directing the mutual reinforcing between image fusion and SOD. Specifically, a Progressive Cross-modal Interaction (PCI) image fusion subnetwork is designed to exploit local interactions via convolution operations and extend to global interactions utilizing spatial and channel attention mechanisms. Subsequently, a cross-modal Bit-Plane Slicing-based SOD subnetwork (BPS) is developed by incorporating the fused image as a third modality. This component employs bit-plane slicing and the deformable convolution technique to effectively extract irregular semantic information embedded in fusion features. The refined semantic information then guides the feature extraction process of the source modalities in a reweighted fashion. By cascading these two subnetworks, BPS leverages final semantic results to direct PCI towards focusing more on semantic information. Ultimately, through this semantic-guided mutual enhancement process, SMR-Net excels in both producing high-quality fused images and achieving effective salient object detection. Our extensive experiments on image fusion and SOD tasks convincingly demonstrate the superiority of our network over existing state-of-the-art alternatives without introducing noticeable computational costs. Compared to nearest competitors, our method demonstrates a stronger generalization ability with 26% fewer parameters.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：红外与可见光图像具有互补特性（红外突出热目标，可见光保留纹理细节），但现有跨模态图像融合方法多聚焦于提升融合图像质量，而高层次的显著目标检测（SOD）任务往往被独立处理，二者缺乏协同。同时，现有方法忽略语义信息对融合和检测的相互促进作用。
- **核心问题**：如何设计一种轻量级网络，利用语义信息引导图像融合与SOD相互增强，同时显著提升两个任务的性能，并降低计算成本。
- **整体含义**：提出SMR-Net，通过语义驱动的相互增强机制，实现高质量融合图像与精确显著目标检测的统一框架，且参数量比最接近的竞争者减少26%。

### 2. 论文提出的方法论
- **核心思想**：语义引导的相互增强。网络由两个子网络组成：渐进式跨模态交互融合子网络（PCI）和基于位平面切片的SOD子网络（BPS）。PCI负责生成富含语义信息的融合图像，BPS利用融合图像引导SOD，同时其SOD损失反向传播指导PCI聚焦显著对象。
- **关键技术细节**：
  - **PCI子网络**：两个阶段交互。局部交互阶段：通过卷积操作提取互补特征，利用通道注意力（GAP+卷积+Sigmoid）进行特征重加权。全局交互阶段：同时使用空间注意力和通道注意力对局部交互后的特征进行跨模态过滤，减少冗余。
  - **BPS子网络**：将融合图像作为第三模态输入。在每个特征提取层后，对融合特征先通过卷积压缩为单通道图，然后进行4位位平面切片（Bit-Plane Slicing）提取高频和语义信息，再拼接恢复通道。随后引入可变形卷积（Deformable Convolution）捕获不规则显著目标形状。最终语义特征通过Sigmoid函数对可见光和红外特征进行重加权。
  - **交替训练**：PCI提供高显著信息指导BPS，BPS的SOD损失更新PCI参数，使融合更关注显著区域。
- **损失函数**：融合损失 = 强度损失 + 梯度损失 + SOD损失（加权BCE和加权IoU）。

### 3. 实验设计
- **数据集**：
  - **融合子任务**：TNO（15对）、RoadScene（40对）、MSRS（80对）用于泛化测试。
  - **SOD子任务**：VT5000（训练集+2500对测试）、VT1000（1000对）、VT821（821对）。
- **基准与对比方法**：
  - **融合对比**：9种，包括FusionGAN、PMGI、GANMcC、SDNet、CUFD、STDFusionNet、TarDAL、DIVFusion、IRFS。
  - **SOD对比**：10种，包括MTMR、SGDL、ADF、MIDD、APNet、CSRNet、OSRNet、TAGFNet、IRFS、LAFB。
- **评估指标**：
  - **融合**：PSNR、MSE、VIF、Qabf。
  - **SOD**：S-measure、自适应F-measure、自适应E-measure、MAE，并绘制PR曲线和F-measure曲线。

### 4. 资源与算力
- 论文明确提到：实验使用 **NVIDIA GeForce RTX 3090 GPU（24GB显存）**，CPU为3.20 GHz Intel(R)。
- 训练设置：总epochs=10，batch size=4，使用Adam优化器。
- **未明确说明训练时长**，但指出模型轻量（参数量31.52M，FLOPs相对较低，见Figure 1）。

### 5. 实验数量与充分性
- **实验组数**：
  - 融合任务：定性图4展示3个场景（TNO、RoadScene、MSRS）；定量表1展示4个指标×3个数据集，共12组对比。
  - SOD任务：定性图5展示6个典型样本；定量表2展示4个指标×3个数据集，共12组对比；另附PR曲线和F-measure曲线。
  - 消融实验：表3针对BS和DC模块（4种配置）；表4针对不同位平面层组合（8种配置）。
- **充分性与公平性**：对比方法涵盖近年最新（2020-2024），均为公开可复现方法；消融实验完整，验证了核心组件的必要性。但仅使用一组超参数（α=1），未进行超参数敏感性分析。

### 6. 论文的主要结论与发现
- SMR-Net在融合和SOD任务上均取得最佳或次佳结果，且参数量减少26%。
- **SOD性能提升**：在VT5000上，MAE比基于ResNet的最优方法降低6.25%；在VT821上MAE降低16.67%。
- **融合质量提升**：在TNO、RoadScene、MSRS上多项指标领先（如PSNR、Qabf），且视觉上更好地保留红外显著目标和可见光纹理。
- 消融实验表明：BS+DC模块联合使用可使F-measure提升21.3%，且单独使用BS或DC均带来显著增益；第四位平面（Bit4）独立使用效果最佳。

### 7. 优点
- **方法创新**：语义引导相互增强机制新颖，首次将位平面切片引入跨模态SOD，结合可变形卷积处理不规则目标。
- **轻量高效**：参数量仅31.52M，FLOPs低于多数对比方法，便于实际部署。
- **实验充分**：覆盖多个公共数据集，对比方法全面，定量定性结合，消融实验深入。
- **泛化性强**：在未见过的数据集（MSRS、TNO等）上融合指标仍领先。

### 8. 不足与局限
- **计算时间未报告**：仅给出参数量和FLOPs，未提供实际运行时间（如FPS），公平比较欠完整。
- **模态局限**：仅针对RGB-T（红外-可见光）场景，未验证在RGB-D、多光谱等其他模态上的泛化能力。
- **训练规模较小**：Epochs仅10，可能未充分收敛；未报告学习率衰减或early stopping策略。
- **VT1000上非全优**：在VT1000数据集上MAE略高于LAFB（0.020 vs 0.018），说明在特定场景仍有提升空间。
- **超参数未调优**：α=1固定设置，但融合损失中强度与梯度权重可能随数据集变化最优，缺乏敏感性分析。
- **应用限制**：方法依赖对齐的多模态图像对，在未对齐或动态场景下性能可能下降；未测试遮挡、雾霾等极端条件。

（完）
