---
title: Generative Medical Segmentation
title_zh: 生成式医学分割
authors: "Jiayu Huo, Xi Ouyang, Sébastien Ourselin, Rachel Sparks"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32402/34557"
tags: ["query:medical-seg"]
score: 9.0
evidence: 使用潜空间映射的生成式医学图像分割方法
tldr: 传统鉴别式分割在跨数据集泛化上受限。本文提出生成式医学分割（GMS），利用预训练视觉基础模型提取图像和掩码的潜表征，并学习从图像到掩码的轻量级映射函数。该方法在多个医学图像分割数据集上展示了更强的泛化能力。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 传统鉴别式分割模型泛化能力有限，难以适应多样数据集。
method: 基于预训练视觉基础模型，在潜空间中学习图像到掩码的映射函数。
result: 在多个数据集上表现优于传统鉴别式方法，尤其跨数据集泛化。
conclusion: 生成式范式为医学图像分割提供了新思路，提升了泛化性。
---

## Abstract
Rapid advancements in medical image segmentation performance have been significantly driven by the development of Convolutional Neural Networks (CNNs) and Vision Transformers (ViTs). These models follow discriminative pixel-wise classification learning paradigm and often have limited ability to generalize across diverse medical imaging datasets. In this manuscript, we introduce Generative Medical Segmentation (GMS), a novel generative approach to perform image segmentation. GMS employs a robust pre-trained vision foundation model to extract latent representations for images and corresponding ground truth masks, followed by a lightweight model that learns a mapping function from the image to the mask in the latent space. Once trained, the model can generate estimated segmentation masks using the pre-trained vision foundation model to decode the predicted latent mask representation back into image space. The design of GMS leads to fewer trainable parameters in the model, reducing the risk of overfitting and enhancing its generalization capability. Our experimental analysis across five open-source datasets in different medical imaging domains demonstrates GMS outperforms existing discriminative and generative segmentation models. Furthermore, GMS is able to generalize well across datasets of the same imaging modality from different centers. Our experiments suggest GMS offers a scalable and effective solution for medical image segmentation.

---

## 论文详细总结（自动生成）

# 生成式医学分割（GMS）论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：传统医学图像分割方法（基于CNN或Vision Transformer）遵循**判别式像素级分类范式**，在训练数据有限时容易过拟合，且对跨中心、跨模态数据的**泛化能力有限**。现有生成式模型（如GAN、扩散模型）要么训练不稳定，要么推理耗时，要么参数量巨大，难以平衡性能与效率。
- **核心问题**：如何设计一种**参数量少、泛化能力强、推理高效**的生成式分割框架，避免判别式模型在域偏移时的性能下降。
- **整体含义**：提出GMS，将分割任务转化为**潜空间中的图像到掩码映射**，利用预训练视觉基础模型（Stable Diffusion VAE）的强表征能力和轻量级映射网络，实现了对多种医学图像数据集的最佳分割效果，尤其是**跨数据集泛化**能力显著优于现有方法。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：使用预训练VAE（来自Stable Diffusion）将图像和真实掩码编码到潜空间，然后训练一个轻量级**潜映射模型（Latent Mapping Model, LMM）** 学习从图像潜码到掩码潜码的映射函数，最后用预训练解码器将预测的掩码潜码解码回图像空间。整个过程中，预训练编码器和解码器**冻结**，仅更新LMM参数。
- **关键技术细节**：
  - **图像分词器（Image Tokenizer）**：采用Stable Diffusion VAE的编码器E和解码器D，输入图像/掩码被编码为4通道的3D张量（尺寸为原图的1/8）。该VAE在大型自然图像上训练，具备强零样本泛化能力。
  - **潜映射模型（LMM）**：由两个卷积块（Conv+PReLU+组归一化）+ 一个自注意力层组成，**无下采样层**以避免空间信息损失（因为输入已在潜空间下采样）。自注意力机制捕获全局依赖。
  - **损失函数**：
    - 潜空间匹配损失 \( \mathcal{L}_{lm} = \|Z_M - \hat{Z}_M\|_2^2 \)
    - 图像空间分割损失 \( \mathcal{L}_{seg} = 1 - \frac{2 \cdot M \odot \hat{M}}{M + \hat{M}} \)（Dice损失）
    - 总体损失 \( \mathcal{L} = \mathcal{L}_{lm} + \mathcal{L}_{seg} \)
- **算法流程**（文字说明）：
  1. 输入图像I和掩码M，通过冻结的预训练编码器E得到潜码 \(Z_I, Z_M\)。
  2. 将 \(Z_I\) 输入LMM，得到预测的掩码潜码 \(\hat{Z}_M\)。
  3. 通过冻结的预训练解码器D将 \(\hat{Z}_M\) 解码为预测掩码 \(\hat{M}\)。
  4. 使用联合损失更新LMM参数。

## 3. 实验设计

- **数据集**：5个开源医学图像分割数据集，涵盖不同成像模态：
  - **BUS**（乳腺超声，163张），**BUSI**（乳腺超声，647张）
  - **GlaS**（结肠组织病理，85训练/80测试）
  - **HAM10000**（皮肤镜，10015张）
  - **Kvasir-Instrument**（内窥镜工具分割，590张）
- **基准与对比方法**：
  - **CNN基线**：UNet、MultiResUNet、ACC-UNet、nnUNet、EGE-UNet
  - **Transformer基线**：SwinUNet、SME-SwinUNet、UCTransNet
  - **生成式基线**：MedSegDiff-V2、SDSeg、GSS
  - **域泛化基线**：MixStyle、DSU（基于DeepLab-V3+ResNet50）
- **实验设置**：
  - 除GlaS保持官方划分外，其他数据集随机80%训练/20%测试。
  - 评估指标：Dice系数（DSC）、IoU、Hausdorff距离95%分位（HD95）。
  - 图像resize至224×224，在线数据增强（随机翻转、旋转、HSV色彩抖动）。
  - 训练1000 epoch，batch size=8，AdamW优化器，余弦退火学习率（初始2e-3），预测阈值0.5后二值化。

## 4. 资源与算力

- **GPU型号**：单张NVIDIA A100 40G GPU。
- **训练时长**：未明确说明具体训练时间，但提及1000个epoch，batch size=8，输入尺寸224×224，模型参数量仅1.5M（极轻量）。
- **推理速度**：GMS推理速度为15.38 samples/s，慢于CNN/Transformer（如UNet 36.65 samples/s），但优于其他生成式模型（如MedSegDiff-V2仅0.31 samples/s）。GPU内存占用11.86 GFLOPS。

## 5. 实验数量与充分性

- **实验组数**：主要有三类实验：
  1. **主实验**：在5个数据集上对比11种方法（表1、表2），共5×11=55个组合，报告DSC/IoU/HD95。
  2. **域泛化实验**：BUS ↔ BUSI交叉测试（表3），对比所有基线和两种域泛化方法，共2组（A→B和B→A），涉及14种方法。
  3. **消融实验**：
     - 损失函数消融（表4）：在BUSI、HAM10000、Kvasir上分别测试单独使用 \( \mathcal{L}_{lm} \)、单独使用 \( \mathcal{L}_{seg} \)、联合使用。
     - 图像分词器消融（表5）：对比VQ-VAE和SD-VAE在三个数据集上的表现。
  4. **计算复杂性分析**（表6）：对比GPU内存、GFLOPS、推理速度。
- **充分性评估**：
  - **充分**：覆盖了多模态、多尺度数据集（超声、组织病理、皮肤镜、内窥镜），对比方法涵盖CNN、Transformer、生成式三大类别，域泛化实验验证了跨中心能力。
  - **客观公平**：所有方法使用相同数据划分和评估指标，超参数统一（如输入尺寸、batch size、优化器），且开源代码可复现。但未进行统计显著性检验（如配对t检验或Wilcoxon检验）。
  - **偏差风险**：仅使用2D图像，未在3D数据集（如CT/MRI体数据）上验证；域泛化实验仅在两个超声数据集间进行，模态单一。

## 6. 论文的主要结论与发现

- **性能领先**：GMS在全部5个数据集的DSC/IoU/HD95指标上均优于所有对比方法（表1、表2），仅IoU在Kvasir上略低于nnUNet（90.02 vs 90.20）。
- **域泛化能力突出**：在BUS↔BUSI跨中心实验中，GMS的DSC和HD95均最优（表3），甚至超越专门设计的域泛化方法（MixStyle、DSU）。
- **参数量极小**：仅1.5M可训练参数（除EGE-UNet的0.05M外，最少），远小于其他模型（如nnUNet 20.6M，UNet 14.0M，SwinUNet 27.2M，GSS 49.8M）。
- **损失函数互补**：联合使用潜空间损失和图像空间损失效果最佳（表4）。
- **SD-VAE优于VQ-VAE**：作为图像分词器，SD-VAE带来更高DSC和更低HD95（表5）。
- **生成式范式有效**：通过充分挖掘预训练视觉模型的知识，生成式分割可超越传统判别式方法。

## 7. 优点

- **创新性**：首次将分割任务转化为潜空间映射问题，利用冻结的预训练VAE极大地减少了可训练参数，降低了过拟合风险。
- **泛化能力**：跨数据集（同一模态不同中心）表现优异，归因于预训练模型在大规模自然图像上学习的域无关表征。
- **轻量高效**：LMM仅含卷积块和单头自注意力，参数量1.5M，推理速度远快于其他生成式模型（如扩散模型）。
- **实验全面**：涵盖5个数据集、11种对比方法、消融研究、域泛化实验、计算复杂性分析，结论可靠。
- **代码开源**：提供GitHub仓库，可复现。

## 8. 不足与局限

- **缺乏3D验证**：仅针对2D医学图像，未在3D数据（如CT、MRI体数据）上实验，限制了在三维场景中的应用。
- **域泛化测试单一**：仅在两超声数据集间进行交叉验证，未涵盖不同模态（如超声→CT）或更多中心，泛化结论普适性有限。
- **推理速度仍低于CNN/Transformer**：GMS推理速度（15.38 samples/s）慢于UNet（36.65 samples/s）和UCTransNet（26.67 samples/s），虽然远优于扩散模型，但在实时性要求高的场景中可能仍需优化。
- **未进行统计显著性检验**：实验仅报告均值，未给出方差或进行显著性比较，难以判断差异是否随机。
- **依赖预训练质量**：GMS性能高度依赖于Stable Diffusion VAE的预训练质量，若目标域与自然图像差异极大（如低信噪比超声），效果可能下降（但实验中仍表现最佳）。
- **缺乏多任务或小样本分析**：未探索在极少量训练样本（如<50张）下的性能，而该场景在医学中常见。

（完）
