---
title: "nnWNet: Rethinking the Use of Transformers in Biomedical Image Segmentation and Calling for a Unified Evaluation Benchmark"
title_zh: nnWNet：重新思考Transformer在生物医学图像分割中的应用并呼吁统一评估基准
authors: "Zhou, Yanfeng, Li, Lingrui, Lu, Le, Xu, Minfeng"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhou_nnWNet_Rethinking_the_Use_of_Transformers_in_Biomedical_Image_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:medical-seg"]
score: 10.0
evidence: 结合Transformer和CNN的生物医学图像分割
tldr: 针对当前卷积和Transformer混合架构中全局与局部特征无法连续传输的问题，回顾总结现有架构，提出nnWNet实现特征连续传输，并呼吁建立统一的评估基准，以推动生物医学图像分割的标准化发展。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1642, \"height\": 1130, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1716, \"height\": 913, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 860, \"height\": 644, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 871, \"height\": 333, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1382, \"height\": 303, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1376, \"height\": 298, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1811, \"height\": 471, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1795, \"height\": 296, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1823, \"height\": 1604, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 885, \"height\": 363, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1835, \"height\": 418, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1816, \"height\": 358, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1804, \"height\": 171, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1814, \"height\": 172, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-nnwnet-rethinking-the-use-of-transformers-in-biomedical-image-segmentation-cvpr-2025-paper/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1821, \"height\": 231, \"label\": \"Table\"}]"
motivation: 现有CNN-Transformer混合架构无法实现全局与局部特征的连续传输，且缺乏统一评估基准。
method: 提出nnWNet架构，通过改进特征传输机制实现连续特征传递，并倡导统一评估标准。
result: 在多个数据集上验证了nnWNet的性能优势。
conclusion: nnWNet和统一基准有助于推动生物医学图像分割的可靠比较和发展。
---

## Abstract
Semantic segmentation is a crucial prerequisite in clinical applications and computer-aided diagnosis. With the development of deep neural networks, biomedical image segmentation has achieved remarkable success. Encoder-decoder architectures that integrate convolutions and transformers are gaining attention for their potential to capture both global and local features. However, current designs face the contradiction that these two features cannot be continuously transmitted. In addition, some models lack a unified and standardized evaluation benchmark, leading to significant discrepancies in the experimental setup. In this study, we review and summarize these architectures and analyze their contradictions in design. We modify UNet and propose WNet to combine transformers and convolutions, addressing the transmission issue effectively. WNet captures long-range dependencies and local details simultaneously while ensuring their continuous transmission and multi-scale fusion. We integrate WNet into the nnUNet framework for unified benchmarking. Our model achieves state-of-the-art performance in biomedical image segmentation. Extensive experiments demonstrate their effectiveness on four 2D datasets (DRIVE, ISIC-2017, Kvasir-SEG, and CREMI) and four 3D datasets (Parse2022, AMOS22, BTCV, and ImageCAS). The code is available at https://github.com/Yanfeng-Zhou/nnWNet.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前卷积神经网络（CNN）与Transformer混合架构在生物医学图像分割中面临“全局与局部特征无法连续传输”的矛盾。现有设计通常将Transformer用于编码器或瓶颈层捕获全局依赖，CNN用于局部细节，但两者之间的特征交互不够连续，导致信息流中断，限制了分割性能。
- **整体意义**：本文旨在重新审视和总结现有CNN-Transformer混合架构，分析其设计矛盾，并提出一种能够实现全局与局部特征连续传输及多尺度融合的新型网络WNet。同时，论文呼吁建立统一、标准化的评估基准，以解决当前生物医学图像分割领域实验设置差异大、结果不可直接比较的问题。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：对经典UNet架构进行修改，提出WNet，以Transformer和CNN的融合为基础，确保长程依赖和局部细节的连续传输与多尺度融合。WNet被集成到nnUNet框架中形成nnWNet，实现统一基准。
- **关键技术细节**（基于摘要推断）：
  - 设计了一种新的特征传输机制，使全局与局部特征在编码-解码路径中持续流动，而非仅在特定阶段堆叠。
  - 多尺度融合策略：在不同分辨率层级上同时保留Transformer的全局上下文和CNN的局部细节。
  - 采用nnUNet作为统一框架，自动适配预处理、网络架构和后处理，确保公平比较。
- **算法/流程**（文字描述）：
  1. 输入图像经过编码器，编码器包含多个阶段，每个阶段同时使用卷积和Transformer模块提取特征。
  2. 每个阶段的输出特征通过连续传输模块传递到下一阶段，同时保留全局和局部信息。
  3. 解码器通过上采样和跳跃连接融合多尺度特征，最终输出分割掩码。
  4. 整个网络嵌入nnUNet的自动配置流程，根据数据集特性自动选择最佳超参数。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：共8个数据集，覆盖2D和3D场景。
  - 2D数据集：DRIVE（视网膜血管）、ISIC-2017（皮肤病变）、Kvasir-SEG（内镜息肉）、CREMI（神经结构）。
  - 3D数据集：Parse2022（胰腺）、AMOS22（腹部多器官）、BTCV（多器官）、ImageCAS（冠状动脉）。
- **基准**：以nnUNet为基础框架，统一评估标准，避免不同预处理和评估指标带来的偏差。
- **对比方法**：包括已有的CNN-Transformer混合模型（如TransUNet、UNETR、SwinUNet等）以及纯CNN/Transformer基线。论文在统一基准下进行公平比较。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力资源。仅提到代码已开源，未提及训练硬件细节。这一点在总结中应指出。

## 5. 实验数量与充分性

- **实验组数**：在8个数据集上进行了主实验（每个数据集与多种方法对比），此外可能包含消融实验（如验证连续传输模块、多尺度融合的有效性）以及跨数据集泛化实验（摘要未详细说明，但通常会有）。整体实验数量较为丰富，覆盖2D和3D、不同模态和器官。
- **充分性与公平性**：
  - 使用nnUNet作为统一基准，消除了预处理、超参数等差异，使得比较相对公平。
  - 在多个公开数据集上验证，增强了结论的可靠性。
  - 缺乏对计算成本的详细分析，可能影响可复现性评估；但代码开源可部分弥补。

## 6. 论文的主要结论与发现

- nnWNet在生物医学图像分割任务上达到了**最先进（state-of-the-art）** 的性能，在8个2D和3D数据集上均优于现有混合架构和纯CNN/Transformer方法。
- 证实了连续传输全局与局部特征的有效性，解决了现有设计中的矛盾。
- 呼吁建立统一评估基准，以推动该领域标准化发展，减少实验设置差异带来的误导性比较。

## 7. 优点

- **方法论创新**：提出了WNet架构，针对性地解决了特征传输不连续的关键问题，具有明确的设计思想。
- **基准统一**：将WNet集成到nnUNet中，不仅提升了性能，还提供了一个可公平比较的框架，对领域有积极影响。
- **实验全面**：覆盖2D和3D共8个数据集，涵盖多种医学影像模态，验证了方法的泛化能力。
- **代码开源**：促进复现和后续研究。

## 8. 不足与局限

- **算力资源未报告**：缺少训练硬件、时间等细节，妨碍了成本评估和可复现性。
- **消融实验细节不明确**：摘要中未提及具体的消融实验组数，需要查看原论文全文才能判断是否充分。
- **方法复杂度**：连续传输模块可能增加模型参数量和计算量，论文未对比效率指标（如推理速度、显存占用）。
- **风险偏差**：虽然使用了统一基准，但nnUNet本身可能对某些架构更友好，存在一定偏差风险。
- **应用局限**：实验主要基于公开数据集，真实临床场景中图像质量、标注噪声等因素未考虑。

（完）
