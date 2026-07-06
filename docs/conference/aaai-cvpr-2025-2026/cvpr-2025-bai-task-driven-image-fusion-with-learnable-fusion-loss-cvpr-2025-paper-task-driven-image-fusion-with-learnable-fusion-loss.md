---
title: Task-driven Image Fusion with Learnable Fusion Loss
title_zh: 任务驱动的可学习融合损失图像融合
authors: "Bai, Haowen, Zhang, Jiangshe, Zhao, Zixiang, Wu, Yichen, Deng, Lilun, Cui, Yukun, Feng, Tao, Xu, Shuang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Bai_Task-driven_Image_Fusion_with_Learnable_Fusion_Loss_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 任务驱动的多模态图像融合与可学习损失
tldr: 针对现有融合方法使用预定义目标与下游任务不匹配的问题，提出任务驱动图像融合TDFusion，引入可学习融合损失，由下游任务损失以元学习方式指导融合损失生成。该方法提升了融合图像对下游任务（如检测、分割）的适应性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1761, \"height\": 857, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 947, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1799, \"height\": 451, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1799, \"height\": 449, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 865, \"height\": 458, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1812, \"height\": 759, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 447, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-bai-task-driven-image-fusion-with-learnable-fusion-loss-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 870, \"height\": 321, \"label\": \"Table\"}]"
motivation: 预定义融合目标与下游任务不匹配，限制模型灵活性。
method: 提出TDFusion，包含由损失生成模块建模的可学习融合损失，通过元学习由任务损失监督。
result: 在多个下游任务上提升性能，融合图像质量更高。
conclusion: 可学习融合损失有效弥合融合与下游任务之间的差距。
---

## Abstract
Multi-modal image fusion aggregates information from multiple sensor sources, achieving superior visual quality and perceptual features compared to single-source images, often improving downstream tasks. However, current fusion methods for downstream tasks still use predefined fusion objectives that potentially mismatch the downstream tasks, limiting adaptive guidance and reducing model flexibility. To address this, we propose Task-driven Image Fusion (TDFusion), a fusion framework incorporating a learnable fusion loss guided by task loss. Specifically, our fusion loss includes learnable parameters modeled by a neural network called the loss generation module. This module is supervised by the downstream task loss in a meta-learning manner. The learning objective is to minimize the task loss of fused images after optimizing the fusion module with the fusion loss. Iterative updates between the fusion module and the loss module ensure that the fusion network evolves toward minimizing task loss, guiding the fusion process toward the task objectives. TDFusion's training relies entirely on the downstream task loss, making it adaptable to any specific task. It can be applied to any architecture of fusion and task networks. Experiments demonstrate TDFusion's performance through fusion experiments conducted on four different datasets, in addition to evaluations on semantic segmentation and object detection tasks. The code is available at https://github.com/HaowenBai/TDFusion.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：多模态图像融合常用于提升下游视觉任务（如语义分割、目标检测）的性能，但现有融合方法大多采用**预定义的融合损失**（如像素级强度、梯度保持等），这些损失目标与下游任务存在潜在不匹配，导致融合图像无法针对性地优化任务表现，且模型灵活性受限。
- **整体含义**：本文旨在**让融合过程自适应地服从下游任务的需求**，通过学习一个可动态调整的融合损失函数，使融合网络生成的图像能最小化下游任务的损失，从而弥合融合与高层任务之间的鸿沟。

## 2. 方法论

- **核心思想**：提出 **TDFusion** 框架，包含三个模块：
  - 融合网络 **F**（参数 θ_F）
  - 下游任务网络 **T**（参数 θ_T）
  - **损失生成模块 G**（参数 θ_G）：输出融合损失中像素级权重 {w_a, w_b}，控制强度项对红外和可见光信息的选择。
- **关键技术细节**：
  - **可学习融合损失**：由强度项和梯度项组成，强度项中权重 w_a, w_b 由 G 生成，并通过 Softmax 保证和为1，梯度项采用 Sobel 算子保留最大梯度。
  - **元学习训练**：受 MAML 启发，交替执行**内更新**和**外更新**：
    - 内更新：使用当前 G 生成的融合损失，对融合网络和任务网络各做一步更新得到临时参数 θ_F' 和 θ_T'。
    - 外更新：用临时网络处理元测试集图像，计算下游任务损失，反向传播更新 G。
    - 目标：使得经过内更新后的融合网络在下游任务上损失最小化。
  - **整体流程**：每次训练迭代中，先执行 M 次内/外更新优化 G，再执行 N 步标准训练优化 F 和 T（使用当前 G 生成的损失和任务损失）。如此交替直至收敛。
- **算法流程**（文字描述）：
  - 初始化 θ_F, θ_T, θ_G。
  - 每轮 epoch：
    - 从训练集抽取元训练集和元测试集。
    - 对 M 步：内更新（式4,5），外更新（式6）优化 θ_G。
    - 对 N 步：用全训练集更新 θ_F 和 θ_T（式8,9）。

## 3. 实验设计

- **数据集**：4个公开多模态数据集，均带下游任务标注：
  - MSRS（1083/361 训练/测试）
  - FMB（1220/280）
  - M3FD（4200对，自行划分3150/1050 训练/测试，300对用于融合评估）
  - LLVIP（原始12025/3463，筛选后1203/347）
- **Benchmark**：融合任务评估指标：EN、SF、SCD、VIF、Q^AB/F、SSIM；下游任务：SegFormer（语义分割）和 YOLOv8（目标检测），指标包括 mAcc、mIoU、mAP50、mAP75 等。
- **对比方法**：7种先进融合方法 —— TarDAL、SegMIF、MURF、EMMA、DCINN、MRFS、TIMFusion。
- **公平性**：所有下游任务评估中，统一使用 SegFormer 和 YOLOv8 作为任务网络，并单独为每种融合方法重新训练300个 epoch，避免先验优势。

## 4. 资源与算力

- 明确提及：**单个 NVIDIA RTX 3090 GPU**。
- 超参数：epoch 数 L=50，内部更新步数 M=200，优化器 Adam，学习率 1e-4，batch size=2，梯度系数 α=1。
- 文中未详细说明总训练时长，但基于上述配置，可推得训练时间在合理范围内（通常数小时至半天）。

## 5. 实验数量与充分性

- **实验组数**：
  - 融合性能对比：4个数据集 × 7种对比方法，定量表1（共4个子表）。
  - 下游任务性能对比：4个数据集 × 2种任务，表2（包含 mAcc、mIoU、mAP50、mAP75 等多项指标）。
  - 消融实验：5组配置（固定权重、去除梯度项、融合网络受任务损失影响、取消融合网络专训、替换为直接加权融合），表3。
  - 可视化分析：包括融合图像对比（图2）、分割/检测结果对比（图3/4）、可学习损失权重可视化（图5）。
- **充分性评价**：实验覆盖多场景、多任务、多指标，并进行了消融验证，设计较为全面。消融实验揭示了各组件的重要性，任务驱动可学习损失可视化也直观展示了模型对红外/可见光信息的自适应选择。综合来看，实验设计客观、公平、充分。

## 6. 主要结论与发现

- TDFusion 在**融合指标**上，于多数数据集上取得最优或次优结果，尤其在 SCD、Q^AB/F 上优势明显，说明融合图像信息更完整、细节更丰富。
- 在**下游任务**（语义分割和目标检测）中，TDFusion 显著优于所有对比方法，例如在 FMB 上 mIoU 提升约2.1%（vs 次优），在 M3FD 上 mAP50 提升约2.5%。
- **可学习损失权重可视化**表明：模型能根据任务需求动态调整红外和可见光信息的侧重，例如语义分割更关注纹理和边界，目标检测更关注高亮目标区域。
- 消融实验证实了可学习损失、梯度项、专用融合训练阶段、以及元学习机制各自的重要贡献。

## 7. 优点

- **方法创新**：首次将元学习引入融合损失的设计，使融合目标完全由下游任务驱动，无需手动设计损失函数。
- **架构无关**：融合网络和任务网络均可任意替换，框架通用性强。
- **理论分析**：给出了权重更新公式的推导，说明优化机制如何将任务损失传导至融合损失。
- **实验全面**：在4个数据集上覆盖2种典型下游任务，并进行了详细的消融和可视化分析，验证充分。

## 8. 不足与局限

- **计算开销**：元学习的内外更新需要维护计算图并进行二阶梯度计算，训练复杂度高于标准端到端方法，尤其在每次迭代中需多次更新 G。
- **任务依赖**：损失生成模块需要预训练或同时训练下游任务网络，对于任务网络本身性能或标注质量敏感，若任务网络较弱可能影响融合效果。
- **实验覆盖**：仅验证了语义分割和目标检测两种任务，未探索其他下游应用（如跟踪、超分辨率等），通用性有待进一步验证。
- **应用限制**：融合过程假设红外与可见光图像已配准，未考虑实际中可能存在的较大未对齐情况（虽有提及但实验未涉及）。
- **消融分析可更细致**：例如未讨论超参数 α 的敏感性、内更新步数 M 的影响等。

（完）
