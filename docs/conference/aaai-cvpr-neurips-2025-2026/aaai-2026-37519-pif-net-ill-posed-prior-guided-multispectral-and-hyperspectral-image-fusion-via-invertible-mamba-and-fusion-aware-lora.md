---
title: "PIF-Net: Ill-Posed Prior Guided Multispectral and Hyperspectral Image Fusion via Invertible Mamba and Fusion-Aware LoRA"
title_zh: PIF-Net：基于不适定先验引导的可逆Mamba和融合感知LoRA的多光谱与高光谱图像融合
authors: "Baisong Li, Xingwang Wang, Haixiao Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37519/41481"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于可逆Mamba和融合感知LoRA的多光谱/高光谱图像融合
tldr: 该文针对多光谱与高光谱融合中因光谱-空间信息权衡和观测有限导致的不适定问题，提出PIF-Net。显式引入不适定先验，设计可逆Mamba结构实现高效全局光谱建模，并采用融合感知LoRA降低计算量。实验表明该方法在恢复空间细节和光谱保真度上优于现有方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37519/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 851, \"height\": 575, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37519/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1769, \"height\": 964, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37519/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 802, \"height\": 616, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37519/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 862, \"height\": 527, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37519/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1838, \"height\": 1092, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37519/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 857, \"height\": 644, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37519/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1829, \"height\": 1000, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37519/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 869, \"height\": 138, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37519/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 871, \"height\": 132, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37519/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 131, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37519/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 881, \"height\": 263, \"label\": \"Table\"}]"
motivation: 多光谱与高光谱融合任务固有的不适定性未被有效处理。
method: 引入不适定先验，设计可逆Mamba进行全局光谱建模，结合融合感知LoRA提高效率。
result: 在空间和光谱质量上均取得最佳性能。
conclusion: 显式不适定先验引导有效提升了融合质量与鲁棒性。
---

## Abstract
The goal of multispectral and hyperspectral image fusion (MHIF) is to generate high-quality images that simultaneously possess rich spectral information and fine spatial details. However, due to the inherent trade-off between spectral and spatial information and the limited availability of observations, this task is fundamentally ill-posed. Previous studies have not effectively addressed the ill-posed nature caused by data misalignment. To tackle this challenge, we propose a fusion framework named PIF-Net, which explicitly incorporates ill-posed priors to effectively fuse multispectral images and hyperspectral images. To balance global spectral modeling with computational efficiency, we design a method based on an invertible Mamba architecture that maintains information consistency during feature transformation and reconstruction, ensuring stable gradient flow and process reversibility.
Furthermore, we introduce a novel fusion module called the Fusion-Aware Low-Rank Adaptation module, which dynamically calibrates spectral and spatial features while keeping the model lightweight. Extensive experiments on multiple benchmark datasets demonstrate that PIF-Net achieves significantly better image restoration performance than current state-of-the-art methods while maintaining model efficiency.

---

## 论文详细总结（自动生成）

# 论文总结：PIF-Net: Ill-Posed Prior Guided Multispectral and Hyperspectral Image Fusion via Invertible Mamba and Fusion-Aware LoRA

## 1. 核心问题与整体含义（研究动机和背景）
- **任务**：多光谱与高光谱图像融合（MHIF），旨在生成同时具备高空间分辨率和高光谱分辨率的融合图像。
- **核心挑战**：该任务本质上是**不适定问题（ill-posed）**，因为光谱与空间信息存在固有矛盾，且观测数据有限。此外，两种模态（MSI与HSI）之间在光谱特性、空间分辨率和数据分布上存在显著差异，导致信息错位和模态差距，进一步加剧了不适定性。
- **现有方法不足**：单分支或双分支融合方法未能有效处理信息错位和不可逆信息损失，导致融合性能受限。现有缓解不适定性的方法（如结构感知、生成对抗、扩散模型）多依赖低级固定模式，难以捕获融合引入的语义偏移。可逆神经网络（INN）虽能保证信息无损，但在跨模态融合中尚未充分利用空间引导。

## 2. 论文提出的方法论
### 核心思想
- 提出**PIF-Net**，显式引入**不适定先验**（ill-posed prior）指导融合过程。设计**双分支架构**：频率分支利用可逆Mamba模块实现双向信息流与结构保持；空间分支则利用低频引导和轻量级LoRA模块提取丰富的空间纹理特征。

### 关键技术细节
1. **总体结构**：
   - 输入：低分辨率高光谱图像（LRHSI）\(X \in \mathbb{R}^{h \times w \times C}\) 和高分辨率多光谱图像（HRMSI）\(Y \in \mathbb{R}^{H \times W \times c}\)，输出高分辨率高光谱图像 \(\hat{Z}\)。
   - 频率分支：使用Haar小波变换（HWT）分解LRHSI为低频分量\(X_L\)和高频分量\(X_H\)，并通过L个可逆Mamba模块在频域建模，再经逆小波变换生成光谱参考图像\(\bar{Z}\)。
   - 空间分支：通过**高频语义感知模块（HFSPM）**融合高频上采样特征与不适定残差先验，得到全局空间语义参考；利用**FAM-LoRA模块**（融合感知多头低秩适配）进行空间修复，输出最终融合图像。

2. **不适定残差先验提取模块（IPRPEM）**：
   - 受Residue Channel Prior启发，自适应捕获光谱域不适定特征，增强后续特征融合的判别能力。计算公式：
     \[
     C_R = \max_{c \in D} \bar{X}_s - \min_{c \in D} \bar{X}_s,\quad X_R = \text{ReLU}(\text{Conv}(X_s - Y_s) \cdot C_R)
     \]
   - 通过调制因子\(C_R\)放大残差线索，引导网络关注全局结构变化。

3. **可逆Mamba块**：
   - 采用**分段频谱Mamba模块（SSMM）**构建仿射耦合层，实现低频与高频特征的双向可逆映射：
     \[
     X^{i+1}_H = X^i_H + I_1(X^i_L),\quad X^{i+1}_L = X^i_L \odot \exp(I_2(X^{i+1}_H)) + I_3(X^{i+1}_H)
     \]
   - 可逆设计保证特征变换过程中信息一致、梯度稳定。

4. **融合感知多头LoRA（FAM-LoRA）**：
   - 输入主空间特征\(Y_i\)和辅助引导特征\(X_{Ri}\)，经过通道变换、LKA（大核注意力）和SE（挤压-激励）增强空间和通道选择性；将梯度冻结后注入SE输入；通道分为4个头独立应用低秩适配，实现轻量高效语义融合。

5. **损失函数**：
   - 由三部分组成：\(\ell_1\)融合损失\(L_1\)、可逆性正则项\(L_{\text{inv}} = -\lambda_{\text{inv}} \log \det(\partial \bar{Z}/\partial X)\)、余弦相似度损失\(L_{\text{cos}}\)。权重设为\(\lambda_{\text{inv}}=0.01\)，\(\lambda_{\text{cos}}=0.1\)。

## 3. 实验设计
- **数据集**：Chikusei (2517×2335, 128波段)、PaviaU (610×340, 103波段)、Houston (349×1905, 144波段)。HRMSI通过模拟生成，LRHSI通过高斯模糊（3×3核，标准差0.5）和下采样获得。
- **基准与对比方法**：Brovey、GSA、HSRnet、Fusformer、PSRT、U2Net、3DT-Net、SMGU-Net（共8种SOTA方法）。
- **评价指标**：PSNR (↑)、SSIM (↑)、SAM (↓)、ERGAS (↓)。
- **尺度**：×2、×4、×8超分辨率任务。

## 4. 资源与算力
- **GPU型号**：NVIDIA A30 GPU（单卡）。
- **训练超参数**：隐藏维度D=64，块数L=4，AdamW优化器，初始学习率1e-4，每200 epoch减半，batch size=8，训练500 epochs。
- **未明确说明**：GPU数量（可能为单卡）、总训练时间、显存占用等细节未提供。

## 5. 实验数量与充分性
- **定量对比**：在3个数据集、3个尺度下共9组实验，报告所有指标，结果全面。
- **消融实验**：共4组：
  - 不适定残差先验权重β（0/0.4/0.8/1）对PSNR的影响。
  - 有无可逆Mamba块（PSNR 37.18→39.82 dB）。
  - 有无FAM-LoRA模块（PSNR 36.92→39.82 dB）。
  - 损失项消融：L1、L1+Linv、L1+Lcos、全部组合。
- **效率对比**：在PaviaU ×4上比较参数量和推理延迟（对数坐标），PIF-Net仅1.73M参数、9.3 ms延迟。
- **充分性评价**：实验设计较完整，覆盖主要组件和超参数，消融实验清晰验证各模块贡献。对比方法多样且权威，结果一致性高。但未进行跨域泛化实验（如真实传感器数据）或更大规模异质数据集测试。

## 6. 论文的主要结论与发现
- PIF-Net在Chikusei、Houston、PaviaU三个数据集上，所有尺度（×2/4/8）的PSNR、SSIM、SAM、ERGAS均显著优于现有SOTA方法，尤其在大尺度（×4/×8）下优势更明显。
- 可逆Mamba结构有效平衡了全局光谱建模与计算效率，保证了信息无损传递和梯度稳定。
- FAM-LoRA模块以极低参数增量实现高效的语义融合，提升了空间细节和光谱保真度。
- 不适定先验引导、可逆架构和融合感知LoRA三者协同，解决了MHIF中信息错位和不可逆损失问题。

## 7. 优点
- **创新性**：首次将可逆Mamba与融合感知LoRA结合用于MHIF，显式处理不适定性。
- **架构设计**：双分支频域+空域融合，频率分支利用小波分解和可逆模块实现无损全局建模；空间分支利用低频引导和LoRA实现轻量高效细粒度恢复。
- **高效性**：仅1.73M参数，推理延迟9.3 ms，在SOTA方法中达到最优精度-效率平衡，适合实时应用。
- **消融实验充分**：逐模块、逐损失项验证，结论可靠。

## 8. 不足与局限
- **实验覆盖范围有限**：仅使用三个遥感高光谱数据集，且HRMSI为模拟合成，缺乏真实传感器采集的异源数据验证（如实际航空/卫星数据）。
- **计算资源细节不足**：未报告GPU数量、训练总耗时、显存占用等，难以复现资源需求。
- **潜在偏差风险**：仅实验单一尺度因子（s=2/4/8），未探索非整数缩放或任意尺度融合；且所有数据预定义训练/测试划分，未进行交叉验证。
- **应用限制**：方法依赖LRHSI和HRMSI配准，实际应用中可能存在地理偏移或光谱响应差异，鲁棒性未评估。此外，模型在极端噪声或缺失波段情况下的性能未知。

（完）
