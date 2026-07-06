---
title: "OTPNet: ODE-inspired Tuning-free Proximal Network for Remote Sensing Image Fusion"
title_zh: OTPNet：受ODE启发的免调参近端网络用于遥感图像融合
authors: "Wei Yu, Zonglin Li, Qinglin Liu, Xin Sun"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33048/35203"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于ODE的遥感图像融合网络
tldr: 该论文针对深度学习融合方法依赖手动网络设计和调参、缺乏可解释性的问题，提出OTPNet，一种受常微分方程启发的免调参近端网络，将遥感图像融合分解为两个优化问题，通过深度先验正则化，实现了可解释且自适应的融合。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33048/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1802, \"height\": 716, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33048/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1822, \"height\": 784, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33048/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1644, \"height\": 1073, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-33048/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1789, \"height\": 485, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33048/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1829, \"height\": 295, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33048/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1826, \"height\": 469, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33048/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33048/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 869, \"height\": 238, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-33048/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 870, \"height\": 214, \"label\": \"Table\"}]"
motivation: 现有深度方法手动设计网络和调参，可解释性和适应性不足。
method: 提出ODE启发的近端分裂算法，将融合分解为两个优化子问题。
result: 在多个遥感数据集上融合质量优于现有方法。
conclusion: OTPNet为遥感图像融合提供了可解释且免调参的新途径。
---

## Abstract
Remote sensing image fusion aims to reconstruct a high spatial and spectral resolution image by integrating the spatial and spectral information from multiple remote sensing sensor data. Despite the remarkable progress of deep learning-based fusion methods, most existing methods rely on manual network architecture design and hyperparameter tuning, lacking sufficient interpretability and adaptability. To address this limitation, we propose a novel neural Ordinary Differential Equation (ODE)-inspired tuning-free proximal splitting algorithm, which splits remote sensing image fusion as two optimization problems regularized by deep priors to model the fusion of spatial and spectral. Firstly, based on the physical properties of spatial and spectral information, the two problems are optimized by two proximal splitting operators to iteratively integrate spatial-spectral complementary information, eliminating or suppressing redundant information to reduce fusion errors. Secondly, considering the efficiency of neural ODE in reducing optimization error, we utilize a high-order numerical scheme to customize the proximal operator theoretically without additional handcrafted design and parameter tuning. Finally, by incorporating the numerical scheme as a solver into the proximal optimization algorithm, we derive an ODE-inspired Tuning-free Proximal Network, dubbed OTPNet, which achieves efficient and robust fusion reconstruction. Extensive experiments on nine datasets across three different remote sensing image fusion tasks show that our OTPNet outperforms existing state-of-the-art approaches, which validates the effectiveness of our method.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

遥感图像融合旨在通过整合多个遥感传感器的空间与光谱信息，重建高空间和高光谱分辨率的图像。现有深度学习方法虽然取得了显著进展，但大多依赖手动设计的网络架构和超参数调优，缺乏充分的可解释性和适应性。为解决这一问题，本文提出了一种受常微分方程（ODE）启发的免调参近端分裂网络（OTPNet），将遥感图像融合分解为两个由深度先验正则化的优化问题，通过高阶ODE数值方案定制近端算子，实现可解释且自适应的融合重建。

### 2. 论文提出的方法论：核心思想、关键技术细节

**核心思想**：基于物理退化模型，将融合问题分解为空间重建和光谱重建两个子优化问题，分别用两个近端算子迭代求解，并引入高阶ODE数值方案设计近端网络，无需手动参数调优。

**关键技术细节**：
- **退化模型**：HR-MS图像 $H$ 通过空间降采样矩阵 $D$ 得到LR-MS $L$，通过光谱响应矩阵 $S$ 得到PAN $P$，即 $L = DH$，$P = HS$。
- **优化目标**：$\min_H \frac{1}{2}\|L - DH\|^2 + \frac{1}{2}\|P - HS\|^2 + R(H)$，其中 $R$ 为隐式先验。
- **近端分裂**：使用Douglas-Rachford（DR）分裂算法，交替更新两个近端算子：
  - 光谱近端算子：$\text{prox}_{R_P,\gamma}(H) = \arg\min_H \gamma\|P - HS\|^2 + R_P(H)$
  - 空间近端算子：$\text{prox}_{R_L,\gamma}(H) = \arg\min_H \gamma\|L - DH\|^2 + R_L(H)$
- **免调参迭代**：引入可学习参数 $(\mu,\lambda,\alpha,\beta)$，将迭代步骤表达为残差形式，自适应调整步长。
- **ODE启发近端网络**：将近端算子映射为神经网络层，采用四阶Runge-Kutta（RK4）数值方案设计网络前向传播，公式如下：
  - $N^{t+1} = N^t + \omega_4(k_1+2k_2+2k_3+k_4)$
  - $k_1 = F(N^t)$, $k_2 = F(N^t+\omega_1 k_1)$, $k_3 = F(N^t+\omega_2 k_2)$, $k_4 = F(N^t+\omega_3 k_3)$
- **截断误差分析**：RK4的局部截断误差为 $O(h^5)$，远优于前向Euler法的 $O(h^2)$，从而精度更高、稳定性更好。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：涵盖三个融合任务共9个数据集：
  - Pan-Sharpening：WorldView-3、GaoFen-2、QuickBird
  - 高光谱图像超分辨率（HSR）：PaviaCentre、Botswana4、Chikusei
  - 多光谱与高光谱融合（MHF）：CAVE、Harvard、NTIRE2020
- **基准**：按90%训练、10%验证划分；输入尺寸分别为 $64\times64$（PAN）和 $C\times16\times16$（LR-MS）等。
- **对比方法**：
  - 传统方法：Brovey、GS、IHS、SFIM
  - 模型驱动方法：GPPNN、MDCUN
  - 深度学习方法：PANnet、SRPPNN、MUCNN、LAGnet、PMACnet、PANFormer

### 4. 资源与算力

论文中未明确说明使用的GPU型号、数量及训练时长。仅提及在PyTorch框架下实现，使用AdamW优化器（$\beta_1=0.9, \beta_2=0.999$），训练120k步，初始学习率2e-4，每30k步减半，批量大小64。未提供具体硬件配置和运行时间。

### 5. 实验数量与充分性

实验较为充分：
- 在9个数据集上进行了定量对比（PSNR、SSIM、SAM、ERGAS）和定性可视化（绝对误差图）。
- 消融实验包括：
  - 不同近端分裂算法对比（GP、ADMM、HQS、DR）
  - 不同数值ODE方案对比（前向Euler、后向Euler、线性多步法、二阶格式、RK2、RK4）
  - 免调参与固定参数对比
- 还展示了性能曲线、训练损失曲线及计算复杂度对比。
- 实验设计客观、公平：采用不同任务和公开数据集，对比方法覆盖传统、模型驱动和深度学习方法。

### 6. 论文的主要结论与发现

- OTPNet在三个遥感融合任务中均取得最优结果，在Pan-Sharpening任务中（如WorldView-3）PSNR达39.255，显著优于所有对比方法。
- 高阶RK数值方案（RK4）比低阶方案（如Euler）PSNR提升超过2.2 dB，验证了高阶ODE在近端网络中的有效性。
- 免调参策略自适应平衡空间和光谱信息，优于固定参数设置。
- DR分裂算法优于GP、ADMM和HQS，更适用于空间-光谱信息交替优化。

### 7. 优点

- **创新性强**：首次将高阶ODE数值方案（RK4）与近端分裂优化结合，用于遥感图像融合，实现免调参且可解释的网络设计。
- **理论支撑**：通过截断误差分析论证了高阶RK方法的精度优势，提升了方法的可信度。
- **实验全面**：覆盖3个典型融合任务、9个数据集，对比12种方法，消融实验系统，结果优越。
- **实用性**：免调参设计减少了手工调整负担，提升了方法的适应性和泛化潜力。

### 8. 不足与局限

- **计算资源未说明**：缺少GPU型号、数量及训练时长等细节，不利于复现和效率评估。
- **任务范围有限**：仅针对遥感图像融合，未在其他多源/多模态融合任务（如医疗图像、图像超分辨率等）上验证。
- **潜在过拟合风险**：在特定传感器数据集上表现优异，但对不同传感器的泛化能力未充分讨论。
- **消融实验细节缺失**：如不同近端算子结构、网络深度等超参数的影响未深入探讨。

（完）
