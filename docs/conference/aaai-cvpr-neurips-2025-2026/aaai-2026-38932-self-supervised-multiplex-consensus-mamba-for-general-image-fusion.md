---
title: Self-supervised Multiplex Consensus Mamba for General Image Fusion
title_zh: 自监督多路共识Mamba用于通用图像融合
authors: "Yingying Wang, Rongjin Zhuang, Hui Zheng, Xuanhua He, Ke Cao, Xiaotong Tu, Xinghao Ding"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38932/42894"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于Mamba的自监督通用图像融合
tldr: 通用图像融合要同时支持多种任务而不增加复杂度。本文提出SMC-Mamba，引入模态无关特征增强模块和多路共识机制，通过自适应门控和频率旋转扫描保持细节并增强全局表征。在多个融合任务上，SMC-Mamba实现了与专用方法相当甚至更好的性能，且模型统一。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38932/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1797, \"height\": 1020, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38932/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1658, \"height\": 416, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38932/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1661, \"height\": 393, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38932/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 679, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38932/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 1037, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38932/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 878, \"height\": 1198, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38932/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 807, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38932/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1847, \"height\": 601, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38932/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 882, \"height\": 486, \"label\": \"Table\"}]"
motivation: 专用融合技术针对特定任务，难以泛化到通用场景。
method: 提出模态无关特征增强和多路共识机制，基于Mamba架构进行自监督学习。
result: 在多种融合任务上取得最优或相当性能，且模型统一。
conclusion: SMC-Mamba为通用图像融合提供了高效且可扩展的解决方案。
---

## Abstract
Image fusion integrates complementary information from different modalities to generate high-quality fused images, thereby enhancing downstream tasks such as object detection and semantic segmentation. Unlike task-specific techniques that primarily focus on consolidating inter-modal information, general image fusion needs to address a wide range of tasks while improving performance without increasing complexity. To achieve this, we propose SMC-Mamba, a Self-supervised Multiplex Consensus Mamba framework for general image fusion. Specifically, the Modality-Agnostic Feature Enhancement (MAFE) module preserves fine details through adaptive gating and enhances global representations via spatial-channel and frequency rotational scanning. The Multiplex Consensus Cross-modal Mamba (MCCM) module enables dynamic collaboration among experts, reaching a consensus to efficiently integrate complementary information from multiple modalities. The cross-modal scanning within MCCM further strengthens feature interactions across modalities, facilitating seamless integration of critical information from both sources. Additionally, we introduce a Bi-level Self-supervised Contrastive Learning Loss (BSCL), which preserves high-frequency information without increasing computational overhead while simultaneously boosting performance in downstream tasks. Extensive experiments demonstrate that our approach outperforms state-of-the-art (SOTA) image fusion algorithms in tasks such as infrared-visible, medical, multi-focus, and multi-exposure fusion, as well as downstream visual tasks.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有图像融合方法多为任务专用设计（如红外-可见、医学、多聚焦、多曝光），难以泛化到多种场景。同时，CNN受限于局部感受野，Transformer计算开销大（二次复杂度），且现有方法倾向于保留低频信息而丢失高频细节，影响下游任务性能。
- **核心问题**：如何设计一个**通用图像融合框架**，能够动态适应不同模态和任务，以线性复杂度建模全局依赖，同时增强高频细节保持而不增加额外计算负担。
- **背景**：Mamba（State Space Models）具有线性复杂度和内容感知的全局建模能力，成为CNN和Transformer的有力替代。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出**SMC-Mamba**（Self-supervised Multiplex Consensus Mamba），包含三个核心组件：
  1. **Modality-Agnostic Feature Enhancement (MAFE) 模块**：通过局部门控和全局空间-通道/频率旋转扫描增强单模态特征，同时保留局部细节与全局上下文。
  2. **Multiplex Consensus Cross-modal Mamba (MCCM) 模块**：采用**混合专家（MoE）** 架构，多个专家独立完成跨模态融合，通过门控网络动态选择Top-k专家（k=2），并引入**负载平衡损失**、**专家多样性损失**和**共识损失**，使专家先探索多样化融合策略，后期收敛到统一表示。
  3. **Bi-level Self-supervised Contrastive Learning Loss (BSCL)**：在特征层和像素层利用Haar小波提升方案分解高低频，通过对比学习拉近融合图像的高频分量与源图像的高频分量，推远低频分量，从而增强高频纹理。
- **关键技术细节**：
  - **跨模态扫描**：在MCCM中设计空间和通道的跨模态扫描，实现模态间双向交互。
  - **频率旋转扫描**：将特征变换到频域（幅度/相位）进行扫描，再反变换回空间域。
  - **损失函数**：总体损失包括BSCL（特征级L_fcl和像素级L_pcl）、MCCM损失（L_wb, L_div, L_cons）、SSIM损失和强度损失，超参数λ1-λ5设为0.8, 0.4, 1, 1, 1。
- **算法流程**：输入源图像 → 浅层特征提取 → MAFE增强 → MCCM（门控选择专家 + 跨模态Mamba融合）→ 生成融合特征 → 结合BSCL损失优化。

## 3. 实验设计：使用了哪些数据集/场景，基准是什么，对比了哪些方法
- **数据集与场景**：
  - **红外-可见图像融合（IVIF）**：训练集MSRS，测试集MSRS、RoadScene、M3FD。
  - **医学图像融合（MDIF）**：Harvard医疗数据集（CT-MRI, PET-MRI, SPECT-MRI）。
  - **多聚焦融合（MFIF）**：训练集MFI-WHU，测试集Lytro和MFI-WHU。
  - **多曝光融合（MEIF）**：训练集MEF，测试集MEF benchmark。
- **评估指标**：MI, SF, AG, CC, SCD, VIF, Qabf, MS-SSIM, Nabf等。
- **对比方法**：
  - 通用框架：IFCNN, U2Fusion, SwinFusion, PSLPT, TC-MoA, Fusionmamba1/2, MLFuse, LFDT-Fusion。
  - 任务专用方法：IVIF（LRRNet, YDTR, SemLA, CDDFuse）；MDIF（EMFusion, MSRPAN, TUFusion, ALMFnet）；MFIF（GCF, FusionDN, MFF-GAN, ZMFF）；MEIF（DPE-MEF, AGAL, BHF-MEF, SAMT-MEF）。
- **下游任务**：语义分割（DeepLabV3+ on MSRS），检测（未详述但提及）。

## 4. 资源与算力
- **文中明确说明**：使用单张NVIDIA RTX 3090 GPU，PyTorch实现，Adam优化器（β=0.9），batch size=1，初始学习率2×10⁻⁴（余弦退火每1000迭代减半），训练总epoch数未明确给出（仅提到时间衰减权重λ(t)设计）。
- **参数与计算量**：核心模型参数量0.149M，FLOPs 46.105G，推理时间288.545ms（480×640分辨率）。

## 5. 实验数量与充分性
- **实验组数**：覆盖4种融合任务（IVIF、MDIF、MFIF、MEIF），每个任务在多个测试集上评估；对比方法数量多（>20种）。
- **消融实验**：在MSRS上进行了全面的消融：
  - 替换Mamba为Conv/Window Attention/Self Attention；
  - 移除MAFE、MCCM模块；
  - 移除BSCL子损失；
  - 移除各项MCCM损失（L_wb, L_div, L_cons, L_mccm）；
  - 移除空间-通道/频率/跨模态扫描；
  - 双向扫描与单向扫描对比。
- **下游任务验证**：语义分割mIoU和定性图。
- **充分性评价**：实验设计较为充分，覆盖了核心模块、损失函数、扫描策略和骨干网络，对比方法多样且包括SOTA。但未在MDIF/MEIF上展示消融结果，下游任务仅展示了分割未展示检测，略有不足。

## 6. 论文的主要结论与发现
- SMC-Mamba在IVIF和MFIF任务上，在绝大多数指标上取得了**最佳或第二佳**成绩（见表1、2），视觉质量优于对比方法。
- 在语义分割下游任务中，mIoU达到79.3%，高于所有对比方法（包括任务专用方法）。
- 消融结果证实了每个组件的有效性：MAFE、MCCM、BSCL、扫描策略均显著提升性能。
- Mamba作为骨干结构在效率和性能上优于卷积和自注意力。

## 7. 优点：方法或实验设计上的亮点
- **统一框架**：可处理多种图像融合任务（红外-可见、医学、多聚焦、多曝光），无需任务特定修改。
- **创新性**：将Mamba与MoE结合，并设计跨模态扫描；提出自监督BSCL损失，在不增加推理成本前提下增强高频细节。
- **效率**：参数量仅0.149M，FLOPs 46G，推理速度较快（288ms），兼顾了性能与效率。
- **消融全面**：在核心设计、损失函数、扫描策略等方面均进行了详细消融，验证了每个模块的必要性。
- **下游受益**：展示了融合结果对语义分割的正面作用。

## 8. 不足与局限
- **实验覆盖不全**：在MDIF和MEIF任务上仅提供了定量结果简述（未在正文中展示详细表格），消融实验仅在IVIF上执行，未能提供跨任务的一致消融。
- **泛化风险**：训练数据来自特定数据集（如MSRS、MFI-WHU等），不同场景（如极端光照、传感器差异）下的鲁棒性未充分讨论。
- **计算资源**：虽然模型轻量，但训练使用单GPU且batch size=1，可能未充分探索更大规模数据或分布式训练下的表现。
- **未提及局限性**：论文未明确讨论方法的不足（如专家数量固定的影响、门控噪声的敏感性、跨模态扫描在非对齐模态下的适用性等）。
- **下游任务局限**：仅展示了分割结果，未提供检测性能或更多下游任务的定量比较。

（完）
