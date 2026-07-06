---
title: "A²RNet: Adversarial Attack Resilient Network for Robust Infrared and Visible Image Fusion"
title_zh: A2RNet：对抗攻击鲁棒网络用于稳健红外与可见光图像融合
authors: "Jiawei Li, Hongwei Yu, Jiansheng Chen, Xinlong Ding, Jinlong Wang, Jinyuan Liu, Bochao Zou, Huimin Ma"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32504/34659"
tags: ["query:image-fusion"]
score: 9.0
evidence: 用于红外与可见光融合的对抗攻击鲁棒网络
tldr: 现有融合方法忽略恶意干扰对融合结果的影响。本文提出A2RNet，通过对抗攻击范式与反攻击损失函数进行训练，增强模型对恶意攻击的鲁棒性。实验表明该网络在各种攻击下仍能保持高质量融合，为安全关键应用提供了保障。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 848, \"height\": 686, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1797, \"height\": 542, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1809, \"height\": 669, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 862, \"height\": 631, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1829, \"height\": 666, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 852, \"height\": 374, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 854, \"height\": 213, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-32504/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 858, \"height\": 181, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32504/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 875, \"height\": 657, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32504/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1727, \"height\": 243, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-32504/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 259, \"label\": \"Table\"}]"
motivation: 现有融合方法缺乏对恶意干扰的鲁棒性考虑。
method: 设计对抗范式与反攻击损失函数，进行对抗训练增强鲁棒性。
result: 在多种攻击下融合质量保持稳定，优于现有方法。
conclusion: A2RNet为鲁棒图像融合提供了有效防御机制。
---

## Abstract
Infrared and visible image fusion (IVIF) is a crucial technique for enhancing visual performance by integrating unique information from different modalities into one fused image. Exiting methods pay more attention to conducting fusion with undisturbed data, while overlooking the impact of deliberate interference on the effectiveness of fusion results. To investigate the robustness of fusion models, in this paper, we propose a novel adversarial attack resilient network, called A2RNet. Specifically, we develop an adversarial paradigm with an anti-attack loss function to implement adversarial attacks and training. It is constructed based on the intrinsic nature of IVIF and provide a robust foundation for future research advancements. We adopt a Unet as the pipeline with a transformer-based defensive refinement module (DRM) under this paradigm, which guarantees fused image quality in a robust coarse-to-fine manner. Compared to previous works, our method mitigates the adverse effects of adversarial perturbations, consistently maintaining high-fidelity fusion results. Furthermore, the performance of downstream tasks can also be well maintained under adversarial attacks.

---

## 论文详细总结（自动生成）

# 论文中文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有的红外与可见光图像融合（IVIF）方法主要关注无干扰数据下的融合效果，忽略了恶意对抗攻击对融合结果的破坏性影响。当面对精心设计的对抗扰动时，现有融合网络会生成带有明显伪影、噪声的劣质融合图像，进而影响下游任务（如目标检测、语义分割）的性能。
- **研究动机**：为了提升融合模型在实际安全关键场景（如自动驾驶）中的鲁棒性，迫切需要一种能够主动抵御对抗攻击的融合框架。
- **整体含义**：本文首次将对抗攻击与防御范式系统性地引入IVIF任务，提出了一个从对抗样本生成到对抗训练的完整方案，并设计了专门的鲁棒网络结构，使得融合图像在遭受攻击时仍能保持高保真度，同时保障下游任务性能。

## 2. 方法论
### 核心思想
- 基于IVIF任务双输入、无真实标签的特点，设计一个针对融合任务的**对抗攻击与训练范式**，采用**伪标签**替代真实标签，结合**反攻击损失函数**（anti-attack loss, La）指导对抗样本生成与对抗训练。
- 网络结构采用 **U-Net** 作为主干，并引入**基于Transformer的防御精炼模块（DRM）**，实现“粗到细”的噪声过滤与特征精炼。

### 关键技术细节
1. **对抗样本生成（PGD攻击）**：
   - 对红外和可见光图像分别生成扰动 δ_ir、δ_vis，约束为 l∞范数 ≤ ε（ε=4/255）。
   - 迭代公式（以红外为例）：δ^{k+1}_ir = δ^k_ir + α · sign(∇_{x_ir+δ^k_ir} L_adv(N((x_ir+δ^k_ir, x_vis+δ^k_vis); θ), y))，其中 L_adv 是反攻击损失的一部分，y 是伪标签。
2. **对抗训练**：
   - 目标函数为 min-max 形式：min_θ E[ max_{||δ||_∞≤ε} L_adv(N(x_adv_ir, x_adv_vis; θ), y) ]。
   - 训练时同时输入干净样本对和对抗样本对，分别计算损失 L_clean 和 L_adv，总损失 La = L_clean + L_adv，确保两者平衡。
   - 伪标签由简单CNN（使用 l1+SSIM 损失训练）生成，避免偏向某一种SOTA方法。
3. **网络结构（A²RNet）**：
   - 主干：U-Net，利用上/下采样操作过滤噪声。
   - DRM：置于U-Net瓶颈层与跳跃连接中（连接Encoder-1/3到Decoder-4/2），包含5个对抗鲁棒块（ARB）。
   - ARB：基于Transformer，包含自注意力层和前馈层。自注意力中使用**Mercer核函数**对Q、K进行特征映射，增强鲁棒表示；同时调整乘法顺序（先K×V），将复杂度从O(N²)降至O(N)。
   - 上/下采样采用 **PixelShuffle/Unshuffle** 操作。

### 算法流程（文字说明）
- 每个epoch：对每个mini-batch，先用PGD迭代生成对抗样本（迭代次数I=3）；然后将干净样本对和对抗样本对分别输入网络，计算L_clean和L_adv，反传更新网络参数θ。

## 3. 实验设计
- **数据集**：M³FD 和 MFNet 两个公开数据集，用于训练和测试。
- **Benchmark**：与7种SOTA方法对比，包括 TarDAL、SeAFusion、IGNet、PAIF、CoCoNet、LRRNet、EMMA。PAIF是唯一具有鲁棒性的对比方法。
- **评价指标**：
  - 融合质量：EN、SD、PSNR、CC、SCD。
  - 下游任务：目标检测（mAP@.5）、语义分割（mIoU）。
- **对抗设置**：推断时PGD迭代次数I=20，ε=4/255，α=1/255，l∞约束。

## 4. 资源与算力
- 论文明确说明：所有实验在一台 Intel(R) Xeon(R) Gold 6271C CPU 和一块 NVIDIA Tesla A100 GPU 上完成，使用 PyTorch 框架。
- 训练参数：批量大小4，总epoch=50，优化器Adam，学习率0.001。
- 未提及具体的训练时长、GPU数量等详细信息。

## 5. 实验数量与充分性
- **实验组数**：
  - 主实验：在M³FD和MFNet上进行了融合结果定性对比（多组场景）、定量指标对比（EN/SD/PSNR/CC/SCD柱状图）。
  - 下游任务：检测任务（M³FD数据集）和分割任务（MFNet数据集）的定性/定量结果（mAP@.5和mIoU表）。
  - 消融实验：模块消融（U-Net vs. DRM）、对抗训练消融（AT vs. non-AT）、损失函数消融（La vs. w/o La），并展示了定性图和定量表。
- **充分性评价**：
  - 实验覆盖全面：包括融合本身和两个典型下游任务，消融实验验证了各核心组件的作用。
  - 对比方法均为近期SOTA，且统一使用相同攻击参数，公平性较好。
  - 定量指标选取合理（5种融合指标+2种下游指标）。
  - 不足之处：未在不同攻击强度（不同ε）下进行鲁棒性测试，也未与其他防御方法（如PAIF的不同变体）进行更深层比较。

## 6. 主要结论与发现
- A²RNet在对抗攻击下生成的融合图像在定性（视觉无伪影、细节保留好）和定量（EN等指标最高）上均显著优于现有方法。
- 下游任务（检测和分割）在攻击下的性能下降最小，mAP@.5和mIoU均为最优或次优。
- 消融实验证明：U-Net主干可过滤大部分扰动，DRM进一步精炼特征并补充缺失细节；对抗训练和反攻击损失La对鲁棒性至关重要，且La需要与网络结构协同工作而非即插即用。

## 7. 优点
- **方法创新性**：首次为IVIF任务设计端到端的对抗攻击与训练范式，并明确提出“融合鲁棒性”这一新目标。
- **网络设计精巧**：U-Net+DRM的组合能有效过滤噪声同时保持细节；DRM中的Mercer核函数自注意力降低了复杂度并增强了鲁棒性。
- **实验充分**：涵盖融合和两个下游任务，消融实验完整，对比方法先进且设置公平。
- **实用价值高**：在安全敏感的自动驾驶等领域具有重要应用潜力，且代码开源（GitHub链接提供）。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了一种攻击强度（ε=4/255），未分析不同扰动幅度下的鲁棒性变化；也未考虑其他攻击方式（如FGSM、CW等）。
- **伪标签依赖**：使用简单CNN生成伪标签，若标签质量不佳可能限制上限；未对比直接使用SOTA融合结果作为伪标签的效果。
- **计算资源消耗**：对抗训练需要额外生成对抗样本，训练时间较长，但论文未提供具体训练时间对比。
- **应用限制**：仅针对红外-可见光双模态，未推广到其他多模态融合任务（如医学图像、遥感图像）。
- **未考虑白盒/黑盒攻击**：仅采用白盒PGD攻击，未评估对黑盒攻击的迁移鲁棒性。

（完）
