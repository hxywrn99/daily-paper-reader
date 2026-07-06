---
title: "DuSSS: Dual Semantic Similarity-Supervised Vision-Language Model for Semi-Supervised Medical Image Segmentation"
title_zh: DuSSS：基于双重语义相似性监督的视觉语言模型用于半监督医学图像分割
authors: "Qingtao Pan, Wenhao Qiao, Jingjiao Lou, Bing Ji, Shuo Li"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32674/34829"
tags: ["query:medical-seg"]
score: 9.0
evidence: 使用视觉语言模型的半监督医学图像分割
tldr: 半监督医学图像分割常受低质量伪标签误差影响。本文提出DuSSS，利用视觉语言模型通过双重对比学习增强跨模态语义一致性，改善伪标签质量。在多个医学图像数据集上的实验表明，DuSSS有效提升了分割精度，减轻了对像素级标注的依赖。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 半监督医学图像分割中伪标签质量差，跨模态信息利用不足。
method: 提出双重对比学习和双重语义相似性监督的视觉语言模型框架DuSSS。
result: 在多个半监督医学图像分割任务上取得了最优性能。
conclusion: DuSSS有效结合了视觉和语言信息，提升了半监督分割质量。
---

## Abstract
Semi-supervised medical image segmentation (SSMIS) uses consistency learning to regularize model training, which alleviates the burden of pixel-wise manual annotations. However, it often suffers from error supervision from low-quality pseudo labels. Vision-Language Model (VLM) has great potential to enhance pseudo labels by introducing text prompt guided multimodal supervision information. It nevertheless faces the cross-modal problem: the obtained messages tend to correspond to multiple targets. To address aforementioned problems, we propose a Dual Semantic Similarity-Supervised VLM (DuSSS) for SSMIS. Specifically, 1) a Dual Contrastive Learning (DCL) is designed to improve cross-modal semantic consistency by capturing intrinsic representations within each modality and semantic correlations across modalities. 2) To encourage the learning of multiple semantic correspondences, a Semantic Similarity-Supervision strategy (SSS) is proposed and injected into each contrastive learning process in DCL, supervising semantic similarity via the distribution-based uncertainty levels. Furthermore, a novel VLM-based SSMIS network is designed to compensate for the quality deficiencies of pseudo-labels. It utilizes the pretrained VLM to generate text prompt guided supervision information, refining the pseudo label for better consistency regularization. Experimental results demonstrate that our DuSSS achieves outstanding performance with Dice of 82.52%, 74.61% and 78.03% on three public datasets (QaTa-COV19, BM-Seg and MoNuSeg).

---

## 论文详细总结（自动生成）

# DuSSS：基于双重语义相似性监督的视觉语言模型用于半监督医学图像分割

## 1. 核心问题与整体含义（研究动机与背景）

- **问题**：半监督医学图像分割（SSMIS）常依赖一致性学习，但低质量伪标签会导致错误监督，制约模型性能。视觉语言模型（VLM）可通过文本提示引入多模态监督信息来增强伪标签，但面临跨模态不确定性：一个图像可能对应多个文本描述，反之亦然。
- **整体含义**：本文提出DuSSS框架，首次将VLM引入SSMIS伪标签增强，通过双重语义相似性监督解决跨模态对齐不确定性问题，从而提升伪标签质量，改进半监督分割性能。

## 2. 论文提出的方法论

### 核心思想
- 两步框架：**Step 1** VLM预训练（带DuSSS）→ 学习跨模态和模态内的语义对应，降低不确定性；**Step 2** 文本引导的SSMIS网络 → 利用预训练VLM生成文本引导掩码，补偿伪标签质量缺陷。

### 关键技术细节
- **语义相似性监督（SSS）**：
  - 将语义嵌入转换为多元高斯分布，用2-Wasserstein距离衡量分布差异作为不确定性水平 \(D_u\)。
  - 语义相似度 \(D_s\) = 欧氏距离。
  - 定义相对不确定性 \(\tilde{D}_u = D_u / D_s\)，SSS得分 \(D_{\text{SSS}} = e^{-\lambda \tilde{D}_u}\)。
- **双对比学习（DCL）**：
  - **跨模态对比学习（CMC）**：基于SSS修改余弦相似度 \(s\_{\text{im}} = 1 - (1 - \text{sim}) \cdot D_{\text{SSS}}\)，用InfoNCE损失对齐图像-文本对。
  - **模态内对比学习（IMC）**：在图像和文本各模态内部进行对比学习，同样注入SSS。
- **文本引导的SSMIS**：
  - 利用预训练VLM的patch级图像特征和文本特征，通过解码器生成文本引导掩码 \(y^{\text{text}}_u\)。
  - 教师-学生网络（EMA更新），合并文本引导掩码和教师伪标签得到融合伪标签。
  - 损失函数：监督损失 \(L_{\text{sup}}\) + 半监督损失 \(L_{\text{semi}}\)（包括 \(L_{\text{merged\_semi}}\) 和 \(L_{\text{text\_semi}}\)）+ 文本引导损失 \(L_{\text{tg}}\)。
- **理论分析**：证明SSS使高不确定性图像-文本对的修正余弦相似度更高，从而缓解对齐不确定性。

## 3. 实验设计

### 数据集
- **QaTa-COV19**：9258张COVID-19胸部X光片（7145训练/2113测试），含文本标注。
- **BM-Seg**：23个CT扫描，共1517切片，选取270张（200训练/70测试）。
- **MoNuSeg**：44张显微镜图像（30训练/14测试），细胞核分割。

### Benchmark与对比方法
- 对比13种SOTA方法，包括：
  - 全监督：U-Net、CLIP、ViLT。
  - 半监督（无文本）：MT、CCT、BCP、MC-Net、SS-Net、UCMT。
  - 带文本的VLM方法：LViT、CMITM、ASG、MGCA。
- 评估指标：Dice和mIoU。
- 设置两种标注比例：25%和50%。

## 4. 资源与算力

- 实现框架：Pytorch，系统Ubuntu 20.04.4 LTS。
- GPU：单块24GB V100 GPU。
- 学习率：QaTa-COV19和BM-Seg为3e-4，MoNuSeg为1e-3。
- Batch size：QaTa-COV19 32，BM-Seg 16，MoNuSeg 4。
- 早停策略：若连续20个epoch性能未提升则停止训练。
- **未明确说明总训练时长和GPU数量，仅提及使用单块V100**。

## 5. 实验数量与充分性

- **主要对比实验**：Table 1在三个数据集上分别报告了25%和50%标注比例下的Dice/mIoU，共6组结果。
- **消融实验**：Table 2对SSS、DCL、Ltg三个组件进行组合消融，共7种设置，在三个数据集上均报告结果。
- **定性分析**：Fig.4展示不同方法的分割可视化对比。
- **不确定性分析**：Fig.5展示DuSSS对相似歧义文本的鲁棒激活效果。
- **公平性**：所有对比方法均采用相同实验设置和评估指标，消融实验控制变量。
- **充分性**：覆盖多模态、多任务、多标注比例，实验设计客观、全面。

## 6. 主要结论与发现

- DuSSS在三个数据集上均取得最佳或最先进性能（50%标注下QaTa-COV19 Dice 82.52%，BM-Seg 74.61%，MoNuSeg 78.03%），甚至超过部分全监督方法。
- 组件消融验证了SSS、DCL和文本引导损失各自贡献显著。
- 不确定性分析表明DuSSS能有效应对跨模态对齐不确定性，对相似但歧义的文本产生稳定激活。
- **结论**：DuSSS通过双重语义相似性监督和文本引导伪标签增强，显著提升了半监督医学图像分割质量。

## 7. 优点

- **方法创新**：
  - 首次将VLM集成到SSMIS伪标签生成中，提出文本引导掩码机制。
  - SSS策略通过分布不确定性监督语义相似度，新颖地解决了跨模态对齐不确定性。
  - DCL同时进行跨模态和模态内对比学习，增强语义一致性表征。
- **实验严谨**：在三类不同医学图像任务上评估，对比方法全面（13种），消融实验完整。
- **理论支撑**：提供简单理论证明，说明SSS如何缓解不确定性。
- **性能优越**：在多种标注比例下均达到SOTA，且参数量和计算成本较低（与U-Net相近）。

## 8. 不足与局限

- **算力资源信息不完整**：未明确GPU数量（可能仅一块）和总训练时长，不利于可重复性评估。
- **应用限制**：方法依赖文本注释，在缺少文本描述的场景下无法直接使用。
- **数据集规模有限**：BM-Seg和MoNuSeg样本量较小，泛化性需更多验证。
- **参数敏感性未深入分析**：SSS中的超参数λ对性能影响未进行详细调参讨论。
- **未与其他VLM方法的计算开销对比**：虽参数量与U-Net相近，但实际推理时间与内存占用未提及。
- **理论分析较简单**：仅提供简短推导，缺乏更严格的收敛性或泛化性分析。

（完）
