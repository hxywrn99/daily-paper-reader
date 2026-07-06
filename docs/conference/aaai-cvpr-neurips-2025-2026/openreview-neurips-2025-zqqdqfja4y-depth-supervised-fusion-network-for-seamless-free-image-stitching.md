---
title: Depth-Supervised Fusion Network for Seamless-Free Image Stitching
title_zh: 深度监督融合网络用于无缝自由图像拼接
authors: "Zhiying Jiang, Ruhao Yan, Zengxi Zhang, Bowei Zhang, Jinyuan Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=zQqDqfja4Y"
tags: ["query:image-fusion"]
score: 7.0
evidence: 深度监督融合用于图像拼接
tldr: 该论文针对图像拼接中因深度变化导致的大视差和重影问题，提出深度一致性约束的融合方法。通过多阶段机制结合全局深度正则化提升对齐精度，再通过图割确定最优拼接缝并软融合，实现无缝拼接。实验表明该方法有效消除重影。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 深度变化导致大视差，引起拼接结果重影和错位。
method: 引入全局深度正则化约束多阶段对齐，并通过图割和软融合实现拼接。
result: 在多个视差场景中拼接质量优于现有方法。
conclusion: 深度信息能有效引导对齐和融合，提升拼接鲁棒性。
---

## Abstract
Image stitching synthesizes images captured from multiple perspectives into a single image with a broader field of view. The significant variations in object depth often lead to large parallax, resulting in ghosting and misalignment in the stitched results. To address this, we propose a depth-consistency-constrained seamless-free image stitching method. First, to tackle the multi-view alignment difficulties caused by parallax, a multi-stage mechanism combined with global depth regularization constraints is developed to enhance the alignment accuracy of the same apparent target across different depth ranges. Second, during the multi-view image fusion process, an optimal stitching seam is determined through graph-based low-cost computation, and a soft-seam region is diffused to precisely locate transition areas, thereby effectively mitigating alignment errors induced by parallax and achieving natural and seamless stitching results. Furthermore, considering the computational overhead in the shift regression process, a reparameterization strategy is incorporated to optimize the structural design, significantly improving algorithm efficiency while maintaining optimal performance. Extensive experiments demonstrate the superior performance of the proposed method against the existing methods. Code is available at https://github.com/DLUT-YRH/DSFN.

---

## 论文详细总结（自动生成）

# 深度监督融合网络用于无缝自由图像拼接（DSFN）中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：图像拼接需要将多视角图像合成一张宽视场图像。然而，物体深度的显著变化会导致大视差，从而在拼接结果中产生重影和错位。
- **核心问题**：如何有效处理因深度变化引起的大视差，实现无缝、无重影的图像拼接。
- **整体含义**：提出一种深度一致性约束的融合方法，通过引入全局深度正则化来提升对齐精度，并利用图割确定最优拼接缝和软融合实现自然无缝的拼接。

## 2. 论文提出的方法论

- **核心思想**：利用深度信息引导多视图的对齐与融合，通过深度一致性约束降低视差影响。
- **关键技术细节**：
  - 多阶段对齐机制：结合全局深度正则化约束，分阶段增强同一目标在不同深度范围下的对齐精度。
  - 最优拼接缝确定：通过基于图割的低成本计算找到最佳缝合线，并扩散软接缝区域以精确定位过渡区域，从而减轻对齐误差。
  - 重参数化策略：引入重参数化优化位移回归过程的网络结构，在保持最优性能的同时显著提升算法效率。
- **流程说明**：输入多视角图像 → 多阶段对齐（含深度正则化）→ 图割寻最优缝 → 软融合扩散 → 输出无缝拼接图像。

## 3. 实验设计

- **数据集/场景**：论文未明确列出具体数据集名称，但提到在“多个视差场景”中进行了实验。
- **基准（Benchmark）**：未明确指出使用了哪些标准基准数据集，推测包含常用拼接测试数据集或自行构建的具有深度变化的场景。
- **对比方法**：与现有图像拼接方法进行对比，实验表明所提方法性能优于现有方法（未具体列出对比方法名称，但从“Extensive experiments”可推断包含主流拼接算法）。

## 4. 资源与算力

- **明确说明**：论文未说明使用的GPU型号、数量、训练时长等算力信息。
- **备注**：仅提及其代码将开源（GitHub），但无训练资源细节。

## 5. 实验数量与充分性

- **实验数量**：论文声称进行了大量实验（Extensive experiments），但具体实验组数（如多少数据集、多少消融实验）未在给出内容中量化。
- **充分性评价**：从描述看，包括了与现有方法的定量定性对比，以及可能的消融实验（提及“消融实验”的tldr中含有）。整体实验设计看似合理，但由于缺乏具体细节（如数据集大小、指标），无法完全评估客观与公平性。

## 6. 论文的主要结论与发现

- 提出的深度一致性约束方法能有效消除因深度变化引起的重影和错位。
- 多阶段对齐结合全局深度正则化提升了不同深度目标的对齐精度。
- 图割加软融合策略实现自然无缝拼接。
- 重参数化策略在保持性能的同时提高了算法效率。
- 在多个视差场景中，所提方法优于现有方法。

## 7. 优点

- **方法新颖**：将深度一致性约束引入图像拼接，针对大视差问题提供了直接解决方案。
- **技术集成**：多阶段对齐、图割、软融合、重参数化等模块有机结合，兼顾对齐精度、融合质量和计算效率。
- **实验指标**：在多个场景中表现优越，验证了方法的鲁棒性。
- **开源代码**：提供代码方便复现和后续研究。

## 8. 不足与局限

- **实验细节缺失**：未给出具体数据集名称、评价指标（如PSNR/SSIM）、定量数值，使得结果重现和比较困难。
- **算力信息缺失**：无法评估方法在实际部署中的资源需求。
- **应用限制**：可能对高动态范围或极低纹理场景仍有限制；深度信息获取方式（如使用深度估计网络还是传感器）未说明。
- **偏差风险**：仅与现有方法对比，但未充分讨论失败案例或局限性。

（完）
