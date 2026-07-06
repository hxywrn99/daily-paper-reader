---
title: A Hybrid Space Model for Misaligned Multi-modality Image Fusion
title_zh: 用于未对齐多模态图像融合的混合空间模型
authors: "Yi Xiao, Jia Wang, Zhu Liu, Di Wang, Jinyuan Liu, Risheng Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38078/42040"
tags: ["query:image-fusion"]
score: 9.0
evidence: 多模态图像融合与混合空间建模
tldr: 多模态图像融合中未对齐和几何变形导致伪影。现有方法局限于欧氏空间，忽略分层结构。本文提出统一框架，联合优化欧氏空间和双曲空间，同时保留局部细节和层次关系。实验证明该模型在多种多模态融合任务上有效抑制了伪影，提升了融合质量。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38078/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 592, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38078/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1841, \"height\": 962, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38078/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1835, \"height\": 332, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38078/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1841, \"height\": 337, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38078/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 876, \"height\": 302, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38078/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 875, \"height\": 595, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38078/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1841, \"height\": 391, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38078/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 864, \"height\": 448, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38078/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 884, \"height\": 340, \"label\": \"Table\"}]"
motivation: 现有方法忽略多模态表示中的层次结构，且无法处理未对齐问题。
method: 联合优化欧氏空间和双曲空间，在统一框架内处理局部细节与层次结构。
result: 在多种多模态融合任务上有效抑制伪影，提升融合质量。
conclusion: 混合空间模型为多模态融合提供了新的几何视角，增强了鲁棒性。
---

## Abstract
Infrared and visible image fusion aims to integrate complementary information, such as thermal saliency from infrared imagery and fine-grained texture details from visible imagery. However, real-world multi-modal misalignment and geometric deformation often introduce severe artifacts. Most existing methods focus on feature extraction within Euclidean space, thereby neglecting the inherent hierarchical structures embedded in multimodal representations. While Euclidean space excels at preserving local structural details and supporting efficient computation, hyperbolic space is naturally suited for modeling hierarchical relationships due to its geometric properties. Building upon these observations, this paper proposes a unified framework that jointly optimizes image registration and fusion through a dual-space architecture. This architecture synergistically combines the local fidelity of Euclidean geometry with the hierarchical modeling capability of hyperbolic geometry to enhance multimodal representation learning. Specifically, this paper introduces Hyperbolic Coupled Contrastive Learning Optimization (HCCLO), which aligns and optimizes the hierarchical structures of infrared and visible embeddings in hyperbolic space. Moreover, this paper designs a task-adaptive dual-space features fusion mechanism, which dynamically balances and fuses Euclidean local features with hyperbolic hierarchical representations, thereby improving adaptability for downstream tasks. Extensive experiments on misaligned multimodal datasets demonstrate that our method achieves state-of-the-art performance, while effectively capturing both spatial dependencies and hierarchical semantics.

---

## 论文详细总结（自动生成）

# 用于未对齐多模态图像融合的混合空间模型——论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究任务**：红外与可见光图像融合，旨在结合红外图像的热显著性信息和可见光图像的细粒度纹理细节。
- **现有局限**：真实场景中多模态图像往往存在未对齐和几何变形，导致融合结果产生严重伪影；现有方法大多局限于欧氏空间进行特征提取，忽略了多模态表示中固有的层次结构（如前景与背景的语义层次）。
- **动机**：欧氏空间擅长保留局部结构细节、计算高效，而双曲空间因其负曲率几何特性天然适合建模层次关系。作者提出将两者结合，构建统一的配准与融合框架，以同时处理未对齐问题和层次结构建模。

## 2. 方法论：核心思想、关键技术细节
### 核心思想
- 提出**双空间架构（Dual-Space Architecture）**，联合优化图像配准与融合。利用双曲空间的层次建模能力弥补欧氏空间的不足，通过对比学习对齐跨模态层次结构，并自适应融合两种空间特征。

### 关键技术细节
1. **整体流程**：
   - 共享编码器提取模态不变特征；
   - 全局变换矩阵预测器进行粗配准；
   - 双空间编码器：欧氏编码器（Transformer-CNN + 多窗口注意力）提取局部细节；双曲编码器（基于Poincaré球模型）提取层次特征，并在其中应用**HCCLO**；
   - 任务自适应双空间特征融合（**TDFF**）得到融合表示；
   - 配准解码器（多尺度密集子网络）和融合解码器（双通道注意力）输出最终结果。

2. **双曲空间建模**：
   - 使用**Poincaré球模型**，曲率c可训练；
   - 定义Möbius加法、双曲距离、指数映射/对数映射等运算。

3. **Hyperbolic Coupled Contrastive Learning Optimization (HCCLO)**：
   - 利用SAM生成的显著性掩码M将特征分为前景和背景；
   - 红外分支对比损失：\( L_{ir} = \max(d_p(I, I^+) - d_p(I, V^+) + \varepsilon, 0) \)（将红外前景与可见光前景拉远）；
   - 可见光分支对比损失：\( L_{vis} = \max(d_p(V, V^-) - d_p(V, I^-) + \varepsilon, 0) \)（将可见光背景与红外背景拉近）；
   - 通过在双曲空间进行对比学习对齐跨模态层次结构。

4. **Task-adaptive Dual-Space Features Fusion (TDFF)**：
   - 将双曲特征 \(H_{ir+vi}\) 和欧氏特征 \(E_{ir+vi}\) 分别经过1×1卷积压缩；
   - 拼接后通过Softmax生成两个空间权重图 \(w_0, w_1\)，满足 \(w_0 + w_1 = 1\)；
   - 最终融合特征：\(F_{fused} = w_0 \odot H_{ir+vi} + w_1 \odot E_{ir+vi}\)。

## 3. 实验设计
### 数据集
- **训练集**：RoadScene、M3FD、MSIFT（随机采样128×128块）。
- **测试集**：RoadScene、M3FD、TNO（24张图像）。
- **未对齐模拟**：对红外图像施加仿射变换和弹性变换（平移 [0.0, 0.01]px，弹性σ=32，核97–101）以模拟真实非刚性变形。

### 评价指标
- **配准**：归一化互相关（NCC）、互信息（MI）。
- **融合**：QNCIE、MI、VIF、Qabf（数值越高越好）。

### 对比方法
- 配准：CrossRAFT、UMFusion、SuperFusion、MURF、IMF、MulFS-CAP（部分方法也参与融合）。
- 融合（同时与配准结合）：CDDFuse、CoCoNet（使用CrossRAFT作为预配准）以及其他上述方法。
- 总计对比了8种以上先进方法。

## 4. 资源与算力
- **GPU**：NVIDIA V100（单卡）。
- **训练轮数**：1200 epochs。
- **优化器**：Adam，初始学习率1e-4，每300 epoch衰减。
- **未提及**：具体GPU数量、显存占用、数据并行策略等。

## 5. 实验数量与充分性
- **主要实验**：
  - 配准结果定量对比（表2，TNO、RoadScene两个数据集）。
  - 融合结果定量对比（表1，TNO、M3FD、RoadScene三个数据集）。
  - 融合结果定性对比（图4，可视化RoadScene）。
  - 配准误差可视化（图3，RoadScene）。
- **消融实验**：
  - HCCLO有效性（表3及图5）。
  - TDFF有效性（表3及图6）：对比单独使用欧氏、单独使用双曲、无TDFF双空间简单组合等。
- **鲁棒性验证**：增加变形强度测试（文中提及但未见详细表格），说明模型对极端几何畸变的鲁棒性。
- **评价**：实验设计较充分，覆盖多个标准数据集和多种先进对比方法，消融实验验证了各组件的必要性。但消融实验仅在RoadScene上展示数值，缺少在TNO和M3FD上的消融结果；真实未对齐场景测试不足（仅使用模拟变形）。

## 6. 主要结论与发现
- 提出的HMMF框架在所有评价指标上均达到**SOTA**，尤其在跨数据集上（如TNO上QNCIE=0.9011，MI=5.8358）显著领先。
- 有效消除由未对齐引起的重影和边缘伪影，同时保留更多背景纹理细节，融合结果更符合人类视觉感知。
- 双曲空间有助于建模跨模态层次结构，HCCLO和TDFF对提升配准精度和融合质量均有明显贡献。

## 7. 优点
- **创新性**：首次将双曲空间引入红外-可见光图像融合与配准任务，利用其层次建模能力解决未对齐问题。
- **方法论优势**：
  - 联合优化配准与融合，而非传统两阶段流水线，使融合反馈配准。
  - HCCLO利用SAM自动生成语义先验，无需人工标注，设计简洁有效。
  - TDFF实现自适应特征融合，动态权衡双曲和欧氏信息，适应不同任务。
- **实验扎实**：在多个数据集、多种指标、多类方法上进行广泛对比，消融实验全面。

## 8. 不足与局限
- **模态覆盖有限**：仅针对红外与可见光融合，未验证在其他多模态任务（如医学、遥感）上的泛化性。
- **计算开销**：双曲空间运算（指数映射、对比损失等）可能带来额外计算负担，文中未报告训练/推理速度或参数量。
- **真实数据缺失**：所有未对齐数据均为模拟生成，缺乏在真实未对齐场景下的测试（如实际硬件采集的未对齐图像）。
- **消融实验局限**：消融分析仅在RoadScene数据集上给出定量结果，未在TNO或M3FD上呈现，降低了结论的普适性。
- **下游任务评估缺失**：未在语义分割、目标检测等下游任务上验证融合结果的有效性，尽管文中提及潜在受益。

（完）
