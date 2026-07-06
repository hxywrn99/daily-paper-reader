---
title: Text-Guided Channel Perturbation and Pre-Trained Knowledge Integration for Unified Multi-Modality Image Fusion
title_zh: 基于文本引导的通道扰动与预训练知识整合的统一多模态图像融合
authors: "Xilai Li, Xiaosong Li, Weijun Jiang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37581/41543"
tags: ["query:image-fusion"]
score: 10.0
evidence: 统一多模态图像融合框架，通道扰动与预训练知识整合
tldr: 现有统一多模态图像融合方法因模态差异大导致梯度冲突，泛化性差。本文提出UP-Fusion，通过语义感知通道扰动抑制冗余信息，并利用预训练知识整合强化关键特征，构建统一融合框架。在多种多模态图像融合任务（如可见光-红外、医学图像）上的实验结果表明，该方法在融合质量和泛化性方面均优于现有方法，验证了通道扰动和知识整合策略的有效性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37581/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 845, \"height\": 458, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37581/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1837, \"height\": 961, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37581/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 873, \"height\": 968, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37581/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1838, \"height\": 1202, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37581/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 880, \"height\": 618, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37581/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1843, \"height\": 249, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37581/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 876, \"height\": 318, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37581/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1850, \"height\": 764, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37581/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1842, \"height\": 364, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37581/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1753, \"height\": 388, \"label\": \"Table\"}]"
motivation: 解决统一多模态图像融合中模态差异导致的梯度冲突和泛化性差的问题。
method: 提出语义感知通道扰动模块和预训练知识整合策略，构建统一的融合框架。
result: 在多个融合任务上性能提升，并验证了泛化能力。
conclusion: UP-Fusion有效统一了多模态图像融合，兼顾性能与泛化性。
---

## Abstract
Multi-modality image fusion enhances scene perception by combining complementary information. Unified models aim to share parameters across modalities for multi-modality image fusion, but large modality differences often cause gradient conflicts, limiting performance. Some methods introduce modality-specific encoders to enhance feature perception and improve fusion quality. However, this strategy reduces generalisation across different fusion tasks. To overcome this limitation, we propose a unified multi-modality image fusion framework based on channel perturbation and pre-trained knowledge integration (UP-Fusion). To suppress redundant modal information and emphasize key features, we propose the Semantic-Aware Channel Pruning Module (SCPM), which leverages the semantic perception capability of a pre-trained model to filter and enhance multi-modality feature channels. Furthermore, we proposed the Geometric Affine Modulation Module (GAM), which uses original modal features to apply affine transformations on initial fusion features to maintain the feature encoder modal discriminability. Finally, we apply a Text-Guided Channel Perturbation Module (TCPM) during decoding to reshape the channel distribution, reducing the dependence on modality-specific channels. Extensive experiments demonstrate that the proposed algorithm outperforms existing methods on both multi-modality image fusion and downstream tasks.

---

## 论文详细总结（自动生成）

### 论文详细总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有统一多模态图像融合（Unified MMIF）方法面临两大矛盾：
  - 单编码器结构（Single AE）虽共享参数、泛化性好，但因缺乏显式模态交互，融合质量欠佳。
  - 多编码器结构（Multiple AE）通过独立模态编码器提升质量，却易过拟合特定模态分布，导致跨任务泛化能力差。
- **研究动机**：旨在平衡泛化能力与融合质量，构建一种既能有效建模模态交互、又能避免过度依赖特定模态特征的统一融合框架。
- **整体含义**：提出 **UP-Fusion**（Unified multi-modality image fusion based on channel perturbation and pre-trained knowledge integration），首次将文本引导的通道扰动与预训练语义知识融入统一融合模型，在多模态视觉任务（红外-可见光、医学图像）上取得领先性能。

#### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：通过语义感知筛选冗余通道、几何仿射调制保留模态判别性、文本引导通道扰动消除模态标记，实现跨模态特征的统一建模与泛化。
- **整体架构**：基于Transformer块的4层编码器 + 4层解码器（Restormer结构）。多模态输入经卷积后进入编码器，通过三个关键模块协同工作。
  - **Semantic-Aware Channel Pruning Module (SCPM)**  
    - 作用：在编码器入口抑制冗余通道、强化显著特征。
    - 流程：  
      1. 通道注意力机制计算重要性权重 \( \omega_C \)。  
      2. 预训练ConvNeXt提取全局语义特征，经线性映射得到语义权重 \( \omega_S \)。  
      3. 加权融合：\( \omega_F = \omega_C + \alpha \cdot \sigma(\omega_S) \)，其中 \( \alpha \) 可学习，\( \sigma \) 为Sigmoid。  
      4. 按Top-k（70%）保留显著通道，1×1卷积恢复通道数，再沿通道分割为两份，分别送入对应模态的GAM模块。
  - **Geometric Affine Modulation Module (GAM)**  
    - 作用：对SCPM产出的预融合特征进行几何仿射调制，保留模态判别性但避免直接注入模态特征导致过拟合。
    - 流程：  
      1. 对原始模态特征做全局平均池化得到 \( F^G_A, F^G_B \)。  
      2. 通过两层1×1卷积（含ReLU）预测缩放因子 \( \gamma \) 和偏移项 \( \beta \)：\( [\gamma_M, \beta_M] = Conv_{1\times1}(ReLU(Conv_{1\times1}(F^G_M))) \)。  
      3. 调制：\( F^O_M = F^{use}_M \cdot (1 + \gamma) + \beta \)。
  - **Text-Guided Channel Perturbation Module (TCPM)**  
    - 作用：在解码阶段利用文本语义引导通道重排，降低对特定模态通道的依赖。
    - 流程：  
      1. 拼接多模态特征，经通道注意力与Top-k（50%）筛选重要通道。  
      2. 1×1卷积扩增通道至2倍以增加冗余。  
      3. 用CLIP编码的文本特征（如“preserving fine details, semantic structures…”）通过两层MLP生成引导权重，产生通道重排索引。  
      4. 以重排后特征为Query，原始特征为Key/Value，进行自注意力交互，再经前馈网络输出。
- **损失函数**：梯度损失 \( L_{grad} \) + L1损失 \( L_{l1} \)，确保细节与结构保持。

#### 3. 实验设计
- **数据集**：
  - 红外-可见光融合（IVIF）：MSRS、LLVIP、M3FD。
  - 医学图像融合（MEIF）：哈佛医学院脑部数据集（CT-MRI, PET-MRI, SPECT-MRI）及荧光-相位对比（GFP-PC）数据集。
- **Benchmark**：仅用LLVIP数据集训练单一权重，测试所有任务。
- **对比方法**（共10种）：
  - IVIF专用：SAGE (CVPR 25), TDFusion (CVPR 25), CrossFuse (INF 24)。
  - MEIF专用：ALMFNet (TCSVT 23), MMIF-INet (INF 25), GeSeNet (TNNLS 23)。
  - 通用MMIF：GIFNet (CVPR 25), EMMA (CVPR 24), TIMFusion (PAMI 24), CoCoNet (IJCV 24)。
- **评估指标**：Q_NCIE, Q_P, VIF, SSIM, Q_AB/F。
- **下游任务**：
  - 语义分割：MSRS数据集，BANet分割网络。
  - 目标检测：M3FD数据集，YOLOv7检测网络。

#### 4. 资源与算力
- 明确说明：PyTorch 2.1.1环境，4张GeForce RTX 3090 GPU。
- 训练设置：100个epoch，随机裁剪192×192，batch size=2，初始学习率0.0001，余弦退火衰减至0.00001，Adam优化器。
- 未提及训练总时长，但算力资源充足。

#### 5. 实验数量与充分性
- **实验数量**：
  - 定性比较：在IVIF三个数据集及MEIF四个数据集上展示多组视觉对比（Fig.3, Fig.4）。
  - 定量比较：Tab.1（IVIF三个数据集）、Tab.2（MEIF两个数据集）列出所有指标对比。
  - 消融实验：Tab.3及Fig.5，分别去除SCPM、GAM、TCPM、通道注意力（CA，在TCPM与SCPM中分别去除）、预训练ConvNeXt，共6组对比。
  - 下游任务：Tab.4（分割mIoU）、Tab.5（检测mAP）。
- **充分性评价**：实验覆盖主要融合任务、多种模态组合、多个主流数据集，消融设计完整，对比方法涵盖最新顶尖模型，下游任务验证实用价值。实验设计客观、公平。

#### 6. 论文的主要结论与发现
- UP-Fusion在IVIF与MEIF任务上均优于现有任务专用和通用融合方法，尤其在跨任务泛化上表现突出（仅用IVIF数据训练，在医学图像任务上获得多指标第一）。
- 文本引导通道扰动（TCPM）有效消除了模态标记，增强了解码器对不同模态的适应能力；SCPM与GAM协同保证了编码阶段特征的有效性与模态判别性。
- 下游实验（分割、检测）进一步证明了融合质量的提升可带来实际应用收益。

#### 7. 优点
- **方法创新性**：引入文本引导的通道扰动概念至图像融合领域，结合预训练语义知识（ConvNeXt, CLIP），突破了传统统一模型与模态专用模型之间的性能-泛化权衡。
- **架构统一性**：单一训练权重即可处理多种异质模态融合任务，无需为每类任务分别训练，降低了部署成本。
- **实验全面性**：涵盖定性/定量、消融、下游任务，对比方法丰富，结果有说服力。
- **可复现性**：代码已开源（GitHub），训练设置清晰。

#### 8. 不足与局限
- **训练数据偏差**：仅在LLVIP（可见光-红外）上训练，虽然泛化到医学图像，但未在医学数据上微调，其泛化程度可能受数据分布差异限制（例如脑部医学图像与自然场景差异大）。
- **模态覆盖有限**：仅测试了IVIF与脑部医学融合，未涉及遥感、多聚焦等其他常见融合任务，通用性有待进一步验证。
- **文本模板固定**：TCPM中文本输入为人工设计的固定描述（“The image contains multiple views...”），未探讨不同文本描述对结果的影响，可能引入先验偏差。
- **计算开销**：使用预训练ConvNeXt和CLIP推理，增加模型复杂度，但论文未分析推理速度或参数量。
- **下游任务单一**：仅使用BANet和YOLOv7，未在其他检测/分割框架上验证，可能存在适配偏差。

（完）
