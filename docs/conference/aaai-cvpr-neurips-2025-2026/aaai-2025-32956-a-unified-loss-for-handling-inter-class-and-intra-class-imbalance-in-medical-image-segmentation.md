---
title: A Unified Loss for Handling Inter-Class and Intra-Class Imbalance in Medical Image Segmentation
title_zh: 处理医学图像分割中类别间和类别内不平衡的统一损失函数
authors: "Feilong Xu, Feiyang Yang, Xiongfei Li, Xiaoli Zhang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32956/35111"
tags: ["query:medical-seg"]
score: 10.0
evidence: 针对医学图像分割类别不平衡的损失函数
tldr: 针对医学图像分割中常见的类别间和类别内不平衡问题，提出了一种统一损失函数Balance Loss，通过分别控制多数类的难样本挖掘和易样本权重，有效兼顾两类不平衡，在多个分割任务上优于现有损失函数。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 现有损失函数通常只针对一种不平衡，难以同时处理两类不平衡且泛化性差。
method: 提出Inter-class和Intra-class平衡损失，再统一为Balance Loss，结合频率和难易程度调整权重。
result: 在多个医学分割基准上，Balance Loss显著提升了分割准确性和鲁棒性。
conclusion: 统一处理两类不平衡是提升医学分割损失函数泛化能力的关键。
---

## Abstract
In utilizing deep learning techniques for medical image segmentation, two types of imbalance issues are observed: inter-class imbalance between majority and minority classes and intra-class imbalance between easy and hard samples. However, existing loss functions typically confuse these issues, leading to enhancements that cater to only one aspect. Moreover, loss functions optimized for specific tasks often exhibit limited generalizability. To address these issues, we propose Inter-class and Intra-class Balance loss, as well as a unified loss termed Balance loss. The Inter-class Balance loss controls the extent of hard sample mining for majority class samples by considering the frequency of minority classes present in each input image. This approach requires no manual adjustment weights and adapts automatically to different datasets. The Intra-class Balance loss enhances the network's ability to learn from hard samples by performing mining on hard samples within each class. We evaluate our loss functions on five segmentation tasks with varying degrees of class imbalance. The experimental results show that our proposed Balance loss enhances segmentation performance compared with the current loss functions and exhibits superior robustness.

---

## 论文详细总结（自动生成）

# 论文总结：A Unified Loss for Handling Inter-Class and Intra-Class Imbalance in Medical Image Segmentation

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：医学图像分割中存在两类不平衡问题：**类别间不平衡**（多数类与少数类样本数量差异，如背景占绝大部分）和**类别内不平衡**（同一类别中简单样本与困难样本的差异，如边缘、模糊区域）。现有损失函数通常只针对其中一类，导致性能提升受限，且针对特定任务设计的损失函数泛化性差（在轻度不平衡任务上甚至不如经典交叉熵）。
- **动机**：需要一种能同时处理两类不平衡、且无需手动调整权重、能自适应不同不平衡程度的统一损失函数。
- **整体含义**：通过分析FP/FN计算偏差的根源，提出Inter-class Balance Loss (Inter-CBL)和Intra-class Balance Loss (Intra-CBL)，并组合为Balance Loss，在五个分割任务上验证其有效性和鲁棒性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：
  - **Inter-CBL**：基于对FP/FN计算偏差的理论分析（偏差大小与多数类和少数类样本数量之差正相关，而非比例），在多数类中仅保留与少数类样本数相同的**最难像素**（损失最大的前f个），赋予与少数类相同的权重，其余多数类像素权重降低。无需手动调权，自动适应数据集。
  - **Intra-CBL**：改进TopK loss，在每个类别内部进行困难样本挖掘，但**不丢弃简单样本**，而是动态调整权重：简单像素越多，其权重自动降低，困难像素权重升高，从而稳定训练。
  - **Balance Loss**：通过线性加权组合Inter-CBL和Intra-CBL：`BL = α * Intra-CBL + (1-α) * Inter-CBL`。
  - **两阶段训练策略**：初期仅使用Intra-CBL（因为Inter-CBL在未收敛时不稳定），待网络收敛到一定程度后再切换到Balance Loss。收敛判断标准：batch内超过一半图像的前景和背景的第f个最佳概率值大于阈值t。

- **关键技术细节**：
  - Inter-CBL公式：对前景所有像素计算CE，对背景中前f个最难点计算CE，其余背景像素权重降低（等效于WCE的变体）。
  - Intra-CBL公式：对每个类别分别计算困难像素和简单像素的加权损失，简单像素权重随训练动态减小。
  - 默认参数：`t=0.9`, `α=0.5`。

## 3. 实验设计
- **数据集与场景**（5个分割任务，4个数据集）：
  - **CVC-ClinicDB**（息肉分割，轻度不平衡，前景比例9.3%）
  - **BUSI**（乳腺超声肿瘤分割，轻度不平衡，前景比例9.4%）
  - **KiTS19**（肾脏和肾脏肿瘤分割，高度不平衡，肾脏前景3.1%，肿瘤1.8%）
  - **NIH**（胰腺分割，高度不平衡，前景0.4%）
- **基准方法**：CE、Focal Loss、Dice Loss、Tversky Loss、Combo Loss、Unified Focal Loss、RCE Loss。所有对比方法均使用原文推荐的最优超参数。
- **评估指标**：Dice Similarity Coefficient (DSC) 和 Hausdorff Distance (HD)。
- **网络与训练**：U-Net (2D)，SGD优化器，初始学习率0.1，batch size=24，早停策略（验证集10个epoch无改善则学习率减半，20个epoch无改善则停止）。数据增强：随机旋转、翻转、弹性变形。每个实验进行3次独立运行取均值和标准差。

## 4. 资源与算力
- **文中明确信息**：使用NVIDIA GeForce RTX 4090 GPU，PyTorch框架。
- **未明确信息**：未提及使用的GPU数量、训练总时长、能源消耗等详细算力资源。因此无法评估训练成本。

## 5. 实验数量与充分性
- **实验数量**：
  - 主对比实验：在5个任务上比较了7种损失函数，每个任务报告DSC和HD（3次运行）。
  - 消融实验1：验证Inter-CBL有效性（与各任务次优方法对比）。
  - 消融实验2：验证Intra-CBL有效性（与TopK loss的两种变体在不同阈值下对比）。
  - 消融实验3：α参数敏感性分析（在0.1-0.9范围内）。
- **充分性**：
  - 覆盖了从轻度到高度不平衡的多种任务，对比方法全面（包括交叉熵族、Dice族、复合损失）。实验设计公平（统一网络、统一训练设置，对比方法使用官方推荐超参数）。消融实验验证了各组件贡献和超参数鲁棒性。但未涉及多类分割任务，也未与更先进的网络结构（如Transformer）结合。

## 6. 主要结论与发现
- **Balance Loss在所有任务上均取得最优或接近最优的DSC和HD**，尤其在高度不平衡任务（肾脏肿瘤、胰腺）上提升显著，在轻度不平衡任务（息肉、乳腺）上也优于或持平于其他损失。
- **Inter-CBL单独使用即可超过大多数现有损失**，证明其对类别间不平衡的有效性。
- **Intra-CBL比TopK loss更鲁棒**，避免了TopK因参数选择不当导致训练崩溃或性能下降。
- **α在0.1-0.9范围内均能保持良好性能**，接近0.5时最佳，表明平衡两类不平衡的重要性。
- **理论分析得到实证支持**：FP/FN计算偏差是模型偏向多数类的根因，Inter-CBL通过减轻该偏差提升性能。

## 7. 优点
- **理论创新**：揭示FP/FN计算偏差与类别间样本数量差的关系，为损失函数设计提供新视角。
- **方法简洁有效**：无需手动调权，自动适应不同不平衡程度；两阶段训练策略确保稳定性。
- **实验全面且客观**：覆盖5个任务、多种不平衡程度，对比7种主流损失，消融实验完整，结果具有统计显著性（3次运行标准差小）。
- **泛化性强**：在轻度与高度不平衡任务上均表现优异，克服了现有损失函数“偏科”问题。
- **代码可复现性**：超参数默认设置简单（t=0.9，α=0.5），易于推广。

## 8. 不足与局限
- **两阶段训练复杂度**：需要判断收敛时刻，增加训练流程的复杂性；未来可探索单阶段训练。
- **仅验证2D分割且限于U-Net**：未在多类分割（如多个器官）、3D分割或更先进网络（如nnU-Net、Transformer）上测试，泛化性有待进一步验证。
- **超参数t和α仍有一定影响**：虽然鲁棒性较强，但最优值可能随任务变化，需轻微调整。
- **算力细节缺失**：未报告训练时长、GPU数量等，难以评估计算成本。
- **偏差风险**：仅在公共数据集上实验，未涉及真实临床数据；且仅使用DSC和HD指标，缺少临床相关评估（如边界误差、体积一致性等）。

（完）
