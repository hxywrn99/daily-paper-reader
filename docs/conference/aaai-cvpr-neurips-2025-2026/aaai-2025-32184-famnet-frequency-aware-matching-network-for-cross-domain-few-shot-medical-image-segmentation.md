---
title: "FAMNet: Frequency-aware Matching Network for Cross-domain Few-shot Medical Image Segmentation"
title_zh: FAMNet：频率感知匹配网络用于跨域小样本医学图像分割
authors: "Yuntian Bo, Yazhou Zhu, Lunbo Li, Haofeng Zhang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32184/34339"
tags: ["query:medical-seg"]
score: 10.0
evidence: 跨域小样本医学图像分割
tldr: 针对现有小样本医学图像分割模型无法处理不同成像技术导致的域偏移问题，提出FAMNet，利用频域相似性进行跨域匹配，结合频率感知匹配模块和多尺度特征，在少量标注数据下实现跨域泛化分割。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 不同成像技术带来域偏移，现有小样本分割模型无法泛化到新目标域。
method: 提出频率感知匹配网络，包含频率感知匹配模块和多尺度特征融合，利用频域相似性适应跨域差异。
result: 在跨域小样本医学分割任务上显著优于现有方法。
conclusion: FAMNet通过频域对齐有效缓解了域偏移，拓展了小样本分割的适用性。
---

## Abstract
Existing few-shot medical image segmentation (FSMIS) models fail to address a practical issue in medical imaging: the domain shift caused by different imaging techniques, which limits the applicability to current FSMIS tasks. To overcome this limitation, we focus on the cross-domain few-shot medical image segmentation (CD-FSMIS) task, aiming to develop a generalized model capable of adapting to a broader range of medical image segmentation scenarios with limited labeled data from the novel target domain.
Inspired by the characteristics of frequency domain similarity across different domains, we propose a Frequency-aware Matching Network (FAMNet), which includes two key
components: a Frequency-aware Matching (FAM) module and a Multi-Spectral Fusion (MSF) module. The FAM module tackles two problems during the meta-learning phase: 1) intra-domain variance caused by the inherent support-query bias, due to the different appearances of organs and lesions, and 2) inter-domain variance caused by different medical imaging techniques. Additionally, we design an MSF module to integrate the different frequency features decoupled by the FAM module, and further mitigate the impact of inter-domain variance on the model's segmentation performance.
Combining these two modules, our FAMNet surpasses existing FSMIS models and Cross-domain Few-shot Semantic Segmentation models on three cross-domain datasets, achieving state-of-the-art performance in the CD-FSMIS task.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义
- **研究动机**：现有小样本医学图像分割（FSMIS）模型无法处理不同成像技术（如CT与MRI、不同MRI序列、不同机构采集）导致的**域偏移**问题，导致在未见过的目标域上性能急剧下降。作者定义了一个新任务：**跨域小样本医学图像分割（CD-FSMIS）**，目标是在仅有少量标注的未见目标域上实现泛化分割。
- **核心挑战**：①**域内差异**（support-query bias）：同一器官在不同图像中大小、脂肪含量、病理状态等变化大，导致原型网络难以找到相似的支持-查询对；②**域间差异**：不同成像模态在空间域相似度低，但在频域中高频和低频带差异显著，中频带相对相似。
- **整体意义**：提出利用频域相似性特性来同时缓解域内和域间差异，使模型能在少量标注下适应新域，拓展FSMIS的实用性。

## 2. 论文提出的方法论
- **核心思想**：利用频域分解，对支持-查询前景特征进行分频带匹配，对域差异大的频带（高频、低频）采用反向注意力加权以降低依赖，对域无关的中频带采用正向注意力加权以增强共同特征；再通过cross-attention融合各频带特征，提取域不变信息。
- **关键技术细节**：
  - **特征提取**：ResNet-50（MS-COCO预训练）提取支持/查询图像特征。
  - **粗预测生成（CPG）**：基于原型网络，用支持前景原型对查询特征计算余弦相似度，生成粗分割掩模。
  - **频率感知匹配（FAM）模块**：
    - 多谱解耦：对前景特征做FFT变换，用带通滤波器分为低、中、高三个频带，再IFFT回空间域。
    - 联合空间匹配（JSM）：每频带内通过可学习的线性映射将支持/查询特征投影到联合空间，计算注意力矩阵A。
    - 差异化加权：对域无关频带（中频）直接用A增强相似部分；对域敏感频带（低、高频）用(1-A)抑制相似部分。再融合得到每个频带的融合特征F_b。
  - **多光谱融合（MSF）模块**：将低/高频特征作为Key/Value，中频特征作为Query，通过cross-attention提取中频中保留的域不变信息，并与中频特征相加，经ReLU后输出融合特征F_f。
  - **最终预测**：对F_f做全局平均池化得到最终前景原型，再计算与查询特征的余弦相似度得到最终掩模。
- **公式与算法流程**（文字说明）：
  1. 特征编码 → 粗预测生成（公式1-2）
  2. 前景提取 → 自适应平均池化 → FFT → 分频滤波 → IFFT → 得到三频带特征（公式3-6）
  3. 每频带内：线性映射 → 计算注意力矩阵A → 根据频带类型进行正向或反向加权 → MLP融合 → 得到F_b（公式7-11）
  4. MSF：中频作为Q，低/高频作为K/V → cross-attention → 相加 → ReLU → 得到F_f（公式12-14）
  5. 全局平均池化 → 余弦相似度 → 最终预测（公式15-16）
  6. 损失：二元交叉熵（最终预测 + 粗预测）（公式17-18）

## 3. 实验设计
- **使用的数据集与场景**：
  - **Cross-Modality**（腹部）：Abdominal CT vs. Abdominal MRI（来自CHAOS和MICCAI挑战），4类（左肾、右肾、肝、脾）。
  - **Cross-Sequence**（心脏）：LGE MRI vs. b-SSFP MRI（来自多序列心脏分割挑战），3类（LV-BP、LV-MYO、RV）。
  - **Cross-Institution**（前列腺）：UCLH vs. NCI 前列腺T2-MRI，3类（膀胱、中央腺、直肠）。
- **Benchmark**：采用Sorensen-Dice系数（与FSMIS常用指标一致）。
- **对比方法**：6种FSMIS方法（PANet、SSL-ALPNet、ADNet、QNet、CATNet、RPT）和2种CD-FSS方法（PATNet、IFA）。均在1-way 1-shot设置下评估。

## 4. 资源与算力
- 论文明确提到：使用 **一块 NVIDIA GeForce RTX 4080S GPU** 进行训练。
- 训练超参数：39K iterations（3000 iterations/epoch，batch size=1）。未提及总训练时间。算力资源合理，但对工业级大规模训练而言相对有限。

## 5. 实验数量与充分性
- **实验数量**：主实验在三个跨域数据集（每个数据集双向评估）共6个子实验；此外提供了：
  - 组件消融（表3）：基线+CPG、基线+CPG+FAM、基线+CPG+FAM+MSF。
  - 注意力加权策略消融（表4）：对不同频带采用不同的A或1-A组合（4种变体）。
  - 附录中还有其他消融（如方法对比、超参数分析、可视化结果）。
- **充分性判断**：实验覆盖了三种典型跨域场景（模态、序列、机构），消融验证了各模块的有效性，对比方法包括同一任务的最新方法，结果客观。但附录中未列出具体结果数据（仅引用），可能削弱完整性。总体实验设计较充分、公平。

## 6. 论文的主要结论与发现
- 提出的FAMNet在三个跨域数据集上均达到**最优**性能，在Cross-Modality上平均Dice提升2.78%~7.46%，在Cross-Sequence上提升1.33%~9.30%。
- FAM模块通过分频带差异化注意力同时解决了域内support-query偏差和域间差异。
- MSF模块进一步利用中频带提取域不变信息，抑制域敏感频带的影响。
- 实验表明：直接丢弃高/低频特征会导致性能下降，而通过交叉注意力提取中频信息可有效保留域不变特征。

## 7. 优点
- **创新性**：将频域分解与元学习结合，提出针对域敏感频带的反向注意力机制，思路清晰且有效。
- **方法简洁**：整体架构清晰，FAM和MSF模块设计符合直觉，代码已开源。
- **实验全面**：涵盖多个跨域场景，并进行了充分的消融和对比实验，结果显著领先。
- **实用性**：提出CD-FSMIS新任务，扩展了FSMIS在临床中真实场景的适用性。

## 8. 不足与局限
- **实验覆盖**：仅使用1-way 1-shot设置，未探讨更高shot或更多类别的情况（如5-shot），泛化性验证不够全面。
- **数据集规模**：所有数据集均较小（20~82个扫描），结论在更大规模多中心数据上的鲁棒性需进一步验证。
- **偏差风险**：粗预测生成依赖于超像素伪标签（来自ADNet），可能引入噪声；频带划分比例（3:4:3）仅通过实验确定，缺乏理论指导。
- **应用限制**：方法需要医学图像频域特性支撑，对自然图像或其它类型域偏移可能不适用；未考虑有监督预训练与全监督设置的对比。
- **未提及计算时间**：虽有GPU型号，但未报告训练/推理耗时，无法评估实际效率。

（完）
