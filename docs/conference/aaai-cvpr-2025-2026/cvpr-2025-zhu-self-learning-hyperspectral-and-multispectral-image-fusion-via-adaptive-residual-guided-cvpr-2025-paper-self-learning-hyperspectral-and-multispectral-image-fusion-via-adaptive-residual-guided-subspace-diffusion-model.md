---
title: Self-Learning Hyperspectral and Multispectral Image Fusion via Adaptive Residual Guided Subspace Diffusion Model
title_zh: 自学习高光谱与多光谱图像融合：自适应残差引导子空间扩散模型
authors: "Zhu, Jian, Wang, He, Xu, Yang, Wu, Zebin, Wei, Zhihui"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhu_Self-Learning_Hyperspectral_and_Multispectral_Image_Fusion_via_Adaptive_Residual_Guided_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 自学习高光谱与多光谱图像融合
tldr: 针对现有深度学习方法需大量高光谱训练数据且实际数据稀缺的问题，本文提出了自适应残差引导子空间扩散模型（ARGS-Diff），仅利用观测图像进行自学习融合。模型通过轻量级光谱和空间子网络提取特征，并引入自适应残差引导机制。实验表明，该方法在不依赖外部训练数据的情况下取得了与监督方法相当的融合效果，显著提升了实际应用中的可行性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1755, \"height\": 786, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1811, \"height\": 1244, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1813, \"height\": 266, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 335, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 803, \"height\": 313, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 844, \"height\": 882, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 820, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 822, \"height\": 340, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 821, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 655, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 866, \"height\": 419, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 784, \"height\": 204, \"label\": \"Table\"}]"
motivation: 现有深度学习方法依赖大量高光谱训练数据，实际数据稀缺。
method: 提出自适应残差引导子空间扩散模型，仅利用观测图像进行自学习融合。
result: 在不需额外训练数据的情况下取得与监督方法相当的融合效果。
conclusion: 自学习方法有效降低了对训练数据的依赖，提升实用性。
---

## Abstract
Hyperspectral and multispectral image (HSI-MSI) fusion involves combining a low-resolution hyperspectral image (LR-HSI) with a high-resolution multispectral image (HR-MSI) to generate a high-resolution hyperspectral image (HR-HSI). Most deep learning-based methods for HSI-MSI fusion rely on large amounts of hyperspectral data for supervised training, which is often scarce in practical applications. In this paper, we propose a self-learning Adaptive Residual Guided Subspace Diffusion Model (ARGS-Diff), which only utilizes the observed images without any extra training data. Specifically, as the LR-HSI contains spectral information and the HR-MSI contains spatial information, we design two lightweight spectral and spatial diffusion models to separately learn the spectral and spatial distributions from them. Then, we use these two models to reconstruct HR-HSI from two low-dimensional components, i.e, the spectral basis and the reduced coefficient, during the reverse diffusion process. Furthermore, we introduce an Adaptive Residual Guided Module (ARGM), which refines the two components through a residual guided function at each sampling step, thereby stabilizing the sampling process. Extensive experimental results demonstrate that ARGS-Diff outperforms existing state-of-the-art methods in terms of both performance and computational efficiency in the field of HSI-MSI fusion.

---

## 论文详细总结（自动生成）

# 论文总结：Self-Learning Hyperspectral and Multispectral Image Fusion via Adaptive Residual Guided Subspace Diffusion Model

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：高光谱图像（HSI）与多光谱图像（MSI）融合的目标是生成高空间分辨率的高光谱图像（HR-HSI）。现有深度学习方法通常依赖大量成对的高光谱训练数据，但实际中这类数据稀缺，难以获取。
- **动机**：传统模型方法（如CNMF、HySure）依赖手工特征，处理高维非线性数据时性能受限；而深度学习网络（如卷积或Transformer）需要大量监督数据，且扩散模型（如PLRDiff、S2CycleDiff）虽生成质量好，但推理速度慢、计算开销大，难以部署在边缘设备（如无人机）。
- **核心思想**：提出一种**自学习**方法，仅利用观测到的LR-HSI和HR-MSI，无需外部训练数据，通过两个轻量级扩散模型分别学习光谱和空间分布，再在子空间中重建HR-HSI。

## 2. 方法论

### 核心思想
- 将HR-HSI分解为两个低维分量：**光谱基（spectral basis）** E 和**约化系数（reduced coefficient）** A，即 Z = A ×₃ E。其中d远小于光谱通道数C。
- 分别设计**光谱网络**（全连接网络，5层）和**空间网络**（U-Net风格，9个卷积层），利用观测数据自学习训练：
  - 光谱网络从LR-HSI中随机选取d个像素谱作为输入训练。
  - 空间网络从HR-MSI中随机提取patch，重复d次形成多通道输入训练。
- 在逆扩散过程中，使用两个网络分别估计E和A，并通过**后验采样**（posterior sampling）结合观测条件（LR-HSI X 和 HR-MSI Y）进行迭代更新。

### 关键技术细节
1. **前向扩散**：对A和E分别加噪，遵循标准扩散过程。
2. **逆扩散方程**（以A为例）：
   - 预测噪声：sθ(At, t) 为空间网络输出。
   - 条件引导修正：使用梯度下降调整预测噪声：
     ```
     ŝθ(At,t) = sθ(At,t) - ρ₁·∇_At L(Â₀, Ê₀, X, Y)
     ```
     其中引导函数L包含：
     - 空间一致性项：||H(Â₀ ×₃ Ê₀) - X||²
     - 光谱一致性项：λ₁ ||Â₀ ×₃ Ê₀ ×₃ R - Y||²
   - 实际中采用Adam优化器加速收敛。
3. **自适应残差引导模块（ARGM）**：
   - 在每一步采样获得A_{t-1}和E_{t-1}后，计算残差损失L(A_{t-1}, E_{t-1}, X, Y)（类似引导函数），再通过梯度步更新两个分量：
     ```
     A_{t-1} := A_{t-1} - ρ₁·r·∇_{A_{t-1}} L
     E_{t-1} := E_{t-1} - ρ₂·(1/r)·∇_{E_{t-1}} L
     ```
     其中r为控制相对步长的比例。
   - 作用：对齐光谱与空间分量，补偿预测误差，稳定采样过程。
4. **最终输出**：T步后得到A₀和E₀，乘积即为HR-HSI Z₀。

### 算法流程
- 初始化：从高斯噪声采样A_T和E_T；Adam动量初始为0。
- 循环t = T到1：
  1. 由式(11)估计Â₀和Ê₀。
  2. 计算引导损失L(Â₀, Ê₀, X, Y)。
  3. 更新Adam动量。
  4. 计算修正后的噪声估计ŝθ和ĉζ。
  5. 采样A_{t-1}和E_{t-1}。
  6. ARGM更新：计算残差损失并梯度更新。
- 返回Z₀ = A₀ ×₃ E₀。

## 3. 实验设计

### 数据集
- **模拟数据**：三个公开数据集
  - Pavia University：256×256×103，放大因子4，添加SNR=35噪声。
  - Chikusei：256×256×128。
  - KSC：256×256×176。
- **真实数据**：DFC2018 Houston数据集（实测高光谱数据）。

### Benchmark与对比方法
- 传统模型：CNMF、HySure。
- 深度学习：CuCaNet、MIAE。
- 扩散模型：PLRDiff、S2CycleDiff。
- 评价指标：PSNR（↑）、SAM（↓）、ERGAS（↓）、SSIM（↑）。

### 对比结果
- 三个模拟数据集上，ARGS-Diff在所有指标均为**最佳**，PSNR分别比第二名（MIAE）高1.31 dB、1.37 dB、1.27 dB。
- 真实数据上，ARGS-Diff生成图像颜色自然、伪影少，优于对比方法。
- 推理时间仅12-13秒，远快于PLRDiff（79秒）和S2CycleDiff（297秒）。

## 4. 资源与算力

- **硬件**：NVIDIA GeForce RTX 4090 GPU（论文明确提及）。
- **GPU数量**：未说明，推测为单块GPU。
- **训练时长**：论文未报告训练所需的具体时间。仅提到推理时间约12秒。
- 整体算力需求较低（参数量仅21.85M，显存2.11GB）。

## 5. 实验数量与充分性

- **三组模拟数据集**（Pavia、Chikusei、KSC）的完整对比实验。
- **一组真实数据实验**（Houston）。
- **消融实验**：分析ARGM模块效果（w/ vs w/o），三个数据集上PSNR提升0.47~0.61 dB，SAM改善0.11~0.16。
- **超参数敏感性分析**：
  - 子空间维度d（1~20）：d=8最优。
  - 采样步数T（100~1000）：T=500饱和。
  - 步长ρ₁和ρ₂（0.01~0.07）：搜索网格，ρ₁=ρ₂=0.05最优。
- **模型规模与计算消耗对比**：与PLRDiff和S2CycleDiff比较参数量、显存、推理时间。
- **总体评价**：实验设计全面，覆盖不同场景、噪声水平、消融和参数分析，对比方法多样，结果具有说服力。但未在更多真实传感器数据或更高噪声场景下测试，略显不足。

## 6. 主要结论与发现

- ARGS-Diff能在**无需外部训练数据**的条件下，实现优于现有监督/无监督融合方法的性能。
- 轻量级网络设计使得推理速度极快（12秒），且参数量仅为其他扩散模型的1/20~1/30，显存需求低（2.11GB），适合边缘设备部署。
- ARGM模块有效稳定了双分量联合更新的采样过程，提升重建质量。
- 子空间分解结合自学习扩散是HSI-MSI融合的有效范式。

## 7. 优点

1. **自学习框架**：无需配对高光谱训练数据，极大降低应用门槛。
2. **高效轻量**：网络参数量小、推理快、内存占用低，适合实时或嵌入式场景。
3. **ARGM创新**：通过残差引导同时优化光谱和空间分量，解决了双分量联合更新不稳定问题。
4. **全面实验**：多个数据集、多种对比方法、消融与参数分析，验证充分。
5. **真实数据验证**：在DFC2018 Houston上展示实际应用效果。

## 8. 不足与局限

1. **噪声假设**：实验中仅考虑SNR=35的高斯噪声，未测试更低SNR或非高斯噪声下的鲁棒性。
2. **子空间维度d固定**：d设为8，需针对不同数据调整，缺乏自适应选择机制。
3. **未讨论不同退化模型**：仅考虑双三次下采样和固定光谱响应，实际退化可能更复杂（如模糊、混叠）。
4. **未与其他自学习方法对比**：如基于深度图像先验（DIP）或零样本调优方法，对比公平性可提升。
5. **真实数据仅一个**：Houston数据集，缺乏多种传感器或场景的评估，推广性有待验证。
6. **训练时长未报告**：虽推理快，但训练时间可能较长，文中未提及，影响对整体效率的评估。

（完）
