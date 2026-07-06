---
title: "Medverse: A Universal Model for Full-Resolution 3D Medical Image Segmentation, Transformation and Enhancement"
title_zh: Medverse：全分辨率3D医学图像分割、变换与增强的通用模型
authors: "Jiesi Hu, Jianfeng Cao, Yanwu Yang, Chenfei Ye, Yixuan Zhang, Hanyang Peng, Ting Ma"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/42490/46451"
tags: ["query:medical-seg"]
score: 9.0
evidence: 通用3D医学图像分割、变换与增强模型
tldr: 针对现有医学图像分析模型任务单一、难以泛化的问题，本文提出Medverse通用模型。基于上下文学习范式，Medverse在22个数据集上联合训练，可同时完成3D医学图像分割、变换和增强等任务。实验表明，该模型在多任务上达到了高保真度预测，并具备全局解剖理解能力。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有模型无法同时实现高保真预测和全局解剖理解，缺乏多任务统一模型。
method: 采用上下文学习范式，在22个数据集上训练通用模型，支持分割、变换和增强。
result: 在多种医学图像处理任务上取得了高保真度结果，具备全局解剖理解能力。
conclusion: Medverse展示了上下文学习在统一医学图像分析任务上的巨大潜力。
---

## Abstract
In-context learning (ICL) offers a promising paradigm for universal medical image analysis, enabling models to perform diverse image processing tasks without retraining. However, current ICL models for medical imaging remain limited in two critical aspects: they cannot simultaneously achieve high-fidelity predictions and global anatomical understanding, and there is no unified model trained across diverse medical imaging tasks (e.g., segmentation and enhancement) and anatomical regions. As a result, the full potential of ICL in medical imaging remains underexplored. Thus, we present Medverse, a universal ICL model for 3D medical imaging, trained on 22 datasets covering diverse tasks in universal image segmentation, transformation, and enhancement across multiple organs, imaging modalities, and clinical centers. Medverse employs a next-scale autoregressive in-context learning framework that progressively refines predictions from coarse to fine, generating consistent, full-resolution volumetric outputs and enabling multi-scale anatomical awareness. We further propose a blockwise cross-attention module that facilitates long-range interactions between context and target inputs while preserving computational efficiency through spatial sparsity. Medverse is extensively evaluated on a broad collection of held-out datasets covering previously unseen clinical centers, organs, species, and imaging modalities. Results demonstrate that Medverse substantially outperforms existing ICL baselines and establishes a novel paradigm for in-context learning.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：当前医学图像分析中的上下文学习（ICL）模型存在两个关键局限：一是无法同时实现高保真预测（fine-grained detail）与全局解剖理解（global anatomical awareness）；二是缺乏一个在多种医学图像处理任务（如分割、增强、变换）及多解剖区域上统一训练的3D模型。
- **研究动机**：ICL在自然语言和2D医学图像中已展现出潜力，但3D医学图像的高分辨率导致直接端到端处理不可行，现有方法（滑动窗口、下采样）会破坏解剖连续性和保真度。亟需一个统一的3D ICL模型，能够泛化到未见过的中心、器官、模态和任务。
- **整体含义**：提出Medverse，首个全分辨率3D医学图像通用ICL模型，在多任务联合训练下，通过粗到细的渐进预测和高效跨注意模块，显著提升分割、变换和增强任务的性能，为3D医学图像ICL树立新范式。

## 2. 方法论：核心思想、关键技术细节与流程
- **核心思想**：基于**下一尺度自回归上下文学习（NA-ICL）**框架，使模型在多个分辨率步骤中从粗到细逐步细化预测，同时利用全局语义和局部细节；并引入**块状交叉注意力模块（BAM）**，实现上下文与目标之间的长程交互，同时通过空间稀疏性保持计算效率。
- **关键技术细节**：
  - **NA-ICL框架**：模型包含三个3D U-Net分支（自回归上下文、语义上下文、目标图像），其中自回归上下文分支与语义上下文分支共享权重。每个步骤t，模型接收下采样后的目标图像x(t)和上一级的预测作为自回归上下文A(t-1)，输出当前步预测ˆy(t)。分辨率逐级加倍，直到全分辨率（步骤数T由输入尺寸决定）。对于t≥2，使用滑动窗口处理，每个窗口从上一级对应区域上采样后的预测作为自回归上下文。
  - **BAM模块**：将特征图划分为p×p×p个非重叠块，每个块内计算Query和Key的均值池化，生成B个块级token进行注意力计算，Value保留细粒度信息。复杂度从O((H'W'D')²)降至O(p⁶)，p固定（文中p=4），保证计算稳定。该模块放置在编码器的目标到上下文融合和解码器的上下文到目标融合中。
  - **损失函数**：分割任务使用平滑L1损失的变体；变换和增强任务同时对强度和强度差异使用平滑L1损失。
- **算法流程**（文字描述）：
  1. 根据输入尺寸计算步骤数T。
  2. 对于t=1：将输入下采样到最低分辨率，语义上下文也相应下采样，自回归上下文A(0)为空，模型输出粗预测ˆy(1)。
  3. 对于t=2,...,T：将当前分辨率目标图分成128³块，每个块在该分辨率下处理。从上一级预测中裁剪对应区域并上采样至当前块大小作为自回归上下文。语义上下文也与目标块空间对齐裁剪。模型输出该块细粒度预测。所有块预测拼接成完整体积。
  4. 最终步骤T输出全分辨率预测。

## 3. 实验设计
- **数据集**：共使用27个公开数据集（40362个3D扫描），涵盖T1、T2、FLAIR、MRA、DWI、ADC、PD、CT等多种模态和脑、腹部、前列腺、肺等解剖区域。其中22个用于训练和验证（9:1随机分割），5个作为留出集测试泛化性（未见中心、器官、物种、模态）。
  - 留出集：脑和腹部来自未见中心（PPMI，FLARE22），鼻窦（Nasal Seg）作为未见器官，小鼠（microCT）作为未见物种，PET作为未见模态（ADNI）。
- **基准（Benchmark）**：对比四类方法：
  - **任务专用全监督模型**：3D U-Net、Swin-UNETR（在留出集上训练，作为上界）。
  - **任务专用少样本模型**：同上但仅用4个样本训练。
  - **已有ICL模型**：Painter、SegGPT、UniverSeg、Neuralizer（2D，切片后组合）、Neuroverse3D（3D但仅支持128³输入）。
- **任务类型**：分割（12类解剖结构）、变换（颅骨剥离、模态转换）、增强（偏场校正、高斯噪声去除、椒盐噪声去除、图像修复）。
- **评价指标**：分割用Dice系数（%），变换和增强用PSNR。
- **实验数量**：主实验包括多任务对比（表1、表2）、定性结果（图4、图5）、上下文大小影响（图6）、消融实验（表3）、计算效率分析。

## 4. 资源与算力
- **文中未明确说明训练所用的GPU型号、数量及训练时长**。仅提到推理时单块128³ patch使用8个上下文样本和1个自回归上下文需1.16秒，显存占用9.14GB（固定）。BAM模块计算量极低（2.01e-1 GFLOPs）。但训练细节缺失是本文的不足之一。

## 5. 实验数量与充分性
- **实验数量**：较为充分。包括：
  - 表1：18种分割目标（6个类别在4种泛化场景下）对比。
  - 表2：6种变换/增强任务对比。
  - 图4、5：多组定性可视化。
  - 图6：不同上下文大小（1,2,4,8,16）的影响。
  - 表3：NA-ICL和BAM的消融实验（3组）。
  - 补充材料（未详看但提及有更多定性结果和伪代码）。
- **客观性与公平性**：
  - 任务专用模型在留出集上训练（避免了域偏移），但ICL模型不进行微调，对比基准设置合理。
  - 2D ICL模型按切片处理并重组，可能是劣势；但文中已承认其性能差，且符合实际应用。
  - 可复现性：代码开源、超参数(p=4, m=66, patch=128³)明确。但未报告多次运行的标准差，可能不足。
- **结论**：实验覆盖面广（多种未见分布），消融验证关键组件有效性，证据较充分。但缺乏统计显著性检验。

## 6. 主要结论与发现
- Medverse在分割任务上平均Dice 87.27%，超过第二名Neuroverse3D约6个百分点（81.10%）；在变换和增强任务上平均PSNR 28.30，略超Neuroverse3D（27.08），但显著优于2D ICL模型。
- NA-ICL框架能提供多尺度解剖感知，消除滑动窗口伪影，提升保真度（图5）。
- BAM模块通过块级注意力实现长程交互，比简单拼接带来1-2%的Dice提升和PSNR改善（表3）。
- 模型对上下文数量具有鲁棒性，随着上下文增加性能持续提升。
- 在未见中心、器官、物种、模态上均表现良好，展示强泛化性。

## 7. 优点
- **创新性**：首次提出适用于3D全分辨率的粗到细自回归ICL框架，解决高分辨率输入与ICL的矛盾。
- **效率**：BAM将注意力复杂度从平方级降至常数级（p³固定），推理内存固定9.14GB，支持任意大体积。
- **通用性**：在22个数据集上联合训练分割、变换、增强三种任务，模型统一，单模型多用途。
- **泛化性验证全面**：测试了未见中心、器官、物种、模态，证明跨域迁移能力。
- **开源可用**：代码公开，便于复现和应用。

## 8. 不足与局限
- **训练资源未报告**：未说明GPU型号、数量、训练时长，影响可复现性和能耗评估。
- **缺乏统计显著性**：未报告多次重复实验的标准差或置信区间，结果可靠性存疑。
- **对比方法限制**：3D ICL对比方法少（仅有Neuroverse3D），且Neuroverse3D输出分辨率受限，不公平。缺少与MAE、vox2vec等通用模型对比。
- **任务覆盖面有限**：增强任务（如噪声去除）结果仍低于全监督上界，说明性能差距仍存。
- **数据依赖性**：训练数据主要来自公开数据集，可能存在隐含偏差（如图像质量、人群）。
- **实验充分性**：缺少对NA-ICL不同尺度数量的消融，以及BAM中块大小p、投影维度m的灵敏度分析。
- **应用限制**：模型需要上下文样本，在仅有测试数据无任何标签时无法独立工作。且当前仅支持分割、变换、增强，未涵盖检测、分类等任务。

（完）
