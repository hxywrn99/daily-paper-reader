---
title: "Show and Segment: Universal Medical Image Segmentation via In-Context Learning"
title_zh: 展示与分割：基于上下文学习的通用医学图像分割
authors: "Gao, Yunhe, Liu, Di, Li, Zhuowei, Li, Yunsheng, Chen, Dongdong, Zhou, Mu, Metaxas, Dimitris N."
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Gao_Show_and_Segment_Universal_Medical_Image_Segmentation_via_In-Context_Learning_CVPR_2025_paper.pdf"
tags: ["query:medical-seg"]
score: 10.0
evidence: 基于上下文学习的通用医学图像分割
tldr: 针对医学图像分割中任务多样、难以泛化的问题，提出Iris框架，通过上下文参考图像引导，无需微调即可适应新分割任务，利用轻量级上下文任务编码模块从参考图像-标签对中提取任务信息指导目标分割。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 849, \"height\": 701, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1592, \"height\": 687, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 852, \"height\": 655, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 599, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 768, \"height\": 779, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 857, \"height\": 342, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1801, \"height\": 740, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1623, \"height\": 739, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 779, \"height\": 227, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-gao-show-and-segment-universal-medical-image-segmentation-via-in-context-learning-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 864, \"height\": 203, \"label\": \"Table\"}]"
motivation: 现有方法需要任务特定训练或微调，难以泛化到新类别。
method: 提出Iris框架，通过上下文参考图像引导，包含轻量级上下文任务编码模块提取任务信息。
result: 在多种医学分割任务上无需微调即可实现有竞争力的性能。
conclusion: Iris为通用医学图像分割提供了一种高效、灵活的解决方案。
---

## Abstract
Medical image segmentation remains challenging due to the vast diversity of anatomical structures, imaging modalities, and segmentation tasks. While deep learning has made significant advances, current approaches struggle to generalize as they require task-specific training or fine-tuning on unseen classes. We present Iris, a novel In-context Reference Image guided Segmentation framework that enables flexible adaptation to novel tasks through the use of reference examples without fine-tuning. At its core, Iris features a lightweight context task encoding module that distills task-specific information from reference context image-label pairs. This rich context embedding information is used to guide the segmentation of target objects. Given a decoupled architecture on 3D data processing, Iris supports diverse inference strategies including one-shot inference, context example ensemble, object-level context example retrieval, and in-context tuning. Through comprehensive evaluation across twelve datasets, we demonstrate that Iris performs strongly compared to specialized supervised models on in-distribution tasks. On seven held-out dataset, Iris shows superior generalization to out-of-distribution data and unseen classes. Further, Iris's task encoding module can automatically discover anatomical relationships across datasets and modalities, offering insights into cross-modality medical objects without explicit anatomical supervision.

---

## 论文详细总结（自动生成）

# 论文总结：Show and Segment: Universal Medical Image Segmentation via In-Context Learning

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：医学图像分割面临解剖结构、成像模态和临床任务的巨大多样性。现有深度学习方法通常需要为每个任务单独训练模型，或针对未见过的类别进行微调，缺乏泛化性和适应性。尤其是在实际临床中，新任务不断涌现，传统方法难以快速部署。
- **背景**：已有的解决方案包括任务专用模型（需每任务独立训练）、多任务通用模型（无法处理新类别）、SAM类交互式模型（依赖人工交互，不适合高通量自动处理）。这些方法均存在局限性。
- **目标**：提出一种**无需微调**、仅通过少量参考示例即可自动适应任意新分割任务的通用框架，同时保持高性能和计算效率。

## 2. 方法论

- **核心思想**：通过上下文学习（In-Context Learning），使用一对参考图像-标签对作为“任务定义”，提取紧凑的任务嵌入，指导查询图像的分割。关键设计是**解耦任务编码与推理**，使任务嵌入可被复用，避免针对每个查询重复编码参考信息。
- **关键技术细节**：
  - **任务编码模块（Task Encoding Module）**：由两个并行流组成：
    - 前景特征编码：对参考图像的高分辨率特征与原始掩膜逐元素相乘，再池化得到前景嵌入（`T_f`），保留小目标细节。
    - 上下文特征编码：利用像素混洗（PixelShuffle）和像素反混洗（PixelUnshuffle）实现内存高效的高分辨率特征-掩膜融合，再通过可学习查询令牌经交叉注意力与自注意力提取上下文嵌入（`T_c`）。最终任务嵌入 `T = [T_f; T_c]`。
  - **掩膜解码模块（Mask Decoding Module）**：基于查询的架构，通过双向交叉注意力在查询特征与任务嵌入之间交换信息，最终解码出多类分割掩膜（单次前向即可输出所有类别）。
  - **训练策略**：采用情节训练（episodic training），从同一数据集中采样参考-查询对，组合Dice损失与交叉熵损失。并加入数据增强、查询扰动、随机类别丢弃等提高泛化。
  - **灵活推理策略**：
    - 单次推理：仅需一个参考示例，预计算任务嵌入并复用。
    - 上下文集成：对多个参考示例的任务嵌入取平均。
    - 对象级上下文检索：先获得粗分割结果，再为每个类别独立检索最相似的参考嵌入（优于图像级检索）。
    - 上下文微调：仅优化任务嵌入，保持模型参数固定。
- **公式流程**：
  - 模型输入：参考集 `S = {(x_i^s, y_i^s)}` 和查询图像 `x_q`。
  - 输出：预测掩膜 `ŷ_q = fθ(x_q; S)`。
  - 对于多类分割，分解为多个二值任务，但单次前向完成。

## 3. 实验设计

- **数据集**：
  - **训练集（12个公开数据集）**：涵盖头部、胸部、腹部等不同身体区域，CT、MRI、PET等多种模态，包括器官、组织、病变等分割目标。按75%/5%/20%分割训练/验证/测试。
  - **评估场景**：
    - 分布内（In-distribution）：在12个训练集测试集上评估。
    - 分布外泛化（OOD）：使用5个留出数据集：ACDC（心脏MRI）、SegTHOR（胸部CT）、IVDM3Seg（三个MRI模态：CSI-inn/opp/fat）。
    - 新类别适配：MSD Pancreas（胰腺肿瘤）、Pelvic1K（骨骼）。
- **基准方法**：
  - 任务专用模型：nnUNet。
  - 多任务通用模型：CLIP-driven、UniSeg、Multi-Talent。
  - 交互式基础模型：SAM、SAM-Med2D、SAM-Med3D（使用地面真值模拟交互）。
  - 上下文学习方法：SegGPT、UniverSeg、Tyche-IS。
- **实现细节**：Iris使用3D UNet编码器从头训练，Lamb优化器，学习率2e-3，权重衰减1e-5，训练80K迭代，batch size 32，2K预热，输入体积128×128×128。

## 4. 资源与算力

- 文中明确提及：实验基于**单张NVIDIA A100 GPU**进行。
- 训练迭代数为80K，batch size为32，未报告总训练时长，但可推断为中等规模。
- 推理效率对比：在10个查询体积、1个参考体积、15类的情况下，Iris耗时2秒，而UniverSeg（1切片参考）需659秒，SAM-Med3D需15.2秒（不含交互时间）。

## 5. 实验数量与充分性

- **实验组数量**：
  - **In-distribution对比**：12个数据集，与6类方法（含多种变体）对比。
  - **OOD泛化**：5个留出数据集，涵盖跨中心、跨模态、新类别。
  - **新类别适配**：2个数据集（MSD Pancreas、Pelvic）。
  - **效率对比**：4种方法（含自身）的推理时间、内存、参数量。
  - **推理策略分析**：4种策略在OOD上的表现曲线（使用不同百分比上下文样本）。
  - **消融实验**：3个组件（高分辨率处理、前景池化、查询编码）的贡献，以及查询令牌数量影响。
  - **任务数量影响**：训练任务数量与OOD性能关系。
  - **任务嵌入可视化**：t-SNE分析。
- **充分性与公平性**：实验覆盖了多种场景（分布内/外、新类、效率），对比方法全面，包含当前主流基线，且使用统一训练/测试划分。SAM方法通过提供真实标签模拟交互，但实际交互次数未严格对齐（可能略有利）。所有方法均在同一硬件环境下比较。实验设计较为充分客观。

## 6. 主要结论与发现

- Iris在12个分布内数据集上达到84.52%平均Dice，与nnUNet等任务专用模型持平或略优，显著优于其他自适应方法（如UniverSeg 61.20%）。
- 在OOD场景中，Iris在所有5个留出数据集上均优于所有对比方法，尤其是在大域偏移（如CSI-fat）和3D任务（SegTHOR）上表现突出。
- 在新类别上（胰腺肿瘤、骨盆），Iris仅用一个参考示例即获得28.28%和69.03% Dice，远超其他自适应方法（最佳竞争者11.97%/61.92%）。
- 效率方面：Iris比UniverSeg快数百倍，比SAM-Med3D快7倍以上。
- 任务嵌入可视化表明，Iris能自动发现跨数据集、跨模态的解剖结构相似性（如腹部器官聚集、血管/膀胱邻近），无需显式解剖监督。
- 训练任务多样性对泛化至关重要；更多样化的任务（涵盖多身体区域）带来更强的OOD性能。

## 7. 优点

- **创新性**：首次在3D医学图像分割中实现解耦式上下文学习，兼顾高性能与高效率。
- **灵活性**：支持多种推理策略（单次、集成、检索、微调），适应不同应用场景。
- **可解释性**：任务嵌入自动揭示解剖关系，提供知识迁移洞见。
- **实用性**：无需微调即可处理新任务，适合动态临床环境；单次前向完成多类分割，计算开销极低。
- **实验全面**：涵盖分布内、分布外、新类别、效率、消融、可视化等多维度分析。

## 8. 不足与局限

- **训练任务多样性依赖**：泛化性能受训练任务数量和多样性的影响，文中已指出需自动化方法生成更多样化任务，目前仍需人工标注。
- **新类别性能仍存差距**：在胰腺肿瘤（28.28%）等高度复杂的新类别上，与监督上限（nnUNet 54.56%）仍有较大差距，表明在极度稀疏/异质目标上仍受限于参考信息有限。
- **仅评估Dice指标**：未报告其他指标如Hausdorff距离、体积精度等，可能掩盖边界质量差异。
- **实验环境的潜在偏差**：仅使用A100 GPU，未测试其他硬件；训练时间未报告，可复现性细节略缺。
- **参考示例选择影响**：虽然提出了对象级检索，但初始随机参考的选择可能影响最终结果，文中虽有多次实验验证稳定性，但未系统分析失败案例。
- **医学伦理与临床适用性**：未讨论模型在真实临床部署中的安全性与偏差风险（如对不同种族/性别/设备的泛化），也未涉及模型不确定性量化。

（完）
