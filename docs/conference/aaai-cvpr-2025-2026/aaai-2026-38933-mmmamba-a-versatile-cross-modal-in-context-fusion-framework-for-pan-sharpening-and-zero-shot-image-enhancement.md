---
title: "MMMamba: A Versatile Cross-Modal in Context Fusion Framework for Pan-Sharpening and Zero-Shot Image Enhancement"
title_zh: MMMamba：面向全色锐化与零样本图像增强的通用跨模态上下文融合框架
authors: "Yingying Wang, Xuanhua He, Chen Wu, Jialing Huang, Suiyun Zhang, Rui Liu, Xinghao Ding, Haoxuan Che"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38933/42895"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于Mamba的跨模态上下文融合全色锐化
tldr: 针对传统CNN在全色锐化中适应性有限以及交叉注意力计算效率低的问题，本文提出MMMamba框架，利用状态空间模型实现高效的跨模态上下文融合。该框架在融合过程中充分利用全色和多光谱图像的互补信息，并在全色锐化和零样本图像增强任务上均取得优异性能，展现了良好的通用性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: CNN和交叉注意力在全色锐化中存在局限性。
method: 利用状态空间模型构建跨模态上下文融合框架。
result: 在全色锐化和零样本增强上均取得优异性能。
conclusion: 状态空间模型为多模态融合提供了高效新范式。
---

## Abstract
Pan-sharpening aims to generate high-resolution multispectral (HRMS) images by integrating a high-resolution panchromatic (PAN) image with its corresponding low-resolution multispectral (MS) image. To achieve effective fusion, it is crucial to fully exploit the complementary information between the two modalities. Traditional CNN-based methods typically rely on channel-wise concatenation with fixed convolutional operators, which limits their adaptability to diverse spatial and spectral variations. While cross-attention mechanisms enable global interactions, they are computationally inefficient and may dilute fine-grained correspondences, making it difficult to capture complex semantic relationships. Recent advances in the Multimodal Diffusion Transformer (MMDiT) architecture have demonstrated impressive success in image generation and editing tasks. Unlike cross-attention, MMDiT employs in-context conditioning to facilitate more direct and efficient cross-modal information exchange. In this paper, we propose MMMamba, a cross-modal in-context fusion framework for pan-sharpening, with the flexibility to support image super-resolution in a zero-shot manner. Built upon the Mamba architecture, our design ensures linear computational complexity while maintaining strong cross-modal interaction capacity. Furthermore, we introduce a novel multimodal interleaved (MI) scanning mechanism that facilitates effective information exchange between the PAN and MS modalities. Extensive experiments demonstrate the superior performance of our method compared to existing state-of-the-art (SOTA) techniques across multiple tasks and benchmarks.

---

## 论文详细总结（自动生成）

# MMMamba论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：全色锐化（Pan-sharpening）旨在融合高分辨率全色（PAN）图像和低分辨率多光谱（MS）图像，生成高分辨率多光谱（HRMS）图像。现有方法存在两大局限：
  - **CNN方法**：依赖通道拼接和固定卷积算子，无法自适应建模跨模态复杂关系。
  - **Transformer交叉注意力机制**：虽能捕获全局交互，但计算复杂度为二次方（O(N²)），且加权平均会平滑高频细节，信息流单向，交互深度有限。
- **研究动机**：受多模态扩散Transformer（MMDiT）中“上下文条件化”（in-context conditioning）范式的启发——该范式将多模态token拼接后通过自注意力联合处理，实现双向深度交互——但直接使用Transformer会导致图像融合中计算负担过重。因此，论文希望设计一种**线性复杂度、强跨模态交互**的框架。
- **整体含义**：提出了MMMamba，基于状态空间模型（Mamba）的跨模态上下文融合框架，首次将in-context conditioning范式引入全色锐化，并支持零样本迁移至MS图像超分辨率任务。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 2.1 核心思想
- **基于Mamba架构**：利用状态空间模型（SSM）的线性计算复杂度（O(N)）和动态选择机制，实现高效长程依赖建模。
- **上下文融合范式**：丢弃传统的通道拼接或交叉注意力，将PAN和MS的token交织成一个统一序列，通过共享的SSM进行双向信息流处理。
- **零样本泛化**：训练时只需全色锐化数据；推理时仅输入MS图像（去掉PAN），即可执行超分辨率，无需微调。

### 2.2 关键组件
#### 2.2.1 网络架构
- **输入**：上采样的LRMS图像 \(I_{ms} \in \mathbb{R}^{H \times W \times C}\) 和 PAN图像 \(I_p \in \mathbb{R}^{H \times W \times 1}\)。
- **编码器**：分别用门控卷积编码器 \(E_{ms}^\phi\) 和 \(E_p^\phi\) 提取浅层特征 \(F_{ms}\) 和 \(F_p\)（维度均为 \(B \times C \times H \times W\)）。
- **MMMamba块**：多个堆叠的块，每个块包含：
  1. **层归一化 + 线性投影**
  2. **深度卷积 + SiLU激活**
  3. **多模态交错扫描（MI-Scan）**：核心创新。
  4. **逐元素乘法与求和**，再经线性投影输出。
- **解码器**：卷积解码器 \(D_\phi\) 得到最终MS特征 \(F_{ms}^{final}\)，与上采样LRMS相加得到HRMS结果 \(I_{hms}\)。

#### 2.2.2 多模态交错扫描（MI-SSM）
- **Token化与交错**：将MS和PAN特征划分为不重叠的patch，按四个预定义方向（ltr_utd, utd_ltr, rtl_dtu, dtu_rtl）进行patch级交错，形成融合序列。
- **局部窗口内交替扫描**：在每个方向序列上，将序列分成两部分（对应于MS和PAN的交替位置），先在第一个局部窗口扫描MS部分，再切换到同一窗口的PAN部分，重复直到所有窗口完成。这样确保对应空间位置的信息能直接交换。
- **多方向聚合**：将四个方向的扫描结果相加，得到最终输出。

#### 2.2.3 损失函数
- 使用L1损失：\(\mathcal{L} = \| I_{gt} - I_{hms} \|_1\)。

## 3. 实验设计：数据集、基准、对比方法

### 3.1 数据集
- **三颗卫星数据**：
  - **WorldView-II (WV2)**：包含工业区和自然景观。
  - **GaoFen2 (GF2)**：包含山脉和河流。
  - **WorldView-III (WV3)**：城市环境。
- **数据生成**：遵循Wald协议，在降分辨率下生成测试集（即无真实高分辨率GT，需降采样后模拟）。
- **全分辨率场景**：额外使用GF2全分辨率数据（FGF2）进行无参考评价。

### 3.2 对比方法
- **传统方法**：GFPCA, LRTCFPan, Brovey, IHS, SFIM。
- **深度学习方法**：SRPPNN, INNformer, FAME, SFINet++, WaveletNet, Pan-Mamba, CFLIHPs。

### 3.3 评价指标
- **全参考指标**：PSNR, SSIM, SAM, ERGAS。
- **无参考指标**：空间失真 \(D_S\)，光谱失真 \(D_\lambda\)，全局无参考质量 \(QNR\)。

### 3.4 实验构成
1. **降分辨率场景**（表1）：在三个数据集上定量对比。
2. **全分辨率场景**（表2）：在FGF2数据集上无参考评价。
3. **零样本超分辨率**（表4）：在WV2上，将MMMamba与Bicubic、SFINet++、Pan-Mamba对比。
4. **消融实验**（表3）：在WV2上验证核心范式、扫描策略等。
5. **计算效率比较**（表5）：FLOPs和参数量。

## 4. 资源与算力

- **GPU**：单个Nvidia V100 GPU。
- **框架**：PyTorch。
- **优化器**：Adam，梯度裁剪norm=4.0，初始学习率5×10⁻⁴，余弦衰减至5×10⁻⁸。
- **训练轮次**：WV2上200 epoch，GF2和WV3上500 epoch。
- **未明确说明**：具体训练时间（小时/天）、批大小、总参数量（0.2453M）、FLOPs（5.0616G）已给出。

## 5. 实验数量与充分性

- **数量**：共进行了5组主要实验（降分辨率×3数据集 + 全分辨率 + 零样本），加上消融实验（6组变体），总计超过10组对比。
- **充分性**：
  - **正面**：覆盖三个不同卫星、不同场景的数据集；评估指标全面（4个全参考+3个无参考）；对比方法含传统和最新深度方法（7种以上）；消融实验验证了关键组件（Mamba vs Transformer、上下文融合 vs 通道拼接、交错 vs 顺序拼接、多方向 vs 单向扫描、局部 vs 全局扫描）。
  - **不足**：仅在一个V100上训练，未报告多GPU或不同精度（fp16/bf16）影响；未进行跨数据集泛化测试（如WV2上训练的模型测试GF2）；零样本实验仅在WV2上进行，未在其他数据集验证。

## 6. 论文的主要结论与发现

- **性能优势**：MMMamba在三个数据集上全面超越所有SOTA方法，尤其在WV2上PSNR达42.3120（比CFLIHPs高0.40dB），GF2上47.9932（高0.61dB）。
- **全分辨率泛化**：在FGF2上无参考指标也优于大部分方法，仅DS略高于WaveletNet，但QNR最高（0.8312）。
- **零样本超分辨率**：PSNR 36.49，SSIM 0.9114，远超Bicubic和对比方法，证明上下文融合框架的泛化能力。
- **计算效率**：5.06G FLOPs、0.245M参数，比SRPPNN（21.1G）等更高效，但略高于INNformer（1.3G）和Pan-Mamba（3.0G）。
- **消融验证**：
  - Mamba优于Transformer（+0.91dB PSNR）。
  - 上下文融合优于通道拼接（+1.02dB）。
  - 交错拼接优于顺序拼接（+5.84dB）。
  - 多方向扫描优于单方向（+0.22dB）。
  - 局部窗口扫描优于全局扫描（+0.11dB）。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 首次将in-context conditioning范式引入全色锐化，避免了交叉注意力的二次复杂度和单向信息流。
  - 设计的多模态交错扫描（MI-Scan）实现了局部窗口内PAN和MS的交替扫描，增强了空间对齐的双向交互。
  - 零样本超分辨率能力展示了框架的通用性，无需额外训练。
- **实验设计**：
  - 同时覆盖降分辨率和全分辨率场景，验证了方法在理想和真实条件下的表现。
  - 消融实验全面（6个变体），清晰证明了每个组件的贡献。
  - 计算效率对比帮助定位模型复杂度。

## 8. 不足与局限

- **实验覆盖有限**：
  - 零样本超分辨率仅在WV2上验证，未在GF2/WV3上测试，泛化性证据不足。
  - 仅训练了单尺度（s=4）场景，未讨论其他分辨率比例。
  - 未与最新的扩散模型（如论文引用的Diffusion）进行直接对比（如CFLIHPs是流模型，但非扩散类）。
- **偏差风险**：
  - 全分辨率评价使用无参考指标（Dλ、DS、QNR），这些指标可能受数据依赖影响，且结论不完全一致（如Ours的DS不是最优）。
  - 传统方法（IHS、Brovey等）和深度方法在指标上差距悬殊，部分比较意义有限。
- **应用限制**：
  - 需同时输入PAN和MS对应图像，若卫星传感器不提供PAN则无法使用。
  - 零样本超分辨率仅适用于MS图像增强，无法直接迁移到其他任务（如去噪、修复）。
- **复现细节**：代码已开源，但未提供预训练模型或详细超参数设置（如批次大小、数据增强策略）。

（完）
