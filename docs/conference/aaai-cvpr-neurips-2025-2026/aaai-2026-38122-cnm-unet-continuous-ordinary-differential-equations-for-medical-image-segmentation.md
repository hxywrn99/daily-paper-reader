---
title: "CNM-UNet: Continuous Ordinary Differential Equations for Medical Image Segmentation"
title_zh: CNM-UNet：连续常微分方程用于医学图像分割
authors: "Tianqi Xu, Yashi Zhu, Quansong He, Yue Cao, Kaishen Wang, Zhang Yi, Tao He"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38122/42084"
tags: ["query:medical-seg"]
score: 10.0
evidence: 使用连续常微分方程的UNet医学图像分割
tldr: 针对离散化ODE方法在模型紧凑性、计算精度和效率间的权衡，提出CNM-UNet，用单个连续神经记忆ODE块替换全部解码层，在保持精度的同时显著降低计算开销，在多个医学分割任务上取得领先性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有离散ODE方法在模型紧凑性、计算精度和效率之间存在固有权衡，连续ODE解决方案因高计算成本、长训练时间和泛化性差而鲜有研究。
method: 提出连续神经记忆ODE UNet，用单个CNM块替换所有解码层，实现连续ODE集成。
result: 在医学图像分割任务中实现了更优的精度-效率平衡。
conclusion: CNM-UNet验证了连续ODE在医学图像分割中的有效性和高效性。
---

## Abstract
Integrating Ordinary Differential Equations (ODEs) with U-shaped neural networks has emerged as a novel direction in medical image segmentation. Current networks predominantly employ discretization methods incorporating ODEs. However, these methods face inherent trade-offs between model compactness, computational accuracy, and efficiency. Continuous ODE solutions were rarely studied because they face three limitations: high computational costs, long training time, and poor generalization ability. To address these limitations, we propose an innovative Continuous Neural Memory ODE UNet (CNM-UNet), which replaces all hierarchical decoder layers in vanilla UNet with a single Continuous Neural Memory ODEs Block (CNM-Block) decoder, significantly reducing computation costs and improving training efficiency. CNM-UNet leverages ODEs' dynamic properties to establish continuous temporal feature extraction. For alleviating the generalization problem, a DUal SElf-updated (DUSE) strategy based on test-time adaptation principles is introduced to enhance cross-domain generalization. Experimental results demonstrate CNM-UNet's comprehensive advantages in computational capacity, convergence speed, and cross-domain adaptability, offering new insights for practical deployment of continuous ODE methodologies for medical image segmentation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：医学图像分割中，U形网络（如UNet）与常微分方程（ODE）的结合成为新方向。现有方法主要采用离散化ODE（如DiODE-UNets），但面临模型紧凑性、计算精度与效率之间的固有权衡。离散化数值求解器（如欧拉法）存在局部截断误差累积、时间步约束导致的稳定性问题。
- **核心问题**：连续ODE解决方案（如NODEs-UNet）虽能提升精度，但存在三大局限：高计算成本、长训练时间、泛化能力差，限制了实际部署。
- **研究动机**：设计一种既保持连续ODE高精度又能大幅降低计算开销、缩短训练时间、并增强跨域泛化能力的方法。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：用单个**连续神经记忆ODE块（CNM-Block）** 替换vanilla UNet中所有分层解码器，实现轻量连续的隐状态提取；并引入**DUal SElf-updated（DUSE）策略**基于测试时自适应（TTA）原则增强跨域泛化。
- **关键技术细节**：
    - **CNM-Block架构**：
        - 输入经Softplus激活和BN后，送入连续ODE求解器（采用**Dormand-Prince-Shampine 5阶Runge-Kutta（dopri5）** 方法）。
        - 动力学由神经记忆ODE（nmODE）定义：\( f = -y + \sin^2(y + C(\gamma)) \)，其中 \( y \) 为网络输入，\( \gamma \) 为全局可学习参数（初始化为零）。
        - 利用增广伴随法（adjoint method）计算梯度，内存复杂度 \( O(1) \)。
    - **多尺度特征融合**：将编码器各级输出乘以可学习权重，经卷积映射到统一通道空间，通过上采样和加权求和融合后输入CNM-Block。
    - **DUSE策略**：
        - **梯度统计自适应**：在训练时记录各时间步状态梯度 \( f_i \) 的均值和方差（全局统计）；测试时通过绝对距离损失动态调整控制 \( \gamma \) 的卷积层权重。
        - **Prompt驱动的BN统计校准**：为每个测试图生成低频提示，通过FFT操作得到适配图像；利用加权BN损失（赋予CNM-Block内BN层更高权重）优化提示，校准BN统计量。
    - **算法流程**（Algorithm 1）：对每个测试图像，先通过提示生成适配图像，进行两次前向/反向传播，分别更新CNN-Block权重和提示。

### 3. 实验设计：数据集、场景、基准与对比方法
- **数据集**：
    - **ISIC2018**：皮肤镜图像分割，约2700张，resize至256×256。
    - **OD/OC数据集**：青光眼视盘/视杯分割，来自5个公开资源（RIM-ONE-r3、REFUGE、ORIGA、Drishti-GS、REFUGE验证集），共超2000例，resize至512×512。
    - **Polyp数据集**：息肉分割，来自4个公开集（CVC-ClinicDB、ETIS-LaribPolypDB、Kvasir-Seg、BKAI-IGH-NeoPolyp），共超2800例，resize至352×352。
- **基准场景**：
    - **源域内分割**：在ISIC2018、Polyp-A/C/D上训练并测试。
    - **跨域泛化**：在OD/OC-E上训练，分别在OD/OC-A/B/C/D及连续序列上测试；在Polyp-D上训练，分别在Polyp-A/B/C及连续序列上测试。
    - **鲁棒性**：在ISIC2018上施加6种图像损坏（亮度、对比度、散焦模糊、高斯噪声、散粒噪声、饱和度）严重程度5，测试DUSE效果。
- **对比方法**：
    - **分割性能对比**：UNet、ResUNet、Att-UNet、MALUNet、EGE-UNet、Continuous UNet、DiODE-UNet（EED/LMD）、nmS4M-UNet、nmSSM-UNet等。
    - **TTA泛化对比**：AdaBN、Tent、CoTTA、EATA、SAR、VPTTA。

### 4. 资源与算力
- **硬件**：单张RTX 4090 GPU。
- **软件**：PyTorch。
- **训练时长**（表1）：
    - CNM-UNet：1.80小时
    - Continuous UNet：14.90小时
    - DiODE-UNet：1.12小时
    - Vanilla UNet：1.38小时
- **其他资源**：批量大小8，训练300 epoch。内存消耗：CNM-UNet 5.66GB，Continuous UNet 14.24GB，DiODE-UNet 4.34GB，Vanilla UNet 6.19GB。

### 5. 实验数量与充分性
- **实验组数**：
    - 源域内分割：4个数据集（ISIC2018 + Polyp-A/C/D）。
    - 泛化性能：2个数据集（OD/OC含4个子集+连续场景，Polyp含3个子集+连续场景），对比7种TTA方法。
    - 消融研究：初始时间步Δ t 参数敏感性（图2），图像损坏鲁棒性（表4，6种损坏）。
- **充分性评价**：实验覆盖了多个医学领域（皮肤、眼科、结肠镜）、多种分布偏移（不同采集域、图像损坏），对比方法丰富（包括轻量级和连续ODE类），消融合理。实验结果报告了均值和标准差（除部分方差极小处省略），重复5次，评估公平。结论可信。

### 6. 论文的主要结论与发现
- **性能-效率平衡**：CNM-UNet在多个数据集上达到最优或次优的mIoU/DSC，同时参数比Vanilla UNet少约4M，GFLOPs大幅降低（如在Polyp上降低24.54），训练时间比Continuous UNet缩短约13小时。
- **泛化能力**：DUSE策略在跨域测试中显著优于“Source Only”和多数TTA方法（如OD/OC上DSC提升0.374~0.214，Polyp上提升0.064~0.084），且在图像损坏场景下表现更鲁棒。
- **连续ODE的可行性**：证实了连续ODE方法（特别是单解码器设计）在医学图像分割中的实用性和高效性，为实际部署提供了新路径。

### 7. 优点：方法或实验设计上的亮点
- **方法论创新**：
    - 首次用单个连续nmODE块替代多层离散解码器，实现连续隐状态提取，同时大幅压缩参数和计算量。
    - 利用dopri5自适应步长求解器，兼顾精度与效率。
    - DUSE策略针对连续ODE模型的泛化缺陷，结合梯度统计和Prompt学习，巧妙修正参数及BN统计，无需修改模型全部参数。
- **实验设计亮点**：
    - 多数据集、多域、多场景验证（源域内+跨域+损坏鲁棒性），全面展示优势。
    - 与最新离散ODE方法（nmS4M-UNet等）及连续UNet直接对比，且控制公平条件。
    - 消融实验关键参数（Δt）及损坏鲁棒性，提供了实用指导。

### 8. 不足与局限
- **实验覆盖**：仅在皮肤、眼科、结肠镜数据集上验证，其他医学领域（如CT、MRI）未测试，通用性待进一步验证。
- **偏差风险**：泛化实验中训练域与测试域分布差异可能不具代表性；损坏实验仅采用一种严重程度（级别5），未探索不同程度影响。
- **应用限制**：连续ODE求解器（dopri5）仍比离散方法慢（CNM-UNet训练时间1.80h vs DiODE-UNet的1.12h），且推理时需多次前向计算，实时性可能受限；DUSE策略需为每个测试图像生成提示并两次更新，增加了测试时开销。
- **方法局限性**：CNM-Block中γ参数为全局固定，可能无法适应更复杂的多模态特征；DUSE的提示生成（低频率）假设可能不适用于所有分布偏移类型。
- **论文未讨论**：未与Transformer或Mamba类架构对比；未分析模型在多类分割或小目标分割上的表现；未提供代码链接（仅提到GitHub仓库，但需手动验证）。

（完）
