---
title: Energy-guided Dual Domain-invariant Prompting Framework with Fourier Regularization for Generalized Few-Shot Medical Segmentation
title_zh: 能量引导的双域不变提示优化框架及傅里叶正则化用于泛化少样本医学分割
authors: "Shaolei Liu, Yuting Wu, Dongchen Zhu, Jiamao Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/39554/43515"
tags: ["query:medical-seg"]
score: 10.0
evidence: 少样本医学分割与域泛化
tldr: 针对医学分割在跨域场景下泛化不足的问题，提出能量引导的双域不变提示优化框架，结合傅里叶正则化，在少样本设置下有效提取域不变特征，显著提升了对未见领域的分割性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有医学SAM泛化方法在跨域场景下性能下降，且依赖全标注源域数据不现实。
method: 提出双域不变提示优化框架，利用能量引导和傅里叶正则化增强域不变特征学习。
result: 在多个跨域少样本分割任务上取得了最优性能，尤其对未见领域泛化能力强。
conclusion: 结合提示优化和正则化策略可有效提升医学分割模型的域泛化能力。
---

## Abstract
Precise segmentation of organ and tissue lesions is essential for clinical diagnosis and treatment. Despite the progress of deep learning and foundation segmentation models, their domain generalization capability remains limited particularly when dealing with cross-domain scenarios or unseen data, leading to significant performance degradation. Current medical SAM-based generalization methods face two primary challenges: First, existing prompt-tuning strategies inadequately capture key domain-invariant features; Second, the reliance on fully labeled source domain data is unrealistic in clinical practice. To address these challenges, we propose a novel Dual domain-Invariant Prompt Optimization (DIPO) enhanced by energy-guided augmentation and frequency consistency regularization for few-shot medical image segmentation generalization. Our approach introduces a multi-band momentum enhancement strategy to dynamically augment source data by leveraging diverse frequency bands of the Fourier amplitude spectrum. Furthermore, we integrate multiscale geometric representation-based non-subsampled shearlet transform and text prompts to strengthen the extraction of shape- and texture-related domain-invariant features. Finally, we employ frequency consistency regularization to refine model robustness using predictions from unlabeled data. Experimental results in prostate and fundus datasets demonstrate that our method significantly outperforms current state-of-the-art methods.

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有基于SAM的医学图像分割泛化方法存在两个主要不足：① prompt-tuning策略未能充分捕获关键域不变特征；② 依赖全标注的源域数据，这在临床实际中难以实现。
- **背景**：医学图像分析中，模型在跨域场景（不同成像设备、模态、参数）下泛化能力严重下降。单源域泛化（SDG）旨在仅利用一个标注源域训练，使模型适用于多个未见目标域。现有方法（如DAPSAM, MoSE）虽引入SAM，但仍假设源域全标注，且缺乏对域不变特征的显式学习。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出能量引导的双域不变提示优化（DIPO）框架，通过动态增强源域数据、结合多尺度几何表示和文本提示提取域不变特征，并利用无标注数据的频率一致性正则化提升鲁棒性。
- **关键技术细节**：
  - **能量引导的多频带动量增强（EMME）**：对源域图像进行傅里叶变换，按低频、中频、高频三个频带分别计算振幅谱的均值和方差，通过动量更新得到全局统计量，再使用Box-Muller变换生成目标域扰动统计，将源域振幅谱归一化并映射到目标域统计空间，最终反傅里叶变换得到增强图像。目的是减少源域与目标域的域间隙。
  - **双域不变提示优化（DIPO）**：① **频率提示**：对源域图像进行非下采样剪切波变换（NSST），获得多尺度、多方向的频率子带（低频保留结构，高频捕获边缘纹理），并通过频率自注意力机制提取域不变特征作为频率结构提示。② **文本提示**：利用ChatGPT自动生成域不变描述（器官位置、解剖、病理特征）和域特定描述（设备灰度范围），通过CLIP编码器提取域不变文本特征。③ 将频率提示、文本提示与图像嵌入拼接，经两层1×1卷积生成最终的双域不变提示。
  - **频率一致性正则化（FCR）**：对未标注源域数据，要求模型对原始图像和EMME增强后的图像输出一致预测，使用KL散度作为一致性损失，提升模型对输入变化的鲁棒性。
- **总损失**：监督损失（CE + Dice） + λ * 一致性损失，λ采用ramp-up策略逐渐增大。

### 3. 实验设计
- **数据集**：
  - **前列腺分割**：116例T2加权MRI，来自6个不同域（A-F），按DAPSAM方法处理为384×384。
  - **眼底视盘/视杯（OD/OC）联合分割**：RIGA+数据集，包含5个域（BinRushed, Magrabia, Messidor BASE1-3），选择BinRushed为源域，其他三个为未见目标域。
- **评估指标**：Dice相似系数（DSC）。
- **对比方法**：
  - 非SAM方法：CSDG, MaxStyle, D-Norm, SLAug, CCSDG, Mamba-Sea等。
  - SAM变体：SAMed, SAMMed, DeSAM, DAPSAM, Med-SA。
- **设置**：测试两种标注比例：100%源域标注和25%源域标注。

### 4. 资源与算力
- **文中说明**：使用单个NVIDIA RTX 3090 GPU，批量大小8，训练150个epoch。优化器ADAM，学习率1e-4，权重衰减5e-4。未提及模型参数量或训练总时长；训练轮次最大200（早停160）。文中未详细说明能源消耗或算力总量。

### 5. 实验数量与充分性
- **实验组数**：
  - 主要对比实验：在两个数据集上，分别测试100%和25%标注设置，共4组主要结果（前列腺6域平均+各域，眼底3域平均+各域）。
  - 消融实验：
    - 组件消融（EMME、频率提示、文本提示、FCR）：14种组合（表3）。
    - 在DAPSAM和Med-SA上添加EMME和FCR的效果验证（表4）。
    - NSST分解层数影响（图6）：测试3层最好，但未展示多组数值。
    - 频带划分半径影响（图7）：R1=4, R2=2最佳，覆盖多个半径组合。
- **充分性与公平性**：对比方法均为近年来同期SOTA，且复现实验设置一致。消融实验覆盖了所有关键组件，验证了各模块贡献。但实验仅在2D分割任务上验证（前列腺和眼底），未涉及3D数据或更多模态。总体客观公平。

### 6. 主要结论与发现
- 在100%标注设置下，前列腺分割平均DSC达83.56%，眼底OD/OC平均达92.95%，显著优于所有对比方法。
- 在更具挑战性的25%标注设置下，方法依然保持最佳，前列腺平均75.84%（超出第二名DAPSAM 2.99%），眼底平均82.48%（超出1.83%），表明对标注依赖显著降低。
- 各消融组件均带来性能提升，且EMME和FCR可迁移至其他方法（DAPSAM, Med-SA）进一步提升性能。

### 7. 优点
- **创新性**：首次将能量引导动态增强、NSST多尺度几何表示、文本提示相结合用于少样本医学分割域泛化，解决了现有方法域不变特征学习不足和全标注假设不现实的问题。
- **实用性**：在部分标注（25%）下仍保持高性能，更贴近临床实际。
- **泛化性**：所提EMME和FCR可即插即用于其他SAM基方法，提升其性能。
- **理论支撑**：基于Parseval定理和自由能差异，从频带能量角度缩小域差异。

### 8. 不足与局限
- **实验覆盖**：仅验证了前列腺和眼底两个2D分割任务，未在更多模态（如CT、超声、3D MRI）或更复杂的病变类型上测试，泛化到其他场景的能力尚未证明。
- **数据隐私风险**：提示生成依赖ChatGPT（被引用为原文描述），实际应用中可能存在隐私和合规问题（敏感医疗文本需本地化处理）。
- **方法依赖**：NSST和CLIP均需额外计算开销（推理时需对图像做NSST分解和文本编码），可能增加部署复杂度。
- **未提到失败案例**：论文未分析在哪些域或何种情况下性能依然较差，提供负面结果有助于更全面理解方法边界。
- **超参数敏感性**：频带划分半径R1/R2、NSST分解层数等需要手动调整，自适应方法值得探索。
- **对照实验差异**：消融实验中对“EMME+FCR”组合单独结果未系统展示（表3最后一行是全部组件，未单独展示EMME+FCR无提示的结果）。

（完）
