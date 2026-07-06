---
title: Binarized Neural Network for Multi-spectral Image Fusion
title_zh: 用于多光谱图像融合的二值神经网络
authors: "Hou, Junming, Chen, Xiaoyu, Ran, Ran, Cong, Xiaofeng, Liu, Xinyang, You, Jian Wei, Deng, Liang-Jian"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Hou_Binarized_Neural_Network_for_Multi-spectral_Image_Fusion_CVPR_2025_paper.pdf"
tags: ["query:image-fusion"]
score: 9.0
evidence: 二值神经网络用于多光谱图像融合
tldr: 针对深度学习全色锐化方法在资源受限卫星上部署困难的问题，提出二值神经网络BNNPan。通过分析二值化对不同频率成分的信息退化，设计优先集成双流结构，在极低比特宽度下保持融合质量。实验表明该方法在显著降低存储和计算开销的同时，性能接近全精度网络，为星载多光谱图像融合提供高效解决方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 777, \"height\": 583, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 854, \"height\": 452, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1730, \"height\": 854, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 853, \"height\": 607, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1720, \"height\": 460, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1719, \"height\": 459, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 860, \"height\": 292, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 866, \"height\": 349, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 830, \"height\": 529, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1791, \"height\": 767, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1751, \"height\": 812, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 857, \"height\": 204, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-hou-binarized-neural-network-for-multi-spectral-image-fusion-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 859, \"height\": 176, \"label\": \"Table\"}]"
motivation: 现有深度学习全色锐化方法计算和存储开销大，难以部署于资源受限的卫星平台。
method: 提出BNNPan，一种基于二值神经网络的全色锐化网络，通过分析频域退化设计优先集成双流结构。
result: 在多个数据集上，BNNPan在极低比特宽度下性能接近全精度网络，显著降低资源需求。
conclusion: 二值神经网络可有效用于多光谱图像融合，为星载应用提供轻量化方案。
---

## Abstract
Pan-sharpening technology refers to generating a high-resolution (HR) multi-spectral (MS) image with broad applications by fusing a low-resolution (LR) MS image and HR panchromatic (PAN) image. While deep learning approaches have shown impressive performance in pan-sharpening, they generally require extensive hardware with high memory and computational power, limiting their deployment on resource-constrained satellites. In this study, we investigate the use of binary neural networks (BNNs) for pan-sharpening and observe that binarization leads to distinct information degradation across different frequency components of an image. Building on this insight, we propose a novel binary pan-sharpening network, termed BNNPan, structured around the Prior-Integrated Binary Frequency (PIBF) module that features three key ingredients: Binary Wavelet Transform Convolution, Latent Diffusion Prior Compensation, and Channel-wise Distribution Calibration. Specifically, the first decomposes input features into distinct frequency components using Wavelet Transform, then applies a "divide-and-conquer" strategy to optimize binary feature learning for each component, informed by the corresponding full-precision residual statistics. The second integrates a latent diffusion prior to compensate for compromised information during binarization, while the third performs channel-wise calibration to further refine feature representation. Our BNNPan, developed with the proposed techniques, achieves promising pan-sharpening performance on multiple remote sensing datasets, surpassing state-of-the-art binarization algorithms.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：高分辨率多光谱（HRMS）图像在遥感领域（地图制图、军事、环境监测）有广泛应用。然而，受限于传感器硬件，直接获取HRMS图像困难。全色锐化（pan-sharpening）技术通过融合低分辨率多光谱（LRMS）图像和纹理丰富的全色（PAN）图像来生成HRMS图像。近年来，基于深度学习的方法性能显著提升，但通常需要庞大的计算资源和存储空间（高端GPU），难以部署在资源受限的卫星上。
- **核心问题**：如何在保持全色锐化性能的同时，大幅降低计算和存储开销，使之适用于星载平台。论文探索将二值神经网络（BNN）引入全色锐化任务，并观察到二值化会导致不同频率成分的信息退化程度不同。基于此洞察，设计了一种高效的二值全色锐化网络BNNPan。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：构建一个基于二值神经网络的全色锐化网络，核心模块为**Prior-Integrated Binary Frequency (PIBF)**，包含三个关键组件：
  - **Binary Wavelet Transform Convolution (BWTC)**：利用离散小波变换（DWT）将输入特征分解为四个频带（LL、LH、HL、HH），对每个频带分别进行二值卷积。针对二值化后与全精度特征的分布差异，设计了一种**全精度统计归一化二值卷积单元（NBC）**：先对每个频带的全精度特征计算均值和方差，然后对二值卷积输出进行归一化（调整均值和方差使其接近全精度分布），最后与残差相加。通过“分而治之”策略优化各频带的二值特征学习。
  - **Latent Diffusion Prior Compensation (LDPC)**：二值化过程会丢失信息，因此引入一个潜在扩散模型来生成紧凑的潜在先验特征 $\hat{z}$，通过缩放因子和偏移量注入到BWTC输出中，以补偿丢失的信息。训练分三阶段：第一阶段用HRMS真实图像（GT）训练潜在编码器，第二阶段训练扩散模型生成该先验，第三阶段联合微调。
  - **Channel-wise Distribution Calibration (CDC)**：对LDPC输出进行通道注意力校准，生成通道权重向量，对特征进行重标定，并与BWTC输出相加。
- **公式/算法流程**：
  - 整体映射：$\hat{z} = \phi(z_T, c)$，其中$c$由PAN和上采样MS图像通过潜在编码器得到；$SR = \mathcal{N}(P, M_U, \hat{z}) + M_U$。
  - BWTC中：$X_{HH}, X_{HL}, X_{LH}, X_{LL} = DWT(X)$；对每个$X_f$，通过二值卷积得到$\tilde{X}_b$，然后使用全精度统计$(\mu_f, \sigma_f)$归一化：$\hat{X}_b = \frac{\sigma_f}{\lambda} \cdot \frac{\tilde{X}_b - \mu_b}{\sigma_b} + \mu_f + \eta$（含可学习参数），最后加残差得$\hat{X}$；再通过IDWT合成空间特征$Y$。
  - LDPC：$z_w, z_b = Split(Linear(\hat{z}))$，$\tilde{Y} = Y \odot z_w + z_b$。
  - CDC：$W_c = \Phi(Linear(Cat(Mean(|\tilde{Y}|, \tilde{Y}), Std(\tilde{Y}))))$，$O = W_c \odot \tilde{Y} + Y$。

## 3. 实验设计

- **数据集**：三个遥感基准数据集：WorldView-3 (WV3)、Gaofen-2 (GF2)、QuickBird (QB)。
- **场景/评估**：包括降分辨率评估（Reduced-resolution，与GT比较）和全分辨率评估（Full-resolution，无GT，使用Dλ、Ds、HQNR等无参考指标）。
- **对比方法**：
  - **全精度（Full-precision）基线**：PNN、MSDCNN、GPPNN、FusionNet、LAGConv、HFEAN、HFIN等。
  - **二值（Binary）方法**：BNN、IRNet、ReActNet、BTM、E2FIF、FABNet、BBCU。
- **评价指标**：降分辨率使用SAM、ERGAS、Q2n、PSNR；全分辨率使用Dλ、Ds、HQNR。表格中给出均值和标准差。

## 4. 资源与算力

论文在**实验部分**（第4节）和**补充材料**中未明确说明使用的GPU型号、数量及训练时长。仅在正文提及“由于篇幅限制，数据集、基线和实验设置的详细内容见补充材料”，但用户提供的文本中未包含补充材料内容。因此，**文中未明确给出具体算力信息**。

## 5. 实验数量与充分性

- **实验数量**：
  - 在三个数据集（WV3、GF2、QB）上进行了降分辨率和全分辨率评估，共6组主要定量对比。
  - 提供了定性对比（图5、图6的视觉结果和误差图/质量图）。
  - 进行了**消融实验**：表3对三个组件（DWT、归一化Norm、扩散先验LDPC）分别去除，验证其贡献；表4对不同PIBF模块数量（2、4、6）进行对比。
  - 提供了特征可视化（图7、图9）和性能-效率散点图（图8）。
- **充分性与公平性**：
  - 对比了多种主流全精度和二值方法，指标全面，包含标准差，统计可靠。
  - 消融实验覆盖了所有关键组件和模块数量，验证了设计有效性。
  - 定性图上展示了清晰差异，支持定量结论。
  - 但**未进行更广泛的泛化实验**（如跨传感器测试），也未提供统计显著性检验。整体较为充分，但可更深入。

## 6. 主要结论与发现

- 二值神经网络能够有效应用于全色锐化任务，在极低比特宽度下获得接近全精度网络的性能，同时显著降低存储和计算开销。
- 所提出的BNNPan在多个数据集上超越了所有现有二值方法，甚至优于部分全精度方法，在SAM、PSNR、HQNR等指标上表现优异。
- 频域“分治”策略（BWTC）、全精度统计归一化、潜在扩散先验补偿和通道校准均对性能有实质贡献。
- 模型参数量仅为78.2K（4个模块），FLOPs约0.12G，远低于全精度方法，适合星载部署。

## 7. 优点

- **创新性**：首次将二值神经网络引入全色锐化，并针对二值化引起的频域信息退化设计了专门模块（BWTC）。
- **高效性**：仅需极少量参数和计算量，性能却接近甚至超过全精度模型，是高效解决方案。
- **设计巧妙**：利用小波变换分频处理，结合全精度统计归一化缩小分布差距；引入扩散模型作为先验补偿信息损失，三阶段训练策略保证了稳定性。
- **实验全面**：在多个数据集上对比多种方法，包含降分辨率和全分辨率评估，消融实验验证充分。

## 8. 不足与局限

- **资源算力未公开**：缺少训练所需GPU型号、数量、时间等信息，不利于复现和效率比较。
- **实验覆盖有限**：仅测试了三种遥感卫星数据集，未在更多传感器（如WorldView-2、SPOT等）或真实退化场景下验证泛化能力。
- **扩散模型带来的计算开销**：虽然网络主体是二值的，但潜在扩散模块仍为全精度处理，可能增加整体延迟；论文未分析推理时扩散步骤数对效率的影响。
- **未讨论极端情况**：如输入图像噪声大、光谱差异极端等条件下，二值网络是否仍能保持稳健性。
- **缺少与模型剪枝、蒸馏等轻量化方法的对比**：只比较了二值方法，未与其他压缩技术（如8-bit量化）对比。
- **应用限制**：二值网络在硬件上还需专用指令集（如XNOR、bitcount）才能充分发挥加速优势，通用处理器上收益可能有限。

（完）
