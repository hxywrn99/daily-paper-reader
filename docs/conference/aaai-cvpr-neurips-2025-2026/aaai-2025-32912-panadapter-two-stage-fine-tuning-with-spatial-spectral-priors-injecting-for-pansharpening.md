---
title: "PanAdapter: Two-Stage Fine-Tuning with Spatial-Spectral Priors Injecting for Pansharpening"
title_zh: PanAdapter：结合空间-光谱先验注入的两阶段微调用于全色锐化
authors: "RuoCheng Wu, Zien Zhang, Shangqi Deng, Yule Duan, Liang-Jian Deng"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32912/35067"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于深度学习的全色锐化图像融合
tldr: 全色锐化任务中深度学习模型受限于数据集规模。本文提出PanAdapter两阶段微调方法，利用预训练模型的空间-光谱先验知识，在小样本场景下提升融合性能。实验证明该方法在多个基准上优于现有专门模型，尤其适用于数据稀缺情况。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 836, \"height\": 707, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 807, \"height\": 511, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1641, \"height\": 805, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1790, \"height\": 383, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1793, \"height\": 383, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 762, \"height\": 414, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32912/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 839, \"height\": 426, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32912/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 868, \"height\": 440, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32912/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1754, \"height\": 574, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32912/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 869, \"height\": 439, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32912/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 880, \"height\": 370, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32912/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 879, \"height\": 278, \"label\": \"Table\"}]"
motivation: 现有全色锐化模型受限于数据集大小，难以充分利用预训练知识。
method: 提出两阶段微调框架，注入预训练模型的空间-光谱先验。
result: 在多个全色锐化数据集上取得最优结果，尤其在小样本情况下。
conclusion: PanAdapter有效缓解了数据不足问题，提升了图像融合迁移能力。
---

## Abstract
Pansharpening is a challenging image fusion task that involves restoring images using two different modalities: low-resolution multispectral images (LRMS) and high-resolution panchromatic (PAN). Many end-to-end specialized models based on deep learning (DL) have been proposed, yet the scale and performance of these models are limited by the size of dataset. Given the superior parameter scales and feature representations of pre-trained models, they exhibit outstanding performance when transferred to downstream tasks with small datasets. Therefore, we propose an efficient fine-tuning method, namely PanAdapter, which utilizes additional advanced semantic information from pre-trained models to alleviate the issue of small-scale datasets in pansharpening tasks. Specifically, targeting the large domain discrepancy between image restoration and pansharpening tasks, the PanAdapter adopts a two-stage training strategy for progressively adapting to the downstream task. In the first stage, we fine-tune the pre-trained CNN model and extract task-specific priors at two scales by proposed Local Prior Extraction (LPE) module. In the second stage, we feed the extracted two-scale priors into two branches of cascaded adapters respectively. At each adapter, we design two parameter-efficient modules for allowing the two branches to interact and be injected into the frozen pre-trained VisionTransformer (ViT) blocks. We demonstrate that by only training the proposed LPE modules and adapters with a small number of parameters, our approach can benefit from pre-trained image restoration models and achieve state-of-the-art performance in several benchmark pansharpening datasets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：全色锐化（Pansharpening）是一项具有挑战性的图像融合任务，目标是将低分辨率多光谱图像（LRMS）与高分辨率全色图像（PAN）融合为高分辨率多光谱图像（HRMS）。现有基于深度学习（DL）的端到端专用模型受限于数据集规模（通常仅 500–10,000 张图像），盲目增加参数容易导致过拟合。而预训练模型（尤其在自然图像上预训练的图像复原模型）具有更强的参数规模和特征表示能力，如何将其有效迁移到全色锐化任务是一个关键问题。
- **核心挑战**：图像复原（如超分辨率）与全色锐化之间存在显著的域差异（domain gap），且全色锐化需要同时处理多模态输入和多尺度特征，传统微调方法难以直接适配。
- **目标**：提出一种参数高效的微调框架 PanAdapter，利用预训练模型的空间-光谱先验知识，缓解小数据集问题，提升融合性能。

## 2. 论文提出的方法论

### 2.1 核心思想
- **两阶段渐进式微调策略**：先微调预训练 CNN（EDSR）提取空间和光谱先验，再将这些先验注入到冻结的预训练 ViT（IPT）中，通过级联适配器实现多尺度特征交互。
- **参数高效**：仅训练少量新增的 LPE 模块和适配器，冻结大部分预训练参数。

### 2.2 关键技术细节

#### 第一阶段：局部先验提取阶段（Local Prior Extraction Stage）
- 输入：LRMS 经上采样后与 PAN 拼接得到 Q。
- 网络：两个并行的预训练 EDSR（用于超分辨率）分别处理光谱分支（输入 M）和空间分支（输入 Q），并插入 **局部先验提取（LPE）模块** 来降低通道维度，从多个 CNN 层提取多尺度特征。
- 输出：对两个分支的中间特征进行拼接和下投影，得到 **光谱先验 A**（低分辨率尺度）和 **空间先验 B**（高分辨率尺度）。
- 解码：使用 **隐式神经表示（INR）** 技术（基于 SIREN 激活函数）融合 A、B 并上采样，加上上采样后的 LRMS 得到第一阶段的预测 O1。

#### 第二阶段：多尺度特征交互阶段（Multiscale Feature Interaction Stage）
- 输入：Q 送入冻结的预训练 ViT（IPT），A 和 B 分别送入两个级联适配器分支（低分辨率分支对应 A，高分辨率分支对应 B）。
- 核心模块：
  - **级联令牌融合器（CTF）**：在每一层适配器中，将两个分支的特征进行元素级加权融合，并与 ViT 中间特征进行交叉注意力（Cross-Attention），输出更新后的分支特征。
  - **级联令牌注入器（CTI）**：将融合后的两个分支特征（F<sub>spe</sub> 和 F<sub>spa</sub>）拼接后通过线性投影，作为 Query 与 ViT 特征（Key/Value）进行交叉注意力，将先验信息注入回 ViT 主干，并用可学习参数 s<sub>j</sub>（初始化为 0）控制残差连接。
- 适配器每隔 4 个 ViT 块插入一次（超参数 t=4），最终输出经 INR 解码得到第二阶段预测 O2。

### 2.3 公式说明（文字描述）
- 第一阶段：`{A, B} = SSPEN(Q, M)`；`O1 = Tail1(A, B) + M↑`
- 第二阶段：`{\hat{A}, \hat{B}} = MFIN(A, B, Q)`；`O2 = Tail2(\hat{A}, \hat{B})`
- CTF 融合公式：`\tilde{F}_{spa} = F_{spa} · w(F_{spe}) + F_{spa}`，然后通过交叉注意力、FFN。
- CTI 注入公式：`\hat{F}_{vit} = s · Attention(F_{vit}, F_{fus}) + F_{vit}`

## 3. 实验设计

### 3.1 数据集与场景
- **WV3**（WorldView-3）：8 波段（多光谱），训练 10000 对 64×64 图像，测试 20 对 256×256（降分辨率评估）和 20 对 512×512（全分辨率评估）。
- **QB**（QuickBird）：4 波段，20 对降分辨率测试。
- **GF2**（GaoFen-2）：4 波段，20 对降分辨率测试。
- 使用 Wald 协议生成模拟数据（有 GT）用于训练；全分辨率评估无 GT 图像，采用无参考指标。

### 3.2 Benchmark 与对比方法
- **对比方法**：传统方法（EXP、TV、BDSD-PC）；深度学习方法：PanNet、FusionNet、LAGConv、PMACNet、BiMPan、HMPNet、PanDiff、PanMamba、CANNet。
- 所有 DL 方法在相同数据集上重新训练，超参数按原论文设置。

### 3.3 评估指标
- **降分辨率**：PSNR、Q2n（Q8/Q4）、SAM、ERGAS。
- **全分辨率**：D<sub>λ</sub>、D<sub>s</sub>、QNR。

## 4. 资源与算力

- **文中未明确说明 GPU 型号、数量、训练时长等细节**。仅提到调整内在维度 k 可控制参数量（k 为 48/64/96/128 时，可训练参数量约 1.85%–2.19% 总参数量）。实验表明 PanAdapter 在参数量远小于全微调（115.2M）的情况下性能最佳（可训练参数仅 2.19M）。

## 5. 实验数量与充分性

- **主实验**：在 WV3、QB、GF2 三个数据集上进行了降分辨率和全分辨率评估，报告了均值和标准差，统计显著。
- **消融实验**（WV3 上）：
  - 验证两阶段策略（去掉两阶段直接单阶段训练：SAM 3.573 vs 2.917，ERGAS 2.576 vs 2.149）。
  - 验证 INR 模块（替换为普通卷积上采样：性能下降）。
  - 验证 CTF 模块（去掉 CTF：性能下降）。
  - 验证 CTI 模块（去掉 CTI：性能下降明显，SAM 3.301）。
- **对比其他微调策略**：比较了 VPT、LoRA、Adapter、LST、全微调，PanAdapter 各项指标最优。
- **参数敏感性**：图 7 展示了不同 k 值（48~128）下的性能与参数量，持续优于现有 SOTA。
- **可视化**：提供 GF2 和 WV3 的定性结果（误差图、热力图），显示 PanAdapter 恢复细节更精确。

**充分性评价**：实验较为充分，覆盖多数据集、多指标、关键消融、对比各种 PEFT 方法。但缺乏对更大规模模型（如 ViT-Large）或多任务泛化性的验证。

## 6. 论文的主要结论与发现

- PanAdapter 是首个专门针对全色锐化设计的参数高效微调框架，成功将自然图像预训练模型迁移至遥感图像融合。
- 两阶段训练策略有效降低了域迁移难度，CNN 提取的先验有助于 ViT 捕获局部细节。
- 级联适配器（CTF + CTI）实现了空间和光谱先验的交互与注入，显著提升了融合质量。
- 在 WV3、QB、GF2 三个数据集上，PanAdapter 在所有指标上优于现有 SOTA 方法（尤其 PSNR 在 WV3 上达到 39.473，超过第二好的 CANNet 38.908）。
- 仅微调约 1.85%–2.19% 的参数即可达到甚至超越全微调性能，训练效率高。

## 7. 优点

- **创新性**：首次将 PEFT 引入全色锐化，提出针对性的两阶段渐进式策略，结合 CNN 局部先验与 ViT 全局建模能力。
- **设计巧妙**：LRMS 和 PAN 分别提取光谱/空间先验，通过级联适配器实现多尺度融合；CTF 和 CTI 模块参数高效、可解释性强。
- **实验严谨**：多数据集、多指标、消融完整，对比多种 PEFT 方法，结果统计显著。
- **实用性**：参数量少，训练快速，易于部署；代码开源。

## 8. 不足与局限

- **算力与时间信息缺失**：未报告 GPU 型号、训练耗时等，影响可复现性评估。
- **数据集覆盖有限**：仅使用了 WV3、QB、GF2，缺乏对更多传感器（如 Pleiades、WorldView-2）或不同分辨率比场景的验证。
- **对更大规模模型的扩展性未验证**：预训练 ViT 采用 IPT（约 115M 参数），未尝试更大的 ViT-L/MAE 等；内在维度 k 的上限未探索。
- **域适应理论分析不足**：为何两阶段策略有效仅有实验证明，缺乏理论解释。
- **应用限制**：依赖预训练模型的可用性；若预训练任务与全色锐化差异过大（如去噪 vs. 融合），性能可能受损；目前仅适用于配对数据，未考虑无监督场景。
- **消融实验仅在 WV3 上**，未在 QB/GF2 上重复，可能削弱泛化性结论。
- **对全分辨率指标（Dλ、Ds、QNR）优势不突出**：在 WV3 上 Dλ 和 Ds 指标与 CANNet 接近（如 Ds 0.0304 vs 0.0301），说明空间失真方面提升有限。

（完）
