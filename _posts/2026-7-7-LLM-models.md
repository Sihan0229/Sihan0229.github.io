---
layout: post  
title: "【Agent】开源LLMs名称后缀含义"  
date: 2026-7-5 00:10 +0800  
last_modified_at: 2026-7-7 00:10 +0800  
tags: [Agent]  
math: true  r
toc: true  
excerpt: "开源LLMs名称后缀含义"
---
# 【Agent】开源LLMs名称后缀含义

如果你经常在 Hugging Face、ModelScope、Ollama、LM Studio、llama.cpp 社区里下载模型，会看到很多类似下面这样的名字：

```text
Qwen/Qwen3.6-35B-A3B-FP8
Qwen/Qwen3.5-122B-A10B-GPTQ-Int4
Qwen/SAE-Res-Qwen3-8B-Base-W64K-L0_50
empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF
Qwen/Qwen3-ASR-0.6B-hf
```

这些后缀一般用于区分： **模型系列、参数规模、训练阶段、任务类型、上下文长度、MoE 激活参数、量化格式、文件格式、微调来源、社区改版**，但是开源模型命名没有强制统一标准，最可靠的信息永远是仓库里的 `README`、`model card`、`config.json` 和文件列表。

---

## 1. 一个模型名通常包含哪些信息？

以这个名字为例：

```text
Qwen/Qwen3.5-122B-A10B-GPTQ-Int4
```

可以拆成：

```text
Qwen/                     # 发布组织或用户名
Qwen3.5                   # 模型系列 / 版本
122B                      # 总参数量约 122 billion
A10B                      # MoE 每次推理激活约 10B 参数
GPTQ                      # 量化算法
Int4                      # 4-bit 整数量化
```

再看另一个：

```text
Qwen/SAE-Res-Qwen3-8B-Base-W64K-L0_50
```

可以拆成：

```text
Qwen/                     # 发布组织
SAE-Res                   # Sparse Autoencoder，挂在 residual stream 上
Qwen3-8B-Base             # 目标基础模型
W64K                      # SAE 宽度 64K，不是上下文长度
L0_50                     # 稀疏度：每次前向大约保留 50 个非零特征
```

此外，更多信息可以结合仓库说明。例如 Qwen 的 SAE-Res 模型写了 `SAE width = 65536`、`Top-K = 50`、hook point 是 residual stream；`L0_100` 版本则是每次前向保留 100 个非零特征。

---

## 2. 参数规模：0.6B、1.7B、8B、9B、27B、122B、397B

`B` 是  **billion parameters** ，即十亿参数。

常见写法：

| 后缀     | 含义           |
| -------- | -------------- |
| `0.6B` | 约 6 亿参数    |
| `1.7B` | 约 17 亿参数   |
| `8B`   | 约 80 亿参数   |
| `9B`   | 约 90 亿参数   |
| `27B`  | 约 270 亿参数  |
| `35B`  | 约 350 亿参数  |
| `122B` | 约 1220 亿参数 |
| `397B` | 约 3970 亿参数 |

注意：仓库名里的参数量通常是近似值，Hugging Face 页面显示的参数量也可能和模型名略有不同。例如 `Qwen3-8B-Base` 的模型卡写的是总参数 8.2B、非 embedding 参数 6.95B；因此看到 `8B` 不应理解为精确等于 8,000,000,000 个参数。

对使用者来说，参数量大致决定三件事：

1. **显存/内存需求** ：参数越多，模型权重越大。
2. **推理速度** ：参数越多，一般越慢。
3. **能力上限** ：参数越多不必然更好，但通常推理、知识、复杂任务能力更强。

权重大小不仅仅取决于参数量，还取决于精度：

```text
FP16/BF16 权重大小 ≈ 参数量 × 2 bytes
FP8 权重大小      ≈ 参数量 × 1 byte
Int4 权重大小     ≈ 参数量 × 0.5 byte + scale/metadata 开销
```

例如 8B 模型如果是 BF16，光权重约 16GB；如果是 Int4，权重大约 4GB 多一点。实际运行还要加上 KV cache、激活、框架开销、视觉模块、tokenizer 等额外占用。

---

## 3. Dense 模型与 MoE 模型

很多新模型名里会出现：

```text
30B-A3B
35B-A3B
122B-A10B
397B-A17B
```

这里的 `A` 通常表示  **activated parameters** ，也就是 MoE 模型每个 token 实际激活参与计算的参数量。

例如：

```text
Qwen3.6-35B-A3B
```

表示模型总参数约 35B，但每次推理时只激活约 3B 参数。Qwen3.6 的模型卡也写明：语言模型总参数 35B，激活参数 3B。

### Dense 与 MoE 的区别

| 类型  | 例子                          | 含义                                      |
| ----- | ----------------------------- | ----------------------------------------- |
| Dense | `Qwen3-8B`、`Qwen3.5-27B` | 每个 token 基本都会经过全部主干参数       |
| MoE   | `30B-A3B`、`122B-A10B`    | 总参数很多，但每个 token 只路由到部分专家 |

MoE 的好处是： **存储很大，但计算量相对小** 。例如一个 `35B-A3B` 模型，磁盘/显存里要放接近 35B 的权重，但单 token 计算量更接近激活的 3B，而不是完整 35B。

但 MoE 需要加载很多专家权重，推理框架要支持 MoE 路由，且多卡部署时专家切分、通信、负载均衡会更复杂。对本地部署来说，MoE 的“显存占用”通常按总参数看，“速度”才更接近激活参数。

---

## 4. 训练阶段与任务类型：Base、Instruct、Chat、Thinking、Code、ASR、ForcedAligner

这些后缀通常表示模型的训练阶段或任务类型。

### `Base`

```text
Qwen3-8B-Base
Qwen3.5-9B-Base
```

`Base` 是基础预训练模型。它主要学的是语言建模、知识、代码、推理模式，但没有充分做指令对齐。它不一定适合直接聊天。

适合：

* 继续预训练
* SFT 微调
* RLHF / DPO / GRPO 后训练
* 表征分析
* 机制可解释性
* 做 SAE、探针、特征分析

不适合普通用户直接当 ChatGPT 用。

### `Instruct/Chat`

```text
Qwen3-30B-A3B-Instruct
```

`Instruct` 是指令微调版本。它被训练成能听懂“请帮我写代码”“总结这篇文章”“翻译下面内容”等自然语言指令。

适合：

* 聊天
* 问答
* 写作
* 代码助手
* agent
* 工具调用

早期模型常用 `Chat`，现在很多模型改用 `Instruct`。两者含义接近：都表示面向对话和指令跟随的模型。

### `Thinking`

```text
Qwen3-30B-A3B-Thinking-2507
```

`Thinking` 通常表示模型被设计成更倾向于长链式推理。Qwen3 系列文档里区分了 `Instruct` 和 `Thinking` 模式，并说明部分版本支持通过 chat template 或 `/think`、`/no_think` 控制思考模式。

适合：

* 数学
* 复杂代码
* 逻辑推理
* 多步规划
* agent 任务

缺点是延迟更高、输出更长、成本更高。

### `Code` / `Coder`

```text
Qwen3.5-9B-Claude-Code
Qwen2.5-Coder-7B-Instruct
Qwen3-Coder-30B-A3B-Instruct
```

表示代码方向强化过。通常更擅长：

* 写代码
* 修 bug
* 解释项目
* 代码补全
* repo-level reasoning
* tool use / agentic coding

但不代表通用聊天一定比非 Code 版本更好。

### `ASR`

```text
Qwen3-ASR-0.6B-hf
Qwen3-ASR-1.7B-hf
```

`ASR` 是  **Automatic Speech Recognition** ，自动语音识别。它不是普通文本生成模型，而是把音频转成文字。Qwen3-ASR 技术报告称该系列包括 0.6B 和 1.7B ASR 模型，并支持多语言语音识别。

### `ForcedAligner`

```text
Qwen3-ForcedAligner-0.6B-hf
```

`Forced Aligner` 是强制对齐模型。它通常用于把已有文本和语音对齐，输出更精确的时间戳。例如一句话里每个词、每个 token 对应音频的哪个时间段。

适合：

* 字幕对齐
* 语音数据清洗
* TTS / ASR 数据集制作
* 语音标注

### `Image` / `Image-Text-to-Text` / `Vision`

```text
Qwen-Image-Bench
Qwen3.6-35B-A3B
Image-Text-to-Text
```

这类一般和视觉语言模型有关，即 VLM。它能接收图片和文字，输出文字。Hugging Face 页面上的 `Image-Text-to-Text` 是任务标签，不一定是模型名后缀。Qwen3.6-35B-A3B 的模型卡显示它是带视觉编码器的因果语言模型。

---

## 5. hf、HF、Transformers：`-hf` 是什么意思？

例如：

```text
Qwen3-ASR-0.6B-hf
Qwen3-ForcedAligner-0.6B-hf
```

`hf` 通常表示  **Hugging Face / Transformers 格式** 。也就是说，这个仓库更倾向于用：

```python
from transformers import AutoModel...
```

这类方式加载。

它不等于：

* half precision
* high fidelity
* hidden features

在模型命名里，`hf` 最常见的意思就是 Hugging Face 兼容格式。

与之相对，社区里还会看到：

| 后缀      | 常见含义                                |
| --------- | --------------------------------------- |
| `-hf`   | Hugging Face / Transformers 格式        |
| `-GGUF` | llama.cpp / Ollama / LM Studio 常用格式 |
| `-MLX`  | Apple MLX 生态格式                      |
| `-GPTQ` | GPTQ 量化格式                           |
| `-AWQ`  | AWQ 量化格式                            |
| `-EXL2` | ExLlamaV2 常用量化格式                  |

---

## 6. 上下文长度：32K、64K、128K、1M

常见写法：

```text
32K
64K
128K
1M
1M-context
```

如果出现在普通 LLM 名称里，它通常表示  **上下文窗口长度** ，即模型一次能看多少 token。例如：

```text
Qwythos-9B-Claude-Mythos-5-1M
```

这里的 `1M` 大概率指 1M token 上下文。该仓库的模型卡也明确写到：模型通过 YaRN rope-scaling 默认支持 1,048,576-token，也就是约 1M token 上下文。

但是要注意两个问题。

第一， **能标 1M 不等于本地真的能跑 1M** 。长上下文会让 KV cache 急剧增长。哪怕权重能放进显存，KV cache 也可能放不下。

第二，`W64K` 不一定是上下文长度。比如 Qwen 的 SAE-Res 仓库中，`W64K` 表示 SAE width = 65536，不是 64K 上下文。

判断方法：

* 如果名字类似 `Base-W64K-L0_50`，并且前面有 `SAE`，多半是 SAE 宽度。
* 如果名字类似 `LongContext-128K`、`1M-context`，才更可能是上下文长度。
* 最终以 model card 和 config 为准。

---

## 7. SAE、SAE-Res、W64K、L0_50、L0_100

这类不是普通聊天模型，而是可解释性工具模型。

### `SAE`

`SAE` 是  **Sparse Autoencoder** ，稀疏自编码器。它常用于机制可解释性，把模型隐藏层里的高维激活分解成更稀疏、更可解释的特征。

### `SAE-Res`

`Res` 一般指 residual stream，即 Transformer block 中的残差流。`SAE-Res` 表示这个 SAE 挂在 residual stream 上，而不是 MLP、attention output 或其他 hook point 上。

Qwen-Scope 的模型卡说明，它在 Qwen3 和 Qwen3.5 系列隐藏层中训练了 Sparse Autoencoders，用于提取更解耦、更低冗余、更可解释的特征。

### `W64K` / `W32K` / `W80K` / `W128K`

在 SAE 命名里，`W` 多半是  **width** ，表示 SAE 的特征字典宽度。

例如：

```text
W64K = SAE width 65536
W32K = SAE width 32768
W80K = SAE width 81920 左右
W128K = SAE width 131072
```

这不是模型上下文长度。

### `L0_50` / `L0_100`

`L0` 指稀疏激活的 L0 范数，也就是非零特征数量。

```text
L0_50  = 每次前向大约保留 50 个非零 SAE 特征
L0_100 = 每次前向大约保留 100 个非零 SAE 特征
```

Qwen 的 `L0_50` 模型卡写的是 Top-K 50；`L0_100` 模型卡写的是 Top-K 100。

通俗理解：

```text
W64K-L0_50 = 有 65536 个可解释特征，但每个 token 只激活其中 50 个
```

---

## 8. 量化后缀：FP16、BF16、FP8、Int8、Int4

量化的目标是降低显存和内存占用。Hugging Face Transformers 文档说明，量化通过用更低精度的数据类型表示权重和激活来降低内存与计算成本，并支持 GPTQ、AWQ、bitsandbytes 的 8-bit/4-bit 等方案。

### `FP16` / `F16`

16-bit floating point。老牌半精度格式，很多 GPU 支持好。

### `BF16`

Brain Floating Point 16。指数位更宽，训练稳定性通常比 FP16 更好。很多新模型默认用 BF16 发布。

### `FP8`

8-bit floating point。注意，用户有时会写成 `FB8`，但常见正确写法是 `FP8`。

例如：

```text
Qwen3.6-35B-A3B-FP8
Qwen3.5-397B-A17B-FP8
```

Qwen3.6-35B-A3B-FP8 的模型卡明确写到：这是 FP8 量化权重，采用 fine-grained FP8 quantization，block size 为 128，并兼容 Transformers、vLLM、SGLang、KTransformers 等推理框架。

FP8 和 Int8 不同：

| 格式     | 类型       | 特点                       |
| -------- | ---------- | -------------------------- |
| `FP8`  | 8-bit 浮点 | 有指数和尾数，动态范围较好 |
| `Int8` | 8-bit 整数 | 通常需要 scale/zero point  |
| `Int4` | 4-bit 整数 | 更省显存，但更容易损失精度 |

### `Int4`

4-bit integer quantization。常见于：

```text
GPTQ-Int4
AWQ-Int4
bnb-4bit
Q4_K_M
```

它比 FP16/BF16 小很多，但可能带来精度下降。一般适合本地部署或显存不足时使用。

---

## 9. GPTQ、AWQ、bitsandbytes、GGUF 量化

很多人把 `Int4` 和 `GGUF` 混在一起，其实它们是不同层面的概念。

### `GPTQ`

```text
Qwen3.5-122B-A10B-GPTQ-Int4
```

`GPTQ` 是一种后训练量化算法。Hugging Face 文档解释，GPTQ 会逐行量化权重矩阵，以最小化量化误差；常见配置是 int4，推理时通过 fused kernel 反量化到 fp16 参与计算，从而节省显存并提升传输效率。

适合：

* GPU 推理
* Transformers / vLLM / AutoGPTQ / GPTQModel 生态
* 想加载大模型但显存不足

### `AWQ`

`AWQ` 是 Activation-aware Weight Quantization。它根据激活分布保护重要权重通道，常见也是 4-bit。Transformers 文档支持 AWQ 模型加载；Qwen 文档也介绍过 AutoAWQ 用于 4-bit 量化模型。

### `bitsandbytes`

常见写法不一定出现在模型名里，但你会在代码里看到：

```python
load_in_4bit=True
bnb_4bit_quant_type="nf4"
```

典型后缀：

```text
bnb-4bit
NF4
FP4
```

适合训练和微调场景，尤其是 QLoRA。

### `GGUF`

```text
Qwen3-8B-GGUF
Qwythos-9B-Claude-Mythos-5-1M-GGUF
```

`GGUF` 是一种模型文件格式，不是单一量化算法。Hugging Face 文档说明，GGUF 是为 GGML 和相关执行器设计的二进制格式，优化了模型快速加载和推理；它可以包含张量和标准化 metadata。

GGUF 常用于：

* llama.cpp
* Ollama
* LM Studio
* KoboldCpp
* Jan
* 本地 CPU/GPU 混合推理

GGUF 文件内部可以是多种精度或量化版本：

```text
BF16.gguf
F16.gguf
Q8_0.gguf
Q6_K.gguf
Q5_K_M.gguf
Q4_K_M.gguf
Q3_K_M.gguf
Q2_K.gguf
```

所以：

```text
GGUF = 文件格式 / 推理生态
Q4_K_M = GGUF 文件里的具体量化类型
```

---

## 10. GGUF 里的 Q4_K_M、Q5_K_M、Q6_K、Q8_0

在 GGUF 仓库里，一个模型经常会提供多个文件：

```text
model-Q4_K_M.gguf
model-Q5_K_M.gguf
model-Q6_K.gguf
model-Q8_0.gguf
model-BF16.gguf
```

大致可以这样理解：

| 后缀       | 粗略含义                | 适合场景                        |
| ---------- | ----------------------- | ------------------------------- |
| `BF16`   | BF16 精度，接近原始权重 | 显存/内存充足，想尽量保质量     |
| `F16`    | FP16 精度               | 显存/内存充足                   |
| `Q8_0`   | 8-bit 量化              | 质量接近原模型，体积仍比 F16 小 |
| `Q6_K`   | 6-bit K-quant           | 质量较高，体积适中              |
| `Q5_K_M` | 5-bit K-quant medium    | 质量/大小均衡                   |
| `Q4_K_M` | 4-bit K-quant medium    | 本地部署常用默认选择            |
| `Q3_K_M` | 3-bit 量化              | 更省内存，质量下降更明显        |
| `Q2_K`   | 2-bit 量化              | 极限压缩，质量风险较大          |

`K` 是 llama.cpp / GGML 生态中的 K-quant 系列，`M` 通常表示 medium 变体。实际含义涉及不同张量采用不同量化策略，不建议只按字面理解。

经验选择：

```text
质量优先：BF16 / F16 / Q8_0
均衡选择：Q5_K_M / Q4_K_M
低显存：Q4_K_M / Q3_K_M
极限能跑：Q2_K，但质量可能明显下降
```

有些模型卡会给出推荐。例如 Qwythos 的 GGUF 仓库列出 `Q4_K_M`、`Q5_K_M`、`Q6_K`、`Q8_0`、`BF16` 等文件，并说明 `Q4_K_M` 是不知道怎么选时的起点，`Q8_0` 接近无损。

---

## 11. safetensors、PyTorch `.bin`、`.pt`、GGUF

模型文件格式也经常出现在名称或文件列表里。

### `.safetensors`

Hugging Face 官方推荐的安全张量存储格式。Safetensors 文档说明，它相比 pickle 更安全，同时支持快速、零拷贝读取。

常见于：

```text
model-00001-of-00008.safetensors
model.safetensors
```

适合 Transformers、vLLM、TGI 等服务框架。

### `.bin`

老式 PyTorch 权重格式。现在逐渐被 `.safetensors` 替代。

### `.pt` / `.pth`

PyTorch 保存文件。SAE、LoRA、adapter、研究性 checkpoint 经常用 `.pt`。

### `.gguf`

llama.cpp / Ollama / LM Studio 生态常用。它不仅存权重，还可以包含 tokenizer、chat template、metadata 等信息。GGUF 官方文档也强调它包含加载模型所需的信息，并设计为可扩展格式。

---

## 12. Distill、SFT、DPO、RLHF、Merge、Abliterated

这些后缀描述模型是如何被改造的。

### `Distill`

```text
Qwen3.5-9B-Claude-Opus-4.6-Distill
openNemo-9B-Claude-Opus-4.6-distill
```

`Distill` 是蒸馏。通常表示用更强模型的输出、偏好、推理链或合成数据训练较小模型。

但名字里出现 `Claude`、`Opus` 不代表它是 Anthropic 官方 Claude 模型，也不代表包含 Claude 权重。更多时候，它表示训练数据、输出风格、蒸馏来源或社区命名。具体是否合法、数据来源是否清晰，需要看模型卡。

### `SFT`

Supervised Fine-Tuning，有监督微调。用指令-回答数据训练模型。

### `DPO`

Direct Preference Optimization，用偏好对训练模型，让模型更偏向被选中的回答。

### `RLHF`

Reinforcement Learning from Human Feedback，通过人类偏好训练奖励模型，再强化学习优化模型。

### `Merge`

模型合并。社区常把多个微调模型通过权重插值、task arithmetic、mergekit 等方法合并。

常见名字：

```text
modelA-modelB-merge
Daredevil-8B
Nous-Hermes-Mix
```

这种模型有时很强，有时只是“玄学调参”。必须看评测和数据来源。

### `Abliterated`

```text
openNemo-9B-abliterated
openNemo-9B-abliterated-GGUF
```

`Abliterated` 通常指对模型的拒答方向做了干预，削弱安全拒答机制。Hugging Face 上一篇相关博客把 abliteration 描述为一种不重新训练也能移除模型内置拒答机制的方法；相关研究也将其与 residual stream 中的 refusal direction 联系起来。

它的含义大致是：

```text
更少拒答 / 更少安全对齐 / 更“uncensored”
```

风险是：

1. 安全性下降。
2. 可能更容易输出有害内容。
3. 原本能力可能被破坏。
4. 在正式应用中合规风险更高。

普通研究、部署、产品场景不建议优先选 abliterated 模型。

---

## 13. Claude、Opus、Fable、Mythos

例如：

```text
Qwythos-9B-Claude-Mythos-5-1M
Qwable-9B-Claude-Fable-5
Qwen3.5-9B-Claude-Code
Qwen3.5-9B-Claude-Opus-4.6-Distill
```

这些通常属于 **社区命名或训练数据命名** ，不是标准架构后缀。可能含义包括：

1. 使用某个强模型生成的数据做蒸馏。
2. 模仿某种回答风格。
3. 使用某个内部数据集，例如 `Mythos`、`Fable`。
4. 面向某种任务，例如 `Code`。
5. 只是作者的品牌命名。

例如 Qwythos 的模型卡写到，它是一个 9B reasoning model，post-trained on Claude Mythos / Claude Fable traces，并支持 1M context。

因此看到这类名字时要谨慎：

```text
Claude-xxx ≠ 官方 Claude
Opus-xxx   ≠ 官方 Claude Opus 权重
Fable      ≠ 标准模型结构
Mythos     ≠ 标准模型结构
```

最稳妥的判断方式是看 model card 里的：

* base model 是谁
* 训练数据来源
* 是否 full-parameter fine-tune
* 是否 LoRA merge
* 是否 distill
* license 是否允许商用
* benchmark 是否可信
* 是否给出训练细节

---

## 14. AgentWorld、WebWorld、Image-Bench、Bench

这些通常是任务、数据集或评测方向。

### `AgentWorld`

```text
Qwen/Qwen-AgentWorld-35B-A3B
```

大概率表示面向 agent 任务、工具调用、网页交互、环境交互或 agentic workflow 的模型。

### `WebWorld`

```text
Qwen/WebWorld-8B
Qwen/WebWorld-14B
Qwen/WebWorld-32B
```

通常表示面向网页任务、web navigation、browser agent、网页信息抽取或网页环境交互。

### `Bench`

```text
Qwen-Image-Bench
```

`Bench` 常表示 benchmark，不一定是可直接聊天的模型。可能是评测集、评测工具、评测模型或基准仓库。看到 `Bench` 一定要先看仓库类型，别直接当普通 LLM 下载。

---

## 15. 多模态相关：mmproj、vision projector、CLIP-style vision encoder

GGUF 多模态模型经常会出现额外文件：

```text
mmproj-model-name-F16.gguf
```

`mmproj` 是 multimodal projector，负责把视觉编码器输出映射到语言模型能理解的空间。

对于视觉语言模型，本地推理时往往需要两个部分：

```text
语言模型 GGUF
+ vision projector / mmproj
```

如果只下载语言模型，不下载 `mmproj`，可能只能文本聊天，不能看图。Qwythos 的 GGUF 仓库文件列表里就单独列出了 vision projector，并说明它用于 image input。

---

## 16. MTP

```text
MTP-enabled
MTP-Q4_K_M
```

`MTP` 通常是  **Multi-Token Prediction** ，多 token 预测。它可以用于 speculative decoding 或 draft speculation，让模型一次预测多个 token，提高推理速度。

但 MTP 需要推理框架支持。不是所有 llama.cpp、vLLM、SGLang 版本都能正确使用 MTP head。

如果你只是普通聊天，优先下载非 MTP 版本即可。只有在明确知道自己的推理框架支持 MTP 时，再考虑 MTP 文件。

更多可以参考[大模型命名后缀解析：看懂参数、量化、蒸馏、微调标识，快速筛选适配本地模型19.6](https://bbs.huaweicloud.com/blogs/481056)
