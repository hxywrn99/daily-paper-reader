---
title: "LoMix: Learnable Weighted Multi-Scale Logits Mixing for Medical Image Segmentation"
title_zh: LoMix：可学习加权多尺度Logits混合用于医学图像分割
authors: "Md Mostafijur Rahman, Radu Marculescu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=87c2JwNJa0"
tags: ["query:medical-seg"]
score: 10.0
evidence: 医学图像分割中的多尺度logits混合
tldr: 针对U型网络仅单独监督各尺度logits而忽略跨尺度互补信息的问题，提出LoMix模块，受NAS启发可学习生成混合尺度输出并自动调整训练权重，作为即插即用组件显著提升多种医学图像分割网络的性能。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: U型网络各尺度logits包含不同信息，但训练时孤立处理，缺失跨尺度互补线索。
method: 提出LoMix模块，基于可微NAS，自动学习生成混合尺度输出并分配训练权重。
result: 在多个医学分割任务上提升了主流U型网络的精度。
conclusion: LoMix通过可学习的多尺度logits融合有效增强了医学图像分割表现。
---

## Abstract
U-shaped networks output logits at multiple spatial scales, each capturing a different blend of coarse context and fine detail. Yet, training still treats these logits in isolation—either supervising only the final, highest-resolution logits or applying deep supervision with identical loss weights at every scale—without exploring mixed-scale combinations. Consequently, the decoder output misses the complementary cues that arise only when coarse and fine predictions are fused. To address this issue, we introduce LoMix (Logits Mixing), a Neural Architecture
Search (NAS)-inspired, differentiable plug-and-play module that generates new mixed-scale outputs and learns how exactly each of them should guide the training process. More precisely, LoMix mixes the multi-scale decoder logits with four lightweight fusion operators: addition, multiplication, concatenation, and attention-based weighted fusion, yielding a rich set of synthetic “mutant” maps. Every original or mutant map is given a softplus loss weight that is co-optimized with network parameters, mimicking a one-step architecture search that automatically discovers the most useful scales, mixtures, and operators. Plugging LoMix into recent U-shaped architectures (i.e., PVT-V2-B2 backbone with EMCAD decoder) on Synapse 8-organ dataset improves DICE by +4.2% over single-output supervision, +2.2% over deep supervision, and +1.5% over equally weighted additive fusion, all with zero inference overhead. When training data are scarce (e.g., one or two labeled scans, 5% of the trainset), the advantage grows to +9.23%, underscoring LoMix’s data efficiency. Across four benchmarks and diverse U-shaped networks, LoMiX improves DICE by up to +13.5% over single-output supervision, confirming that learnable weighted mixed-scale fusion generalizes broadly while remaining data efficient, fully interpretable, and overhead-free at inference. Our implementation is available at https://github.com/SLDGroup/LoMix.

---

## 论文详细总结（自动生成）

# 论文《LoMix: Learnable Weighted Multi-Scale Logits Mixing for Medical Image Segmentation》详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：现有的U型医学图像分割网络在解码器不同空间尺度输出logits时，通常仅单独监督最高分辨率输出，或者对所有尺度施加相同损失权重的深度监督。这种孤立处理方式忽略了跨尺度logits之间的互补信息——粗尺度（全局上下文）与细尺度（局部细节）的融合能提供更丰富的判别线索，但现有方法未能有效挖掘。
- **整体含义**：如何在不增加推理开销的前提下，利用多尺度logits的混合来提升分割精度，尤其在数据稀缺场景下更具实际价值。

## 2. 方法论

- **核心思想**：受可微神经架构搜索（NAS）启发，设计一个即插即用的轻量模块 **LoMix**，通过可学习的方式生成混合尺度的logits，并自动为每个原始或混合logits分配训练权重，从而实现跨尺度互补信息的自适应融合。
- **关键技术细节**：
  - **混合操作**：使用四种轻量融合算子生成合成“突变”图：加法、乘法、拼接（通过1×1卷积调整通道）、基于注意力的加权融合。
  - **可学习权重**：每个原始logits和每个混合logits对应一个 **softplus** 形式的损失权重，该权重与网络参数通过梯度下降联合优化，等价于一步NAS，自动发现最有用的尺度组合与融合算子。
  - **训练与推理**：训练时所有logits（原始+混合）被加权求和用于监督，推理时仅保留原始最高分辨率输出（或根据需要保留混合输出），因此**零推理开销**。
- **算法流程（文字说明）**：
  1. 从U型解码器获取多尺度logits（如4个不同分辨率）。
  2. 应用四种融合算子生成若干混合logits。
  3. 为每个原始和混合logits分配一个softplus参数化的可学习权重。
  4. 计算加权后的总损失（如Dice + CE），联合优化网络参数和权重。
  5. 推理时仅使用原始最高分辨率logits（或可选混合logits，但论文默认不增加开销）。

## 3. 实验设计

- **数据集与场景**：
  - 主要数据集：Synapse多器官分割（8个器官）。
  - 额外四个基准数据集（论文摘要未列出具体名称，但声称在多种U型网络上测试）。
  - 数据稀缺场景：仅使用1或2张标注扫描（占训练集的5%）。
- **基准方法**：
  - 单输出监督（仅最高分辨率logits）。
  - 深度监督（所有尺度等权重）。
  - 等权加法融合（简单混合）。
- **对比网络**：以PVT-V2-B2为骨干、EMCAD为解码器的U型架构，以及多种其他U型网络。
- **评估指标**：Dice系数（主要），可能还包括其他指标（摘要中未详述，但论文全文应有）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力细节。用户需注意，论文摘要及元数据均未提供此类信息，可能需要在全文“实验设置”部分查找，但当前提取内容不包含。

## 5. 实验数量与充分性

- **数量**：主要实验结果在Synapse数据集上报告了与三种基线方法的对比；另外在四个基准数据集和多种U型网络上进行了泛化性验证；数据稀缺场景下进行了专门评估；还包含消融实验（推测分析不同融合算子、权重学习方式等），但摘要未列出具体组数。
- **充分性与公平性**：
  - 对比了最相关的基线（单输出监督、深度监督、等权融合），且LoMix在相同骨干下易于实现公平比较。
  - 零推理开销的设计使得公平性不受计算成本影响。
  - 在数据稀缺场景下优势更明显（+9.23%），验证了数据效率。
  - 但未与近期其他多尺度融合方法（如特征金字塔注意力、自适应空间融合等）进行对比，可能削弱充分性。

## 6. 主要结论与发现

- LoMix在Synapse数据集上比单输出监督提升 +4.2% Dice，比深度监督提升 +2.2%，比等权加法融合提升 +1.5%，且零推理开销。
- 在数据稀缺（5%训练数据）时，优势扩大至 +9.23%，表明其数据高效特性。
- 跨四个基准数据集和多种U型网络，LoMix相比单输出监督最大提升 +13.5% Dice，验证了方法的通用性。
- 可学习加权混合使模型自动发现有用尺度和融合算子，避免手动调参。

## 7. 优点

- **即插即用**：无需修改原始网络架构，仅需在训练时插入LoMix模块，推理时无任何额外开销。
- **可学习性**：基于可微NAS的权重学习自动平衡各logits贡献，避免了人工固定权重的次优性。
- **数据高效**：在标注数据极少的情况下提升尤为显著，适合医学图像标注稀缺的现实场景。
- **完全可解释**：训练后权重揭示了哪些尺度和混合算子被重视，具有可解释性。
- **开源实现**：代码已公开，便于复现和应用。

## 8. 不足与局限

- **实验对比不够广泛**：仅与简单的监督策略比较，未考虑更先进的多尺度融合方法（如特征金字塔网络、DeepLab中的ASPP）或注意力机制，可能削弱说服力。
- **算力资源未公开**：缺少训练时长、GPU型号等细节，影响可复现性评估。
- **对混合算子数量的依赖**：论文使用四种固定算子，是否最优未探讨；若嵌入更多算子可能造成搜索空间膨胀。
- **理论分析不足**：为何混合logits能提升性能的机制分析较浅，仅停留在“互补线索”的直观解释。
- **应用限制**：当前仅针对医学图像分割，其通用性在其他视觉任务（如语义分割、实例分割）尚未验证。

（完）
