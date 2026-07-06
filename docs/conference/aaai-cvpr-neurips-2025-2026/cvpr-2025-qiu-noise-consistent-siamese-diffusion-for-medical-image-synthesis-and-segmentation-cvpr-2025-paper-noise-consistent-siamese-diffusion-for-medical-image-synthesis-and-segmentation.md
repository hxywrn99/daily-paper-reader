---
title: Noise-Consistent Siamese-Diffusion for Medical Image Synthesis and Segmentation
title_zh: 噪声一致孪生扩散模型用于医学图像合成与分割
authors: "Qiu, Kunpeng, Gao, Zhiqiang, Zhou, Zhiying, Sun, Mingjie, Guo, Yongxin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Qiu_Noise-Consistent_Siamese-Diffusion_for_Medical_Image_Synthesis_and_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:medical-seg"]
score: 10.0
evidence: 扩散模型用于医学图像分割与合成
tldr: 针对医学图像标注数据稀缺的问题，提出Siamese-Diffusion，包含Mask-Diffusion和Image-Diffusion双组件，通过噪声一致性约束生成逼真的图像-掩码对，有效扩充训练集，提升了分割模型的鲁棒性和准确性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 838, \"height\": 1006, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1827, \"height\": 894, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 670, \"height\": 284, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 868, \"height\": 231, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1812, \"height\": 746, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 359, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 830, \"height\": 400, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 848, \"height\": 225, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 857, \"height\": 684, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 857, \"height\": 372, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 830, \"height\": 422, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 832, \"height\": 317, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-qiu-noise-consistent-siamese-diffusion-for-medical-image-synthesis-and-segmentation-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 828, \"height\": 262, \"label\": \"Table\"}]"
motivation: 现有扩散模型生成医学图像-掩码对时因数据稀缺难以保持保真度，影响分割效果。
method: 提出双组分的Siamese-Diffusion模型，利用噪声一致性约束协同训练Mask-Diffusion和Image-Diffusion。
result: 在多个医学分割基准上，生成的合成数据显著提升了分割性能，尤其在小样本场景下。
conclusion: 生成高质量合成图像-掩码对是缓解医学标注稀缺的有效手段。
---

## Abstract
Deep learning has revolutionized medical image segmentation, yet its full potential remains constrained by the paucity of annotated datasets. While diffusion models have emerged as a promising approach for generating synthetic image-mask pairs to augment these datasets, they paradoxically suffer from the same data scarcity challenges they aim to mitigate. Traditional mask-only models frequently yield low-fidelity images due to their inability to adequately capture morphological intricacies, which can critically compromise the robustness and reliability of segmentation models. To alleviate this limitation, we introduce Siamese-Diffusion, a novel dual-component model comprising Mask-Diffusion and Image-Diffusion. During training, a Noise Consistency Loss is introduced between these components to enhance the morphological fidelity of Mask-Diffusion in the parameter space. During sampling, only Mask-Diffusion is used, ensuring diversity and scalability. Comprehensive experiments demonstrate the superiority of our method. Siamese-Diffusion boosts SANet's mDice and mIoU by 3.6% and 4.4% on the Polyps, while UNet improves by 1.52% and 1.64% on the ISIC2018.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：医学图像分割任务中，标注数据稀缺限制了深度学习模型的性能。扩散模型虽能生成图像-掩码对以扩充数据集，但其自身也面临同样的数据匮乏问题，导致生成的图像缺乏必要的形态学特征（如表面纹理），从而影响下游分割模型的鲁棒性和可解释性。
- **现有方法局限**：
  - **仅用掩码作为先验控制的生成模型**（如 ControlNet, ArSDM）容易陷入低形态保真度的局部最优，合成图像缺少细节。
  - **结合图像和掩码作为联合先验控制的方法**（如 ControlNet 的 mask-image 模式）虽能提高保真度，但生成的图像与真实图像过于相似，多样性差，且受限于配对样本的稀缺性，可扩展性低。
- **整体含义**：需要一种既能保留仅用掩码采样范式的高多样性和可扩展性，又能提升合成图像形态保真度的方法，从而真正可靠地为分割模型提供有效数据扩充。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **Siamese-Diffusion**（孪生扩散）模型，包含两个组件：
  - **Mask-Diffusion**：仅以掩码作为先验控制进行训练和采样（保证多样性和可扩展性）。
  - **Image-Diffusion**：以图像和掩码联合先验控制进行训练（提供更精确的噪声预测，形态保真度高），但在采样阶段不使用。
- **关键技术细节**：
  1. **噪声一致性损失（Noise Consistency Loss, Lc）**：在训练时，让 Mask-Diffusion 预测的噪声与 Image-Diffusion 预测的噪声（经 stop-gradient 处理作为锚点）之间计算 L2 损失。该损失引导 Mask-Diffusion 的参数向高保真度的局部最优方向移动，相当于在参数空间中实现“无图像指导”（Image-Free Guidance）。
  2. **Siamese 架构**：两个组件共享同一个扩散模型（通过“Copy”操作），但使用不同的先验控制嵌入。Image-Diffusion 中使用的掩码特征来自 Mask-Diffusion 但截断梯度，避免双重反向传播。
  3. **密集提示输入模块（Dense Hint Input, DHI）**：针对医学图像的高密度语义，在 ControlNet 基础上增加更密集的残差块和补丁合并模块，以更好提取图像和掩码的细粒度特征。
  4. **在线增强（Online-Augmentation）**：利用 Image-Diffusion 的高精度噪声预测，通过单步采样生成近似图像，再与掩码组合用于 Mask-Diffusion 的训练，进一步扩充训练数据。
- **训练损失**：总损失 L = Lm（Mask-Diffusion 去噪损失）+ Li（Image-Diffusion 去噪损失）+ Lc（噪声一致性损失）+ Lm'（在线增强损失）。
- **采样阶段**：仅使用 Mask-Diffusion，输入任意掩码即可生成高保真且多样化的医学图像。

## 3. 实验设计

- **数据集**：
  - 医学公开数据集：**Polyps**（包含 Kvasir 和 CVC-ClinicDB，共1450张）、**ISIC2016**（900张）、**ISIC2018**（2594张）。
  - 内部自然数据集：**Stain**（500张）、**Faeces**（458张），按3:1:1划分。
- **基准**：
  - 图像质量：FID、KID、CLIP-I、LPIPS、CMMD、MOS（由3位临床医生评估）。
  - 分割性能：mDice、mIoU。
- **对比方法**：
  - 仅掩码方法：SinGAN-Seg、T2I-Adapter、ArSDM、ControlNet。
  - 联合图像-掩码方法：ControlNet（M+I）。
  - 基础增强：Copy-Paste。
- **分割模型**：SANet、Polyp-PVT、CTNet（息肉分割）；UNet、SegFormer（皮肤病变分割）；SegFormer（污渍/粪便分割）。

## 4. 资源与算力

- 文中明确提到：使用 **8 张 NVIDIA 4090 GPU**，批量大小 **6（每 GPU）**，总批量大小 **48**。
- 训练迭代次数：Polyps、ISIC2016、ISIC2018 为 **3000 次**；Stain 和 Faeces 为 **1500 次**。
- 未明确说明单次训练时长，但基于常见扩散模型微调，在如此配置下应为数小时至一天。

## 5. 实验数量与充分性

- **实验类型**：
  - **主实验**：在3个息肉测试集（EndoScene、ClinicDB、Kvasir、ColonDB、ETIS）上评估3种分割模型，对比7种生成方法。
  - **皮肤病变**：ISIC2016/2018 上评估 UNet 和 SegFormer，对比5种方法。
  - **内部数据集**：Stain 和 Faeces 上评估 SegFormer，对比4种方法。
  - **消融实验**：7个设置（DHI、Online-Augmentation、Lc 不同组合），以及不同 wc 值（0.0~2.0）的影响。
  - **可扩展性实验**：数据量倍数（1-4倍）对分割性能的影响。
- **充分性评价**：
  - 实验覆盖了多种医学场景（息肉、皮肤病变、污渍、粪便），使用了多种分割骨干（CNN 和 Transformer），对比方法全面。
  - 消融实验系统性地验证了各组件的贡献。
  - 客观性：使用了多个标准图像质量指标和分割指标，并有人工评估（MOS）。
  - 公平性：所有对比方法均在相同条件下训练，且重复实验（如 Copy-Paste 作为基线）。

## 6. 论文的主要结论与发现

- **主要结论**：
  - Siamese-Diffusion 在图像质量（FID、KID 等）和分割性能上均优于所有现有方法。
  - 在息肉分割中，SANet 的 mDice 提升 3.6%、mIoU 提升 4.4%；在皮肤病变中，UNet 的 mDice 提升 1.52%、mIoU 提升 1.64%。
  - 仅用掩码采样范式结合噪声一致性损失，可同时获得高保真度和高多样性，是缓解数据稀缺的有效途径。
- **重要发现**：
  - 图像质量（FID）与分割性能并不完全正相关；ArSDM 虽图像质量较低，但分割提升好于高保真的 ControlNet（M+I），说明形态多样性同样重要。
  - 使用变换后的随机掩码生成的图像，比使用真实掩码生成的图像对分割模型提升更大，进一步证明了可扩展性优势。
  - 当合成数据量达到原始数据3倍时效果最佳，继续增加至4倍出现性能下降，推测存在“长尾问题”。

## 7. 优点

- **方法创新**：
  - 提出参数空间中的“无图像指导”，通过噪声一致性损失在训练时利用 Image-Diffusion 引导 Mask-Diffusion，采样时无需图像先验，兼顾保真度和多样性。
  - Siamese 架构共享参数，避免了传统知识蒸馏的资源消耗。
  - DHI 模块、在线增强等设计针对医学图像特点进行了优化。
- **实验严谨**：
  - 评估涵盖多种指标和多个数据集，消融实验充分，对比方法包括近期 SOTA。
  - 分析了图像质量与分割任务之间的非单调关系，提供了深入洞察。
- **实用价值**：方法可显著缓解医学影像标注稀缺问题，且生成的图像具有临床可接受的形态特征（经临床医生 MOS 评估）。

## 8. 不足与局限

- **实验覆盖局限**：
  - 仅测试了息肉、皮肤病变、污渍、粪便四种场景，未涉及更广泛的医学影像模态（如 CT、MRI、X光），泛化性需进一步验证。
  - 内部数据集 Stain 和 Faeces 规模较小（500/458 张），可能无法完全代表真实临床分布。
- **潜在偏差**：
  - 掩码生成过程（如尺度变换）可能引入与真实病变分布不一致的偏差，尽管文中表明随机掩码有利于分割，但极端情况下可能生成不合理形态。
  - 在线增强依赖单步采样近似，存在累积误差风险。
- **应用限制**：
  - 方法依赖预训练的 Stable Diffusion V1.5，计算资源需求较高（8×4090），对小团队可能不友好。
  - 噪声一致性损失中的超参数 wc 需在验证集上调优（文中选择1.0），不同数据集可能需要单独调整。
- **未充分讨论**：
  - 当合成数据量增至4倍时性能下降的“长尾问题”仅被提及，未深入分析原因或提出解决策略。
  - 没有对比其他数据增强方法（如 MixUp、CutMix）与生成式增强的组合效果。

（完）
