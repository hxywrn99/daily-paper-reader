---
title: "One Model for ALL: Low-Level Task Interaction Is a Key to Task-Agnostic Image Fusion"
title_zh: 通用模型：低级任务交互是实现任务无关图像融合的关键
authors: "Cheng, Chunyang, Xu, Tianyang, Feng, Zhenhua, Wu, Xiaojun, Tang, Zhangyong, Li, Hui, Zhang, Zeyang, Atito, Sara, Awais, Muhammad, Kittler, Josef"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Cheng_One_Model_for_ALL_Low-Level_Task_Interaction_Is_a_Key_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 利用低级视觉任务实现任务无关图像融合
tldr: 针对高级图像融合方法依赖复杂语义桥接、任务泛化性差的问题，提出利用低级视觉任务（如数字摄影融合）实现有效特征交互的新范式。通过像素级监督指导无监督多模态融合，GIFNet统一模型能处理多种融合任务，在未见场景下也保持高性能，显著提升了融合的通用性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 440, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 520, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 868, \"height\": 236, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1808, \"height\": 806, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 794, \"height\": 397, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 865, \"height\": 222, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 865, \"height\": 845, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 869, \"height\": 307, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 832, \"height\": 440, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1810, \"height\": 493, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 866, \"height\": 423, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-cheng-one-model-for-all-low-level-task-interaction-is-a-key-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 867, \"height\": 256, \"label\": \"Table\"}]"
motivation: 现有图像融合方法过度依赖高层任务语义，导致任务间交互困难、泛化能力受限。
method: 提出GIFNet，通过低级视觉任务交互和像素级监督学习共享特征，实现单模型多任务融合。
result: 在多种融合任务和未见场景上，GIFNet均取得优于现有方法的效果，展示了强大的泛化能力。
conclusion: 低级任务交互是构建通用图像融合模型的关键，为无监督多模态融合提供了新思路。
---

## Abstract
Advanced image fusion methods mostly prioritise high-level missions, where task interaction struggles with semantic gaps, requiring complex bridging mechanisms. In contrast, we propose to leverage low-level vision tasks from digital photography fusion, allowing for effective feature interaction through pixel-level supervision. This new paradigm provides strong guidance for unsupervised multimodal fusion without relying on abstract semantics, enhancing task-shared feature learning for broader applicability. Owning to the hybrid image features and enhanced universal representations, the proposed GIFNet supports diverse fusion tasks, achieving high performance across both seen and unseen scenarios with a single model. Uniquely, experimental results reveal that our framework also supports single-modality enhancement, offering superior flexibility for practical applications. Our code will be available at https://github.com/AWCXV/GIFNet.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义

现有的高级图像融合方法大多依赖高层视觉任务（如目标检测、语义分割）来提供监督信号，以指导多模态融合。这种范式存在三个关键问题：
- **语义鸿沟**：高层语义特征（如类别、形状）与融合所需的像素级细节之间存在本质差异，导致监督信号次优。
- **任务泛化性差**：每种融合任务需要单独训练模型，无法共享跨任务知识，部署成本高。
- **计算开销大**：引入大型高层模型（如ViLT、SAM）导致GFLOPs剧增，不适合移动设备。

论文的核心洞察是：**低级视觉任务（如数字摄影融合中的多聚焦、多曝光融合）可通过像素级监督提供更自然的特征交互，避免语义鸿沟**。基于此，作者提出了通用图像融合网络（GIFNet），仅用一个模型即可处理多种融合任务（已见和未见场景），并首次将融合技术扩展到单模态图像增强。

## 2. 论文提出的方法论

### 2.1 核心思想
利用低级任务交互代替高层语义交互。具体地，以**多模态融合（IVIF）**作为主任务，**数字摄影融合（MFIF）**作为辅助任务，通过交替训练和交叉融合门控机制（CFGM）实现特征交互。引入**重建任务（REC）**提取任务共享特征，并构建RGB联合数据集来缩小域差距。

### 2.2 关键技术细节
- **三分支架构**：
  - **主任务分支（Multi-Modal, MM）**：处理红外-可见光融合。
  - **辅助任务分支（Digital Photography, DP）**：处理多聚焦融合（MFIF）。
  - **重建分支（REC）**：自编码器结构，以共享RGB模态为重建目标，提取任务无关特征。
- **交叉融合门控机制（CFGM）**：基于SwinTransformer块，通过交替冻结/训练分支，实现主辅任务特征的自适应融合。公式为：
  - \( \hat{x}_m = \text{Self-Att}(x_m) \)
  - \( x_m = \hat{x}_m + \lambda \cdot \text{Cross-Att}(\hat{x}_m, x_a) \)
  其中λ为可学习参数，控制辅助任务的贡献。
- **损失函数**：
  - 公共损失 \( L_{pub} \)：SSIM + MSE，约束重建图像与可见光图像一致。
  - 私有损失：对于MM分支，采用信息加权损失（基于DenseNet梯度）；对于DP分支，使用MSE损失（因为有GT）。
- **RGB联合数据集**：从IVIF数据集LLVIP中通过合成模糊生成多聚焦数据，确保共享场景，减少域差异。

### 2.3 算法流程
- 训练阶段：交替将MM或DP设为主分支，冻结另一分支，优化对应分支的私有损失+公共损失。
- 推理阶段：仅输入一对多模态图像，通过共享编码器提取特征，经CFGM融合后由全局解码器输出结果。

## 3. 实验设计

### 3.1 数据集与场景
- **训练数据**：仅使用LLVIP训练集（IVIF任务）及其增强数据（MFIF任务）。
- **评估任务**（已见：IVIF、MFIF；未见：MEIF、NIR-VIS、Pansharpening、Medical）：
  - IVIF: LLVIP, TNO
  - MFIF: Lytro, MFI-WHU
  - MEIF: SCIE
  - NIR-VIS: VIS-NIR Scene
  - Remote: QuickBird
  - Medical: Harvard

### 3.2 对比方法
- 专用方法：Text-IF, CDDFuse, DDFM, LRRNet, ZMFF, UNIFusion, MEF-GAN, SPD-MEF, IID-MEF, P2Sharpen, ZeroSharpen, CoCoNet, TextFusion等。
- 通用方法：U2Fusion, MUFusion, SDNet, MURF, IFCNN等。

### 3.3 评估指标
- 相关性指标：VIF, SCD
- 非参考指标：EI, AG
- 分类任务：Top-1, Top-5 accuracy（CIFAR100）

## 4. 资源与算力

论文中**未明确说明训练所使用的GPU型号、数量以及训练时长**。仅在效率对比（Table 4）中给出了模型大小（3.30 MB）和计算量（9.96 GFLOPs），但未提及训练资源。因此，我们无法确定训练算力需求。

## 5. 实验数量与充分性

论文进行了大量实验，涵盖多个方面：
- **主消融实验（Table 1）**：6组配置，研究MTL、CFGM、REC、不同任务组合（IVIF+MFIF、IVIF+MEIF）的影响。
- **CFGM门控机制消融（Fig.5, Fig.6）**：对比自适应λ与固定混合策略（Max、Sum）。
- **已见任务对比（Table 2 (a),(b)）**：MFIF和IVIF任务，各2个数据集。
- **未见任务对比（Table 2 (c)-(f)）**：4种任务，共4个数据集。
- **单模态增强实验（Table 3, Fig.8）**：CIFAR100分类任务，对比8种融合方法。
- **效率对比（Table 4）**：对比6种多任务方法。

实验覆盖了6种不同的融合任务，以及单模态场景，消融设计完整，对比方法全面，定量与定性分析结合。结论具有充分性和客观性。

## 6. 论文的主要结论与发现

1. **低级任务交互显著优于高层语义交互**：通过像素级监督实现特征共享，消除了语义鸿沟，且计算量降低96%以上。
2. **单模型即可处理多种融合任务**：GIFNet在已见任务（IVIF、MFIF）上达到或超越专用方法，在未见任务（MEIF、NIR-VIS、Pansharpening、Medical）上也表现最优或次优。
3. **首次实现融合模型对单模态增强**：在CIFAR100分类任务中，使用GIFNet增强后的图像训练分类器，准确率超越原始数据和其他融合方法。
4. **交叉融合门控机制和重建分支是性能关键**：消融实验确认了CFGM和REC的不可或缺性，且MFIF作为辅助任务优于MEIF。

## 7. 优点

- **创新性**：首次提出利用低级任务交互（低层视觉任务）替代高层语义，为通用融合提供了新范式。
- **高效性**：模型仅3.3 MB，GFLOPs仅9.96，远低于现有方法，适合嵌入式设备。
- **泛化性**：仅用IVIF+MFIF数据训练，即可泛化到四种未见融合任务，且效果优异。
- **拓展性**：将融合模型用于单模态增强，拓展了图像融合的应用边界。
- **实验严谨**：消融实验覆盖多个组件、任务组合、门控策略；对比方法包含专用和通用方法；定量指标全面。

## 8. 不足与局限

- **训练算力未公开**：缺少GPU型号、训练时间等信息，不利于复现和资源评估。
- **辅助任务选择依赖经验**：仅通过消融实验发现MFIF优于MEIF，但缺乏理论分析说明为何MFIF更合适。
- **未见任务上的性能仍有提升空间**：在Remote和Medical任务上，VIF和SCD虽领先，但EI和AG并非最高，说明细节保留可能存在不足。
- **缺乏真实低功耗设备部署验证**：虽然计算量低，但未在手机等设备上测试实际运行速度和功耗。
- **数据构建依赖合成**：多聚焦数据由可见光图像人工模糊生成，可能与真实传感器噪声存在差异，可能影响实际泛化。
- **对输入对齐敏感**：方法依赖图像已配准，未处理未配准情况（如MURF所做），在真实场景中可能受限。

（完）
