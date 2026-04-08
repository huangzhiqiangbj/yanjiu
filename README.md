# Cursor Skills Collection

这个仓库整理了我当前在项目中使用的一组 `Cursor Agent Skills`，内容来自原项目里的 `.cursor/skills/`，已单独抽出为可公开分享的 skills 仓库。

## 适用方式

你可以把其中某个 skill，或整个仓库，放到以下位置使用：

- 个人级：`~/.cursor/skills/`
- 项目级：`.cursor/skills/`

常见安装方式：

```bash
# 方式 1：把整个仓库克隆到个人 skills 目录
git clone <your-repo-url> ~/.cursor/skills

# 方式 2：把仓库克隆到任意位置，再复制部分 skill
cp -R <repo>/<skill-name> ~/.cursor/skills/
```

## 仓库内容

当前包含这些顶层 skill 目录：

- `autoresearch`
- `china-edtech-policy-expert`
- `document-to-html-presentation`
- `education-insight-architect`
- `feixiang-product-expert`
- `format-word-documents`
- `humanizer`
- `k12-ai-education-frontier-weekly-report`
- `k12-ai-research-radar`
- `k12-edtech-research`
- `openclaw-skill-vetter`
- `pua`
- `research-writing-skill`
- `scientific-brainstorming`
- `self-improving-agent-master`
- `three-review-three-proof-verifier`
- `visualization`

其中有些目录本身是“套件型 skill”，会包含子 skill 或附属资源，例如：

- `research-writing-skill`
- `visualization`
- `k12-ai-education-frontier-weekly-report`

## 主要主题

这组 skills 主要覆盖以下场景：

- 中国教育科技、AI 教育与政策研究
- 基础教育AI学术文献检索、候选筛选与研究雷达
- 周报、研究简报、Word/HTML 交付工作流
- 图表、信息图、S2 交叉表等可视化能力
- 学术写作、文献综述、统计分析与图表生成
- 文本去 AI 味、质量校验、自我复盘与技能安全审查

## 说明

- 仓库保留了原有 skill 的目录结构，便于直接迁移到 Cursor 的 skills 目录中使用。
- 少数 skill 带有明显的中文语境、教育行业语境或特定产品/组织语境，公开使用前请自行确认是否符合你的场景。
- 若你希望他人可以明确复用和二次分发，建议后续再补充 `LICENSE`。
