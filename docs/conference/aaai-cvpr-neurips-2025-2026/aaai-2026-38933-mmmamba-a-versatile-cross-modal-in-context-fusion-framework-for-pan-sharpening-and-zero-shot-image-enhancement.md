---
title: "MMMamba: A Versatile Cross-Modal in Context Fusion Framework for Pan-Sharpening and Zero-Shot Image Enhancement"
title_zh: MMMamba：用于全色锐化和零样本图像增强的通用跨模态上下文融合框架
authors: "Yingying Wang, Xuanhua He, Chen Wu, Jialing Huang, Suiyun Zhang, Rui Liu, Xinghao Ding, Haoxuan Che"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38933/42895"
tags: ["query:image-fusion"]
score: 9.0
evidence: 用于全色锐化的跨模态融合框架
tldr: 该论文针对传统CNN融合方法适应性差和注意力计算效率低的问题，提出MMMamba，一种基于Mamba架构的跨模态融合框架，用于全色锐化和零样本图像增强。该方法有效利用互补信息，在多个基准上取得优异性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38933/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1833, \"height\": 1194, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38933/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1653, \"height\": 847, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-38933/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 851, \"height\": 601, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38933/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1836, \"height\": 185, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38933/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1840, \"height\": 598, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38933/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1574, \"height\": 445, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38933/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 748, \"height\": 199, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-38933/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 578, \"height\": 328, \"label\": \"Table\"}]"
motivation: CNN融合方法适应性差，交叉注意力计算效率低且稀释细粒度对应关系。
method: 利用Mamba架构构建跨模态融合框架，实现高效全局交互。
result: 在全色锐化和图像增强任务上达到最优性能。
conclusion: MMMamba为多模态融合提供了高效且可扩展的解决方案。
---

## Abstract
Pan-sharpening aims to generate high-resolution multispectral (HRMS) images by integrating a high-resolution panchromatic (PAN) image with its corresponding low-resolution multispectral (MS) image. To achieve effective fusion, it is crucial to fully exploit the complementary information between the two modalities. Traditional CNN-based methods typically rely on channel-wise concatenation with fixed convolutional operators, which limits their adaptability to diverse spatial and spectral variations. While cross-attention mechanisms enable global interactions, they are computationally inefficient and may dilute fine-grained correspondences, making it difficult to capture complex semantic relationships. Recent advances in the Multimodal Diffusion Transformer (MMDiT) architecture have demonstrated impressive success in image generation and editing tasks. Unlike cross-attention, MMDiT employs in-context conditioning to facilitate more direct and efficient cross-modal information exchange. In this paper, we propose MMMamba, a cross-modal in-context fusion framework for pan-sharpening, with the flexibility to support image super-resolution in a zero-shot manner. Built upon the Mamba architecture, our design ensures linear computational complexity while maintaining strong cross-modal interaction capacity. Furthermore, we introduce a novel multimodal interleaved (MI) scanning mechanism that facilitates effective information exchange between the PAN and MS modalities. Extensive experiments demonstrate the superior performance of our method compared to existing state-of-the-art (SOTA) techniques across multiple tasks and benchmarks.

---

## 论文详细总结（自动生成）

# MMMamba 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：全色锐化（Pan-sharpening）旨在融合高分辨率全色（PAN）图像和低分辨率多光谱（MS）图像，生成高分辨率多光谱（HRMS）图像。传统 CNN 方法依赖固定卷积的通道拼接，对空间和光谱变化的适应性差；交叉注意力机制虽能全局交互，但计算效率低，且加权平均会稀释高频细节和细粒度对应关系。此外，现有方法通常只能处理单一任务（如融合或超分辨），缺乏零样本泛化能力。
- **背景**：多模态扩散变换器（MMDiT）的 in-context conditioning 范式在图像生成中展现出高效跨模态交互，但直接应用在图像融合中会带来自注意力二次复杂度。Mamba 架构（状态空间模型）具有线性复杂度，适用于长序列建模。
- **整体含义**：论文提出基于 Mamba 的跨模态上下文融合框架 MMMamba，首次将 in-context conditioning 引入全色锐化，实现线性复杂度、双向信息流和零样本泛化到图像超分辨率任务。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 抛弃传统的通道拼接或交叉注意力，采用 **in-context conditioning** 范式：将 PAN 和 MS 特征的 token 拼接成统一序列，由 Mamba 状态空间模型联合处理，实现深层双向交互。
- 基于 **Mamba** 架构保持线性计算复杂度，设计 **多模态交错（MI）扫描机制** 促进跨模态信息交换。

### 关键技术细节
- **网络架构**：
  - 双分支编码器（gated convolutional encoders）分别提取 PAN 和 MS 浅层特征 \(F_p\) 和 \(F_{ms}\)。
  - 多个 **MMMamba 块** 处理浅层特征：层归一化 → 线性投影 → 深度卷积 + SiLU 激活 → MI-SSM → 逐元素乘加 → 线性投影输出。
  - 卷积解码器 \(D_\phi\) 输出最终 MS 特征 \(F_{final}^{ms}\)，与上采样 LRMS 相加得到 HRMS。
- **多模态交错（MI）扫描**：
  - **Tokenization**：将 PAN 和 MS 特征分割成非重叠 patch，沿着四个方向（左→右+上→下、上→下+左→右、右→左+下→上、下→上+右→左）进行交错拼接，形成融合序列。
  - **MI-Scan**：将融合序列拆分为两个部分，在每个局部窗口内交替扫描这两个部分，实现跨模态局部交互。多方向扫描结果求和得到最终输出。
- **损失函数**：L1 损失 \(\mathcal{L} = \|I_{gt} - I_{hms}\|_1\)。

### 算法流程（文字说明）
1. 输入：上采样 LRMS \(I_{ms}\) 和 PAN \(I_p\)。
2. 通过门控卷积编码器提取浅层特征 \(F_{ms}, F_p\)。
3. 经过 \(N\) 个 MMMamba 块逐步融合：每个块内特征经 LN、线性投影、DWConv+SiLU 后，进行多方向 MI 扫描，再与 SiLU 激活的投影融合，最后线性投影输出。
4. 最后一个块的 MS 特征经卷积解码器得到残差，与上采样 LRMS 相加得最终 HRMS。

## 3. 实验设计

- **数据集与场景**：
  - **WorldView-II (WV2)**：工业区和自然景观，分辨率多样。
  - **GaoFen2 (GF2)**：山地和河流。
  - **WorldView-III (WV3)**：城市环境。
  - 根据 Wald 协议生成降分辨率测试集（无原图真值）。
  - **全分辨率评估**：使用原始 GF2 (FGF2) 数据，无降采样。
  - **零样本超分辨率**：仅输入 MS 图像（去除 PAN），在 WV2 上测试。
- **Benchmark 对比方法**：
  - 传统：IHS, Brovey, SFIM, GFPCA, LRTCFPan。
  - 深度学习：SRPPNN, INNformer, FAME, WaveletNet, SFINet++, Pan-Mamba, CFLIHPs。
- **评价指标**：
  - 全参考：PSNR ↑, SSIM ↑, SAM ↓, ERGAS ↓。
  - 无参考：空间失真 \(D_S\) ↓, 光谱失真 \(D_\lambda\) ↓, 质量无参考指数 QNR ↑。

## 4. 资源与算力

- **实现框架**：PyTorch。
- **GPU**：单个 **Nvidia V100** GPU。
- **训练配置**：
  - 优化器：Adam，梯度裁剪范数 4.0。
  - 学习率：初始 \(5\times10^{-4}\)，余弦衰减至 \(5\times10^{-8}\)。
  - 训练轮次：WV2 200 epochs，GF2 和 WV3 500 epochs（根据数据量调整）。
- **未明确说明**：每轮训练时间、总训练时长、推理速度。但提供了 FLOPs（5.06 G）和参数量（0.2453 M）。计算效率与其他方法的对比如表5。

## 5. 实验数量与充分性

- **实验组数**：共展示 5 个主要表格和 3 个图示：
  1. 缩减分辨率场景：三个数据集上的定量对比（表1）及 WV3 可视化（图2）。
  2. 全分辨率场景：FGF2 上的无参考指标（表2）。
  3. 零样本超分辨率：WV2 上的定量（表4）和可视化（图3）。
  4. 消融实验（表3）：包含 2 组共 6 个变体：
     - 核心范式：去掉 Mamba（替换为 Transformer）、去掉上下文融合（替换为通道拼接）、去掉交错（替换为顺序拼接）。
     - 扫描策略：去掉多方向（单方向）、去掉局部窗口（全局扫描）。
  5. 计算效率对比（表5）：与 7 种 SOTA 方法比较 FLOPs 和参数量。
- **充分性评估**：
  - **客观公平**：使用统一 Wald 协议生成测试数据，相同评测指标，对比方法涵盖传统和深度学习主流。消融实验控制变量，代价相似。
  - **不足**：零样本超分辨率仅测试了 WV2 一个数据集，未在 GF2/WV3 上验证泛化性；全分辨率场景仅测了 GF2；消融实验仅在 WV2 上运行，未在多数据集交叉验证趋势。

## 6. 主要结论与发现

- **全色锐化性能**：MMMamba 在 WV2、GF2、WV3 三个数据集上 **全面超越 SOTA**。例如在 WV2 上 PSNR 达 42.3120（优于第2名 CFLIHPs 的 41.9077），GF2 上达 47.9932（高出 0.61 dB）。
- **全分辨率场景**：在 FGF2 上，QNR 指标达 0.8312，接近第一（SFINet++ 0.8471），但光谱失真 \(D_S\) 为 0.1113（低于 SFINet++ 的 0.1170，更优？表中显示 SFINet++ 的 \(D_S=0.1108\) 更小，而 ours=0.1113，稍差；但总体 QNR 接近。原文称“consistently outperforms”，实际上 QNR 略低于 SFINet++，需注意细节）。
- **零样本超分辨率**：在不重新训练的情况下，仅用 MS 输入即实现超分辨率，PSNR 36.49，远超 Bicubic 和调整后的 SFINet++、Pan-Mamba。
- **效率**：0.2453 M 参数、5.06 G FLOPs，在模型大小和计算量之间取得平衡，优于多数大模型（如 SRPPNN 21 G FLOPs）。
- **消融关键**：Mamba 骨干优于 Transformer；交错设计、多方向扫描、局部窗口均带来显著提升。

## 7. 优点

- **方法创新**：首次将 in-context conditioning 范式用于全色锐化，结合 Mamba 实现线性复杂度，避免了二次复杂度的自注意力。
- **扫描机制**：MI 扫描通过空间相邻 token 的交错排列和局部交替扫描，充分利用 PAN 和 MS 的互补空间信息，优于简单拼接。
- **零样本泛化**：单一训练模型即可完成全色锐化和超分辨率，无需微调，实用性强。
- **实验全面**：覆盖三个卫星数据集、缩减/全分辨率场景、零样本任务、多组消融，定量和可视化并重。
- **消融分析清晰**：逐步验证了核心组件（Mamba、上下文融合、交错、多方向、局部窗口）的必要性，且计算开销几乎不变。
- **计算效率好**：参数和 FLOPs 在同类中处于中等，但性能领先。

## 8. 不足与局限

- **实验覆盖有限**：
  - 零样本超分辨率仅在 WV2 上测试，缺少在 GF2、WV3 上的验证，泛化性证据不足。
  - 全分辨率场景仅用 GF2，且 QNR 指标略低于 SFINet++，未做进一步分析或解释。
  - 未与其他 Mamba 变体（如 LEVM、Pan-Mamba 之外的 Mamba 融合方法）对比，仅与 Pan-Mamba 比较。
- **应用限制**：
  - 论文仅聚焦全色锐化和超分辨，未探索在更通用的多模态融合任务（如多光谱-高光谱、SAR-光学）上的表现。
  - 需依赖上采样步骤（预处理），可能引入额外误差。
  - L1 损失简单，未尝试感知/对抗损失等更复杂的监督。
- **偏差风险**：
  - 消融仅在 WV2 进行，不同数据集可能呈现不同规律（例如全分辨率场景下 QNR 未绝对领先）。
  - 训练和评估基于 Wald 协议（降分辨率模拟），真实全分辨率场景的真实性有待验证（FGF2 仅用无参考指标，缺乏真值确认）。
- **可复现性**：提供了代码仓库，但未说明是否包含完整训练、评测脚本和预训练模型，可能影响复现。

（完）
