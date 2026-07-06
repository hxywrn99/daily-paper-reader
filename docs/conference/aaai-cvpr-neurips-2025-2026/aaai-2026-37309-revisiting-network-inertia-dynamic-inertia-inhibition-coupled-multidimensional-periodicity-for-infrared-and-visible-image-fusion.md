---
title: "Revisiting Network Inertia: Dynamic Inertia Inhibition Coupled Multidimensional Periodicity for Infrared and Visible Image Fusion"
title_zh: 重访网络惰性：动态惯性抑制耦合多维周期性的红外与可见光图像融合
authors: "Yufeng Chen, Yuan Sun, Hao Pan, Xujian Zhao, Jian Dai, Zhenwen Ren, Xingfeng Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37309/41271"
tags: ["query:image-fusion"]
score: 9.0
evidence: 动态惯性抑制的红外与可见光图像融合
tldr: 该文发现深度网络在红外与可见光融合中的权重更新减缓现象（网络惰性），提出AIDFusion。通过动态惯性抑制学习策略(DIILS)逐步调节多级预测的协同学习，充分挖掘网络各层潜力。实验表明该方法在轻量模型上取得最优融合质量且计算高效。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1801, \"height\": 519, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1849, \"height\": 527, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1818, \"height\": 662, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 876, \"height\": 846, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 843, \"height\": 516, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1841, \"height\": 617, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37309/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 859, \"height\": 253, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37309/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1814, \"height\": 537, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37309/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1815, \"height\": 580, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37309/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 848, \"height\": 421, \"label\": \"Table\"}]"
motivation: 深层网络存在权重更新减慢的网络惰性，限制表征能力。
method: 设计动态惯性抑制学习策略，逐步调节多级预测的协同学习。
result: 轻量模型在红外-可见融合基准上达到最优性能。
conclusion: 抑制网络惰性可有效提升融合任务中深度网络的潜力。
---

## Abstract
Infrared and visible image fusion (IVIF) technology has become a frontier of great interest due to the ability to integrate information from multiple sources. However, the progressive slowdown of weight updates in deep networks (i.e., “network laziness” phenomenon), makes existing methods far from realizing the full characterization potential. To this end, we propose a lightweight fusion method for IVIF, Anti-Inert Dynamic Fusion (AIDFusion), to fully utilize the potential of the network at all levels. Specifically, by progressively regulating the collaborative Learning process of multi-level prediction in the network, Dynamic Inertia Inhibition Learning Strategy (DIILS) is proposed to adaptively and efficiently inhibit inertia accumulation. Subsequently, to deeply explore the representation potential while breaking through the performance threshold, lightweight Multi-dimensional modulation fusion module (MMFM) is specifically proposed to capture comprehensive multi-view and multi-scale features efficiently. Finally, considering the semantic bias between the prediction maps of DIILS and the fusion feature of MMFM, Fourier Analysis Convolution (FAConv) is designed in feature recovery as a bridge between prediction and fusion to accomplish the implicit periodic modeling. Based on the above study, extensive experiments on three public IVIF datasets demonstrate the dual advantages of AIDFusion in terms of fusion performance and computational overhead compared to state-of-the-art baseline methods.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：深度神经网络在红外与可见光图像融合（IVIF）训练的中期阶段出现“网络惰性”现象——权重更新幅度显著减缓，导致网络表征潜力未能充分发挥。现有方法往往通过堆叠更复杂的模型来提升性能，但加重了计算负担并加剧了网络惰性。
- **研究动机**：从“网络利用率”的新视角出发，通过轻量级网络结合定制化的学习策略和模块，充分挖掘网络各层潜能，在不增加过多计算开销的前提下实现高质量融合。
- **整体含义**：提出了一种轻量级融合方法AIDFusion（Anti-Inert Dynamic Fusion），旨在利用动态惯性抑制策略、多维调制融合模块和傅里叶分析卷积，同时提升融合性能与计算效率。

## 2. 方法论

### 核心思想
通过三项关键技术协同抑制网络惰性、提升表征能力、弥合语义偏差：
- **DIILS（Dynamic Inertia Inhibition Learning Strategy）**：动态惯性抑制学习策略，避免训练中期权重停滞。
- **MMFM（Multi-dimensional Modulation Fusion Module）**：轻量级多维调制融合模块，高效捕获多视角多尺度特征。
- **FAConv（Fourier Analysis Convolution）**：傅里叶分析卷积，隐式周期性建模以桥接融合与预测。

### 关键技术细节

#### (1) DIILS
- **渐进式层次优化（PHO）**：利用多级预测层进行监督，根据epoch分阶段引入监督信号。前一半epoch仅使用最后一层预测损失，之后每增加T/10 epoch加入一个更浅层的预测损失，直至所有层参与。
- **自适应协同融合损失（L_ACF）**：对每个预测层，计算相关性损失（基于皮尔逊相关系数）、梯度损失（基于Sobel梯度最大选择）、强度损失（基于软加权MSE）。各层损失根据其大小自适应加权（浅层损失权重更高）。

#### (2) MMFM
- **多视角稀疏注意力（MPSA）**：从HW、HC、WC三个视角对特征进行reshape、池化和信息熵加权，抑制冗余视角，保留关键信息。
- **多尺度循环Transformer（MSRT）**：将输入沿通道分为4份，逐份进行不同尺度的自注意力计算（通过池化降低空间维度），并复用前一份输出，最后1×1卷积融合。

#### (3) FAConv
- 将输入特征图分成两路：一路用正弦/余弦函数进行周期性映射，另一路用GELU激活。拼接后输出，替代普通卷积，提供明确的周期性先验。

### 整体框架流程
1. 输入IR和VI，经ResNet编码器提取L层特征。
2. 每层特征与上一层融合模块输出、解码器输出一起输入MMFM，得到本层融合特征。
3. 融合特征经解码器（含FAConv）恢复，再经预测模块得到该层融合图像。
4. 所有预测层融合图像与IR、VI计算L_ACF损失；同时重建VI和IR用于重建损失。
5. 总损失 = λ1×重建损失 + λ2×L_ACF。

## 3. 实验设计

### 数据集
- **MSRS**：主流IVIF基准，用于训练和测试。
- **M3FD**：用于泛化测试。
- **FMB**：用于泛化测试。

### Benchmark与对比方法
- 对比10种基线方法：SwinFusion、LRRNet、CDDFuse、DATFuse、DSFusion、CrossFuse、MRFS、EMMA、SAGE、GIFNet。
- 评价指标：EN（信息熵）、SD（标准差）、CC（相关系数）、SCD（结构相似度差异）、SSIM、MS-SSIM，以及计算开销（Params、GFLOPs、GPU内存）。

## 4. 资源与算力
- **GPU**：单块NVIDIA RTX 4090。
- **训练设置**：MSRS训练集，100个epoch，batch size=12，输入尺寸352×352，优化器SGD，初始学习率0.001。
- **未明确说明**：训练总时长（小时/分钟）以及具体迭代次数未给出，但提供了epoch数和batch size。

## 5. 实验数量与充分性

- **定量比较**：在MSRS上对比11种方法（含本方法），报告6项指标及参数量、GFLOPs、内存；在M3FD和FMB上报告6项指标（无计算开销对比）。
- **定性比较**：在MSRS上展示可视化结果图（图6），分析不同场景下的融合表现。
- **泛化比较**：直接在M3FD和FMB上测试（模型仅在MSRS训练），验证跨数据集泛化能力。
- **消融实验**：在MSRS上逐次去除DIILS、MMFM、FAConv及其组合，共7组实验，展示各组件贡献（表3），并附可视化对比（图7）。
- **充分性评估**：实验较为全面，覆盖了主要数据集和主流方法，定量定性结合，消融设计合理。但消融实验仅在MSRS上进行，未在M3FD和FMB上验证；未进行与更大规模或更新方法的对比（如多任务融合方法）；未进行统计显著性测试或多次重复实验结果；此外，仅一个数据集上训练，其他两个用于泛化，但泛化方法未进行微调，实际上是对零样本泛化的测试，合理但不够充分。

## 6. 主要结论与发现

- AIDFusion在三个数据集上均取得最优或次优的融合性能，尤其在CC、SCD、MS-SSIM指标上领先，且计算开销极低（GFLOPs排名第一，参数量和内存排名靠前）。
- 网络惰性现象确实存在，DIILS能有效保持权重更新活跃，提升最终性能。
- MMFM在极小计算代价下捕获多视角多尺度特征，显著增强表征能力。
- FAConv通过周期性建模加速特征恢复，提升融合质量。
- 消融实验证实三个组件均不可或缺，且具有互补性。

## 7. 优点

- **创新视角**：首次系统性地在IVIF任务中识别并缓解“网络惰性”，从“利用率”而非“堆叠复杂度”角度提升性能。
- **轻量高效**：整体设计紧凑，GFLOPs仅1.85，远低于多数对比方法，利于实际部署。
- **多维特征处理**：MPSA和MSRT从多个维度和尺度进行特征增强，体现了细致的设计考量。
- **先验引入**：FAConv将傅里叶周期性先验嵌入卷积，提供明确学习方向。
- **实验全面**：覆盖三个数据集、10种基线、7组消融，定量定性结合，并给出了代码链接，可复现性强。

## 8. 不足与局限

- **实验覆盖**：消融实验仅在MSRS上进行，未在M3FD和FMB上验证各组件的泛化贡献；对比方法虽然包含近年工作，但仍缺少部分最新方法（如2025年后的其他方法）。
- **指标局限**：主要使用像素级和结构相似度指标，未采用下游任务（如目标检测、分割）的性能评估，对于任务级融合价值论证不足。
- **网络惰性机理**：文中对网络惰性的解释主要基于实验观察（权重Frobenius范数变化），缺乏理论分析或更深入的机制验证；该现象是否普遍存在于其他融合架构尚需更多证据。
- **训练时间**：未报告实际训练时长，仅提供epoch数，无法全面评估训练效率。
- **参数调优**：损失函数中超参数α,β,γ,λ1,λ2的选取未进行灵敏度分析，可能存在调优偏差。
- **应用限制**：仅限于红外与可见光融合，未扩展至其他多模态融合（如医学影像、遥感等）；对极端图像质量（如过暗、过噪）的鲁棒性未测试。

（完）
