---
title: Self-Learning Hyperspectral and Multispectral Image Fusion via Adaptive Residual Guided Subspace Diffusion Model
title_zh: 基于自适应残差引导子空间扩散模型的自学习高光谱与多光谱图像融合
authors: "Zhu, Jian, Wang, He, Xu, Yang, Wu, Zebin, Wei, Zhihui"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhu_Self-Learning_Hyperspectral_and_Multispectral_Image_Fusion_via_Adaptive_Residual_Guided_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 10.0
evidence: 自学习高光谱与多光谱图像融合，扩散模型
tldr: 高光谱与多光谱图像融合通常依赖大量超光谱数据进行监督训练，实际中数据获取困难。本文提出ARGS-Diff，一种仅利用观测图像的自学习扩散模型，通过轻量光谱空间分支提取特征，并设计自适应残差引导子空间扩散实现融合。无需任何额外训练数据，在多个公开数据集上性能超越监督方法，为数据受限场景提供了有效方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1755, \"height\": 786}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1811, \"height\": 1244}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1813, \"height\": 266}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 335}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 803, \"height\": 313}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 844, \"height\": 882}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 820, \"height\": 342}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 822, \"height\": 340}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 821, \"height\": 342}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 655, \"height\": 342}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 866, \"height\": 419}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-self-learning-hyperspectral-and-multispectral-image-fusion-via-adaptive-residual-guided-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 784, \"height\": 204}]"
motivation: 解决高光谱图像融合中训练数据不足的问题。
method: 设计轻量光谱空间分支和自适应残差引导的子空间扩散模型。
result: 无需额外训练数据，融合性能超越监督方法。
conclusion: ARGS-Diff实现了数据高效的高光谱图像融合。
---

## Abstract
Hyperspectral and multispectral image (HSI-MSI) fusion involves combining a low-resolution hyperspectral image (LR-HSI) with a high-resolution multispectral image (HR-MSI) to generate a high-resolution hyperspectral image (HR-HSI). Most deep learning-based methods for HSI-MSI fusion rely on large amounts of hyperspectral data for supervised training, which is often scarce in practical applications. In this paper, we propose a self-learning Adaptive Residual Guided Subspace Diffusion Model (ARGS-Diff), which only utilizes the observed images without any extra training data. Specifically, as the LR-HSI contains spectral information and the HR-MSI contains spatial information, we design two lightweight spectral and spatial diffusion models to separately learn the spectral and spatial distributions from them. Then, we use these two models to reconstruct HR-HSI from two low-dimensional components, i.e, the spectral basis and the reduced coefficient, during the reverse diffusion process. Furthermore, we introduce an Adaptive Residual Guided Module (ARGM), which refines the two components through a residual guided function at each sampling step, thereby stabilizing the sampling process. Extensive experimental results demonstrate that ARGS-Diff outperforms existing state-of-the-art methods in terms of both performance and computational efficiency in the field of HSI-MSI fusion.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
高光谱图像（HSI）与多光谱图像（MSI）融合的目标是将低分辨率高光谱图像（LR-HSI）与高分辨率多光谱图像（HR-MSI）融合，生成高分辨率高光谱图像（HR-HSI）。现有深度学习方法大多依赖大量配对的HSI-MSI数据进行有监督训练，然而在实际遥感应用中，这类标注数据往往难以获取。为解决训练数据稀缺的问题，本文提出一种**自学习**框架，仅利用观测到的LR-HSI和HR-MSI本身，无需任何额外训练数据，即可完成高质量融合。

## 2. 方法论：核心思想、关键技术细节与算法流程

### 核心思想
- 基于HSI可分解为两个低维分量：**光谱基**（E）和**缩减系数**（A），且HR-HSI = A ×₃ E（模态-3乘积）。
- 设计两个轻量级扩散网络：**光谱网络**（全连接网络）和**空间网络**（类UNet结构），分别从LR-HSI和HR-MSI中自学习光谱分布和空间分布。
- 在逆向扩散过程中，这两个网络分别重建E和A，最后合并得到HR-HSI。
- 引入**自适应残差引导模块（ARGM）**，在每个采样步后同时修正E和A，以稳定双重更新的收敛过程。

### 关键技术细节
- **自学习训练**：光谱网络以LR-HSI中随机选取的d个像素光谱（d×C）为输入；空间网络以HR-MSI中随机抽取的d个波段拼成的patch（H_patch×W_patch×d）为输入，分别训练噪声预测。
- **逆向扩散条件采样**：利用后验采样理论，将LR-HSI和HR-MSI作为条件，通过梯度项修正噪声估计，同时更新A和E。公式中使用Adam优化器加速。
- **ARGM**：在每一步采样后，计算当前A_t-1和E_t-1与观测的残差损失，通过梯度下降更新两个分量，使它们更好地对齐。

### 算法流程（文字描述）
1. 初始化：从标准正态分布采样A_T和E_T。
2. 对于t = T, T-1, …, 1：
   - 根据当前A_t、E_t和网络预测，利用式(11)估计干净分量Â0、Ê0。
   - 计算条件引导损失（式10），并用Adam更新梯度，修正网络输出（式15）。
   - 依据式(12)采样得到A_t-1、E_t-1。
   - **ARGM步骤**：计算残差损失（式16），通过式(17)更新A_t-1和E_t-1。
3. 最终输出Z_0 = A_0 ×₃ E_0。

## 3. 实验设计

### 数据集与场景
- **模拟实验**：三个公开数据集——Pavia University（256×256×103）、Chikusei（256×256×128）、KSC（256×256×176）。LR-HSI由双三次下采样（scale=4）生成；HR-MSI由光谱响应函数模拟得到。
- **真实数据实验**：DFC2018 Houston数据集，直接使用真实高光谱图像，评估实际应用性能。

### 评价指标
- PSNR（↑）、SAM（↓）、ERGAS（↓）、SSIM（↑）。

### 对比方法
- 传统方法：CNMF、HySure。
- 深度学习方法：CuCaNet、MIAE。
- 扩散模型方法：PLRDiff、S²CycleDiff。

## 4. 资源与算力
文中明确说明：“All experiments were carried out on an **NVIDIA GeForce RTX 4090 GPU**。”但**未提及具体使用的GPU数量、训练时长等细节**。推理时间在表1-3中给出（ARGS-Diff约12-13秒），但网络的自学习训练所需时间未单独报告。

## 5. 实验数量与充分性
论文进行了**较为充分的实验**，包括：
- **三个模拟数据集**的完整定量对比（表1-3），每个数据集均报告四项指标。
- **一个真实数据集**的视觉对比（图3）。
- **消融实验**：验证ARGM的有效性（表4），在三个数据集上均提升0.47~0.61 dB PSNR。
- **超参数敏感性分析**：对子空间维度d和采样步数T（图5），以及步长ρ1、ρ2（表5）进行了系统搜索。
- **模型规模和计算消耗对比**（表6）：与PLRDiff、S²CycleDiff对比参数量、内存和推理时间。
- **中间结果可视化**（图4）：展示ARGM对缩减系数A的重建改善。

实验设计**客观公平**：对比方法包含传统、深度学习及扩散模型最新方法，指标全面，数据集多样，且所有方法在相同硬件上运行（推测）。但**未进行交叉验证或统计显著性检验**。

## 6. 主要结论与发现
- ARGS-Diff在三个模拟数据集上的**PSNR均超过所有对比方法**，分别达到42.33 dB、41.90 dB、43.63 dB，比第二名MIAE高出1.27~1.37 dB。
- **推理速度显著优于其他扩散模型**：仅需12秒（PLRDiff 79秒，S²CycleDiff 297秒），且参数量仅21.85M（约为PLRDiff的1/20，S²CycleDiff的1/30），GPU内存仅2.11 GB。
- 在真实数据Houston上，ARGS-Diff生成的图像色彩更自然，伪影更少。
- ARGM模块有效提升融合质量与采样稳定性，且额外时间开销极小（1-2秒）。

## 7. 优点
- **自学习范式**：完全依赖观测图像训练，无需外部HR-HSI标签，实用性强。
- **轻量级设计**：网络参数量少、计算资源消耗低，适合部署于资源受限的边缘设备。
- **双重引导稳定机制**：结合后验梯度与ARGM残差修正，解决了双分量同步更新的不稳定性。
- **高效推理**：仅需500步采样即可达到先进性能，且显存占用低，适合快速应用。
- **广泛的实验验证**：在模拟和真实数据上均展示出优越性。

## 8. 不足与局限
- **超参数敏感**：子空间维度d、采样步数T、步长ρ1/ρ2需要手动调优，不同数据集可能需重新调整。
- **噪声假设局限**：退化模型假设高斯噪声，实际场景中噪声分布可能更复杂（如泊松噪声），论文未讨论鲁棒性。
- **训练时间未报告**：虽然推理快，但自学习训练本身需要时间，文中未提供训练时长，不利于完整评估。
- **缺少统计显著性检验**：实验仅报告单次结果，未使用多次重复实验或置信区间。
- **真实数据集实验仅视觉对比**：Houston数据没有地面真值，无法定量评价，削弱了说服力。
- **未探索更复杂退化**：如空间模糊、光谱偏移等实际退化场景未测试。

（完）
