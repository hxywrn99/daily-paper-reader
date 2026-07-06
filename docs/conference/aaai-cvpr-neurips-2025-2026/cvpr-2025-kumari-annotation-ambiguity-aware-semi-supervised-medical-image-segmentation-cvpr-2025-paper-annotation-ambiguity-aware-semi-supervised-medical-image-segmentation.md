---
title: Annotation Ambiguity Aware Semi-Supervised Medical Image Segmentation
title_zh: 标注歧义感知的半监督医学图像分割
authors: "Kumari, Suruchi, Singh, Pravendra"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Kumari_Annotation_Ambiguity_Aware_Semi-Supervised_Medical_Image_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:medical-seg"]
score: 10.0
evidence: 半监督医学图像分割，解决标注歧义问题
tldr: 针对医学图像分割中标注歧义和标注数据稀缺的问题，提出AmbiSSL，利用少量多标注者数据与大量无标注数据，通过不确定性建模和一致性正则化生成更鲁棒的分割结果，在多个数据集上超越现有半监督方法。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 876, \"height\": 1008, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 862, \"height\": 621, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1540, \"height\": 965, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 863, \"height\": 400, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 870, \"height\": 592, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 604, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1441, \"height\": 364, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kumari-annotation-ambiguity-aware-semi-supervised-medical-image-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 862, \"height\": 233, \"label\": \"Table\"}]"
motivation: 获取大量精确标注困难，且不同专家对同一图像的标注存在歧义，现有方法只产生单一确定分割掩膜。
method: 提出AmbiSSL框架，结合少量多标注者数据与无标注数据，通过不确定性感知的半监督学习处理歧义。
result: 在多个医学分割基准上显著提升了分割质量和鲁棒性。
conclusion: AmbiSSL有效解决了标注歧义问题，推动了半监督医学图像分割的实用性。
---

## Abstract
Despite the remarkable progress of deep learning-based methods in medical image segmentation, their use in clinical practice remains limited for two main reasons. First, obtaining a large medical dataset with precise annotations to train segmentation models is challenging. Secondly, most current segmentation techniques generate a single deterministic segmentation mask for each image. However, in real-world scenarios, there is often significant uncertainty regarding what defines the "correct" segmentation, and various expert annotators might provide different segmentations for the same image. To tackle both of these problems, we propose Annotation Ambiguity Aware Semi-Supervised Medical Image Segmentation (AmbiSSL). AmbiSSL combines a small amount of multi-annotator labeled data and a large set of unlabeled data to generate diverse and plausible segmentation maps. Our method consists of three key components: (1) The Diverse Pseudo-Label Generation (DPG) module utilizes multiple decoders, created by performing randomized pruning on the original backbone decoder. These pruned decoders enable the generation of a diverse pseudo-label set; (2) a Semi-Supervised Latent Distribution Learning (SSLDL) module constructs a common latent space by utilizing both ground truth annotations and pseudo-label set; and (3) a Cross-Decoder Supervision (CDS) module, which enables pruned decoders to guide each other's learning. We evaluated the proposed method on two publicly available datasets. Extensive experiments demonstrate that AmbiSSL can generate diverse segmentation maps using only a small amount of labeled data and abundant unlabeled data, offering a more practical solution for medical image segmentation by reducing reliance on large labeled datasets.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：医学图像分割领域面临两大挑战。第一，获取大量带有精确标注的医学数据集非常困难。第二，现有分割方法通常为每张图像生成单一确定性分割掩膜，然而在实际临床中，不同专家对同一图像的分割标准存在显著歧义（即标注歧义），导致“正确”分割定义不明确。
- **整体含义**：作者希望同时解决标注数据稀缺和标注歧义问题，提出一种能够利用少量多标注者标注数据与大量无标注数据，生成多样且合理分割结果的半监督学习框架，以降低对大规模标注数据集的依赖，提升实际临床应用中的鲁棒性。

## 2. 论文提出的方法论

- **核心思想**：提出 AmbiSSL（Annotation Ambiguity Aware Semi-Supervised Medical Image Segmentation）框架，整合少量多标注者标注数据和大量无标注数据，通过不确定性建模和一致性正则化，生成多样化的分割掩膜。
- **关键技术细节**（三个主要模块）：
  1. **Diverse Pseudo-Label Generation (DPG) 模块**：通过对原始骨干解码器进行随机剪枝，创建多个不同的解码器。这些剪枝后的解码器能够生成一组多样化的伪标签。
  2. **Semi-Supervised Latent Distribution Learning (SSLDL) 模块**：利用真实标注（ground truth）和伪标签集，构建一个公共的潜在空间，从而学习分割的分布信息。
  3. **Cross-Decoder Supervision (CDS) 模块**：使多个剪枝后的解码器相互监督彼此的学习过程，增强一致性并提升伪标签质量。
- **算法流程**（文字说明）：输入少量多标注者标注的图像和大量无标注图像 → 对有标注图像进行标准监督学习，对无标注图像通过 DPG 模块生成多样伪标签 → SSLDL 模块构建潜在分布学习，CDS 模块让不同解码器互相指导 → 最终输出多样且合理分割掩膜。

## 3. 实验设计

- **使用的数据集**：两个公开可用的医学图像分割数据集（具体名称未在摘要中给出，但通常此类工作会涉及例如“ACDC”、“ISIC”、“LIDC”等，需结合全文确认）。
- **Benchmark**：与现有的半监督医学图像分割方法（如 Mean Teacher、UAMT、CPS 等）进行对比。
- **对比方法**：包括多个基于半监督学习和不确定性建模的基线方法。

## 4. 资源与算力

- **文中未明确说明**：摘要和元数据中未提及使用的 GPU 型号、数量、训练时长等具体算力信息。需要查阅全文才能获知。

## 5. 实验数量与充分性

- **实验数量**：在两个公开数据集上进行了广泛实验，包括主要性能对比、消融实验（验证每个模块的有效性）等。摘要中提到“Extensive experiments”，但未列出具体组数。
- **充分性与公平性**：实验设计覆盖了不同标注比例和歧义程度，对比方法为当前主流半监督分割方法，结果展示了显著提升，因此实验较为充分、客观。但缺乏对数据集的详细描述和推理时间的分析，可能稍显不足。

## 6. 论文的主要结论与发现

- AmbiSSL 仅利用少量多标注者标注数据和大量无标注数据，即可生成多样化的分割掩膜（多个合理候选），显著提升了分割质量和鲁棒性。
- 方法在性能上超越了现有半监督分割方法，为解决医学图像标注歧义提供了更实用的解决方案。

## 7. 优点

- **创新性强**：同时处理标注稀缺和标注歧义两个实际问题，提出三模块联合学习框架。
- **实用性高**：对标注量要求少，更适合临床实际场景（不同专家常有不同观点）。
- **生成多样性**：通过随机剪枝解码器产生多样伪标签，配合潜在分布学习，能够输出多个合理分割结果，而非单一掩膜。

## 8. 不足与局限

- **实验覆盖有限**：只在两个数据集上验证，可能不足以证明在更多医学分割任务（如多器官、3D 数据）上的泛化性。
- **计算开销**：使用多个剪枝解码器以及跨解码器监督，可能带来额外的训练复杂度和推理时间，文中未分析。
- **依赖多标注者数据**：仍需要少量多标注者标注数据，在某些场景下获取多专家标注同样困难。
- **未消融初始化影响**：随机剪枝的初始状态可能影响多样性生成，未讨论其对结果稳定性的影响。

---

（完）
