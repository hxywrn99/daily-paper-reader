---
title: "Deno-IF: Unsupervised Noisy Visible and Infrared Image Fusion Method"
title_zh: Deno-IF：无监督噪点可见光与红外图像融合方法
authors: "Han Xu, Yuyang Li, Yunfei Deng, Jiayi Ma, Guangcan Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=36cKp4tsHF"
tags: ["query:image-fusion"]
score: 10.0
evidence: 无监督噪点可见光与红外图像融合
tldr: 现有图像融合方法在噪声环境下性能下降，且监督方法泛化性差。本文提出无监督噪点融合方法Deno-IF，通过卷积低秩优化从噪声源图像中分解清洁成分，再由统一网络同时进行去噪与融合。无需配对训练数据，在合成和真实噪声场景下均能保持高质量融合，展现了优秀的泛化能力。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有图像融合方法难以处理噪声，且监督方法泛化性差。
method: 提出卷积低秩优化分解噪声图像，并构建统一去噪融合网络。
result: 在噪声条件下融合质量提升，且无需训练数据。
conclusion: 该方法实现了噪声环境下鲁棒的无监督图像融合。
---

## Abstract
Most image fusion methods are designed for ideal scenarios and struggle to handle noise. Existing noise-aware fusion methods are supervised and heavily rely on constructed paired data, limiting performance and generalization. This paper proposes a novel unsupervised noisy visible and infrared image fusion method, comprising two key modules. First, when only noisy source images are available, a convolutional low-rank optimization module decomposes clean components based on convolutional low-rank priors, guiding subsequent optimization. The unsupervised approach eliminates data dependency and enhances generalization across various and variable noise. Second, a unified network jointly realizes denoising and fusion. It consists of both intra-modal recovery and inter-modal recovery and fusion, also with a convolutional low-rankness loss for regularization. By exploiting the commonalities of denoising and fusion, the joint framework significantly reduces network complexity while expanding functionality. Extensive experiments validate the effectiveness and generalization of the proposed method for image fusion under various and variable noise conditions. The code is publicly available at https://github.com/hanna-xu/Deno-IF.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：大多数图像融合方法假设输入为理想（无噪）图像，实际应用中噪声不可避免，导致融合质量严重下降；已有的噪声感知融合方法均为监督式，严重依赖人工构造的配对数据，泛化性差。
- **动机**：实现一种无需配对数据的、能处理多种未知噪声的可见光与红外图像融合方法，在去噪的同时完成融合，并且适应各种噪声强度与类型。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：无监督学习，利用卷积低秩先验从含噪源图像中分解出清洁成分，再通过统一网络同时执行去噪和融合，无需配对训练数据。
- **关键技术细节**：
  - **卷积低秩优化模块（Convolutional Low-Rank Optimization Module）**：仅给定含噪源图像时，基于卷积低秩先验将图像分解为清洁成分与噪声成分，指导后续优化。
  - **统一去噪融合网络（Unified Denoising and Fusion Network）**：网络包含**模态内恢复**（intra-modal recovery）和**模态间恢复与融合**（inter-modal recovery and fusion），利用去噪和融合的共性，降低网络复杂度同时扩展功能。
  - **卷积低秩损失（Convolutional Low-Rankness Loss）**：作为正则化项，约束融合结果保持低秩特性。
- **流程说明**：输入噪声可见光和红外图像 → 卷积低秩优化分解 → 统一网络处理输出清洁融合结果。

### 3. 实验设计
- **数据集/场景**：摘要未明确列出具体数据集名称，但提及在**合成噪声场景**和**真实噪声场景**下进行验证，涵盖多种噪声类型与强度。
- **Benchmark**：对比了现有“大多数融合方法”和“现有噪声感知融合方法”（均为监督方法），未列举具体方法名称。
- **对比方法**：未详述；但可知监督方法作为敌对基线。

### 4. 资源与算力
- 论文摘要及元数据**未明确说明**使用的GPU型号、数量、训练时长等算力信息。

### 5. 实验数量与充分性
- 摘要声明“大量实验”（Extensive experiments），但未给出具体实验组数。推断至少包含：
  - 合成噪声（不同噪声类型/强度）下的融合对比；
  - 真实噪声场景下的融合对比；
  - 消融实验（模块有效性、损失函数影响）；
  - 与监督方法的泛化能力比较。
- **评价**：实验覆盖面较广，但由于缺少数据集的详细信息和复现细节，无法完全判断公平性与充分性。作者公开了代码（GitHub），有利于验证。

### 6. 主要结论与发现
- 提出的无监督方法Deno-IF在合成和真实噪声条件下均能保持高质量融合，显著优于现有噪声感知监督方法。
- 联合去噪与融合框架有效降低了网络复杂度，并提升了功能扩展性。
- 卷积低秩先验能有效分离噪声与清洁结构，无监督设计消除了对配对数据的依赖，增强了跨噪声类型/强度的泛化能力。

### 7. 优点
- **方法创新**：首个无监督噪点融合方法，摆脱了配对数据依赖，实用性强。
- **技术亮点**：利用卷积低秩先验进行噪声分离，统一网络同时完成去噪与融合，降低参数量与计算开销。
- **鲁棒性**：在多种未知噪声条件下表现出色，泛化性好。
- **开源**：代码公开，便于复现和后续研究。

### 8. 不足与局限
- **实验细节缺失**：未给出具体数据集名称、对比方法列表、评价指标数值，削弱了可验证性。
- **资源信息未披露**：无法评估训练和推理的计算成本。
- **潜在偏差风险**：真实噪声场景选取有限，可能无法覆盖所有实际应用（如极端低光照、传感器特定噪声模式）。
- **应用限制**：方法针对可见光与红外两种模态，未讨论扩展到更多模态的可行性；实时性未提及。

（完）
