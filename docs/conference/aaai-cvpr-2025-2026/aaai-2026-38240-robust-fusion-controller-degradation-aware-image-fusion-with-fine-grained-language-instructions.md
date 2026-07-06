---
title: "Robust Fusion Controller: Degradation-Aware Image Fusion with Fine-Grained Language Instructions"
title_zh: 鲁棒融合控制器：具有细粒度语言指令的退化感知图像融合
authors: "Hao Zhang, Yanping Zha, Qingwei Zhuang, Zhenfeng Shao, Jiayi Ma"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38240/42202"
tags: ["query:image-fusion"]
score: 9.0
evidence: 具有语言指令的退化感知图像融合
tldr: 针对现有图像融合方法难以适应实际环境中多样化退化的问题，提出鲁棒融合控制器RFC，利用细粒度语言指令解析出功能条件和空间条件，通过多条件耦合网络生成融合先验，实现退化感知的图像融合。实验证明RFC在多种退化环境下仍能保持高质量融合。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有图像融合方法难以应对实际环境中空间变化的多样化退化。
method: 提出RFC，通过解析语言指令得到功能条件和空间条件，再经多条件耦合网络生成融合先验。
result: 在多种退化环境下实现鲁棒的高质量图像融合。
conclusion: RFC方法有效提升了图像融合在复杂退化场景中的鲁棒性。
---

## Abstract
Current image fusion methods struggle to adapt to real-world environments encompassing diverse degradations with spatially varying characteristics. To address this challenge, we propose a robust fusion controller (RFC) capable of achieving degradation-aware image fusion through fine-grained language instructions, ensuring its reliable application in adverse environments. Specifically, RFC first parses language instructions to innovatively derive the functional condition and the spatial condition, where the former specifies the degradation type to remove, while the latter defines its spatial coverage. Then, a composite control priori is generated through a multi-condition coupling network, achieving a seamless transition from abstract language instructions to latent control variables. Subsequently, we design a hybrid attention-based fusion network to aggregate multi-modal information, in which the obtained composite control priori is deeply embedded to linearly modulate the intermediate fused features. To ensure the alignment between language instructions and control outcomes, we introduce a novel language-feature alignment loss, which constrains the consistency between feature-level gains and the composite control priori. Extensive experiments on publicly available datasets demonstrate that our RFC is robust against various composite degradations, particularly in highly challenging flare scenarios.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）

- **问题**：现有图像融合方法难以应对真实世界中的**空间变化的复合退化**（如同时存在噪声、过曝、光晕、雾霾等，且退化区域不均匀）。传统融合方法仅关注信息聚合，不消除退化；部分最新方法虽能去除单一退化，但对复合退化及空间变化无效。
- **目标**：提升融合模型在恶劣环境下的鲁棒性，实现**退化感知的图像融合**——即根据用户指令，灵活且精确地移除指定区域内的指定退化。
- **背景**：图像融合对自动驾驶、军事侦察等至关重要，而真实场景退化普遍且空间变化，现有方法无法满足实际部署需求。

### 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用**细粒度语言指令**引导融合过程，解析出`功能条件`（要移除的退化类型）和`空间条件`（要增强的区域），通过多条件耦合生成复合控制先验，动态调制融合网络，实现退化感知的融合。
- **关键技术细节**：
  - **语言指令解析**：
    - 使用 **CLIP文本编码器** 将功能描述（如“remove noise”）编码为功能条件 α。
    - 使用 **CLIPSeg**（经微调以适应多模态数据）将空间描述（如“the car”）与可见光/红外图像结合，生成空间响应图 β。微调仅更新解码器最后卷积层，并进行跨模态响应拼接以得到综合空间条件。
  - **复合控制先验生成**：
    - 通过 **多条件耦合（MCC）模块** 将 α 和 β 融合。MCC 采用 FiLM 思想：先用1D卷积生成功能仿射变量（权重 α<sup>w</sup>、偏置 α<sup>b</sup>），对 β 进行仿射变换后与上一级输出相连。堆叠3个MCC模块得到最终复合控制先验 γ，再经2D卷积产生混合仿射变量 {γ<sup>w</sup>, γ<sup>b</sup>}。
  - **可控信息融合**：
    - 设计 **混合注意力融合（HAF）模块**（基于CBAM），依次执行通道注意力和空间注意力增强；然后将复合控制先验以特征级仿射变换（FiLM）的方式嵌入到融合特征中。堆叠4个HAF模块，最终用UNet解码器重建融合图像。
  - **优化正则化**：
    - **退化感知重建损失**：包含对比度损失 L<sub>con</sub>、结构损失 L<sub>str</sub>、色彩损失 L<sub>cor</sub>，分别针对语言指定区域 Λ 和非指定区域 Λ̅ 使用动态权重，保证小区域不被忽视。
    - **语言-特征对齐损失 L<sub>ali</sub>**：计算最后HAF模块输入与输出之间的特征增益 ΔF 与复合控制先验 γ 的余弦相似度，强制内部特征变化与指令一致。

### 3. 实验设计

- **数据集**：基于 MFNet、LLVIP、M3FD、FMB、RoadScene 构建训练集（14,654对图像-文本标注，含模拟退化：低光、光晕、雾、噪声、模糊及其复合）。测试集700对。另外使用 **M3SVD**（真实相机拍摄退化数据）进行泛化测试。
- **Benchmark**：全参考指标（Q<sub>abf</sub>、SSIM、MI、VIF、SCD）用于复合退化场景；无参考指标（SD、AG、EN、SF）用于无退化场景；以及高级任务指标（目标检测 mAP、语义分割 mIoU）。
- **对比方法**：MRFS、CDDFuse、DDFM、CAF、SHIP、FISCNet、ReFusion、SDCFusion、Text-IF（共9种SOTA），对不含退化移除能力的方法使用 InstrucTIR 预处理。Text-IF 以文本告知退化类型。

### 4. 资源与算力

- 文中明确说明：使用 **4块 NVIDIA Tesla P100 GPU（16GB 显存）** 和 **1个 Intel Xeon Gold 5117 CPU**。未提及具体训练时长，但提到使用 AdamW 优化器（初始学习率 2e-4）和多尺度训练策略。

### 5. 实验数量与充分性

- **实验组数较多**：包括全局退化去除、局部退化去除、光晕移除（定性）、复合退化定量对比、无退化对比、泛化实验、消融实验（语言-特征对齐损失、功能条件、空间条件、CLIPSeg微调、模块数量）、高级任务验证（目标检测、语义分割）。
- **充分性评价**：实验覆盖了多种退化类型、全局/局部控制、真实数据泛化、和高级任务验证，对比方法丰富，消融全面。但所有实验结果均基于**模拟退化**（除泛化实验使用M3SVD真实退化外），真实场景的多样性可能仍有限。实验设计基本客观公平，指标选择合理。

### 6. 主要结论与发现

- RFC 能根据语言指令精确移除**全局或局部**的复合退化，尤其在光晕场景下效果显著。
- 在复合退化、无退化、真实泛化数据集上，RFC 在多数全参考/无参考指标上**超越所有对比方法**。
- 目标检测和语义分割验证表明，RFC 融合结果能显著提升高级任务的性能（检测mAP最高，分割mIoU仅次于MRFS）。
- 消融实验证实：语言-特征对齐损失、功能/空间条件、CLIPSeg微调均对性能有重要贡献；3个MCC+4个HAF模块配置最佳。

### 7. 优点

- **创新性**：首次将细粒度语言指令引入图像融合以处理空间变化的复合退化，实现了开放式的“按需融合”。
- **方法先进性**：采用CLIP+CLIPSeg进行指令解析，通过多条件耦合生成动态先验，并用FiLM无缝嵌入融合网络，设计完整。
- **实验全面性**：涵盖多种退化、全局/局部控制、真实数据泛化、高级任务验证，消融细致。
- **可解释性与可控性**：用户可通过自然语言指定任何区域和退化类型，实用性高。

### 8. 不足与局限

- **退化模拟依赖**：训练数据退化均为人工模拟，虽在M3SVD真实数据上泛化较好，但真实场景的退化复杂度（如多种退化耦合、物理真实感）可能未被充分覆盖。
- **算力需求**：使用4块P100 GPU，且模型含CLIP/CLIPSeg等预训练组件，推理/训练成本较高，轻量化部署可能受限。
- **CLIPSeg微调有限**：仅微调解码器最后一层卷积，对于红外模态的适应性可能仍有提升空间（在红外模态度量上可能不如可见光模态精准）。
- **高级任务验证**：语义分割任务中，RFC的mIoU略低于MRFS（专门设计为融合与分割协同的方法），说明在语义保真度上可能仍有改进余地。
- **语言指令范围**：目前指令需明确指定退化类型和区域，对模糊或隐含指令（如“让图片更好看”）的处理能力未验证。

（完）
