---
title: Multitasks-based Deep Evidential Fusion Network for Blind Image Quality Assessment
title_zh: 基于多任务的深度证据融合网络用于盲图像质量评估
authors: "Yiwei Lou, Yuanpeng He, Rongchao Zhang, Yongzhi Cao, Hanpin Wang, Yu Huang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37691/41653"
tags: ["query:image-fusion"]
score: 4.0
evidence: 深度证据融合网络用于盲图像质量评估
tldr: 针对盲图像质量评估中辅助任务集成不足和不确定估计缺乏的问题，提出多任务深度证据融合网络DEFNet，设计可信信息融合策略，先融合子区域特征增强信息丰富度，再进行局部-全局融合。在多个BIQA基准上取得最优性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有BIQA方法融合不充分且缺乏不确定性估计。
method: 提出DEFNet，包含多任务优化和可信信息融合策略，进行子区域和局部-全局融合。
result: 在多个BIQA数据集上达到最优性能。
conclusion: DEFNet有效提升了BIQA的鲁棒性和可靠性。
---

## Abstract
Blind image quality assessment (BIQA) methods often incorporate auxiliary tasks to improve performance. However, existing approaches face limitations due to insufficient integration and a lack of flexible uncertainty estimation, leading to suboptimal performance. To address these challenges, we propose a multitasks-based Deep Evidential Fusion Network (DEFNet) for BIQA, which performs multitask optimization with the assistance of scene and distortion type classification tasks. To achieve a more robust and reliable representation, we design a novel trustworthy information fusion strategy. It first combines diverse features and patterns across sub-regions to enhance information richness, and then performs local-global information fusion by balancing fine-grained details with coarse-grained context. Moreover, DEFNet exploits advanced uncertainty estimation technique inspired by evidential learning with the help of normal-inverse gamma distribution mixture. Extensive experiments on both synthetic and authentic distortion datasets demonstrate the effectiveness and robustness of the proposed framework. Additional evaluation and analysis are carried out to highlight its strong generalization capability and adaptability to previously unseen scenarios.

---

## 论文详细总结（自动生成）

# 基于多任务的深度证据融合网络用于盲图像质量评估（DEFNet）——论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有盲图像质量评估（BIQA）方法常通过引入辅助任务（如场景分类、失真类型分类）来提升性能，但存在两大缺陷：
  - **信息融合不充分**：① 任务间信息碎片化，缺乏深层交互；② 多级特征与跨区域特征融合不足。
  - **不确定性估计缺乏灵活性**：无法同时建模偶然不确定性（aleatoric uncertainty）和认知不确定性（epistemic uncertainty），导致预测过于自信且不可靠。
- **意义**：BIQA 在多媒体处理、医学图像分析等领域至关重要，更鲁棒、更准确的评估方法能提升用户体验和系统可靠性。

## 2. 方法论
### 2.1 核心思想
- 提出**DEFNet**，一种基于多任务学习的深度证据融合网络，同时优化主任务（BIQA）和两个辅助任务（场景分类、失真类型分类）。
- 设计**两级可信信息融合策略**：① 跨子区域融合；② 局部-全局融合。
- 利用**证据学习（Evidential Learning）** 与**正态逆伽马（NIG）分布混合**实现灵活的不确定性估计。

### 2.2 关键技术细节
1. **特征提取与概率得分计算**：
   - 使用冻结的 CLIP（ViT-B/32 + GPT-2）提取全局和局部子图像特征，通过 softmax 得到联合概率 \(\hat{p}(c,s,d|x)\)。
   - 质量得分估计：\( \hat{q}(x) = \sum_{c=1}^5 \hat{p}(c|x) \times c \)，其中 \(\hat{p}(c|x)\) 为所有子图像局部概率的聚合。
2. **多任务优化损失 \(L_M\)**：
   - 包含 BIQA 损失（基于 Fidelity loss 与 Thurstone 模型）、场景分类损失（Fidelity loss）、失真分类损失（交叉熵形式），通过权重 \(\lambda_q, \lambda_s, \lambda_d\) 平衡（采用相对下降率动态更新）。
3. **跨子区域信息融合（Cross Sub-region Fusion）**：
   - 随机采样 4 个子图像，对其概率得分分别进行任务级边缘化（如 \(x_{q,i}^{\text{local}} = \text{softplus}(\sum_c [\sum_{s,d} \hat{p}(c,s,d|x_i^{\text{local}}) \times c])\)）。
   - 将每个子图像的参数输入四个 NIG 分布，通过融合操作（⊕，定义见公式(20)）得到混合 NIG 分布 \(\text{NIG}(x_t^{\text{cross}})\)。
   - 计算跨区域证据损失 \(L_U^t\)，并求和得 \(L_U\)。
4. **局部-全局信息融合（Local-global Fusion）**：
   - 对全局图像类似地得到全局 NIG 参数 \(m_t^{\text{global}}\)。
   - 将每个局部 NIG 分布与全局 NIG 分布融合得到 \(\text{NIG}(m_{t,i}^{\text{fusion}})\)，然后平均得到 \(x_t^{\text{fusion}}\)。
   - 计算局部-全局融合损失 \(L_F^t\)，并求和得 \(L_F\)。
5. **总体损失**：
   \[
   L(\theta) = L_M(\theta) + \lambda_1 L_U(\theta) + \lambda_2 L_F(\theta)
   \]
   其中 \(\lambda_1, \lambda_2\) 为超参数控制各损失贡献。

## 3. 实验设计
### 3.1 数据集与场景
- **合成失真数据集**：LIVE、CSIQ、KADID-10k。
- **真实失真数据集**：BID、LIVE-C、KonIQ-10k。
- **跨数据集评估（零样本）**：TID2013、SPAQ、PIPAL 训练集。
- 每个数据集随机按 70% 训练、10% 验证、20% 测试，重复 10 次。

### 3.2 Benchmark 与对比方法
- 对比方法包括：NIQE、ILNIQE、dipIQ、Ma19、DBCNN、HyperIQA、UNIQUE、TreS、LIQE、CONTRIQUE、VCRNet、Re-IQA、DPNet、QAL-IQA、CDINet、TOPIQ-FR、KGANet、CausalQuality 等。
- 评估指标：Spearman 秩相关系数（SRCC）和 Pearson 线性相关系数（PLCC）。

## 4. 资源与算力
- **硬件**：单张 NVIDIA RTX 4090 GPU。
- **训练配置**：初始学习率 5e-6，训练 80 个 epoch，mini-batch size 为 48（其中 LIVE/CSIQ/BID/LIVE-C 每个 4 张，KADID-10k/KonIQ-10k 每个 16 张）。
- 文中未明确说明总训练时长，但基于常见配置可推断为数小时级。

## 5. 实验数量与充分性
- **主要性能对比**：在 6 个标准数据集上（合成+真实），对比了十余种 SOTA 方法，结果见表 1。
- **跨数据集泛化**：在 TID2013、SPAQ、PIPAL 上进行零样本测试（表 2）。
- **消融实验**：考察了任务组合（BIQA  alone、+场景、+失真、+两者）及损失组件（\(L_M, L_U, L_F\)）的影响，共记录 16 种设置（表 3）。
- **融合策略对比**：将证据融合替换为简单平均，比较 SRCC 提升（图 4）。
- **超参数分析**：调整 \(\lambda_1, \lambda_2\) 的 6 种组合（表 4）。
- **不确定性分析**：对比 LIQE 的置信区间宽度（表 5）。
- **充分性评价**：实验覆盖了主要合成/真实失真场景，消融和对比设计较完整，且均基于 10 次随机划分取平均，结果可靠公平。

## 6. 主要结论与发现
- DEFNet 在所有合成和真实失真数据集上均达到 SOTA 性能（如 LIVE 上 SRCC 0.978，CSIQ 上 0.967，KADID-10k 上 0.942，BID 上 0.910，LIVE-C 上 0.918，KonIQ-10k 上 0.920）。
- 跨数据集测试显示较强泛化能力（TID2013 上 SRCC 0.828，SPAQ 上 0.868），但在 PIPAL 上仅 0.464，说明对极度多样化失真仍需改进。
- 消融实验证明：辅助任务、跨子区域融合、局部-全局融合均显著提升性能，且证据融合优于简单平均（平均 SRCC 提升 2.0%）。
- 不确定性分析表明 DEFNet 比 LIQE 具有更窄的置信区间（0.251 vs 0.286），预测更可靠。

## 7. 优点
- **多任务集成巧妙**：同时利用场景和失真信息，通过联合概率建模实现任务间深度交互。
- **双层次融合设计**：跨子区域融合增强局部多样性，局部-全局融合平衡细节与全局上下文，信息互补充分。
- **先进的不确定性估计**：基于证据学习和 NIG 混合，同时建模偶然与认知不确定性，提升预测可靠性和鲁棒性。
- **实验严谨全面**：涵盖多个数据集、多种对比方法、细致消融和超参数分析，结果具有说服力。

## 8. 不足与局限
- **跨数据集泛化局限**：在 PIPAL 数据集上 SRCC 仅 0.464，表明对高度多样化、非典型的失真类型适应性有限，可能需要更多训练数据或更复杂的域适应机制。
- **超参数敏感性**：\(\lambda_1, \lambda_2\) 取值对性能有影响（过大导致下降），需要仔细调优，实际应用时可能增加部署成本。
- **计算资源依赖**：依赖 CLIP 预训练模型和额外子图像处理，推理时需 15 个子图像，可能对实时性要求较高的场景造成负担。
- **偏差风险**：训练数据主要来自公开数据集，可能隐含特定采集设备或场景偏置，在极端环境下泛化性未充分验证。
- **实验未提及**：训练时间、模型参数量等细节未给出，不利于完整复现比较。

（完）
