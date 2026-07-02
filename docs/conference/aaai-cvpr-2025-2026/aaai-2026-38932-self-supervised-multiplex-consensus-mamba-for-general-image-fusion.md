---
title: Self-supervised Multiplex Consensus Mamba for General Image Fusion
title_zh: 自监督多路共识Mamba用于通用图像融合
authors: "Yingying Wang, Rongjin Zhuang, Hui Zheng, Xuanhua He, Ke Cao, Xiaotong Tu, Xinghao Ding"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38932/42894"
tags: ["query:image-fusion"]
score: 9.0
evidence: 使用自监督Mamba的通用图像融合
tldr: 针对通用图像融合需处理多任务且不增加复杂度的问题，提出自监督多路共识Mamba框架SMC-Mamba，包含模态无关特征增强模块（通过自适应门控和旋转扫描保持细节和全局表示）以及多路共识模块。在多种融合任务上无需特定微调即取得优异性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 通用图像融合需兼顾多任务性能与模型复杂度。
method: 提出SMC-Mamba，包括模态无关特征增强和多路共识模块，采用空间-通道与频率旋转扫描。
result: 在多个通用融合任务上超越任务特定方法。
conclusion: SMC-Mamba以自监督方式实现高效通用图像融合。
---

## Abstract
Image fusion integrates complementary information from different modalities to generate high-quality fused images, thereby enhancing downstream tasks such as object detection and semantic segmentation. Unlike task-specific techniques that primarily focus on consolidating inter-modal information, general image fusion needs to address a wide range of tasks while improving performance without increasing complexity. To achieve this, we propose SMC-Mamba, a Self-supervised Multiplex Consensus Mamba framework for general image fusion. Specifically, the Modality-Agnostic Feature Enhancement (MAFE) module preserves fine details through adaptive gating and enhances global representations via spatial-channel and frequency rotational scanning. The Multiplex Consensus Cross-modal Mamba (MCCM) module enables dynamic collaboration among experts, reaching a consensus to efficiently integrate complementary information from multiple modalities. The cross-modal scanning within MCCM further strengthens feature interactions across modalities, facilitating seamless integration of critical information from both sources. Additionally, we introduce a Bi-level Self-supervised Contrastive Learning Loss (BSCL), which preserves high-frequency information without increasing computational overhead while simultaneously boosting performance in downstream tasks. Extensive experiments demonstrate that our approach outperforms state-of-the-art (SOTA) image fusion algorithms in tasks such as infrared-visible, medical, multi-focus, and multi-exposure fusion, as well as downstream visual tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：通用图像融合需要处理多种任务（红外-可见光、医学图像、多聚焦、多曝光），但现有方法多为任务特定设计，泛化能力差；同时，深度学习方法倾向于保留低频信息而丢失高频细节，影响视觉质量与下游任务性能。
- **背景意义**：单传感器受硬件限制无法捕获完整场景信息，图像融合通过整合互补信息提升后续目标检测、语义分割等任务的性能。传统CNN局部感受野有限，Transformer全局建模但计算量平方增长。状态空间模型（SSM，特别是Mamba）以线性复杂度实现全局建模，为高效通用融合提供了新方向。

## 2. 方法论：核心思想、关键技术细节
### 核心思想
提出**SMC-Mamba**（Self-supervised Multiplex Consensus Mamba），由三个核心模块组成：
- **Modality-Agnostic Feature Enhancement (MAFE)**：通过局部分支（自适应门控）和全局分支（空间-通道扫描 + 频率旋转扫描）增强模态无关特征，保留细节并捕获长程依赖。
- **Multiplex Consensus Cross-modal Mamba (MCCM)**：采用混合专家（MoE）结构，多个交叉模态Mamba专家动态协作，通过门控网络自适应激活Top-k专家，并引入多样性损失和共识损失，促进专家差异化与最终一致性。
- **Bi-level Self-supervised Contrastive Learning Loss (BSCL)**：在特征层和像素层分别进行对比学习，将融合结果的高频成分拉近至源图像的高频成分，推离低频成分，从而强化高频细节而不增加模型复杂度。

### 关键技术细节
1. **MAFE模块**：
   - 局部分支：将浅层特征分块，经深度卷积、GELU门控，得到局部精细特征。
   - 全局分支：分为空间-通道SSM（双向扫描）和频率旋转SSM（对幅度和相位进行旋转扫描后逆傅里叶变换），最后与局部特征拼接。
2. **MCCM模块**：
   - 门控网络：对拼接的模态特征做全局平均池化和最大池化，添加可学习噪声，经Softmax选择Top-2专家。
   - 每个专家执行Algorithm 1：对两个模态特征分别线性投影、SiLU激活，通过交叉模态扫描（空间/通道交互扫描）实现跨模态交互，最后逐元素相乘并相加。
   - 损失函数：包含负载均衡损失 \(L_{wb}\)、专家多样性损失 \(L_{div}\)、共识损失 \(L_{cons}\)，通过时间衰减权重 \(\lambda(t)\) 动态平衡。
3. **BSCL损失**：使用Haar小波提升方案分解特征和图像为高/低频分量，定义特征级对比损失 \(L_{fcl}\) 和像素级对比损失 \(L_{pcl}\)，公式见正文(27)(30)。
4. **总损失**：\(L_{total} = \lambda_1 L_{fcl} + \lambda_2 L_{pcl} + \lambda_3 L_{mccm} + \lambda_4 L_{ssim} + \lambda_5 L_{int}\)，超参数分别设为0.8, 0.4, 1, 1, 1。

## 3. 实验设计
### 数据集与场景
- **红外-可见光融合 (IVIF)**：训练用MSRS，测试用MSRS、RoadScene、M3FD。
- **医学图像融合 (MDIF)**：Harvard medical dataset（含CT-MRI、PET-MRI、SPECT-MRI），各自单独训练测试。
- **多聚焦融合 (MFIF)**：训练用MFI-WHU，测试用Lytro和MFI-WHU。
- **多曝光融合 (MEIF)**：训练用MEF，测试用MEF benchmark。
### Benchmark与对比方法
- **通用融合框架**（9种）：IFCNN、U2Fusion、SwinFusion、PSLPT、TC-MoA、Fusionmamba1、Fusionmamba2、MLFuse、LFDT-Fusion。
- **任务特定方法**：
  - IVIF：LRRNet、YDTR、SemLA、CDDFuse
  - MDIF：EMFusion、MSRPAN、TUFusion、ALMFnet
  - MFIF：GCF、FusionDN、MFF-GAN、ZMFF
  - MEIF：DPE-MEF、AGAL、BHF-MEF、SAMT-MEF
- **评估指标**：MI、SF、AG、CC、SCD、VIF、Qabf、MS-SSIM、Nabf，以及下游任务语义分割IoU。

## 4. 资源与算力
- **GPU**：单张NVIDIA RTX 3090。
- **框架**：PyTorch，优化器Adam（β=0.9），batch size=1，初始学习率2e-4，余弦退火每1000次迭代减半。
- **未明确说明**：具体训练总轮数/迭代次数、总训练时长未报告。仅提到“每1000 iterations halved via cosine annealing”，但未给出总迭代数。参数数量0.149M（表3），FLOPs 46.105G，推理时间288.545ms（480×640分辨率）。

## 5. 实验数量与充分性
- **实验数量丰富**：
  - 定量比较：IVIF任务在3个数据集、MFIF任务在2个数据集上与大量方法对比（各表1-2），每个任务均报告9个指标。
  - 消融实验（表3）：共18种配置，涵盖核心操作替换（Mamba→Conv/Window/Self-Attention）、主干模块移除、损失函数各分量、扫描方案、扫描方向等。
  - 下游任务：语义分割（DeepLabV3+）在MSRS数据集上的IoU对比（表4），并附可视化（图4）。
  - 视觉质量比较：图2（IVIF）、图3（MFIF）。
- **充分性与公平性**：
  - 对比方法全面，包括最新通用框架和任务特定方法，但均固定在同一数据集划分下训练（部分方法可能预训练？未说明）。
  - 消融实验系统，验证了每个组件贡献。
  - 未报告方差或统计显著性检验，但多次实验平均结果可接受。
  - 总体实验设计充分，结论可信。

## 6. 主要结论与发现
- SMC-Mamba在IVIF、MDIF、MFIF、MEIF四个任务上均取得最优或次优定量指标，视觉质量清晰，纹理细节丰富。
- 下游语义分割任务mIoU达到79.3%，高于所有对比方法，验证了融合对下游任务的正向增益。
- 消融实验表明：Mamba优于Conv、Window/ Self-Attention；MAFE、MCCM模块均显著提升性能；BSCL损失增强高频信息；交叉模态扫描有效促进跨模态交互；双向扫描优于单向。

## 7. 优点（方法或实验设计亮点）
- **方法设计新颖**：首次将Mamba与混合专家（MoE）结合用于通用图像融合，并引入交叉模态扫描和频率旋转扫描。
- **自监督对比学习**：BSCL在不增加复杂度前提下强化高频细节，且同时作用于特征和像素两层。
- **动态专家协作**：MCCM通过多样性损失和共识损失动态平衡专家差异与一致性，避免专家退化。
- **统一框架处理多模态**：MAFE模块设计为模态无关，可泛化至不同融合场景。
- **实验验证充分**：在4个融合任务和9个指标上全面对比，消融实验覆盖核心设计，下游任务验证实用性。

## 8. 不足与局限
- **实验覆盖**：未在真实噪声、低光照等退化场景下测试鲁棒性；医学融合仅用Harvard单一数据集，泛化性待验证；多曝光融合仅用MEF数据集，规模较小。
- **偏差风险**：对比方法可能未严格调优至公平比较（如训练策略不同）；未报告多次实验结果的标准差，结果稳定性未知。
- **计算效率**：尽管参数少（0.149M），但FLOPs达46G、推理时间289ms（480×640），对实时应用仍有压力；未与其他Mamba方法（如Fusionmamba1/2）做详细的效率对比。
- **应用限制**：Mamba扫描设计依赖特定序列化，可能对高分辨率图像产生感受野不足；专家门控网络引入随机噪声，可能带来轻微不稳定性。
- **论文未明确讨论局限性**，需读者自行推断。

（完）
