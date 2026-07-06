---
title: "CtrlFuse: Mask-Prompt Guided Controllable Infrared and Visible Image Fusion"
title_zh: CtrlFuse：掩码提示引导的可控红外与可见光图像融合
authors: "Yiming Sun, Yuan Ruan, Qinghua Hu, Pengfei Zhu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37884/41846"
tags: ["query:image-fusion"]
score: 9.0
evidence: 掩码提示引导的可控图像融合
tldr: 现有红外与可见光图像融合方法难以同时兼顾像素级融合准确性与下游任务适应性。本文提出CtrlFuse框架，通过掩码提示实现交互式动态融合，集成多模态特征提取器、参考提示编码器和提示语义融合模块。实验表明该方法能根据任务需求灵活调整融合策略，提升下游感知表现。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 839, \"height\": 431, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1740, \"height\": 1023, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 834, \"height\": 312, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 833, \"height\": 313, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 820, \"height\": 239, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 833, \"height\": 264, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 839, \"height\": 416, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37884/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 834, \"height\": 273, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37884/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 848, \"height\": 1374, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37884/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1661, \"height\": 536, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37884/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 838, \"height\": 457, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37884/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 882, \"height\": 337, \"label\": \"Table\"}]"
motivation: 现有融合方法无法根据语义目标需求动态调整融合过程，需实现可控融合。
method: 提出掩码提示引导的可控融合框架，包括多模态特征提取器、参考提示编码器和提示语义融合模块。
result: 在多个数据集上验证了融合质量与下游任务性能的提升。
conclusion: CtrlFuse实现了灵活可控的融合，能更好适应多样化感知需求。
---

## Abstract
Infrared and visible image fusion generates all-weather perception-capable images by combining complementary modalities, enhancing environmental awareness for intelligent unmanned systems. Existing methods either focus on pixel-level fusion while overlooking downstream task adaptability or implicitly learn rigid semantics through cascaded detection/segmentation models, unable to interactively address diverse semantic target perception needs. We propose CtrlFuse, a controllable image fusion framework that enables interactive dynamic fusion guided by mask prompts. The model integrates a multi-modal feature extractor, a reference prompt encoder (RPE), and a prompt-semantic fusion module (PSFM). The RPE dynamically encodes task-specific semantic prompts by fine-tuning pre-trained segmentation models with input mask guidance, while the PSFM explicitly injects these semantics into fusion features. Through synergistic optimization of parallel segmentation and fusion branches, our method achieves mutual enhancement between task performance and fusion quality. Experiments demonstrate state-of-the-art results in both fusion controllability and segmentation accuracy, with the adapted task branch even outperforming the original segmentation model.

---

## 论文详细总结（自动生成）

# 论文《CtrlFuse: Mask-Prompt Guided Controllable Infrared and Visible Image Fusion》详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有红外与可见光图像融合方法主要关注像素级融合质量，忽略了下游任务（如检测、分割）的适应性；而一些任务驱动方法通过级联检测/分割模型隐式学习刚性语义，无法根据多样化的应用需求灵活、交互地控制融合对不同语义目标的关注程度。
- **整体含义**：本文提出一个可交互控制的融合框架，利用掩码提示（mask prompt）引导融合过程，使融合结果能够动态适应指定的语义目标，从而同时提升融合质量和下游任务性能，为智能无人系统提供全天候、可控的感知能力。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：基于**掩码提示**的可控融合，通过集成预训练的Segment Anything Model（SAM），将用户指定的区域掩码作为交互输入，动态生成语义提示，并显式注入到融合特征中，实现“指哪融哪”的效果，且融合与分割任务相互促进。
- **整体架构**：由四个主要组件构成：
  1. **多模态特征编码器-解码器**：对红外图像（1通道）和可见光图像（3通道）分别提取特征 \(F_{ir}\)、\(F_{vis}\)，拼接后经解码器得到初步融合图像 \(I_{ref}\) 及特征 \(F_{ref}\)。
  2. **参考提示编码器（RPE）**：以掩码提示 \(Prompt_m\) 指导，将红外/可见光特征作为支持特征（support），\(F_{ref}\) 作为查询特征（query），通过交叉注意力与自注意力机制生成对应于指定类别的提示嵌入 \(P_{ir}\)、\(P_{vis}\)。
  3. **提示语义融合模块（PSFM）**：将提示嵌入 \(P\) 与下采样后的编码特征做交叉注意力，再与对应分支的SAM分割掩码 \(M\) 相乘，得到类别增强的提示特征 \(F_{ir}^p\)、\(F_{vis}^p\)，并与初步融合特征相加得到最终融合特征。
  4. **辅助网络（冻结SAM）**：微调SAM的提示编码器与掩码解码器，输出预测掩码用于PSFM及分割损失计算。
- **关键技术细节**：
  - RPE中：对 \(Prompt_m \cdot F_{ir}\) 做平均池化得 \(F_t\)，分别与 \(F_{ir}\)、\(F_{ref}\) 拼接并经卷积得到支持特征 \(F_{supp}\) 和查询特征 \(F_{qry}\)；然后通过可学习查询 \(Q\) 经交叉/自注意力（式4-5）得到提示嵌入 \(P'\)，再输入SAM提示编码器得最终嵌入 \(P\)。
  - PSFM中：下采样特征并序列化，与 \(P\) 做交叉注意力，再恢复空间维度并上采样，乘以分割掩码 \(M\) 得到 \(F^p\)（式6-7）。
  - 联合损失：融合损失 \(L_{fusion}\) + 分割损失 \(L_{seg}\)，端到端优化。

## 3. 实验设计

- **数据集**：
  - **FMB**：1500对红外-可见光图像（车载），含语义分割标签。训练1020/验证280/测试200。
  - **MSRS**：1444对图像（车载），含分割标签。训练1072/验证150/测试200。
  - **DroneVehicle**：无人机车辆检测数据集，无语义标签，使用其他分割模型生成掩码。评估200对图像。
- **Benchmark**：对比8种SOTA方法：LDFusion、SwinFuse、NestFuse、CDDFuse、DIDFuse、SeAFusion、PSFusion、SDCFusion。
- **评估指标**：
  - 融合质量：MSE、PSNR、\(Q_{abf}\)、\(N_{abf}\)、SSIM、SCD。
  - 下游任务：语义分割（mIoU，在MSRS上对比）和物体检测（AP@[0.5:0.95]，在DroneVehicle上微调YOLOv5后测试）。
- **消融实验**：在MSRS上进行，包括：
  - w/o Prompt（去掉掩码提示）
  - w/o Seg（去掉SAM掩码解码器和分割损失）
  - w/o Vis（去掉可见光分支的RPE和PSFM）
  - w/o Ir（去掉红外分支的对应模块）
  - Exchange SQ（交换支持/查询角色）

## 4. 资源与算力

- **训练环境**：4块NVIDIA GeForce RTX 3090 GPU。
- **优化设置**：Adam优化器，学习率 \(1.0 \times 10^{-4}\)，训练150个epoch，batch size 4。
- **未明确说明**：训练总耗时、模型参数量、推理速度等未提供。

## 5. 实验数量与充分性

- **实验组数**：
  - 在3个数据集（FMB、MSRS、DroneVehicle）上进行了定量比较（每个数据集一次）。
  - 在FMB上提供了定性对比图（图3），DroneVehicle定性对比（图4）。
  - 在MSRS上进行了语义分割对比（表2）和定性分割结果（图5）。
  - 在DroneVehicle上进行了物体检测对比（表3）。
  - 一个消融实验（表4）包含5个子实验。
  - 附加分析：SAM微调效果（图6）、可控性展示（图7）、掩码质量敏感性（图8）。
- **充分性与公平性**：
  - 融合质量比较覆盖了多个指标（6个），对比方法为近年代表性工作，较为全面。
  - 下游任务评估使用重新训练的分割模型（DeepLabV3+）和微调的检测模型（YOLOv5），避免过拟合。
  - 消融实验设计合理，验证了各模块的必要性。
  - 但缺少与最新方法的比较（如2025年发表的一些方法未纳入），且仅在三个数据集上评估，泛化性验证有限。

## 6. 主要结论与发现

- 所提CtrlFuse在FMB、MSRS、DroneVehicle三个数据集上，在PSNR、\(N_{abf}\)、\(Q_{abf}\)等融合指标上取得了最佳或次佳结果，表明其能生成高质量、低失真、结构保留好的融合图像。
- 在下游任务方面，CtrlFuse在MSRS语义分割上取得最高mIoU（0.7963），在DroneVehicle物体检测上取得最高AP@all（0.525），特别是在“car”和“bus”类别上表现突出。
- 掩码提示可实现交互式可控融合：针对不同类别（如行人、车辆）提供提示，能显著增强对应区域的感知质量。
- 融合任务与分割任务相互促进：去除分割分支会导致融合质量下降（表4 w/o Seg），说明SAM微调有正面作用。
- 支持/查询特征的设计：使用分支特征（如\(F_{ir}\)）作为支持、\(F_{ref}\)作为查询比互换更有效。

## 7. 优点

- **创新性**：首次将SAM的交互式提示能力引入图像融合，实现用户可控的、语义驱动的融合，突破了传统任务驱动方法的刚性限制。
- **模块化设计**：RPE和PSFM为即插即用模块，可灵活适配不同多模态融合架构。
- **联合优化**：同时优化融合损失和分割损失，通过显式注入语义信息，同时提升融合质量和任务性能，体现了多任务协同优势。
- **鲁棒性验证**：分析了提示掩码的不完整性对结果影响较小（图8），证明模型对输入质量不敏感。
- **代码开源**：提供GitHub仓库，便于复现和后续研究。

## 8. 不足与局限

- **实验规模**：仅在三个公开数据集上验证，数据集规模较小（约1500对），且DroneVehicle的掩码由其他分割模型生成，引入额外误差。
- **计算资源**：训练使用4张3090 GPU，未报告推理速度或参数量，对实际部署的实时性评估缺失。
- **泛化能力**：未在更多样化的场景（如低光照、雨雾等）或更多模态（如RGB+深度）上测试。
- **可控粒度**：依赖SAM的交互能力，当提示掩码覆盖多个类别时，融合效果可能折中，未能探讨复杂语义冲突。
- **应用限制**：仅针对红外-可见光融合，在其他融合任务（如医学图像、多光谱）上的有效性未验证。
- **指标覆盖**：缺少人类主观评估（如MOS得分），仅依靠客观指标和下游任务指标。

（完）
