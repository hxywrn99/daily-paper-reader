---
title: "MdaIF: Robust One-Stop Multi-Degradation-Aware Image Fusion with Language-Driven Semantics"
title_zh: MdaIF：基于语言驱动语义的鲁棒一站式多退化感知图像融合
authors: "Jing Li, Yifan Wang, Jiafeng Yan, Renlong Zhang, Bin Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37548/41510"
tags: ["query:image-fusion"]
score: 10.0
evidence: 退化感知的红外与可见光图像融合，语言驱动语义
tldr: 现有红外与可见光图像融合方法未考虑恶劣天气导致的退化，且网络结构固定难以适应不同退化场景。本文提出MdaIF，利用大语言模型驱动，采用混合专家系统处理雾、雨、雪等天气退化，实现一站式融合。实验表明该方法在多种退化条件下均能生成高质量融合图像，显著优于传统方法，鲁棒性强。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37548/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 861, \"height\": 529, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37548/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1736, \"height\": 759, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37548/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1830, \"height\": 547, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37548/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 871, \"height\": 428, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37548/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 877, \"height\": 339, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-37548/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1832, \"height\": 943, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37548/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1844, \"height\": 879, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-37548/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 676, \"height\": 217, \"label\": \"Table\"}]"
motivation: 解决现有图像融合方法无法适应不同天气退化的问题。
method: 提出大语言模型驱动的混合专家系统，实现多退化场景统一处理。
result: 在多种退化条件下融合质量优于现有方法。
conclusion: MdaIF实现了鲁棒的多退化场景图像融合。
---

## Abstract
Infrared and visible image fusion aims to integrate complementary multi-modal information into a single fused result. However, existing methods 1) fail to account for the degradation visible images under adverse weather conditions, thereby compromising fusion performance; and 2) rely on fixed network architectures, limiting their adaptability to diverse degradation scenarios. To address these issues, we propose a one-stop degradation-aware image fusion framework for multi-degradation scenarios driven by a large language model (MdaIF). Given the distinct scattering characteristics of different degradation scenarios (e.g., haze, rain, and snow) in atmospheric transmission, a mixture-of-experts (MoE) system is introduced to tackle image fusion across multiple degradation scenarios. To adaptively extract diverse weather-aware degradation knowledge and scene feature representations, collectively referred to as the semantic prior, we employ a pre-trained vision-language model (VLM) in our framework. Guided by the semantic prior, we propose degradation-aware channel attention module (DCAM), which employ degradation prototype decomposition to facilitate multi-modal feature interaction in channel domain. In addition, to achieve effective expert routing, the semantic prior and channel-domain modulated features are utilized to guide the MoE, enabling robust image fusion in complex degradation scenarios. Extensive experiments validate the effectiveness of our MdaIF, demonstrating superior performance over SOTA methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有红外与可见光图像融合（IVF）方法存在两个主要缺陷：
  1. 未考虑恶劣天气（雾、雨、雪）对可见光图像造成的退化，导致融合性能下降；
  2. 依赖固定网络架构，难以适应多种不同的退化场景（不同天气的散射特性差异显著）。
- **整体含义**：本文提出一种**一站式多退化感知图像融合框架**（MdaIF），利用大语言模型（BLIP-2 OPT）提取的语言驱动语义先验，动态指导混合专家系统（MoE）处理雾、雨、雪等多样退化场景，无需依赖真实退化标签，也无需分阶段进行恢复再融合，从而实现端到端的鲁棒融合。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

### 核心思想
- 利用预训练视觉语言模型（VLM）从退化可见图像中自动提取“语义先验”（包含天气退化知识和场景特征），进而指导：
  - 退化感知通道注意力模块（DCAM）进行通道域的特征调制；
  - 退化感知混合专家（DMoE）进行自适应专家路由，为不同退化场景匹配合适的专家网络。

### 关键技术细节
- **整体流程**（图3）：
  1. 退化可见图像 \(I_{vi}\) 和红外图像 \(I_{ir}\) 分别通过Transformer编码器（SegFormer结构）提取特征 \(F_{vi}, F_{ir}\)，拼接得到 \(F_{in}\)。
  2. **DSPE（退化感知语义先验提取器）**：将 \(I_{vi}\) 与开放式问题提示 \(P_q\) 输入 BLIP-2 OPT，提取最后一层隐藏特征作为原始语义先验 \(S_{org}\)；再经MLP降维、层归一化、自注意力机制得到精炼语义先验 \(S_{prior}\)（包含天气部分 \(S_{weather}\) 和场景部分 \(S_{scene}\)）。
  3. **DCAM（退化感知通道注意力模块）**：
     - 对 \(S_{prior}\) 进行平均池化、MLP、Sigmoid 得到退化原型得分向量 \(s_K\)（\(K=10\)个原型）。
     - 每个退化原型对应一个可学习的通道响应向量 \(k_i \in \mathbb{R}^C\)，构成原型矩阵 \(W_{proto} \in \mathbb{R}^{K \times C}\)（正交初始化）。
     - 通道权重 \(w_c = \sigma(\sum_{i=1}^K s_K^i \cdot k_i)\)，对 \(F_{in}\) 逐通道调制后与残差相加得到 \(F_{dcam}\)。
  4. **DMoE（退化感知混合专家）**：
     - 用交叉注意力让 \(F_{dcam}\) 与 \(S_{prior}\) 交互，得到增强特征 \(\hat{a}\)。
     - 经过双分支降维、卷积、Softmax 得到专家激活权重 \(w \in \mathbb{R}^{1 \times N}\)（\(N=5\)个专家，每专家含3×3+1×1卷积）。
     - 最终输出 \(F_{dmoe} = \phi(\text{BatchNorm}(\sum w_i E_i))\)。
  5. **解码器**：CNN结构，从 \(F_{dmoe}\) 重建融合图像 \(I_f\)。

- **损失函数**：
  - 融合损失 \(L_{fusion} = L_{inte} + L_{color}\)
  - 集成损失 \(L_{inte}\)：监督融合图像像素强度梯度和强度最大值与源图像（清洁可见图 \(\tilde{I}_{vi}\) 和红外图 \(I_{ir}\)）最大值一致，采用L1范数。
  - 颜色一致性损失 \(L_{color}\)：YCbCr空间的Cb、Cr分量L1差。

## 3. 实验设计：数据集 / 场景、benchmark、对比方法

- **数据集**：
  - 基础数据集：MSRS（1524对，480×640，分1163训练/361测试）和 FMB（1500对，600×800，分1220训练/280测试）。
  - 退化生成：对两个数据集的可见光图像分别合成**雾、雨、雪**三种退化，得到3个退化版本。每个退化版本与原始红外成对，训练集共7149对，测试集1923对（每种退化各1/3，比例1:1:1）。
- **基准（benchmark）**：合成退化后的红外-可见光图像融合任务，以原始清洁可见图像作为融合的参照真值。
- **对比方法**：采用两种策略的Pipeline：
  - **策略Ⅰ**：针对单一退化的专用恢复模型 + 五种SOTA融合方法（SAGE, SegMiF, MetaFusion, TarDAL, SwinFuse）。
  - **策略Ⅱ**：统一恢复模型（DTMR, MPMF-Net, WGWS-Net） + 五种融合方法。
  - 专用恢复模型包括：去雾（DCMPNet, CasDyF-Net, DehazeFormer）、去雨（DRSformer, IDT, NeRD-Rain）、去雪（HDCWNet, InvDSNet, SnowFormer）。
- **指标**：PSNR, SSIM, NABF（越低越好）, MI（互信息，越高越好）。

## 4. 资源与算力

文中明确说明：
- **GPU**：NVIDIA GeForce RTX 4090（24GB显存），配备 AMD Ryzen 9 7950X 16核CPU。
- **训练配置**：初始学习率 \(6\times10^{-5}\)，批次大小15，总训练轮数1000 epochs，使用Adam优化器，学习率采用warm-up + 多项式衰减。
- **无更详细的算力使用量（如总训练时间、多卡并行等）**，文中未提及。

## 5. 实验数量与充分性

- **定量实验**：在MSRS和FMB的三种退化条件下分别进行策略Ⅰ和策略Ⅱ对比。以MSRS策略Ⅰ结果为例（表1），共测试了5种融合方法×3种专用恢复=15种组合，加上Ours，覆盖3种天气共16个方法，每个方法报告4个指标。
- **定性实验**（图6）：展示MSRS和FMB两个数据集上，各天气条件下Ours与BestR（对每个天气选择PSNR最优的恢复模型）+5种融合方法的视觉对比。
- **消融实验**（表2）：在MSRS平均结果上检验DCAM和DMoE的贡献，共4种组合（有/无各模块），报告平均PSNR/SSIM/NABF/MI。
- **额外实验**（论文提及完整结果见扩展版本，包含策略Ⅱ及FMB完整定量表）。
- **充分性评价**：实验设计比较全面，覆盖了两种主流数据集、三种天气、多种竞争方法（包括专用恢复和统一恢复），包含定性和定量评估以及消融分析。但缺少真实退化场景的验证（仅使用合成退化），且未对比端到端的竞品方法（如Text-IF、MMAIF等本文提及的VLM/LLM驱动方法可能被省略），公平性存在一定局限。

## 6. 论文的主要结论与发现

- 本文提出的MdaIF在**所有退化场景**（雾、雨、雪）下的**PSNR、SSIM、MI均优于所有对比的Pipeline方法**，且NABF更低，表明融合质量最高。
- 定性结果显示，Ours生成的融合图像更清晰、伪影更少，能够从退化输入直接产生干净的融合结果（如图1所示）。
- 消融实验证明：
  - 同时使用DCAM和DMoE达到最佳性能（PSNR 17.977，SSIM 1.269，MI 2.390）。
  - 去除DCAM会导致通道调制失效，降低性能。
  - 去除DMoE会导致专家路由退化（仅一个专家被激活，即专家崩溃问题），严重降低性能。
- 语义先验分解（图5）显示不同天气下退化原型得分分布具有区分性，说明VLM提取的先验能有效捕捉退化类型；原型矩阵的通道偏好具有多样性，增强了适应能力。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  1. **一站式端到端**：无需分阶段恢复+融合，避免特征对齐错误和误差累积。
  2. **无需真实退化标签**：利用VLM自动生成语义先验，不需要预先知道天气类型即可自适应融合，更具实用性。
  3. **退化原型分解**：将语义先验分解为多个原型，每个原型学习独立的通道响应模式，使得模型能够以线性组合方式灵活表达不同退化场景。
  4. **专家路由与语义先验交互**：通过交叉注意力让图像特征与语义先验深度交互，避免专家选择不平衡问题（专家崩溃）。
  5. **损失函数设计**：联合最大像素/梯度监督与颜色一致性，同时保真细节和色彩。
- **实验设计亮点**：
  1. 系统性生成多个退化版本，确保每种天气下的测试规模均衡。
  2. 对比策略覆盖面广：包括专用恢复+融合、统一恢复+融合、以及消融分析，证明端到端框架的优越性。
  3. 同时提供定量和定性结果，且定性图包含多个样例（同一退化不同场景），验证泛化性。

## 8. 不足与局限

- **实验局限**：
  1. **仅使用合成退化**：雾、雨、雪均由人工方法模拟，与真实复杂天气场景可能存在差距，缺乏对真实退化图像的验证。
  2. **缺少与直接端到端方法的对比**：文中未与同样利用VLM/LLM的Text-IF、MMAIF、OmniFuse等方法进行定量比较，只对比了传统的恢复+融合Pipeline，其声称的“优于SOTA”可能不够全面。
  3. **未报告统计显著性检验**：缺乏对多次运行结果的方差分析，难以判断性能提升是否显著。
  4. **只测试了MSRS和FMB两个数据集**，均为实验室清晰配对数据，未在真实退化场景（如户外采集）上验证。
- **方法局限性**：
  1. **依赖VLM的推理能力**：BLIP-2 OPT的参数较大（2.7B），推理成本高，可能影响实时应用。
  2. **专家数量固定（N=5）**：未讨论专家数量选择对性能的影响，可能对更复杂场景不够灵活。
  3. **原型数量K=10**：选择依据未说明，可能在其他退化类型（如低光、模糊）下需要调整。
  4. **损失函数基于梯度最大值假设**：对于光照突变或红外与可见光梯度不一致区域，可能导致过平滑。
- **应用限制**：未考虑输入图像未配准的情况，且推理速度未报告，移动端或车载嵌入式部署可能受限。

（完）
