---
title: Projection-Manifold Regularized Latent Diffusion for Robust General Image Fusion
title_zh: 投影-流形正则化潜在扩散用于鲁棒通用图像融合
authors: "Lei Cao, Hao Zhang, Chunyu Li, Jiayi Ma"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=RqE5PlQsU5"
tags: ["query:image-fusion"]
score: 9.0
evidence: 潜在扩散模型用于通用图像融合
tldr: 该论文提出PDFuse，一种无需训练的通用图像融合框架，基于预训练潜在扩散模型和投影-流形正则化。通过将融合定义为受多源图像约束的扩散推理过程，并设计多源信息一致性投影和流形一致性保持机制，实现高保真融合，适应多种模态。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有融合方法依赖特定任务训练，缺乏通用性和鲁棒性。
method: 利用预训练扩散模型，设计投影-流形正则化约束融合过程。
result: 在多种模态融合任务中无需微调即达最优性能。
conclusion: 生成先验和一致性约束相结合为通用融合提供了新范式。
---

## Abstract
This study proposes PDFuse, a robust, general training-free image fusion framework built on pre-trained latent diffusion models with projection–manifold regularization. By redefining fusion as a diffusion inference process constrained by multiple source images, PDFuse can adapt to varied image modalities and produce high-fidelity outputs utilizing the diffusion prior. To ensure both source consistency and full utilization of generative priors, we develop novel projection–manifold regularization, which consists of two core mechanisms. On the one hand, the Multi-source Information Consistency Projection (MICP) establishes a projection system between diffusion latent representations and source images, solved efficiently via conjugate gradients to inject multi-source information into the inference. On the other hand, the Latent Manifold-preservation Guidance (LMG) aligns the latent distribution of diffusion variables with that of the sources, guiding generation to respect the model’s manifold prior. By alternating these mechanisms, PDFuse strikes an optimal balance between fidelity and generative quality, achieving superior fusion performance across diverse tasks. Moreover, PDFuse constructs a canonical interference operator set. It synergistically incorporates it into the aforementioned dual mechanisms, effectively leveraging generative priors to address various degradation issues during the fusion process without requiring clean data for supervising training. Extensive experimental evidence substantiates that PDFuse achieves highly competitive performance across diverse image fusion tasks. The code is publicly available at https://github.com/Leiii-Cao/PDFuse.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有图像融合方法大多针对特定模态（如红外-可见光、医学图像、多聚焦等）设计专用网络，需要大量配对数据进行监督训练，缺乏跨模态的通用性和鲁棒性。实际应用中，融合任务面临多种退化（如噪声、模糊、光照不均等），现有方法难以统一处理。
- **整体含义**：该论文提出一种**无需训练**的通用图像融合框架 PDFuse，利用预训练潜在扩散模型的生成先验，通过投影-流形正则化将融合重新定义为受多源图像约束的扩散推理过程，实现高保真、跨模态的鲁棒融合。

## 2. 方法论

- **核心思想**：在预训练潜在扩散模型（LDM）的推理阶段引入多源图像的约束，使生成过程同时满足源图像信息一致性和流形先验保持。
- **关键技术细节**：
  - **多源信息一致性投影（MICP）**：建立扩散潜变量与源图像之间的投影系统，通过共轭梯度法高效求解，将多源信息注入推理过程，确保融合结果与各源图像在内容上一致。
  - **潜在流形保持引导（LMG）**：对齐扩散变量的潜分布与源图像的潜分布，引导生成过程尊重 LDM 的流形先验，避免模式坍塌或失真。
  - **交替机制**：在每步去噪中交替执行 MICP 和 LMG，平衡保真度与生成质量。
  - **规范干扰算子集**：构建一组可解释的干扰算子（如噪声、模糊等），将其协同嵌入上述双机制中，无需干净数据即可利用生成先验处理各种退化问题。
- **算法流程（文字说明）**：
  1. 输入多张源图像，使用预训练 LDM 的编码器提取潜变量。
  2. 初始化扩散过程的潜变量（通常为随机噪声）。
  3. 对每个去噪步：
     - 执行 LMG：调整潜变量分布使其与源图像潜分布对齐。
     - 执行 MICP：通过投影系统约束潜变量，使其与源图像保持一致（共轭梯度法求解）。
     - 执行标准去噪步（U-Net 预测噪声并更新潜变量）。
  4. 解码潜变量得到最终融合图像。

## 3. 实验设计

- **数据集/场景**：论文仅提及“diverse image fusion tasks”，未具体列出数据集名称。常见通用融合场景包括红外-可见光、医学图像（CT-MRI）、多聚焦、多曝光等。但由于缺乏具体信息，无法确认实际使用的数据集。
- **基准（Benchmark）**：未明确说明使用的 benchmark 或评价指标（如 SSIM、PSNR、MI、VIF 等）。
- **对比方法**：未列出具体的对比算法名称，仅称“highly competitive performance”。

## 4. 资源与算力

- 论文摘要和元数据中**未提及**任何关于 GPU 型号、数量、训练时长等信息。由于 PDFuse 是“无需训练”的框架，推理阶段的计算资源需求取决于预训练 LDM 的大小，但具体配置未被说明。

## 5. 实验数量与充分性

- **实验数量**：没有给出具体实验组数。仅使用“extensive experimental evidence”这一模糊描述，缺乏定量结果表格或曲线。
- **充分性与公平性**：
  - 不足：未公开任何定量指标、消融实验细节、可视化对比案例，实验设计信息严重缺失。
  - 客观性风险：仅凭声明“highly competitive”难以评估方法的真实性能。鉴于该论文为 NeurIPS 2025 接收，预期完整版应包含详细实验，但提供的摘要不足以判断。

## 6. 主要结论与发现

- PDFuse 通过无训练的方式，利用预训练扩散模型的生成先验和投影-流形正则化，在多种图像融合任务上取得极具竞争力的性能。
- 规范干扰算子集使方法能够处理各种退化问题，无需干净数据监督。
- 该工作为通用图像融合提供了新范式：生成先验与一致性约束的协同。

## 7. 优点

- **通用性**：无需针对特定模态训练，可适应红外-可见光、医学、多聚焦等多种融合场景。
- **无需训练**：直接利用预训练模型，降低了数据和计算成本。
- **鲁棒性**：内置干扰算子集，能应对退化图像融合，无需配对干净数据。
- **创新机制**：MICP 和 LMG 的交替设计在保真度与生成质量之间取得平衡，理论上优于传统后处理约束。

## 8. 不足与局限

- **实验信息缺失**：未提供具体数据集、指标、对比方法、消融实验、可视化结果，使性能评估缺乏可信度。
- **算力与效率**：未说明推理速度或计算成本，潜在扩散模型通常迭代步数多，实时性可能较差。
- **依赖预训练模型**：泛化能力受限于预训练 LDM 的域覆盖范围，对于极端域外图像可能失效。
- **干扰算子集设计**：仅提及“canonical interference operator set”，未具体解释定义方式，可复现性存疑。
- **应用限制**：无监督/无训练方法可能在某些任务上不如专用监督方法，缺乏定量对比无法确认实际优势。
- **论文完整性**：当前提供的文本仅为摘要和元数据，无法获取完整公式、算法伪代码及实验细节。

（完）
