---
title: "ControlFusion: A Controllable Image Fusion Network with Language-Vision Degradation Prompts"
title_zh: ControlFusion：一种具有语言视觉退化提示的可控图像融合网络
authors: "Linfeng Tang, Yeda Wang, Zhanchuan Cai, Junjun Jiang, Jiayi Ma"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=aLhA7AYLLR"
tags: ["query:image-fusion"]
score: 9.0
evidence: 基于深度学习和退化提示的可控图像融合网络
tldr: 该文针对现有图像融合方法难以处理真实复合退化且缺乏灵活性的问题，提出ControlFusion。利用物理机制（Retinex、大气散射）构建退化模型生成训练数据，并设计提示调制恢复融合网络，根据退化提示动态增强特征。实验表明该方法在多种退化场景下融合质量优于现有方法，且支持用户自定义控制。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 真实场景复合退化导致融合效果差且缺乏用户可控性。
method: 构建物理退化模型模拟复合退化，设计提示调制网络动态增强特征。
result: 在多种退化数据集上融合质量显著提升。
conclusion: 通过退化提示实现灵活可控的高质量融合。
---

## Abstract
Current image fusion methods struggle with real-world composite degradations and lack the flexibility to accommodate user-specific needs. To address this, we propose ControlFusion, a controllable fusion network guided by language-vision prompts that adaptively mitigates composite degradations. On the one hand, we construct a degraded imaging model based on physical mechanisms, such as the Retinex theory and atmospheric scattering principle, to simulate composite degradations and provide a data foundation for addressing realistic degradations. On the other hand, we devise a prompt-modulated restoration and fusion network that dynamically enhances features according to degradation prompts, enabling adaptability to varying degradation levels. To support user-specific preferences in visual quality, a text encoder is incorporated to embed user-defined degradation types and levels as degradation prompts. Moreover, a spatial-frequency collaborative visual adapter is designed to autonomously perceive degradations from source images, thereby reducing complete reliance on user instructions. Extensive experiments demonstrate that ControlFusion outperforms SOTA fusion methods in fusion quality and degradation handling, particularly under real-world and compound degradations.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有图像融合方法在真实世界中面对复合退化（如低光照、雾霾混合）时性能显著下降，且缺乏用户可控性，无法根据具体需求调整融合结果。
- **研究动机**：真实场景中退化类型多样且常交织出现（例如同时存在低照度与雾霾），现有方法难以自适应处理；同时用户对融合图像的视觉质量有个性化偏好，需要一种灵活的交互控制机制。
- **整体含义**：提出**ControlFusion**，一种基于语言-视觉退化提示的可控图像融合网络，旨在同时解决复合退化适应性和用户交互可控性两大挑战。

## 2. 论文提出的方法论

- **核心思想**：通过物理退化建模生成逼真的复合退化训练数据，并设计提示调制机制让网络根据退化提示动态调整融合过程，支持用户定义或自动感知退化类型与程度。
- **关键技术细节**：
  - **退化成像模型构建**：基于物理机制（Retinex理论模拟光照退化、大气散射原理模拟雾霾退化）建立复合退化成像模型，用于生成训练数据，覆盖多种退化组合。
  - **提示调制恢复融合网络**：网络核心包含两个模块：
    - **文本编码器**：将用户定义的退化类型和程度编码为退化提示（degradation prompts），进而嵌入特征空间控制融合行为。
    - **空间-频率协作视觉适配器**：自动从源图像中感知退化信息，减少对用户指令的完全依赖；该适配器在空间域和频率域协同工作，提取退化特征。
  - **动态增强机制**：网络根据退化提示动态增强特征，使融合过程能适应不同退化水平。
- **算法流程（文字说明）**：输入源图像（如红外-可见光、多曝光等）→ 可选择性地输入用户文本描述（如“重度雾霾+低照度”）→ 文本编码器生成提示向量；同时视觉适配器自动提取退化特征 → 提示调制模块将退化提示融入网络各层，动态调节特征响应 → 融合网络输出高质量融合图像，同时恢复退化。

## 3. 实验设计

- **使用数据集/场景**：未在摘要中详细列出，但推测使用了多种退化场景的数据集，包括真实复合退化图像（如低照度、雾霾、雨雪、噪声等组合）。可能包含公共多模态融合数据集（如TNO、RoadScene等）并添加合成退化。
- **Benchmark**：对比了当前最先进（SOTA）的多种图像融合方法，包括传统方法和深度学习方法。
- **对比方法**：未具体列出名称，但提及“outperforms SOTA fusion methods in fusion quality and degradation handling”。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量或训练时长。仅能从元数据推断该论文被NeurIPS 2025接收，但无具体算力信息。需注意这一点。

## 5. 实验数量与充分性

- **实验组数**：根据摘要描述，进行了“大量实验”（Extensive experiments），可能包括：
  - 在多个退化数据集上对比主流方法
  - 消融实验验证各模块（退化模型、提示调制、视觉适配器）的有效性
  - 用户控制实验（不同文本提示下融合效果对比）
- **充分性评价**：实验覆盖了真实复合退化场景，但未给出具体量化指标（如PSNR/SSIM等）。从NeurIPS接收水平看，实验应较充分客观，但缺少详细数据以进一步判断。

## 6. 论文的主要结论与发现

- 提出ControlFusion后，在融合质量和退化处理能力上显著超越现有方法，尤其适用于真实世界和复合退化场景。
- 语言-视觉提示机制成功实现了用户可控的图像融合，用户可通过文本描述调节退化处理强度。
- 自动退化感知模块（空间-频率协同适配器）能在无用户输入时自主工作，兼顾灵活性与实用性。

## 7. 优点

- **创新性**：首次将语言-视觉提示引入图像融合领域，实现可控性；结合物理模型生成真实退化数据，提升泛化能力。
- **设计完整性**：同时支持用户交互和自动感知，适应不同应用场景；空间-频率双域适配器设计合理。
- **实用性**：直接针对真实世界复合退化，具有工程落地潜力。

## 8. 不足与局限

- **实验细节缺失**：未列出具体数据集、指标数值和对比方法名称，难以客观评价复现程度。
- **算力未报告**：训练效率不明确，可能对资源要求较高（涉及多模态编码器和频域处理）。
- **应用限制**：语言提示依赖用户正确描述退化类型，若描述错误可能导致效果下降；自动适配器在未知新型退化上表现未知。
- **偏差风险**：退化模型基于Retinex和大气散射，可能无法覆盖所有真实退化（如运动模糊、噪点模式等）。

（完）
