---
title: Domain Adaptation Guided Infrared and Visible Image Fusion
title_zh: 域自适应引导的红外与可见光图像融合
authors: "Tianwei Guan, Haozhen Wei, Yuhan Zhou, Jun Ma, Zecheng Xu, Zhiying Jiang, Jinyuan Liu, Xingyuan Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/42435/46396"
tags: ["query:image-fusion"]
score: 9.0
evidence: 域自适应引导的红外和可见光图像融合
tldr: 针对红外-可见光融合在未知天气或场景下性能下降的问题（域偏移），提出DAFusion方法。利用双秩域适配器，分别学习低秩和高秩嵌入空间，快速适应可见光模态的分布变化。实验表明DAFusion在多种域偏移场景下融合质量稳定，显著优于传统方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 红外-可见光融合在未知场景下因域偏移性能下降，且可见光模态分布变化主导，红外稳定。
method: 提出DAFusion，采用双秩域适配器（低秩+高秩嵌入）快速适应可见光模态的分布变化。
result: 在多种域偏移条件下，DAFusion融合质量稳定且优于现有方法。
conclusion: 域自适应能有效提升红外-可见光融合在未知环境下的鲁棒性。
---

## Abstract
Infrared and Visible Image Fusion (IVIF) integrates complementary information from distinct modalities to enhance image quality. However, the effectiveness declines under unseen conditions such as novel weather or scenes, due to domain shifts primarily from variations of data distribution in the visible modality, while the infrared modality remains relatively stable. To overcome domain shifts caused by the imbalance between modalities during image fusion, we propose a Domain Adaptation Guided Infrared and Visible Image Fusion method, termed DAFusion, leveraging a dual-rank domain adapter to enable fast adaptation to diverse adverse conditions during image fusion. Specifically, trainable low-rank and high-rank embedding spaces are respectively used to capture knowledge common across domains (domain-shared) and those unique to target domains (domain-specific). To leverage the dual-rank adapter more effectively, we develop a homeostatic knowledge allotment strategy to integrate the distinct types of knowledge dynamically based on the uncertainty value of target domains. Since domain adaptation typically optimizes for feature alignment across domains and emphasizes invariance rather than preserving specific cues critical for image fusion, while the fusion objective requires retaining discriminative and complementary features, a conflict between the two modules appears. To reconcile this, we further adopt a bi-level optimization framework that structurally decouples the two objectives, enabling the fusion module to steer the adaptation process while benefiting in return from domain-aligned representations. Experimental results on three benchmarks demonstrate that our method significantly outperforms state-of-the-art approaches, achieving both an enhancement in fusion quality and an improvement on subsequent high-level tasks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：红外与可见光图像融合（IVIF）旨在结合两种模态的互补信息，提升图像质量并辅助下游任务。然而，传统融合方法在**未知环境**（如新的天气、光照或场景）下性能显著下降，其主要原因是**可见光模态存在严重的域偏移**（分布变化），而红外模态相对稳定。这种模态不平衡导致模型错误地抑制或丢失可见光中的关键信息，进而降低融合质量与下游任务表现。
- **整体含义**：本文首次系统地将**域自适应**引入IVIF，旨在解决融合任务中因可见光域偏移导致的鲁棒性问题，实现融合质量与下游任务性能的协同提升。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建一个**域自适应引导的图像融合框架（DAFusion）**，通过**双秩域适配器**（dual-rank domain adapter）快速适应可见光模态的分布变化，并采用**稳态知识分配策略**（HKA）动态平衡域共享与域特定知识，最后通过**双层优化**（bi-level optimization）协调域适应与融合目标，实现两者相互促进。
- **关键技术细节**：
  1. **双秩域适配器**：将低秩适配器（down-projection + up-projection，中间维度 \(d_l \ll d\)）和高秩适配器（up-projection + down-projection，中间维度 \(d_h \ge d\)）插入原始网络线性层或Conv层，以残差形式输出融合特征：  
     \[
     f_f = f_o + \lambda_h \times f_h + \lambda_l \times f_l
     \]  
     其中 \(f_h, f_l\) 分别为高秩和低秩分支输出，\(\lambda_h, \lambda_l\) 由HKA动态调节。
  2. **稳态知识分配策略（HKA）**：利用多次前向传播的特征方差计算不确定性 \(U(x)\)，根据 \(U(x)\) 与阈值 \(\Theta\) 的比较动态调整权重：  
     \[
     \begin{cases}
     \lambda_h = 1 + U(x), \lambda_l = 1 - U(x), & U(x) \ge \Theta \\
     \lambda_h = 1 - U(x), \lambda_l = 1 + U(x), & U(x) < \Theta
     \end{cases}
     \]  
     高不确定性时增强域特定知识（高秩分支），低不确定性时增强域共享知识（低秩分支）。
  3. **双层优化框架**：将优化解耦为两层——**低层**优化域适应损失（教师-学生一致性损失 \(L_{DA}\)），**高层**优化融合损失（包含 SSIM、梯度、强度、解耦损失等）。融合损失通过影响适配器参数来引导域适应，使得域适应朝向有利于融合的方向进行。
- **算法流程**：  
  ① 使用教师-学生框架在4种天气（雪、夜、雨、雾）上预训练域适配器，通过EMA更新教师模型。  
  ② 将预训练适配器注入融合模型的可见光编码器。  
  ③ 采用双层优化交替冻结融合网络与适配器参数，联合训练。

## 3. 实验设计：数据集、Benchmark与对比方法

- **数据集与场景**：
  - 融合质量评估：**M3FD**（400张）、**RoadScene**（221张）、**TNO**（57张）、**AWMM**（100张，含雪、夜、雨、雾等挑战场景）。
  - 下游任务：**M3FD**用于目标检测（YOLOv8s），**FMB**用于语义分割（SegFormer-B1）。
- **Benchmark**：与11种SOTA方法对比：TIM、MetaFusion、Text-IF、CDDFuse、DCEvo、BDLFusion、PromptFusion、LRRNet、EMMA、ReFusion、SAGE。
- **评价指标**：融合质量采用**CC**、**PSNR**、**SCD**、**SSIM**；检测任务采用**mAP**；分割任务采用**mIoU**。

## 4. 资源与算力

- **显式描述**：论文中仅指出“Experiments are conducted on a NVIDIA 4080 (32GB) GPU.” 未提及具体训练时长或GPU数量。
- **说明**：由于未提供训练时间、迭代次数或能耗数据，无法评估其计算成本。仅知使用单卡RTX 4080（32GB）。

## 5. 实验数量与充分性

- **实验数量**：
  - 融合质量对比：在4个数据集上分别与11种方法比较（共4×11组定量结果）。
  - 下游任务：目标检测（6类，mAP）和语义分割（14类，部分展示6类，最终用mIoU）。
  - 消融实验：共7组（表3），分别验证低秩、高秩、双秩、HKA、双层优化等组件的贡献。
  - 定性展示：包含融合结果、检测结果、分割结果的多组示例图。
- **充分性与公平性**：
  - 数据集覆盖多种天气/光照条件（雪、夜、雨、雾、低质量、白天），具有代表性。
  - 对比方法涵盖2022~2025年主流SOTA，年代分布合理。
  - 消融设计完整，逐项验证每个模块的有效性。
  - 评价指标全面（像素级、结构级、下游任务级）。整体实验设计较为充分客观。

## 6. 论文的主要结论与发现

- **主要结论**：DAFusion在融合质量和下游任务（检测、分割）上均显著优于所有对比方法，在CC、PSNR、SCD、SSIM等指标上取得最高或次优，mAP和mIoU也达到最佳。
- **关键发现**：
  - 双秩适配器比单一秩更有效，HKA策略进一步提升了性能。
  - 双层优化框架成功解决了域适应与融合目标之间的冲突，实现相互促进。
  - 域自适应引导的融合框架能够显著提升模型对未知场景的鲁棒性。

## 7. 优点

- **方法创新性**：首次将域自适应与IVIF系统结合，提出双秩适配器+HKA+双层优化的一体化方案，思路新颖。
- **模块化设计**：域适配器可作为即插即用模块，便于迁移到其他融合架构。
- **实验全面性**：涵盖多种挑战场景（雪、夜、雨、雾）、多种任务（融合、检测、分割）、多组消融，验证充分。
- **性能优异**：在多个数据集上均取得最好或次好结果，提升明显。

## 8. 不足与局限

- **域覆盖有限**：预训练仅使用4种天气（雪、夜、雨、雾），未覆盖更多实际场景（如强光、雾霾、水下等），泛化性有待进一步验证。
- **计算资源未详述**：未提供训练时间、推理速度等效率指标，难以评估其实用性（尤其在资源受限平台）。
- **双层优化复杂度**：双层优化训练可能比单目标优化更耗时，且对超参数敏感（如阈值 \(\Theta\) 设定为0.2），未做充分敏感性分析。
- **红外模态稳定性的假设**：论文假设红外模态稳定，但在极端温度变化或传感器噪声下红外也可能发生域偏移，未作讨论。
- **下游任务评估范围**：只验证了检测和分割，未涉及其他高级任务（如追踪、检索），通用性有待扩展。

（完）
