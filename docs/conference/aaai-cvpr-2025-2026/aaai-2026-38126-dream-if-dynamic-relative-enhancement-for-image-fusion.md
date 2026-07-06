---
title: "Dream-IF: Dynamic Relative EnhAnceMent for Image Fusion"
title_zh: Dream-IF：图像融合的动态相对增强
authors: "Xingxin Xu, Bing Cao, Dongdong Li, Qinghua Hu, Pengfei Zhu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38126/42088"
tags: ["query:image-fusion"]
score: 9.0
evidence: 动态相对增强用于图像融合
tldr: 针对传统图像融合将增强与融合分离导致次优的问题，提出动态相对增强框架Dream-IF。该框架量化各模态相对主导区域，联合优化增强与融合过程。在多种退化场景下的实验表明，Dream-IF在融合质量和增强效果上均优于分步方法，实现更鲁棒的融合结果。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法将图像增强与融合独立处理，忽略了主导区域指示的增强机会，导致融合质量受损。
method: 提出Dream-IF框架，通过量化各模态相对主导性，动态调整增强策略并与融合联合优化。
result: 在多种退化条件下，Dream-IF在融合质量和增强效果上均优于传统分步方法。
conclusion: 联合优化增强与融合可有效提升图像融合的鲁棒性和质量。
---

## Abstract
Image fusion aims to integrate comprehensive information from images acquired through multiple sources. However, images captured by diverse sensors often encounter various degradations that can negatively affect fusion quality. Traditional fusion methods generally treat image enhancement and fusion as separate processes, overlooking the inherent correlation between them; notably, the dominant regions in one modality of a fused image often indicate areas where the other modality might benefit from enhancement. Inspired by this observation, we introduce the concept of dominant regions for image enhancement and present a Dynamic Relative EnhAnceMent framework for Image Fusion (Dream-IF). This framework quantifies the relative dominance of each modality across different layers and leverages this information to facilitate reciprocal cross-modal enhancement. By integrating the relative dominance derived from image fusion, our approach supports not only image restoration but also a broader range of image enhancement applications. Furthermore, we employ prompt-based encoding to capture degradation-specific details, which dynamically steer the restoration process and promote coordinated enhancement in both multi-modal image fusion and image enhancement scenarios. Extensive experimental results demonstrate that Dream-IF consistently outperforms its counterparts.

---

## 论文详细总结（自动生成）

# 论文《Dream-IF: Dynamic Relative EnhAnceMent for Image Fusion》详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：多模态图像融合（如可见光与红外）中，传感器捕获的图像常受多种退化（如噪声、模糊、雨雾等）影响，导致融合质量下降。传统方法通常将图像增强（恢复）和融合视为独立的两阶段流程，先恢复再融合，忽略了二者之间的内在相关性。
- **关键观察**：在一个模态中占主导的区域（如红外图像的强热源区域）往往对应另一个模态（如可见光）中需要增强的薄弱区域。这种互补性为联合优化提供了机会。
- **目标**：利用融合过程中天然存在的模态相对主导性，同时提升增强和融合的效果，实现更鲁棒的图像融合。

## 2. 方法论
- **核心思想**：提出动态相对增强框架（Dream-IF），在融合网络中嵌入相对增强（Relative Enhancement, RE）模块。RE模块包含两部分：
  - **跨模态增强（Cross Enhancement, CE）**：计算每个模态特征的相对主导性（Relative Dominance, RD）——通过激活函数对特征进行加权，并约束RD之和接近1。利用一个模态的RD指导另一个模态中对应薄弱区域的增强，实现交叉互补。
  - **自增强（Self Enhancement, SE）**：基于RD生成提示（Relative Dominance Prompt, RDP），动态引导自身特征的恢复，使模型对多种退化具有盲恢复能力。
- **网络结构**：采用U-Net型编码器-解码器，编码器和解码器由Restormer块组成（4级对称结构）。在解码器层中引入RE模块，增强后的特征再送入融合块（Restormer+卷积）生成最终融合图像。
- **损失函数**：融合损失 \(L_f\) 包括像素损失、梯度损失、SSIM损失和颜色损失；额外增加相对主导性损失 \(L_{RD}\)，约束各模态RD之和为1。总损失 \(L = L_f + L_{RD}\)。
- **训练方式**：在训练时对干净图像施加随机退化（高斯噪声、泊松噪声、散斑噪声等），模型无需知道退化类型（盲恢复）。

## 3. 实验设计
- **数据集**：LLVIP（12,025对训练，70对测试）、MSRS、M3FD。在LLVIP上训练并测试，在MSRS和M3FD上零样本测试泛化能力。
- **退化设置**：包括高斯噪声（σ=35）、泊松噪声（λ∈[2,4]）、散斑噪声（ε∈[2,25]）、模糊、缩放、下雨等。每张训练图随机施加一种或多种退化。
- **对比方法**：TarDAL、CDDFuse、EMMA、Text-IF、TTD、Text-DiFuse、FusionBooster。在比较有退化场景时，采用了两种策略：
  - 两阶段融合：先用SCUNet恢复，再用对比方法融合。
  - 单阶段融合：直接与Text-IF（需给出退化文本描述）和Text-DiFuse对比。
- **评价指标**：AG（平均梯度）、SF（空间频率）、Qabf（梯度相似性）、SSIM、VIFF（视觉信息保真度），以及用于下游检测的Recall、mAP。
- **消融实验**：在LLVIP上依次去掉CE、SE模块，验证各模块贡献。

## 4. 资源与算力
- 文中明确提到：实验框架使用PyTorch，在**NVIDIA 3090 GPU**上执行。但**未提及具体使用了几张GPU、训练时长**等详细资源信息。

## 5. 实验数量与充分性
- **实验数量**：涵盖了3个数据集（共约1.2万训练对）、7种主流对比方法、5类评价指标、多种退化类型（6种以上）、消融实验（4组消融）、下游目标检测任务（YOLOv11）定量比较。
- **充分性与公平性**：
  - 定性图例丰富（多数据集多场景），定量表格详尽。
  - 在有退化场景中，两阶段对比时统一先使用SCUNet恢复，公平性较好。
  - 消融实验逐模块验证，逻辑清晰。
  - 但与部分方法（如Text-IF）的单阶段对比中，Text-IF需要退化文本提示而Dream-IF无需，条件略有不同，但论文指出这反而体现了Dream-IF的盲恢复优势。

## 6. 主要结论与发现
- Dream-IF在**无退化**情况下，在LLVIP数据集上获得AG（5.926）、SF（19.992）、Qabf（0.679）、SSIM（1.198）等多项指标最优，且VIFF（0.800）与最佳方法（Text-IF）接近。
- **有退化**（两阶段）条件下，Dream-IF在所有指标上均超越其他方法（AG=5.243, SF=17.441, Qabf=0.580, VIFF=0.705）。
- **单阶段盲融合**条件下，Dream-IF在多种退化类型上的LPIPS分数均低于Text-IF和Text-DiFuse，证明其盲恢复能力更强。
- **下游任务**：YOLOv11检测结果显示，Dream-IF的融合结果在Recall、mAP@.5和mAP@.5:.95上均最优。
- 消融证明：CE和SE模块均带来性能提升，联合使用效果最佳。

## 7. 优点
- **创新性**：首次从图像融合的互补性出发，利用相对主导性来引导增强，打破了增强与融合分离的范式。
- **盲恢复能力**：无需知道退化类型，即可同时完成恢复与融合，实用性强。
- **动态适应**：通过prompt-based编码动态生成增强提示，适应不同退化。
- **泛化性**：在未训练的MSRS和M3FD数据集上仍表现优异。
- **下游兼容性**：融合结果显著提升目标检测性能，验证了高层应用价值。
- **实验设计全面**：涵盖无退化、有退化、多数据集、多种退化类型、单/两阶段、消融、下游任务，证据链完整。

## 8. 不足与局限
- **算力细节缺失**：未提供训练耗时、GPU数量等关键资源信息，难以评估方法实际部署成本。
- **退化类型有限**：虽模拟了多种退化，但未覆盖真实场景中的混合退化或未知退化（如运动模糊、JPEG压缩等）。盲恢复的有效性仍有待更复杂的退化验证。
- **模型复杂度讨论不足**：未给出参数量、推理速度等分析，难以评估计算开销。
- **依赖良好配准**：多模态图像融合通常需要精确配准，该框架未明确讨论配准误差对性能的影响。
- **单阶段对比局限**：与Text-IF的单阶段对比中，Text-IF需文本提示，Dream-IF无需，条件不完全对等，虽然作者将此作为优势，但公平性稍有折扣。
- **超参数敏感性**：损失函数中各项权重、RD约束的严格性等未进行系统讨论，可能存在调参偏差。

（完）
