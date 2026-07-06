---
title: "EccoMamba: Enhanced Cross-hierarchical Continuity Orthogonal Mamba for Medical Image Segmentation"
title_zh: EccoMamba：增强跨层次连续性正交Mamba用于医学图像分割
authors: "Junlin Xu, Jincan Li, Feifei Cui, Zhuang Zhang, Jialiang Yang, Shuting Jin, Qiangguo Jin, Yajie Meng"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38109/42071"
tags: ["query:medical-seg"]
score: 10.0
evidence: 增强Mamba架构的医学图像分割
tldr: 针对现有Mamba架构在医学图像分割中无法捕捉层次化解剖特征的问题，提出EccoMamba，一种U型编解码器框架，引入层次聚合增强模块，通过跨层次连续性正交建模提升对复杂结构的表示能力，在多个数据集上取得优异性能。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有Mamba架构依赖固定方向序列建模，难以捕捉层次化解剖特征和空间依赖，限制了复杂医学结构的分割能力。
method: 提出U型编解码器EccoMamba，在编码器下采样路径中引入层次聚合增强模块，实现跨层次连续性正交建模。
result: 在多个医学图像分割基准上超越现有Mamba和卷积方法。
conclusion: EccoMamba通过层次化连续正交建模有效提升了医学图像分割的精度和鲁棒性。
---

## Abstract
Medical image segmentation plays a crucial role in clinical diagnosis, lesion quantification, and preoperative planning. However, existing Mamba-based architectures, which rely on fixed-direction sequence modeling and flatten images into one-dimensional (1D) sequences, struggle to capture hierarchical anatomical features and spatial dependencies, thereby limiting their representational capacity for complex medical structures. To address these limitations, we propose EccoMamba (Enhanced Cross-hierarchical Continuity Orthogonal Mamba), a U-shaped encoder--decoder framework designed for medical image segmentation. In the encoder's downsampling path, we introduce a Hierarchical Aggregation Enhancement (HAE) module that integrates multi-scale convolutions with hierarchical attention mechanisms. The attention branch further incorporates cross-channel interactions, allowing the model to selectively enhance semantically relevant features while suppressing irrelevant background responses. For skip connections, we design a Structural Continuity Orthogonal (SCO) module to preserve spatial continuity by modeling cross-dimensional dependencies via orthogonal Axial Shifts (AS), thereby mitigating directional bias and improving anatomical consistency. Extensive experiments on four benchmark datasets---ISIC 2018, ISIC 2017, Synapse, and ACDC---show that EccoMamba consistently outperforms state-of-the-art methods in both segmentation accuracy and structural fidelity.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：医学图像分割对临床诊断、病灶量化、术前规划至关重要。现有基于 Mamba 的架构虽然具有线性计算复杂度，但存在三个关键缺陷：
  - 将图像展平为一维序列，破坏了医学图像固有的二维/三维空间结构，难以捕捉精细的解剖细节；
  - 单向序列处理引入方向偏差，无法对称感知水平和垂直结构；
  - 缺乏显式多尺度机制，限制了层次化解剖特征建模能力，导致在复杂解剖区域分割性能不佳。
- **整体意义**：提出更适应医学图像空间连续性和多尺度特性的 Mamba 变体，提升分割精度和结构保真度，为基于状态空间模型的医学图像分析提供新思路。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 构建 U 型编解码器框架 EccoMamba，在保留 Mamba 高效长程建模能力的同时，增强局部空间连续性、多尺度解剖结构建模和正交方向依赖关系。

### 关键技术细节
- **层次聚合增强（HAE）模块**（置于编码器下采样路径）：
  - 将输入特征沿通道均分为两部分；
  - 分支一（多尺度卷积）：一路为 1×1 卷积 + 深度可分离 3×3 卷积 + GELU；另一路为 3×3 最大池化 + 1×1 卷积 + GELU，两路径输出融合；
  - 分支二（层次注意力）：先通过通道注意力（全局平均池化与最大池化经两层 MLP 生成权重），再通过空间注意力（通道维度池化 + 7×7 卷积生成权重）；
  - 最终拼接两分支并加入残差连接。
- **结构连续性正交（SCO）模块**（置于跳跃连接）：
  - 将输入特征沿通道均分，分别经 1×1 和 3×3 卷积分支；
  - 每个分支内执行**轴向移位（AS）**：特征分别沿水平、垂直方向移位（零填充保持尺寸），经卷积后相加，再经 1×1 卷积融合；
  - 两分支输出相加后经过层归一化、MLP 和第二个残差连接。
- **损失函数**：联合 Dice 损失和交叉熵损失，平衡优化类别重叠与像素级分类。

## 3. 实验设计

### 数据集
- **ISIC 2018**：皮肤病变分割，2694 张图像，按 7:3 划分（1886 训练/808 测试）。
- **ISIC 2017**：皮肤病变分割，2150 张图像，按官方协议（1500 训练/650 测试）。
- **Synapse**：多器官 CT 分割，8 个器官，30 个受试者（18 训练/12 测试）。
- **ACDC**：心脏 MRI 分割，右心室、左心室、心肌，100 例（70 训练/10 验证/20 测试）。

### Benchmark 方法
- **U-Net 类**：MALUNet, Attention Swin U-Net, ASP-VM-UNet, MSCD-VM-UNet, LeViT-UNet-384, UNetR, TransClaw U-Net。
- **Transformer 类**：TransUNet, Swin-UNet, MISSFormer, SynergyNet, MixFormer, HiFormer-L, CSWin-UNet, GLoG-CSUNet。
- **Mamba 类**：VM-UNet, HC-Mamba, H-VmUNet, CC-ViM, UltraLight VM-UNet, LightM-UNet。

### 评估指标
- ISIC 数据集：敏感性、特异性、准确率、Dice 系数。
- Synapse 和 ACDC：Dice 系数、95% Hausdorff 距离。

## 4. 资源与算力

- **GPU**：单张 NVIDIA RTX 4090。
- **优化器**：AdamW，初始学习率 1×10⁻³，权重衰减 1×10⁻²，余弦学习率衰减。
- **训练配置**：batch size 统一为 24，所有数据集训练 300 个 epoch，选择验证集 Dice 最高的 checkpoint。
- **加速**：采用混合精度训练（AMP）。
- **推理**：Synapse 数据集使用滑动窗口策略减少边界伪影。

## 5. 实验数量与充分性

- **主要实验**：四个数据集各一张主表（表1-3），覆盖皮肤病变、心脏、多器官 CT 等多个场景。
- **消融实验**：表4 在 ISIC 2018 上分别验证 HAE 和 SCO 的单独及联合贡献，共 4 组。
- **模块放置分析**：图5 对比 HAE 放在编码器、解码器或两者皆放的效果，共 3 组。
- **效率对比**：表5 对比参数、FLOPs、内存共 12 种方法。
- **可视化**：图2-4 展示 ISIC 2017、Synapse、ACDC 的定性结果。
- **统计检验**：Synapse 上对 Dice 分数进行配对 t 检验，p<0.01。
- **充分性评价**：实验覆盖多种模态（皮肤镜、CT、MRI）、多种器官、多种 SOTA 方法（CNN、Transformer、Mamba），消融与放置分析齐全，统计检验增强可信度。实验设计客观公平，均在同一设置下复现对比。

## 6. 主要结论与发现

- EccoMamba 在四个数据集上均取得最优或最先进的性能：
  - ISIC 2018 DSC 91.18%（比次优高 2.20pp）；
  - ISIC 2017 DSC 89.39%，准确率 96.84%；
  - ACDC DSC 91.83%；
  - Synapse 平均 DSC 81.51%，HD95 22.02，8 个器官中多数获得最佳或次佳。
- HAE 和 SCO 模块具有互补优势：HAE 提升多尺度上下文（DSC +1.9pp），SCO 增强空间连续性（DSC +1.7pp），两者联合效果最佳。
- HAE 放置在编码器阶段效果优于解码器或双向放置。
- EccoMamba 在参数/FLOPs/内存方面与 heavy 模型（nnFormer、TransUNet）相比更高效，虽比 ultra-light 模型参数多但精度明显更高。

## 7. 优点（方法或实验设计亮点）

- **方法创新性**：
  - HAE 模块将多尺度卷积与层次化注意力（通道+空间）结合，兼顾局部细节与全局语义。
  - SCO 模块通过正交轴向移位模拟跨维度空间依赖，有效克服 Mamba 的方向偏差。
- **设计简洁有效**：两个模块均轻量，不增加过多计算开销。
- **实验全面**：涵盖多模态、多器官、多尺度评估，与 CNN、Transformer、Mamba 三大类方法公平对比。
- **统计显著性验证**：配对 t 检验证明改进非随机。
- **可视化清晰**：定性结果直观展示边界精细度和结构连续性优势。

## 8. 不足与局限

- **未在更大规模或 3D 数据上验证**：仅针对 2D 图像（皮肤镜、CT 切片、MRI 切片），未在完整 3D 体素（如 3D CT/MRI 体数据）上验证，SCO 模块是否天然适用于 3D 有待扩展。
- **计算复杂度中等**：参数量 38.30M、FLOPs 6.22G、内存 0.63GB，与 ultra-light 模型相比仍有差距，在资源极度受限设备上部署可能受限。
- **未讨论潜在失败案例**：未分析在哪些解剖区域或噪声条件下性能下降。
- **未进行跨数据集泛化测试**：仅在单个数据集内评测，未验证学到的表示能否直接迁移到未知分布。
- **超参数调优细节有限**：损失函数中 λ_ce 和 λ_dice 具体值未明确给出，可能影响复现。
- **单卡训练**：未探究多卡分布式训练对大模型扩展的效果。

（完）
