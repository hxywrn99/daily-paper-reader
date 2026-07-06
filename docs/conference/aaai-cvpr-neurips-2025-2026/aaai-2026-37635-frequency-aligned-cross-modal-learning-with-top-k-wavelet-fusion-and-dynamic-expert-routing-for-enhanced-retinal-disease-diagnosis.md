---
title: Frequency-Aligned Cross-Modal Learning with Top-K Wavelet Fusion and Dynamic Expert Routing for Enhanced Retinal Disease Diagnosis
title_zh: 基于频率对齐跨模态学习与Top-K小波融合和动态专家路由的视网膜疾病诊断增强
authors: "Yuxin Lin, Haoran Li, Haoyu Cao, Yongting Hu, Qihao Xu, Chengliang Liu, Xiaoling Luo, Zhihao Wu, Yong Xu, Wei Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37635/41597"
tags: ["query:image-fusion"]
score: 8.0
evidence: 用于医学诊断的多模态融合深度学习
tldr: 现有视网膜疾病多模态融合方法缺乏对模态贡献的自适应调节。本文提出自适应融合框架，包含动态跨模态专家路由和Top-K小波融合模块，用于彩色眼底照片和OCT图像的融合诊断。实验表明该方法能显著提升疾病识别准确率，优于传统融合策略。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37635/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 848, \"height\": 723, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37635/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1821, \"height\": 1038, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37635/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 848, \"height\": 363, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37635/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 854, \"height\": 817, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37635/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1845, \"height\": 510, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37635/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 763, \"height\": 628, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37635/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 844, \"height\": 289, \"label\": \"Table\"}]"
motivation: 现有融合范式无法根据临床场景自适应调节模态贡献。
method: 提出动态跨模态专家路由和Top-K小波融合模块，自适应优化多模态信号。
result: 在视网膜疾病诊断任务上取得了更高的识别精度。
conclusion: 所提框架提高了多模态融合在医学诊断中的适应性和性能。
---

## Abstract
Multimodal fusion of color fundus photography (CFP) and optical coherence tomography (OCT) B-scan images has demonstrated superior diagnostic potential for retinal diseases compared to single-modality approaches. However, existing fusion paradigms - whether through naive concatenation or attention mechanisms - treat cross-modal interactions indiscriminately, lacking adaptive modulation of modality-specific contributions under varying clinical scenarios. We propose an adaptive fusion framework that dynamically routes and refines multimodal signals for enhancing disease recognition. The framework comprises two key components: 1) Dynamic Cross-Modal Expert Routing (CMER), which selectively activates convolutional neural network (CNN) experts from one modality based on contextual guidance from the other, ensuring only the most relevant feature extractors contribute to fusion; and 2) Top-K Expert-Guided Wavelet Fusion (TEWF), which performs discrete wavelet transform (DWT) to decompose selected features into low- and high-frequency subbands. Cross-modal attention is then applied specifically to high-frequency components, where lesion-specific microstructures reside, enabling frequency-aware fusion. Finally, inverse DWT (IDWT) reconstructs the fused representation, weighted by CMER-derived importance scores to amplify informative modality cues while suppressing redundancy. Experimental validation on two multimodal retinal datasets demonstrates that our method achieves state-of-the-art performance, outperforming existing fusion strategies by significant margins in disease classification accuracy and robustness.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与研究背景
- **问题**：现有视网膜疾病多模态融合方法（如CFP与OCT）通常采用简单拼接或统一注意力机制，无法根据临床场景**自适应调节模态贡献**，忽略了不同模态在不同空间尺度和频率带上贡献的差异。
- **意义**：视网膜疾病早期诊断对防止不可逆视力丧失至关重要，多模态融合比单模态更具潜力，但现有方法缺乏对细粒度病理特征（如微小型病变）的高效融合。

## 2. 方法论
- **核心思想**：提出两阶段自适应融合框架，先通过**动态跨模态专家路由（CMER）** 选择相关特征提取器，再通过**Top-K专家引导的小波融合（TEWF）** 对高频成分进行跨模态注意力融合。
- **关键技术细节**：
  - **CMER**：基于MoE思想，为每个模态维护一组CNN专家；另一模态的特征通过路由网络生成路由得分，选择Top-K专家激活，同时引入负载均衡损失（$L_{load}$）防止专家坍缩。
  - **TEWF**：对所选特征做离散小波变换（DWT），得到LL（低频）、LH、HL、HH（高频）子带；仅在三个高频子带上应用双向交叉注意力（OCT→CFP和CFP→OCT），融合后通过逆小波变换（IDWT）重建，再叠加门控注意力和空间注意力，最后按CMER权重加权求和。
  - **分类阶段**：将最终融合特征与原始特征残差连接，经自适应平均池化、展平、拼接后分类。
  - **损失函数**：交叉熵损失 + 动态加权的负载均衡损失（根据$L_{load}$相对大小调整系数）。

## 3. 实验设计
- **数据集**：
  - **MMAD**（多模态AMD数据集）：1,094张CFP + 1,289张OCT，四分类（正常、干性AMD、PCV、湿性AMD）。
  - **GAMMA**（青光眼分级数据集）：100对CFP-OCT，三等级严重度。
- **Benchmark**：均采用官方或文献中的划分协议（MMAD按Wang et al. 2022，GAMMA按80/20划分并五折交叉验证）。
- **对比方法**：
  - 经典多模态融合：LateFusion, Yoo, MM-CNN及其变种（Loose, da）, B-EF, M²LC, MCDO, DE, TMC, EYEMOST及其变种。
  - 视觉transformer：CVSA, MVCINN, MVCNN。
  - 语言-视觉预训练：ClipResNet。
  - 竞赛方法：smartDSP, Eyestar。

## 4. 资源与算力
- **未明确说明**：文中未提及GPU型号、数量、训练时长等计算资源信息。

## 5. 实验数量与充分性
- **两组实验**：在MMAD和GAMMA两个数据集上进行了全面对比。
- **多维度评估**：使用Acc, F1, Kappa, Pre, Sen, Spe等指标；且对MMAD给出各类别细粒度性能（表2）。
- **消融实验**：对CMER、小波变换、交叉注意力各组件进行消融（表4），验证了每个模块的必要性。
- **超参数分析**：分析了专家总数N、路由数K的影响（图3），找到最佳配置（N=6, K=2）。
- **充分性评价**：实验设计**较为全面**，对比方法覆盖了经典、SOTA、竞赛模型；消融充分；但仅在两个中型数据集上验证，未见大规模真实临床部署验证。

## 6. 主要结论
- 提出的CMER+TEWF框架在两个数据集上均**超越所有对比方法**，在MMAD上达到F1=0.925、Acc=0.881，在GAMMA上Acc=0.870、Kappa=0.841。
- 动态专家路由和频率感知融合能有效捕捉病变相关微结构，提高分类准确性和鲁棒性。
- 消融实验证明每个组件不可或缺，超参数分析表明适量专家和路由能平衡容量与泛化。

## 7. 优点
- **创新性**：首次将MoE路由与离散小波分解结合用于多模态医学图像融合，设计巧妙。
- **临床相关性**：针对病变病理细节（高频成分）进行重点融合，符合医学认知。
- **实验结果**：在两个数据集上均取得一致领先，且消融分析清晰，超参数搜索合理。
- **可解释性**：通过频率选择和高频注意力，可在一定程度上解释模型关注区域。

## 8. 不足与局限
- **算力未披露**：无法评估训练成本及复现门槛。
- **数据集规模较小**：MMAD仅千例级，GAMMA仅100例，可能存在过拟合风险，需在更大规模多中心数据上验证。
- **缺少鲁棒性测试**：未涉及图像质量下降、模态缺失、域迁移等临床常见挑战。
- **未与最新预训练大模型比较**：如MedCLIP、BiomedCLIP等，可能低估了对比方法的能力。
- **专家数固定**：动态路由虽好，但专家总数量N需手工设定，缺乏自适应扩展机制。

（完）
