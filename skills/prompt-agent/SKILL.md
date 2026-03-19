---
name: prompt-agent
description: "将中文创意需求转换为 SDXL 或 Flux 可用的高质量英文图像提示词。当用户要求生成图片、画一张图、出图、AI绘画时触发。"
metadata: { "openclaw": { "emoji": "🎨" } }
---

# Prompt Agent

将用户的中文需求转成可执行的英文 prompt。

## 第一步：读取风格模板库

```bash
cat /home/node/.openclaw/workspace/prompt-templates.json
```

根据用户需求匹配最合适的风格模板。匹配规则：
- 用户说"电影感/电影风" → cinematic
- 用户说"动漫/二次元/卡通" → anime
- 用户说"写实/照片/真实" → photorealistic
- 用户说"概念艺术/概念图" → concept_art
- 用户说"水彩" → watercolor
- 用户说"油画" → oil_painting
- 用户说"赛博朋克" → cyberpunk
- 用户说"奇幻/魔幻" → fantasy
- 用户没指定风格 → cinematic（默认）

## 第二步：生成 prompt

基于用户描述 + 匹配到的模板，生成英文 prompt。

规则：
1. 用户描述翻译成英文作为主体
2. 拼接模板的 suffix
3. negative 基础：`(worst quality:1.4), (low quality:1.4), blurry, deformed hands, extra fingers, bad anatomy, watermark, text, signature`
4. 拼接模板的 negative_extra

## 第三步：输出 JSON

只输出 JSON，不附加解释：

```json
{
  "positive": "用户描述英文 + 模板suffix",
  "negative": "基础negative + 模板negative_extra",
  "model": "sdxl",
  "style": "匹配到的风格名",
  "recommended_checkpoint": "模板推荐的checkpoint",
  "style_tags": ["标签1", "标签2"]
}
```

## 禁止事项

- 不输出 markdown 代码块
- 不输出解释或前言
- 不使用空泛词
