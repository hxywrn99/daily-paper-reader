---
title: "Medical Image Segmentation with Minimal Labeling Effort: How Far Can We Push the Limits?"
title_zh: 最小标注努力下的医学图像分割：我们能将极限推到多远？
authors: Yizhe Zhang
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40079/44040"
tags: ["query:medical-seg"]
score: 10.0
evidence: 仅单张标注图像的极端半监督医学图像分割
tldr: 针对医学图像标注成本高的问题，提出MedSMILE框架，在极端的单标注图像半监督设置下，通过基础模型的直推推理生成伪标签，再迭代自训练，首次实现接近全监督性能，极大降低了标注成本。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 医学图像标注昂贵，希望用极少量标注数据达到全监督性能。
method: MedSMILE融合直推与归纳学习，利用基础模型生成伪标签，迭代自训练优化分割模型。
result: 仅用单张标注图像，在多个数据集上达到接近全监督的分割性能。
conclusion: 极端的半监督学习可以大幅减少医学图像分割的标注需求，具有重要实用价值。
---

## Abstract
We demonstrate for the first time that a medical image segmentation model can achieve near fully supervised performance using only a single annotated image and abundant unlabeled data. We present MedSMILE, a novel framework that synergistically integrates transductive and inductive learning for this extreme one-label semi-supervised setting. Its core novelty lies in an iterative loop where a foundation model both bootstraps and refines pseudo-labels for an inductive segmentation model. This process begins with the foundation model performing transductive inference to generate an initial set of pseudo-labels for the unlabeled data pool. This bootstraps an iterative self-training process where the segmentation model is trained and used to generate progressively better labels, with an inter-round refinement step that re-leverages the foundation model to correct errors in uncertain predictions. Experiments on seven datasets across four modalities show MedSMILE recovers 90%–95% of the fully supervised Dice score while decisively outperforming existing semi-supervised techniques that require substantially more annotation. MedSMILE sets a new standard for label-efficient learning in medical image segmentation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：医学图像分割依赖大量像素级标注数据，获取成本高昂、耗时，成为部署自动化工具的瓶颈。现有方法如少样本学习（FSL）、半监督学习（SSL）在标注量极少时性能有限，通常需要多个标注样本（如SSL常用10%标注）。本文探索更极端的场景：能否仅用**一张**标注图像，配合大量无标签数据，达到接近全监督的性能？若可行，将极大降低标注负担。
- **整体含义**：首次证明在极端的“单标注+大量无标注”设定下，医学图像分割模型可恢复全监督性能的90%–95%，大幅超越需更多标注的现有半监督方法，为标注高效学习树立新标杆。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：融合**直推学习（transductive learning）**与**归纳学习（inductive learning）**，形成伪标签生成与迭代优化的良性循环。具体而言，利用预训练基础模型（DINOv2）的强特征先进行直推推理生成初始伪标签，然后通过多轮自训练逐步提升伪标签质量，并在每轮间用基础模型基于特征相似性修正不确定性高的预测。
- **关键技术细节**：
  1. **可选的自监督编码器微调**：对DINOv2进行MoCo式对比学习微调（使用无标签数据），以适应特定模态（如超声）的域偏移，增强特征鲁棒性。
  2. **Round 0：初始伪标签生成**：
     - 从单张标注图像计算每个类别的特征原型（L2正则化后的均值）。
     - 对每张无标签图像，计算其DINOv2 patch特征与每个原型的余弦相似度，上采样后softmax得到初始软伪标签。
  3. **Round 1~R：迭代模型训练与伪标签精炼**：
     - 将当前伪标签（上一轮输出）作为训练集，重新初始化Mask2Former（使用LVD-142M预训练权重）并训练。
     - 用训练好的模型推理所有无标签图像，生成硬伪标签及像素级熵（不确定性度量）。
     - **KNN传播精炼**：将不确定性最高的前10%样本标记为“不确定”，基于DINOv2特征在“确定”样本中搜索K个最近邻，用邻域伪标签的加权平均（权重为余弦相似度）替换不确定样本的伪标签，其余保持。
     - 更新后的伪标签进入下一轮，重复训练与精炼。
  4. **关键设计**：每轮重新初始化Mask2Former，避免模型继承过往错误偏见，强制从当前改进的伪标签中学习。

### 3. 实验设计：数据集 / 场景、基准、对比方法

- **数据集与场景**：涵盖四个模态：
  - **内窥镜**：CVC-ClinicDB, Kvasir, CVC-ColonDB, ETIS（息肉分割，共1450张训练图像，仅1张标注，其余无标签）。
  - **皮肤镜**：ISIC 2018（2594张训练图像，仅1张标注）。
  - **脑MRI**：LGG（2017张切片，仅1张标注）。
  - **超声**：BUSI（570张正常/良性用于训练，仅1张标注；210张恶性用于测试）。
- **基准**：全监督模型（Polyp-PVT, PraNet-V2, SkinFormer, Ms RED, Mask2Former）以及使用全部标注的版本。
- **对比方法**：
  - **半监督方法**：MT, DAN, UA-MT, URPC, CLCC, SLC-Net, MC-Net+, CDMA, SCP-Net, MCF, KnowSAM, DEC-Seg（均使用10%标注，即145或259张，+无标签数据）。
  - **少样本方法**：iMAML（5-shot）。
  - **生成式方法**：GenSeg（使用40张标注图像，无无标签数据）。
- **评估指标**：mDice和mIoU。

### 4. 资源与算力

- **文中明确提及**：完整MedSMILE流程在ISIC数据集上、使用单张消费级GPU（RTX 4070 Ti SUPER）即可在**不到一小时**内完成。未说明其他数据集的具体训练时长，但指出主要计算代价来自Mask2Former训练，与轮数、epoch数和无标签样本数线性相关。每轮精炼包含三次O(N)的DINOv2推理和KNN传播，开销可控。无提及多卡训练或显存需求。

### 5. 实验数量与充分性

- **实验数量**：
  - **主实验**：七个数据集（四个模态）的全流程比较（Tables 1-4），每个数据集均与多种SOTA方法对比（最多13种）。
  - **消融实验**：
    - 有无伪标签精炼（Table 5）：覆盖全部7个数据集。
    - 迭代轮数影响（Table 6）：ISIC、LGG、BUSI三个数据集上比较Round1-3。
    - 自监督微调重要性（超声模态，文内提及：无微调时Dice降至0.259）。
  - **随机性**：息肉分割数据集上报告了5次随机种子运行的均值±标准差（Table 1）。
- **充分性与公平性**：
  - 对比方法均采用官方或广泛认可的实现，且所有半监督方法使用**10%标注**（远超MedSMILE的1张），全监督方法使用全部标注，对比设置合理。
  - 消融实验覆盖多个数据集和关键组件，证实了精炼和迭代的必要性。
  - 但缺乏对基础模型不同版本（如DINOv2不同尺寸）、不同骨干网络（如ViT-B vs. ViT-L）的鲁棒性测试，也未报告所有方法在相同硬件下的时间成本对比。

### 6. 论文的主要结论与发现

1. **单张标注可实现接近全监督性能**：在7个数据集上，MedSMILE恢复了全监督Dice的90%–95%（如Kvasir 95%，CVC-ClinicDB 90%，ISIC 93%等）。
2. **显著优于需10%标注的半监督方法**：即使后者使用145或259张标注，MedSMILE仍取得更高或相当的性能，尤其在CVC-ColonDB和ETIS等挑战性数据集上超出10个百分点以上。
3. **泛化能力优于全监督模型**：在未见过域（CVC-ColonDB, ETIS）上，MedSMILE的Dice甚至超过了一些全监督模型（如CVC-ColonDB: 0.826 vs. Polyp-PVT 0.808），表明利用大量无标签数据可学习更鲁棒、更泛化的特征。
4. **迭代精炼和自监督微调是性能提升关键**：无精炼步骤在多个数据集上下降约1-2个百分点；在超声模态下无微调导致性能崩溃。

### 7. 优点

- **方法创新性**：将直推学习（基础模型生成伪标签+特征传播）与归纳学习（Mask2Former自训练）有机结合，形成循环迭代机制，数据标注量降至理论最低。
- **高效且实用**：仅需一张标注即可启动，在消费级GPU上快速完成，易于推广。
- **实验全面且对比公平**：覆盖4种模态的7个数据集，与13种SOTA方法在**更大标注量**下对比仍胜出，消融实验充分验证各部件贡献。
- **首次证明单标注半监督的可行性**：为极端低标注场景提供了可行方案，具有重要的现实意义。

### 8. 不足与局限

- **对基础模型特征质量敏感**：当DINOv2特征不好时（如超声模态），必须依赖自监督微调，但微调本身耗时且可能仍不足以捕捉细微差异（如细粒度结构分割）。
- **域偏移问题未充分解决**：虽然可选微调，但未与更先进的域适应方法对比。初始伪标签完全依赖单张标注的代表性，若该标注图像不具代表性或噪声大，可能影响后续迭代。
- **实验覆盖有限**：仅测试了二值或较少类别分割（息肉、皮肤病变、肿瘤、脑MRI），未验证多类别、多组织精细分割（如器官分割）或医学图像中更复杂的场景。
- **缺乏与更多最新SSL方法的对比**：未包含一些2024-2025年提出的先进方法（如基于扩散模型的半监督、联邦学习等），且所有对比方法均使用10%标注，未测试它们使用更少标注（如1张）时的表现。
- **无理论收敛保证**：迭代过程未证明收敛性，虽然实验显示多轮改善，但早期轮次若产生严重错误可能无法恢复。
- **未报告完整时间对比**：仅给出ISIC的耗时，未与对比方法在相同硬件下对比训练时间。

（完）
