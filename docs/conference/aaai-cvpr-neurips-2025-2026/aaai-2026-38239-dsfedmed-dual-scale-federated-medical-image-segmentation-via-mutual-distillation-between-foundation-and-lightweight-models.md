---
title: "DSFedMed: Dual-Scale Federated Medical Image Segmentation via Mutual Distillation Between Foundation and Lightweight Models"
title_zh: DSFedMed：基于基础模型与轻量模型相互蒸馏的双尺度联邦医学图像分割
authors: "Hanwen Zhang, Qiaojin Shen, Yuxi Liu, Yuesheng Zhu, Guibo Luo"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38239/42201"
tags: ["query:medical-seg"]
score: 10.0
evidence: 基于基础模型蒸馏的联邦医学图像分割
tldr: 针对联邦学习中部署基础模型计算开销大的问题，提出DSFedMed双尺度联邦框架，通过中央基础模型与轻量客户端模型之间的相互知识蒸馏，在保护隐私的同时实现高效医学图像分割，并利用生成的高质量图像替代公有数据集。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 基础模型在联邦场景中计算、通信和推理成本高，难以直接部署。
method: 提出双尺度联邦框架，中央基础模型与客户端轻量模型通过生成的高质量图像进行相互知识蒸馏。
result: 在多个联邦医学分割数据集上，DSFedMed在降低通信成本的同时保持了高分割性能。
conclusion: 相互蒸馏策略能有效融合基础模型能力与轻量模型效率，推动联邦医学分割实用化。
---

## Abstract
Foundation Models (FMs) have demonstrated strong generalization across diverse vision tasks. However, their deployment in federated settings is hindered by high computational demands, substantial communication overhead, and significant inference costs. We propose DSFedMed, a dual-scale federated framework that enables mutual knowledge distillation between a centralized foundation model and lightweight client models for medical image segmentation. To support knowledge distillation, a set of high-quality medical images is generated to replace real public datasets, and a learnability-guided sample selection strategy is proposed to enhance efficiency and effectiveness in dual-scale distillation. This mutual distillation enables the foundation model to transfer general knowledge to lightweight clients, while also incorporating client-specific insights to refine the foundation model. Evaluations on five medical imaging segmentation datasets show that DSFedMed achieves an average 2 percent improvement in Dice score while reducing communication costs and inference time by nearly 90 percent compared to existing federated foundation model baselines. These results demonstrate significant efficiency gains and scalability for resource-limited federated deployments.

---

## 论文详细总结（自动生成）

# DSFedMed 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基础模型（Foundation Models, FMs）如 SAM、ViT 在医学图像分割中表现优异，但在联邦学习（Federated Learning, FL）场景下直接部署面临三大障碍：高计算需求、巨大的通信开销、以及推理成本高。现有联邦分割方法要么追求高精度（如 FedSAM）但资源消耗大，要么追求效率（如 FedU-Net）但精度不足，难以同时实现高精度与高效率。
- **研究动机**：在资源受限的医疗终端设备上，如何利用中央服务器上强大的基础模型的泛化能力，同时保持客户端轻量模型的低延迟推理和低通信成本，并遵守医疗数据隐私法规（如 GDPR、CCPA），是亟待解决的问题。
- **整体含义**：本文提出一种双尺度联邦框架 DSFedMed，通过中央基础模型与客户端轻量模型之间的相互知识蒸馏，在保护隐私的前提下实现高效、准确的医学图像分割，为联邦学习中部署基础模型提供了实用且可扩展的解决方案。

## 2. 论文提出的方法论

- **核心思想**：构建双尺度（dual-scale）联邦系统：服务器端部署一个高性能的基础分割模型（SAM-B，ViT-B/16），客户端部署轻量级模型（TinySAM）。通过生成的高质量合成图像作为中介，实现两者之间的相互知识蒸馏（Mutual Knowledge Distillation），无需传输真实医疗数据或完整的基模型参数。
- **关键技术细节**：
  1. **高效数据生成**：每个客户端利用其私有图像-掩码对微调一个轻量级 ControlNet 模块（冻结扩散骨干 Stable Diffusion），训练后上传 ControlNet 参数到服务器。服务器利用所有客户端的 ControlNet 生成与各客户端模态和风格一致的图像-掩码对，构成全局合成数据集 \(\tilde{D}\)。该方法避免了直接暴露真实数据，且生成过程低开销（支持 8GB 显存模式）。
  2. **双尺度模型协同训练**：客户端 TinySAM 在私有真实数据上训练，参数通过联邦平均（FedAvg）聚合为全局轻量模型。服务器 SAM 在合成数据集 \(\tilde{D}\) 上微调。两者共享由 ControlNet 建立的一致语义空间，促进知识对齐。
  3. **可学习性引导的相互蒸馏（Learnability-Guided Mutual Distillation）**：
     - 定义每个合成样本的得分 \(s_i = \lambda \cdot (-L_{\text{GT}}^{(i)}) + (1-\lambda) \cdot L_{\text{KL}}^{(i)}\)，其中：
       - \(L_{\text{GT}}\)：服务器 SAM 对该样本预测与真实掩码的交叉熵损失（衡量监督可靠性）。
       - \(L_{\text{KL}}\)：SAM 与 TinySAM 预测之间的双向 KL 散度（衡量模型间分歧，指示知识互补潜力）。
     - 根据得分动态筛选样本，每次选择得分最高的固定比例样本组成当前训练池，用于后续蒸馏。
     - 蒸馏过程：在筛选后的样本上同时优化 SAM 和 TinySAM，交换预测分布进行知识传递。
  4. **异步训练设计**：数据生成和相互蒸馏在服务器端异步进行，客户端无需保持在线，支持新客户端灵活加入。

- **公式流程**（文字描述）：
  - 客户端 \(k\) 训练 ControlNet → 上传参数 \(\Phi_c^{(k)}\) → 服务器生成 \(\tilde{D} = \bigcup_k \{(\tilde{x}_k, m) | m \sim M_k\}\)。
  - 客户端 TinySAM 在私有数据 \(D_{\text{real}}^k\) 上训练 → 上传参数 → 服务器 FedAvg 聚合。
  - 服务器 SAM 在 \(\tilde{D}\) 上训练（监督）。
  - 在每一轮蒸馏中：
    1. 计算合成样本库中每个样本的 \(L_{\text{GT}}\) 和 \(L_{\text{KL}}\)。
    2. 按得分 \(s_i\) 排序，选择 top-k 样本。
    3. 在选定样本上，SAM 和 TinySAM 相互计算 KL 散度并更新双方参数（可训练）。

## 3. 实验设计

- **数据集与场景**：共使用 5 个公开医学图像分割数据集，构建非独立同分布（Non-IID）联邦场景：
  - Fundus（视盘/视杯分割）：多中心，留一域交叉验证。
  - Prostate Cancer（前列腺 MRI 分割）：多中心，留一域交叉验证。
  - Nuclei（细胞核分割）：多组织类型，PanNuke 等，留一域交叉验证。
  - ISIC 2017（皮肤病变分割）：按肤色分3个子集，独立测试集。
  - CHAOS（CT/MRI 肝脏分割）：按模态分2个客户端，独立测试集。

- **Benchmark 与对比方法**（见 Table 1）：
  - 集中式基线：SAM（在合并的私有数据+公有数据上微调）。
  - 联邦基础模型基线：FedSAM（全微调 SAM）、FedMSA（微调 MSA 适配器和解码器）。
  - 联邦轻量模型基线：FedU-Net、FednnU-Net、FedTinySAM（均基于 FedAvg）。
  - 本文方法：DSFedMed（服务器 SAM，客户端 TinySAM，相互蒸馏）。

- **评价指标**：Dice 系数和 IoU（交并比）。
- **额外实验**：
  - 消融实验：分别去除 Mutual KD 和 Learnability-Guided selection。
  - 超参数敏感性：选择率 \(\in [0.2,1.0]\)，权重 \(\lambda \in [0.1,0.9]\)。
  - 生成数据质量评估：对比 pix2pix、DDPM、ControlNet，使用 FID 和 FLD。
  - 不同模型尺度的组合：SAM-B/MSA-L 与 U-Net/TinySAM 搭配。
  - 效率对比：通信开销、推理时间、训练时间、平均 Dice。

## 4. 资源与算力

- 论文明确指出：异步训练阶段（数据生成和相互蒸馏）可在单张 GPU 上完成，推荐 NVIDIA A800 或 A6000。
- 训练时间报告（以 Prostate 数据集为例）：
  - FedSAM 同步训练耗时 1411.67 min，FedMSA 1160 min，FedTinySAM 198.02 min，DSFedMed 异步训练额外 455.5 min（但客户端同步训练仅 198.02 min）。
- 通信开销：DSFedMed 累计（100轮）为 8920 MB，远低于 FedSAM 的 71538 MB（降低约 88%）。
- 推理时间：DSFedMed 使用 TinySAM 推理仅 0.015 s/张，与 FedTinySAM 相同，远优于 FedSAM（0.118 s）。
- 未明确说明使用的 GPU 数量（推测单卡或少量 GPU），但表明资源需求合理。

## 5. 实验数量与充分性

- **实验数量**：覆盖 5 个不同模态（眼底、前列腺 MRI、细胞核、皮肤、肝脏 CT/MRI）的数据集，共 6 个子任务（Fundus-OD, Fundus-OC, Prostate, Nuclei, ISIC, CHAOS）。
- **对比基线**：7 种方法（含集中式、联邦基础模型、联邦轻量模型），表 2 列出了所有任务的 Dice/IoU 结果。
- **消融实验**：2 个关键组件（Mutual KD、LG Selection）的消融，共 4 组（Table 4），并报告训练时间。
- **超参数分析**：2 个超参数（选择率、λ），各取 5 个值，共 10 组（Fig.8）。
- **生成数据质量**：3 种生成方法对比（Table 6）。
- **模型规模组合**：4 种组合（Table 5）。
- **效率分析**：综合表 3 对比所有方法的通信、时间、精度。
- **评估充分性**：实验设计较全面，涵盖多种非 IID 设置、留一法验证、独立测试、消融、超参、效率。但缺乏真实临床部署实验，仅基于公开数据。总体客观公平，对比方法均按标准实现。

## 6. 主要结论与发现

- DSFedMed **在几乎所有任务上取得了最佳或次佳 Dice/IoU**，平均 Dice 比 FedTinySAM 高 3.8%，比集中式 SAM 高 1.4%，比 FedSAM 高 1.9%（Table 2）。
- 相互蒸馏能同时提升基础模型和轻量模型的性能：TinySAM 在域偏移场景（如 Prostate +5.7%, Fundus-OC +5.9%）提升显著；SAM 在特定客户端数据上也有所改善（Fig.7）。
- **效率显著**：相比 FedSAM，通信成本降低 88%，推理时间缩短近 90%（0.015 s vs 0.118 s），且精度更高。
- 可学习性引导的样本选择可将训练时间减半（约 50%），同时提升精度（Table 4）。
- 合成数据质量方面，ControlNet 优于 pix2pix 和 DDPM（Table 6），生成的图像逼真且可控。
- 框架对超参数选择不敏感，选择率 0.2-0.8、λ 0.1-0.9 均能稳定取得优于基线的结果。

## 7. 优点

- **创新性**：首次提出双尺度联邦框架，将基础模型和轻量模型通过相互蒸馏协同工作，兼顾精度和效率。
- **隐私保护**：使用合成数据替代真实数据蒸馏，无需传输真实图像或完整基模型，符合医疗隐私法规。
- **实用性与可扩展性**：异步设计允许新客户端动态加入；客户端仅需微调轻量 ControlNet 和 TinySAM，适合资源受限设备。
- **高效的样本选择机制**：提出可学习性得分，同时考虑监督可靠性和模型分歧，有效筛选最有价值的蒸馏样本，降低计算开销。
- **全面的实验验证**：多模态、多中心、非 IID 设置、消融、超参分析、效率对比等，证据充分。

## 8. 不足与局限

- **实验覆盖的局限性**：
  - 所有实验基于公开数据集，未在真实临床部署环境中验证，实际应用中可能面临更复杂的隐私合规、网络不稳定、设备多样性等问题。
  - 数据生成依赖 ControlNet 和 Stable Diffusion，对于某些罕见病或新模态，生成质量可能下降，进而影响蒸馏效果。
- **偏差风险**：
  - 合成数据可能继承原始预训练模型的偏见（如肤色、人群、设备偏好），导致下游分割模型出现公平性问题，论文未讨论。
  - 留一法验证中，测试域可能包含与训练域不同的分布，虽然模拟了域偏移，但未考虑数据标签噪声或标注不一致。
- **应用限制**：
  - 服务器端需要运行 SAM+ControlNet 生成+蒸馏，虽然单 GPU 可行，但对于超大规模联邦（数百客户端）或高分辨率图像，生成和蒸馏时间可能成为瓶颈。
  - 框架依赖客户端训练两个模型（TinySAM 和 ControlNet），虽然轻量，但仍需额外存储和计算资源。
  - 没有与更先进的联邦学习方法（如个性化 FL、梯度压缩、异步 FL）进行对比，仅与基础方法比较。
- **消融实验**：仅对两个主要组件进行消融，未深入研究不同蒸馏损失函数、不同生成策略、不同基础模型（如 SAM-L vs SAM-B）的细粒度影响。
- **未报告统计显著性**：结果未给出标准差或置信区间，无法判断性能提升是否统计显著。

（完）
