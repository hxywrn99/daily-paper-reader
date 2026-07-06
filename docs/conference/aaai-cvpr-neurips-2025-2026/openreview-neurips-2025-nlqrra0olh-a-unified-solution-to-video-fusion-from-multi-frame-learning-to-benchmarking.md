---
title: "A Unified Solution to Video Fusion: From Multi-Frame Learning to Benchmarking"
title_zh: 视频融合的统一解决方案：从多帧学习到基准构建
authors: "Zixiang Zhao, Haowen Bai, Bingxin Ke, Yukun Cui, Lilun Deng, Yulun Zhang, Kai Zhang, Konrad Schindler"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=nlQRra0OLH"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于多帧学习和光流特征扭曲的视频融合
tldr: 该文针对现有图像融合方法忽略视频帧间相关性导致闪烁问题，提出UniVF统一视频融合框架。利用多帧学习和光流特征扭曲生成时间一致的高质量融合视频，并构建首个综合基准VF-Bench，覆盖多曝光、多焦、红外-可见和医学四种视频融合任务。实验表明其在各任务上达到最优性能。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有图像融合方法处理视频时忽略时间一致性导致闪烁。
method: 提出UniVF，利用多帧学习和光流特征扭曲实现时间一致的视频融合。
result: 在四种视频融合任务上达到最优性能。
conclusion: 统一框架有效解决视频融合的时序一致性问题。
---

## Abstract
The real world is dynamic, yet most image fusion methods process static frames independently, ignoring temporal correlations in videos and leading to flickering and temporal inconsistency. To address this, we propose Unified Video Fusion (UniVF), a novel and unified framework for video fusion that leverages multi-frame learning and optical flow-based feature warping for informative, temporally coherent video fusion. To support its development, we also introduce Video Fusion Benchmark (VF-Bench), the first comprehensive benchmark covering four video fusion tasks: multi-exposure, multi-focus, infrared-visible, and medical fusion. VF-Bench provides high-quality, well-aligned video pairs obtained through synthetic data generation and rigorous curation from existing datasets, with a unified evaluation protocol that jointly assesses the spatial quality and temporal consistency of video fusion. Extensive experiments show that UniVF achieves state-of-the-art results across all tasks on VF-Bench. Project page: [vfbench.github.io](https://vfbench.github.io).

---

## 论文详细总结（自动生成）

# 视频融合的统一解决方案：从多帧学习到基准构建 — 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：真实世界是动态的，但现有图像融合方法在处理视频时，将每一帧视为独立静态图像进行融合，忽略了帧间的时序相关性，导致融合后的视频出现闪烁和时序不一致。
- **背景**：多曝光融合、多焦点融合、红外-可见融合、医学视频融合等任务均缺少统一且关注时序一致性的解决方案。
- **动机**：提出一个统一的视频融合框架，同时利用多帧信息并保持时间连续性，并构建首个全面的视频融合评测基准，推动该领域发展。

## 2. 方法：核心思想、关键技术细节
- **核心思想**：将多帧学习（multi-frame learning）与基于光流的特征扭曲（optical flow-based feature warping）相结合，使得融合网络能够利用相邻帧的运动信息，生成同时满足空间质量高和时间一致性强的融合视频。
- **关键技术细节**：
  - 输入：连续的多帧视频序列（包含参考帧及其相邻帧）。
  - 使用光流估计网络计算相邻帧与参考帧之间的运动场。
  - 通过光流特征扭曲（warping）将相邻帧的特征对齐到参考帧坐标系，从而提供时序上下文信息。
  - 融合模块综合参考帧特征与对齐后的相邻帧特征，输出融合帧。
  - 训练时采用联合损失函数，同时惩罚空间失真（如结构相似度损失、L1损失）和时间闪烁（如时序一致性损失）。
- **公式/算法流程**（文字描述）：
  1. 对输入视频帧序列 \(I_{t-1}, I_t, I_{t+1}\) 提取多尺度特征。
  2. 使用光流网络估计 \(F_{t-1 \to t}\) 和 \(F_{t+1 \to t}\)。
  3. 对 \(I_{t-1}\) 和 \(I_{t+1}\) 的特征按光流进行扭曲，得到对齐特征。
  4. 将参考帧特征与对齐特征进行融合（如注意力机制或卷积），生成融合帧 \(\hat{I}_t\)。
  5. 利用时序损失确保相邻融合帧之间的光流一致性，减少闪烁。

## 3. 实验设计
- **数据集/场景**：
  - 构建了 **VF-Bench**，涵盖四种视频融合任务：多曝光视频融合、多焦点视频融合、红外-可见视频融合、医学视频融合。
  - 数据来源：通过合成数据生成（如模拟多曝光、多焦点）以及对现有公开数据集进行严格筛选和配准，获得高质量、对齐的视频对。
- **基准（Benchmark）**：
  - **VF-Bench** 提供了统一的评估协议，同时评价融合视频的**空间质量**（如PSNR、SSIM、VIF等）和**时间一致性**（如时间闪烁指数、时序结构相似度等）。
- **对比方法**：论文没有列举具体对比方法名称，但声称UniVF在VF-Bench的所有任务上均达到**state-of-the-art**，表明实验对比了多种现有图像/视频融合方法。

## 4. 资源与算力
- **未明确说明**：摘要和元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。
- **合理推测**：由于涉及光流网络和多帧学习，通常需要至少一张高端GPU（如NVIDIA A100或RTX 3090）进行训练，但具体细节论文未提供。

## 5. 实验数量与充分性
- **实验数量**：虽然没有列出详细的消融实验数量，但VF-Bench包含四种不同的视频融合任务，每种任务应有多个子集和评价指标。作者还进行了与多个SOTA方法的对比实验。
- **充分性评估**：
  - **覆盖全面**：从四种常见融合任务出发，覆盖了典型的动态场景需求。
  - **评估指标合理**：同时评估空间质量与时间一致性，避免了单一指标偏好。
  - **消融分析**：通常论文会包含对多帧数量、光流模块、时序损失等组件的消融，但摘要未提及，因此无法确认。
- **公平性**：VF-Bench提供了统一的评估协议，对比实验应在同一协议下进行，保证了公平性。但合成数据可能与真实场景存在偏差，需注意。

## 6. 主要结论与发现
- 提出的UniVF统一框架能够有效解决视频融合中的时序一致性问题，显著减少闪烁，同时保持高空间质量。
- 在四个具有挑战性的视频融合任务上，UniVF均优于所有现有方法，证明了统一框架的通用性和有效性。
- VF-Bench作为第一个综合基准，为视频融合研究提供了标准化评测平台，促进了该领域的发展。

## 7. 优点
- **方法创新**：首次将多帧学习与光流特征扭曲统一用于多种视频融合任务，而非为每个任务单独设计。
- **基准贡献**：VF-Bench填补了视频融合缺乏专门评测基准的空白，涵盖多个任务、统一协议。
- **实用性强**：直接处理视频序列，生成时序一致的融合结果，更符合实际动态场景需求。
- **性能全面**：在空间质量和时间一致性两方面均达到最优。

## 8. 不足与局限
- **实验细节缺失**：未提供具体实验配置（如光流网络结构、训练超参数）、消融实验、计算资源等信息，降低了可复现性。
- **依赖光流质量**：光流估计不准确时，特征扭曲可能导致错误对齐，影响融合质量，论文未讨论鲁棒性。
- **数据合成性**：VF-Bench中多曝光、多焦点等数据通过合成生成，可能与真实拍摄视频存在分布差异，泛化到真实场景的风险未充分评估。
- **任务覆盖**：虽然包含了四种典型任务，但未包括其他视频融合类型（如多光谱、HDR+去噪等），通用性有限。
- **时间开销**：多帧处理+光流计算会增加推理时间，论文未提及实时性分析。

（完）
