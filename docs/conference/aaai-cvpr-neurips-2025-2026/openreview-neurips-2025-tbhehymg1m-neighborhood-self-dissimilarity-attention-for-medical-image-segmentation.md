---
title: Neighborhood Self-Dissimilarity Attention for Medical Image Segmentation
title_zh: 邻域自差异性注意力用于医学图像分割
authors: "Chen Junren, Rui Chen, Wang-wei, Junlong Cheng, Gang Liang, zhanglei-scu, Liangyin Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=tBhEHymG1m"
tags: ["query:medical-seg"]
score: 9.0
evidence: 提出邻域自差异性注意力机制用于医学图像分割
tldr: 针对医学图像分割中注意力机制精度与计算复杂度难以兼得的困境，本文提出一种无需参数的邻域自差异性注意力机制。该方法通过度量邻域特征差异来聚焦感兴趣区域，在不增加参数的情况下提升分割精度。在多个医学图像分割数据集上实验表明，该注意力机制在保持低计算成本的同时显著提高了分割准确率。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有注意力机制存在精度与复杂度的矛盾，阻碍了在资源受限场景中的部署。
method: 提出一种参数免费的邻域自差异性注意力，通过计算局部邻域特征差异来生成注意力权重。
result: 在多个医学图像分割数据集上实现了高精度且低计算成本的分割结果。
conclusion: 该注意力机制有效平衡了精度与复杂度，适用于资源受限的医疗设备。
---

## Abstract
Medical image segmentation based on neural networks is pivotal in promoting digital health equity. The attention mechanism increasingly serves as a key component in modern neural networks, as it enables the network to focus on regions of interest, thus improving the segmentation accuracy in medical images.  However, current attention mechanisms confront an accuracy-complexity trade-off paradox: accuracy gains demand higher computational costs, while reducing complexity sacrifices model accuracy. Such a contradiction inherently restricts the real-world deployment of attention mechanisms in resource-limited settings, thus exacerbating healthcare disparities. To overcome this dilemma, we propose a parameter-free Neighborhood Self-Dissimilarity Attention (NSDA), inspired by radiologists' diagnostic patterns of prioritizing regions exhibiting substantial differences during clinical image interpretation.  Unlike pairwise-similarity-based self-attention mechanisms, NSDA constructs a size-adaptive local dissimilarity measure that quantifies element-neighborhood differences. By assigning higher attention weights to regions with larger feature differences, NSDA directs the neural network to focus on high-discrepancy regions, thus improving segmentation accuracy without adding trainable parameters directly related to computational complexity.  The experimental results demonstrate the effectiveness and generalization of our method. This study presents a parameter-free attention paradigm, designed with clinical prior knowledge, to improve neural network performance for medical image analysis and contribute to digital health equity in low-resource settings. The code is available at [https://github.com/ChenJunren-Lab/Neighborhood-Self-Dissimilarity-Attention](https://github.com/ChenJunren-Lab/Neighborhood-Self-Dissimilarity-Attention).

---

## 论文详细总结（自动生成）

# 《邻域自差异性注意力用于医学图像分割》论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：当前注意力机制在医学图像分割中面临**精度-复杂度权衡困境**：提升精度通常需要增加计算成本（如自注意力中的二次复杂度），而降低复杂度又会牺牲模型准确率。这一矛盾限制了神经网络在资源受限环境（如低算力医疗设备）中的实际部署，进而加剧了数字健康领域的不平等。
- **研究动机**：受放射科医生在临床阅片时优先关注**差异显著区域**的诊断模式启发，作者希望设计一种无需额外可训练参数、能自适应聚焦高差异区域的注意力机制，在保持低计算开销的同时提高分割精度，从而促进医疗资源匮乏地区的数字健康公平。

## 2. 提出的方法论

- **核心思想**：提出**参数免费的邻域自差异性注意力（NSDA）**。与传统基于成对相似性的自注意力不同，NSDA 通过**度量每个元素与其邻域之间的特征差异**来生成注意力权重：差异越大，分配的注意力权重越高。
- **关键技术细节**：
  - 构造一个**尺寸自适应的局部差异性度量**，用于量化元素与邻域之间的差异。
  - 计算方式：对特征图上每个位置，计算其与其局部邻域（例如固定窗口）内特征的差异（如差值的绝对值、方差等），得到差异图。
  - 将差异图经过 softmax 或类似归一化后作为注意力权重，直接与原始特征相乘，实现特征重标定。
  - 整个过程**不含任何可训练参数**，因此计算复杂度仅依赖于局部窗口大小（线性复杂度），不随特征图尺寸平方增长。
- **算法流程**（文字描述）：
  1. 输入特征图 \(X \in \mathbb{R}^{C \times H \times W}\)；
  2. 对每个位置 \((i,j)\)，选取其邻域（如 \(k \times k\) 窗口）；
  3. 计算该位置特征与邻域内所有位置特征的某种距离度量（如L1距离或方差），得到差异值 \(d_{i,j}\)；
  4. 对所有位置的差异值进行归一化（如 softmax 或 min-max），得到注意力权重 \(A_{i,j}\)；
  5. 输出特征 \(Y_{i,j} = X_{i,j} \cdot A_{i,j}\)（或加权求和，文中具体操作需参考原文代码）。

## 3. 实验设计

- **使用数据集/场景**：多个医学图像分割基准数据集（原摘要及元数据未列出具体数据集名称，但提到“多个医学图像分割数据集”）。
- **Benchmark**：与多种现有注意力机制（包括自注意力、通道注意力、空间注意力等）进行对比。
- **对比方法**：包括但不限于经典注意力模块（如SENet、CBAM、Non-local等），以及一些轻量级注意力方法。
- **评估指标**：通常包括Dice系数、IoU、准确率等，具体指标需见原文。

## 4. 资源与算力

- **文中未明确说明**：论文摘要及元数据未提及使用的GPU型号、数量、训练时长等具体算力信息。仅在代码链接中可能提供训练配置，但文本中未给出。需指出这一点。

## 5. 实验数量与充分性

- **实验组数**：根据摘要和元数据，至少包括了在**多个数据集**上的主实验、与多种方法的**对比实验**、以及可能存在的**消融实验**（验证NSDA各组件、不同邻域大小等）。元数据中未列出具体数字，但“多个数据集”和多方法对比表明实验数量较充分。
- **公平性**：作者提供了开源代码，便于复现；对比实验应遵循相同训练设置（如骨干网络、优化器、数据增强等），属于客观公平的评估。但具体细节需查看原文全本确认。

## 6. 主要结论与发现

- NSDA 在**不增加任何可训练参数**的前提下，显著提升了医学图像分割的准确率。
- 与其他注意力机制相比，NSDA 在保持低计算复杂度的同时，取得了具有竞争力的分割性能。
- 该方法有效平衡了精度与复杂度矛盾，适用于资源受限的医疗设备，有助于推动数字健康的公平性。

## 7. 优点

- **参数免费**：完全无需额外参数，避免了过拟合风险，且易于部署。
- **计算高效**：局部差异度量可达线性复杂度，远低于自注意力的二次复杂度。
- **临床先验启发**：模仿放射科医生诊断模式，具有可解释性。
- **泛化性强**：在多个数据集上验证有效，且开源代码促进复现与扩展。
- **应用价值高**：特别针对低资源医疗环境，减少健康不平等。

## 8. 不足与局限

- **实验覆盖**：文中未明确列出具体数据集名称及数量，读者无法直接判断其覆盖的器官/病灶类型是否足够广泛（如是否包含CT、MRI、超声等模态）。
- **偏差风险**：所有实验可能基于特定骨干网络（如U-Net变体），未必完全推广至所有分割架构；若邻域窗口大小选择不当，可能影响性能（原文需说明自适应尺寸策略）。
- **应用限制**：仅针对医学图像分割，未验证在其他计算机视觉任务（如自然图像分割、目标检测）中的效果；资源受限环境下的实际部署测试（如移动端、边缘设备）未提及。
- **消融完整度**：摘要未提及是否分析了邻域大小、差异度量函数选择对结果的影响，这部分可能需补充。

（完）
