---
title: "PIF-Net: Ill-Posed Prior Guided Multispectral and Hyperspectral Image Fusion via Invertible Mamba and Fusion-Aware LoRA"
title_zh: PIF-Net：通过可逆Mamba和融合感知LoRA进行病态先验引导的多光谱与高光谱图像融合
authors: "Baisong Li, Xingwang Wang, Haixiao Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37519/41481"
tags: ["query:image-fusion"]
score: 9.0
evidence: 通过可逆Mamba进行多光谱和高光谱图像融合
tldr: 针对多光谱和高光谱图像融合的严重不适定性问题，提出PIF-Net。引入病态先验指导融合过程，并设计基于可逆Mamba的全局光谱建模模块，同时采用融合感知LoRA降低计算成本。实验表明PIF-Net在多个数据集上取得最先进性能，有效处理了数据未对齐等挑战。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 多光谱与高光谱融合受光谱-空间折中及数据未对齐影响，本质上病态，现有方法未能有效处理。
method: 提出PIF-Net，引入病态先验，设计可逆Mamba进行全局光谱建模，并使用融合感知LoRA提高效率。
result: 在多个多光谱-高光谱融合数据集上达到最先进性能，有效处理未对齐问题。
conclusion: 显式引入病态先验和可逆Mamba能显著提升多光谱-高光谱融合质量。
---

## Abstract
The goal of multispectral and hyperspectral image fusion (MHIF) is to generate high-quality images that simultaneously possess rich spectral information and fine spatial details. However, due to the inherent trade-off between spectral and spatial information and the limited availability of observations, this task is fundamentally ill-posed. Previous studies have not effectively addressed the ill-posed nature caused by data misalignment. To tackle this challenge, we propose a fusion framework named PIF-Net, which explicitly incorporates ill-posed priors to effectively fuse multispectral images and hyperspectral images. To balance global spectral modeling with computational efficiency, we design a method based on an invertible Mamba architecture that maintains information consistency during feature transformation and reconstruction, ensuring stable gradient flow and process reversibility.
Furthermore, we introduce a novel fusion module called the Fusion-Aware Low-Rank Adaptation module, which dynamically calibrates spectral and spatial features while keeping the model lightweight. Extensive experiments on multiple benchmark datasets demonstrate that PIF-Net achieves significantly better image restoration performance than current state-of-the-art methods while maintaining model efficiency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **任务定义**：多光谱与高光谱图像融合（MHIF）旨在结合多光谱图像（MSI）的丰富空间细节与高光谱图像（HSI）的连续光谱信息，生成同时具有高空间分辨率和高光谱分辨率的高质量图像。
- **核心挑战**：该任务本质上是不适定的（ill-posed），主要原因包括：① 光谱与空间信息之间存在固有折中；② 两类图像在光谱特性、空间分辨率及数据分布上存在显著差异，导致严重的信息未对齐和模态差距；③ 现有融合方法大多采用不可逆的信息变换，导致信息丢失。
- **现有方法不足**：单分支或双分支架构未能有效处理未对齐问题；传统模型驱动方法依赖先验知识易失效；深度学习方法（CNN、Transformer）虽好但存在长程依赖捕获不足或计算复杂度高；可逆神经网络（INN）在融合领域应用较少，未充分利用空间引导。
- **本文目标**：提出PIF-Net，显式引入病态先验指导融合，利用可逆Mamba实现双向信息流保持信息一致性，并设计轻量级融合感知LoRA模块，实现高保真、高效率融合。

## 2. 方法论：核心思想、关键技术细节

- **整体架构**：双分支网络。① **光谱分支**：首先用双三次插值上采样LRHSI，经Head模块提取特征，通过Haar小波变换分解为低频（X_L）和高频（X_H）分量；低频分量经过L个可逆Mamba模块进行频域建模，高频分量通过卷积降维；最后经逆小波变换和Tail模块生成光谱参考图像 \(\bar{Z}\)。② **空间分支**：利用低频全局引导和高频语义感知模块（HFSPM）提取空间特征，通过FAM‑LoRA模块进行精细融合，最终生成融合图像 \(\hat{Z}\)。

- **关键技术细节**：
    - **病态残差先验提取模块（IPRPEM）**：基于残差通道先验，自适应提取全局病态先验信息，增强后续特征融合的判别力。公式：\(C_R = \max_c \bar{X}_s - \min_c \bar{X}_s\)，再经ReLU和卷积得到先验特征\(X_R\)。
    - **可逆Mamba块**：采用仿射耦合机制，每个变换单元基于Segmented Spectral Mamba Module（SSMM），支持全局光谱建模和多尺度特征提取。可逆映射：\(X^{i+1}_H = X^i_H + I_1(X^i_L)\)，\(X^{i+1}_L = X^i_L \odot \exp(I_2(X^{i+1}_H)) + I_3(X^{i+1}_H)\)。保证前向和反向信息流可逆。
    - **融合感知多头部LoRA（FAM‑LoRA）**：以主空间特征\(Y_i\)和辅助引导特征\(X_{Ri}\)为输入，通过1×1卷积、LKA、SE注意力增强空间感知和通道选择性；冻结LKA和SE梯度，在SE输入处注入\(X_{Ri}\)；通道维度分为四个头部独立进行低秩适应，实现高效语义融合。
    - **损失函数**：三部分组成：① L1融合损失\(\|\hat{Z} - Z\|_1\)；② 可逆性正则化项\(-\lambda_{inv} \log \det(\partial \bar{Z}/\partial X)\)，保证变换可逆；③ 余弦相似度损失\(\lambda_{cos}(1 - \cos(\bar{Z}, \hat{Z}))\)，增强特征语义一致性。权重\(\lambda_{inv}=0.01\)，\(\lambda_{cos}=0.1\)。

## 3. 实验设计

- **数据集**：使用三个公开高光谱数据集：
    - **Chikusei**：2517×2335像素，128波段。训练：左上1000×2000区域；测试：剩余区域裁剪为680×680 patches。
    - **PaviaU**：610×340像素，103波段。测试：上部340×340区域；其余用于训练。
    - **Houston**：349×1905像素，144波段。测试：左侧349×349区域。
- **数据预处理**：HRMSI通过模拟生成；LRHSI通过高斯模糊（核3×3，标准差0.5）及下采样得到。训练样本裁剪为64×64 ground-truth patches，对应LRHSI为16×16，HRMSI为64×64。
- **评价指标**：PSNR（越高越好）、SSIM（越高越好）、SAM（越低越好）、ERGAS（越低越好）。
- **对比方法**：传统方法（Brovey、GSA）；深度学习方法（HSRnet、Fusformer、PSRT、U2Net、3DT‑Net、SMGU‑Net）。所有方法按官方默认实现运行。
- **实验尺度**：×2、×4、×8三种上采样因子。

## 4. 资源与算力

- **GPU型号**：NVIDIA A30 GPU。
- **训练配置**：
    - 优化器：AdamW，初始学习率1e-4，每200 epoch减半。
    - 批量大小：8。
    - 训练轮数：500 epochs。
    - 模型隐含维度D=64，特征提取块数L=4。
- **模型规模与推理速度**：参数量1.73M，推理延迟9.3 ms（在PaviaU ×4任务上测量，A30 GPU，batch size相同）。
- *注：文中未明确说明训练总时长或使用的GPU数量（推测为单卡）。*

## 5. 实验数量与充分性

- **实验组数量**：
    - **主对比实验**：在三个数据集（Chikusei、Houston、PaviaU）上分别进行×2、×4、×8三种尺度实验，共9组，与8种方法对比（共72个对比结果）。
    - **消融实验**：4项消融研究：
        ① 病态先验加权参数β的影响（β=0,0.4,0.8,1）。
        ② 可逆Mamba块的有无对比。
        ③ FAM‑LoRA模块的有无对比。
        ④ 损失函数分量（L1、L_inv、L_cos）的消融。
    - **效率对比**：在PaviaU ×4任务上比较参数量和推理延迟。
- **充分性评价**：实验设计较为系统，覆盖多种尺度、多个数据集、多个对比方法及关键模块消融，结果客观、公平。但缺少在真实采集数据上的验证（所有实验使用模拟HRMSI和LRHSI），且未做统计显著性检验（如多次运行取均值标准差）。

## 6. 主要结论与发现

- PIF-Net在所有数据集和所有尺度下均取得最佳或次佳性能，尤其在×4和×8大尺度下优势显著。例如在PaviaU ×4上PSNR达42.8673 dB，比第二好方法（3DT‑Net）高0.9045 dB；在Chikusei ×8上PSNR达50.0124 dB，创下新基准。
- 消融实验证实：病态先验（β=1最佳）、可逆Mamba块、FAM‑LoRA模块以及组合损失函数均对融合性能有显著贡献，缺一不可。
- 模型效率出色：仅1.73M参数、9.3 ms推理延迟，即可达到SOTA精度，适合实时应用。

## 7. 优点

- **创新性**：首次将可逆Mamba架构引入MHIF，结合Haar小波实现频域双向信息流，有效解决不可逆信息损失问题。
- **病态先验指导**：明确针对任务的不适定性设计IPRPEM，改善了未对齐情况下的特征提取鲁棒性。
- **轻量高效**：融合感知LoRA模块极大降低了参数量，而LKA和SE的梯度冻结策略进一步减少计算开销，实现精度与效率的平衡。
- **损失函数设计**：巧妙引入雅可比对数行列式作为可逆性正则化，结合L1和余弦相似度，同时保证保真度、可逆性和语义一致性。

## 8. 不足与局限

- **实验覆盖有限**：仅使用三个遥感数据集，且均为模拟退化数据，缺乏在真实多源传感器数据（如不同空间/光谱分辨率的真实MSI和HSI）上的验证，泛化能力待检验。
- **未进行统计可靠性分析**：实验未报告多次重复实验的均值和标准差，无法评估模型性能的稳定性。
- **应用限制**：模型针对特定空间/光谱比例设计，在极低信噪比或高度未对齐场景下的表现未知；此外，LoRA模块的冻结策略可能限制对极端域漂移的适应能力。
- **计算资源细节不完整**：未明确说明训练总时长及多GPU训练是否支持，实际部署时的能效比无法精确判断。

（完）
