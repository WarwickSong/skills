---
name: "tensor-level-explainer"
description: "Explains AI/ML concepts at tensor level. Invoke when user asks how a model mechanism works using shapes, formulas, and CFG-style intuition."
---

# Tensor-Level Explainer

Use this skill when the user wants an AI/ML concept explained in a concrete, engineering-style way: tensor shapes, model inputs/outputs, formulas, and step-by-step intuition.

This skill is especially useful for concepts such as VAE, CFG, diffusion denoising, latent space, attention, conditional velocity fields, flow matching, rectified flow, LoRA, ControlNet, U-Net, DiT, and sampling.

## Core Goal

Do not only explain “what it is”. Explain “what tensors exist at each step, what shape they have, and how they are combined”.

The user prefers explanations similar to this CFG pattern:

```text
x_t:         16 × 16 × 64
pred_uncond: 16 × 16 × 64
pred_cond:   16 × 16 × 64

pred_cfg = pred_uncond + w * (pred_cond - pred_uncond)
```

Then explain the formula in plain language:

```text
差 = pred_cond - pred_uncond
放大后的差 = w × 差
新预测 = pred_uncond + 放大后的差
```

## Response Style

When this skill is invoked, answer in the user's language. Prefer concise Chinese if the user asks in Chinese.

Use this structure:

1. Directly answer the user's suspected interpretation.
2. Correct inaccurate wording gently.
3. Pick a concrete example shape.
4. List the key tensors and their shapes.
5. Write the core formula.
6. Translate the formula into plain-language steps.
7. Separate mathematical meaning from engineering implementation.
8. End with a compressed one-sentence understanding.

## Explanation Template

```text
对，你可以这么理解，但有一个地方要改得更准确：

[纠正术语或误解]

假设当前输入是：

input: H × W × C

模型会输出 / 使用这些 tensor：

tensor_a: H × W × C
tensor_b: H × W × C

核心公式是：

output = ...

也就是：

第一步：...
第二步：...
第三步：...

所以一句话压缩：

[一句话理解]
```

## Important Preferences

- Use `tensor` / `张量`, not `TensorFlow`, unless referring to the TensorFlow framework.
- Avoid vague metaphors unless immediately connected back to tensors.
- Always show what has the same shape and what changes shape.
- If the concept involves latent diffusion, distinguish image space and latent space.
- If the concept involves CFG, make clear whether the model predicts noise, velocity, x0, or another target.
- If the concept involves VAE, distinguish downsampling from the “variational” part.
- If the concept involves velocity fields, explain that the output is a direction/update field with the same shape as the current state.

## VAE-Specific Guidance

For VAE, clarify:

```text
普通 AE：
image → Encoder → fixed latent z

VAE：
image → Encoder → mu, logvar → sample z → Decoder → reconstructed image
```

Use example shapes:

```text
image: 512 × 512 × 3
mu:    64 × 64 × 4
logvar:64 × 64 × 4
z:     64 × 64 × 4
```

Core formula:

```text
std = exp(0.5 * logvar)
z = mu + std * epsilon
epsilon ~ N(0, 1)
```

Clarify:

- “变分” mainly means the encoder outputs a probability distribution, usually parameterized by `mu` and `logvar`.
- Different resolution is caused by encoder/decoder downsampling and upsampling, not by “variational” itself.

## CFG-Specific Guidance

For CFG, use:

```text
pred_cfg = pred_uncond + w * (pred_cond - pred_uncond)
```

Clarify:

- `w = 0`: unconditional.
- `w = 1`: normal conditional prediction.
- `w > 1`: stronger push toward condition.

If the model predicts velocity instead of noise, replace `pred` with `v`:

```text
v_cfg = v_uncond + w * (v_cond - v_uncond)
```

## Conditional Velocity Field Guidance

Explain with:

```text
x_t: 16 × 16 × 64
t: scalar or time embedding
c: condition embedding
v_theta(x_t, t, c): 16 × 16 × 64
```

Intuition:

```text
当前 latent x_t
+ 按时间 t 和条件 c 预测出的速度 / 方向 v
→ 采样器沿这个方向更新 x_t
```

If CFG is used:

```text
v_uncond = model(x_t, t, empty_condition)
v_cond   = model(x_t, t, text_condition)
v_cfg    = v_uncond + w * (v_cond - v_uncond)
```

## User Prompt Template to Recommend

When the user asks how to get this explanation style from another model, recommend:

```text
请用 tensor-level 的方式解释 [概念名]。

要求：
1. 假设一个具体输入 shape。
2. 说明模型每一步输出哪些 tensor，它们的 shape 是什么。
3. 写出核心公式。
4. 解释每个 tensor 的直觉含义。
5. 区分数学原理和工程实现。
6. 最后用一句话总结成“可以怎么理解”。
7. 如果我有误解，请像纠正 TensorFlow/tensor 那样直接指出来。
```

Short version:

```text
请按 CFG 那种方式解释：给出输入 shape、模型输出的 tensor、公式、每一步在算什么，以及直觉理解。
```
