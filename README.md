# kids-science-video

把儿童科普 / 绘本做成带配音的讲解视频的 Claude 技能包：事实核查过的讲稿、准确的代码动画、TTS 配音，最终交付 1080p MP4（附 SRT 字幕）。

## 包含的技能

| 技能 | 作用 |
|---|---|
| [`kids-science-video`](skills/kids-science-video/SKILL.md) | 核心流程：讲稿 → 动画 → 配音 → 渲染交付，适用任何主题 |
| [`kids-video-animals`](skills/kids-video-animals/SKILL.md) | 主题参考：动物、海洋生物、植物、栖息地、沙漠、天气、地理 |
| [`kids-video-machines`](skills/kids-video-machines/SKILL.md) | 主题参考：卡车、工程/救援机械、车辆、火车、飞机、船、机器原理 |

核心技能会根据绘本主题按需加载对应的主题参考。

## 安装

把 `skills/` 下的三个文件夹分别作为技能添加到 Claude（claude.ai 的 Skills 设置里上传，或放进 Claude Code 的 `~/.claude/skills/`）。
