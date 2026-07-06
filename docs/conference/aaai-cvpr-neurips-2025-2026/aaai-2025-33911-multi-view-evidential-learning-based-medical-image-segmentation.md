---
title: Multi-view Evidential Learning-based Medical Image Segmentation
title_zh: 基于多视角证据学习的医学图像分割
authors: "Chao Huang, Yushu Shi, Waikeung Wong, Chengliang Liu, Wei Wang, Zhihua Wang, Jie Wen"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33911/36066"
tags: ["query:medical-seg"]
score: 9.0
evidence: 结合传统模型和基础模型的多视角证据学习医学图像分割
tldr: 传统深度学习模型缺乏泛化性，基础模型缺乏领域特异性。本文提出多视图证据学习框架，融合多视角特征的领域特异性和通用知识。通过证据学习整合不同模型优势，在多个医学图像分割数据集上实现更优的准确率和鲁棒性。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 传统模型与基础模型各有局限，如何结合两者优势是挑战。
method: 提出多视角证据学习框架，同时提取领域特异性和通用知识并融合。
result: 在多个数据集上分割性能优于单独使用传统或基础模型。
conclusion: 多视角证据学习有效整合了不同来源的知识，提升了分割泛化性。
---

## Abstract
Medical image segmentation provides useful information about the shape and size of organs, which is beneficial for improving diagnosis, analysis, and treatment. Despite traditional deep learning-based models can extract domain-specific knowledge, they face a generalization bottleneck due to the limited embedded knowledge scope. Vision foundation models have been demonstrated to be effective in extracting generalizable knowledge, but they cannot extract domain-specific knowledge without fine-tuning. In this work, we propose a novel multi-view evidential learning-based framework, which can extract both domain-specific and generalizable knowledge from multi-view features by combining the advantages of traditional and vision foundation models. Specifically, a novel multi-view state space model (MV-SSM) is designed to extract task-related knowledge while removing redundant information within multi-view features. The proposed MV-SSM utilizes Mamba, a state space model, to model cross-view contextual dependencies between domain-specific and generalizable features. Additionally, evidential learning is adopted to quantify the segmentation uncertainty of the model for boundary. In special, variational Dirichlet is introduced to characterize the distribution of the result probabilities, parameterized with collected evidence to quantify uncertainty. As a result, the model can reduce the segmentation uncertainties of boundaries by optimizing the parameters of the Dirichlet distribution. Experimental results on three datasets show that our method obtains superior segmentation performance.

---

## 论文详细总结（自动生成）

# 基于多视角证据学习的医学图像分割：论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：医学图像分割中，传统深度学习模型（如U-Net及其变体）能提取领域特定（domain-specific）知识，但受限于训练数据规模，泛化能力不足；视觉基础模型（如SAM）虽能提取通用（generalizable）知识，但直接用于医学图像时因域差异无法提取领域特定知识，微调又需大量高质量标注数据。如何结合两者优势，同时获取领域特定与通用知识，是当前面临的主要挑战。
- **整体含义**：本文提出一种新颖的多视角证据学习框架，通过融合传统模型与视觉基础模型的多视角特征，提取任务相关知识，并利用证据学习量化边界分割不确定性，从而提升分割精度与鲁棒性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用双编码器架构——SAM图像编码器（冻结）提取通用知识，领域特定编码器提取领域知识；设计多视角状态空间模型（MV-SSM）建模跨视角上下文依赖，去除冗余信息；引入证据学习，用变分狄利克雷分布参数化分割概率分布，量化边界不确定性，通过优化分布参数降低不确定性。

- **关键技术细节**：
  - **多视角状态空间模型（MV-SSM）**：包含多个Pixel-Window Channel Mamba（PWC-Mamba）模块。PWC-Mamba同时建模像素级（局部）和窗口级（全局）信息，采用双向Mamba（BiMam）避免早期输入信息丢失，并引入窗口通道注意力机制（WCA）增强通道间依赖。
  - **Mamba Residual Cross-Fusion Transformer（Mamba Residual CCT）**：在领域特定编码器中，改进CCT融合模块，加入1×1卷积残差连接保留原始信息，并用PWC-Mamba替代单尺度特征提取，实现多尺度全局/局部信息建模，丰富领域知识。
  - **证据学习驱动的不确定性减少**：解码器后使用Softplus激活输出非负证据$E$，构建狄利克雷分布$D(p|\alpha)$（$\alpha = e + 1$）。采用主观逻辑（Subjective Logic）将证据映射为信念质量和不确定性，并设计损失函数优化狄利克雷参数：
    - 交叉熵损失的积分形式：$L_{ice} = \sum_c y_c (\psi(\sum \alpha) - \psi(\alpha_c))$；
    - KL散度损失$L_{KL}$：约束错误类别证据趋近于0；
    - 总损失：$L = L_{Dice} + L_{ice} + \lambda_t L_{KL}$，$\lambda_t$从较小值逐渐增大，防止早期过度正则化。

## 3. 实验设计

- **数据集与场景**：四个医学图像分割数据集：
  - **MoNuSeg**（显微组织细胞核分割，30张图像）
  - **GlaS**（结肠腺分割，85训练/80测试）
  - **TNBC**（三阴性乳腺癌细胞核分割，50张图像）
  - **DRIVE**（视网膜血管分割，20训练/20测试）
- **评价指标**：Dice系数和IoU（mIoU）。
- **对比方法**：涵盖传统领域特定模型（U-Net、UNet++、AttUNet、Swin-UNet、TransUNet、UCTransNet、TGANet等）、基础模型（SAM、MedSAM、VMUNet、VMUNet v2）以及混合方法（LViT-T等），共十余种SOTA方法。

## 4. 资源与算力

- **明确说明**：所有实验在单张NVIDIA A100 GPU（80GB显存）上进行。
- **未提及**：训练时长、批大小（设为2）、优化器（AdamW，余弦退火学习率1e-3）等已给出，但未报告具体训练迭代次数或总时间。

## 5. 实验数量与充分性

- **实验组数**：
  - 主实验：在4个数据集上对比11+种方法，报告Dice和IoU。
  - 消融实验：在MoNuSeg和GlaS两个数据集上对三个核心组件（不确定性模块、Mamba Residual CCT、MV-SSM）进行逐一消融，共4组（基线+逐步添加组件）。
  - 定性可视化：展示部分分割结果图（图2）。
- **充分性评价**：
  - **优点**：对比方法全面，覆盖主流传统模型和基础模型；消融实验清晰展示了各组件的贡献；数据集涵盖细胞核、腺体、血管等不同任务，有一定多样性。
  - **不足**：消融实验仅在两个数据集上进行，未报告在其他数据集的消融结果；未进行参数敏感性分析（如$\lambda_t$的选择）；未与其他多视角方法（如Liu et al. 2024a）详细比较（但论文在Related Work中提到了它们，实验表未直接列出）；未进行统计显著性检验（如配对t检验）。

## 6. 主要结论与发现

- 所提出的多视角证据学习框架在所有四个数据集上均取得最优或次优的Dice和IoU，显著优于单独使用传统模型或基础模型的方法。
- MV-SSM能有效融合领域特定与通用特征，提取任务相关知识，带来最大性能提升（在MoNuSeg上Dice提升1.66%，IoU提升2.28%）。
- 证据学习模块通过优化狄利克雷分布参数降低边界分割不确定性，进一步小幅但一致地提升性能。
- 定性结果表明该方法在边界分割上更准确。

## 7. 优点

- **创新性**：首次将多视角学习、状态空间模型（Mamba）与证据学习结合，同时解决泛化瓶颈和边界不确定性两个问题。
- **方法论严谨**：每个模块设计有明确动机，双向Mamba缓解信息丢失，窗口通道注意力兼顾局部与全局，证据学习采用变分狄利克雷并与主观逻辑联系严密。
- **实验扎实**：对比方法数量多，数据集覆盖不同模态，消融实验验证各组件有效性。
- **性能领先**：在多个数据集上Dice和IoU均达新SOTA，例如MoNuSeg Dice 81.88%，GlaS Dice 90.59%，TNBC Dice 83.75%，DRIVE Dice 84.12%。

## 8. 不足与局限

- **计算资源未详尽**：虽给出GPU型号，但未报告训练时长或收敛速度，不方便复现和比较效率。
- **消融实验范围有限**：仅在两个数据集上验证，且未包含所有组合（如单独使用MV-SSM或单独使用不确定性模块的完整结果）；未提供超参数$\lambda_t$的调优过程或敏感性分析。
- **数据集规模较小**：所用数据集图像数量均较少（最大GlaS 165张），模型在更大规模、更多样化数据集上的泛化能力未知。
- **未与其他多视角方法直接对比**：论文提及Liu et al. 2024a等，但实验表中未列出其性能，公平性略受影响（虽然可能因这些方法未在相同数据集上报告结果）。
- **应用限制**：依赖SAM作为基础模型，SAM的推理速度可能成为瓶颈；证据学习模块引入额外复杂度，实际部署时需考虑推理速度与内存消耗。
- **偏差风险**：所有数据集均为公开医学图像，可能不含伪影、模糊或罕见病例，模型对边界不确定性的处理在真实临床场景中的表现有待验证。

（完）
