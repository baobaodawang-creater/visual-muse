---
name: workflow-agent
description: "选择并改写 ComfyUI 工作流模板，输出可直接提交到 ComfyUI API 的完整 JSON。当需要准备渲染任务、选择模型、调整参数时触发。"
metadata: { "openclaw": { "emoji": "⚙️" } }
---

# Workflow Agent

管理和组装 ComfyUI 工作流。

## 模板位置

`/home/node/.openclaw/workspace/workflows/`

## 工作流程

1. 根据 Prompt Agent 的输出选择模板（如 `sdxl_basic.json`）。
2. 用 Python `json` 模块读取模板，替换以下字段：
   - 节点 `6` 的 `inputs.text` → 正向 prompt
   - 节点 `7` 的 `inputs.text` → 反向 prompt
   - 节点 `3` 的 `inputs.seed` → 随机整数
   - 节点 `3` 的 `inputs.steps` → 默认 25
   - 节点 `3` 的 `inputs.cfg` → 默认 7.0
   - 节点 `5` 的 `inputs.width` / `inputs.height` → 默认 1024
3. 输出替换后的完整 JSON（不附加解释文本）。

## 模型选择规则

- 默认写实/通用：`sd_xl_base_1.0.safetensors`
- 快速实验：`flux1-schnell-Q5_K_S.gguf`（需要 Flux 专用工作流）
- 用户未指定时用 SDXL

## 参数默认值

| 参数 | 默认值 |
|------|--------|
| steps | 25 |
| cfg | 7.0 |
| width | 1024 |
| height | 1024 |
| sampler | euler |
| scheduler | normal |

## 禁止事项

- 不修改节点连接关系（只改 inputs 值）
- 数值字段不得写成字符串（seed 必须是 int，cfg 必须是 float）
