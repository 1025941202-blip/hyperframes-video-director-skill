# HyperFrames Video Director Skill

一个面向 Codex/Claude 的短视频制作 Skill，用来把中文文案做成可发布的 HyperFrames 竖版视频。

它沉淀了一个完整工作流：

- 文案拆镜头
- Image Gen / Image2 补画面
- 录屏只放在合适段落
- 使用用户指定或克隆声音配音
- 字幕按最终配音重新对齐
- 导出前做遮挡、字幕、节奏、实际 MP4 自查

## Skill

- `hyperframes-video-director/`

## 适合场景

- 用 HyperFrames 根据文案生成视频
- Codex 自动剪视频
- 给视频加 AI 图片、录屏、动画标题
- 用自己的克隆声音做配音
- 修字幕太低、太小、换行、遮挡、不同步
- 导出前做成片自查

## 安装

For Codex:

```bash
cp -R hyperframes-video-director ~/.codex/skills/
```

For Claude Code:

```bash
cp -R hyperframes-video-director ~/.claude/skills/
```

## 核心原则

文案决定视频内容，最终配音决定字幕时间。

不要在换配音后沿用旧字幕时间轴。不要因为某一段录屏遮挡，就把所有录屏都删掉。录屏应该只在讲操作、界面、真实流程时出现。

## 可选声音克隆路径

原始参考路径：

```text
/Users/mac/Desktop/Obsidian/AI声音克隆
```

同事使用时可以自己选择：

- 如果机器上也有这个项目，就用这个路径。
- 如果路径不存在，就换成自己的声音克隆项目路径。
- 公开 GitHub 时只记录路径和使用规则，不上传模型权重、声音样本或生成音频。

## 推荐交付流程

1. 锁定最终文案。
2. 生成或导入最终配音。
3. 根据文案拆镜头。
4. 在合适片段插入 Image Gen / Image2 图片或录屏。
5. 按最终配音重新切字幕时间。
6. 用 HyperFrames 生成动画和字幕。
7. 运行 lint 和 inspect。
8. 导出 MP4。
9. 抽关键帧或 contact sheet 做真实画面自查。
10. 给用户最终视频路径和检查结果。

## 公开前审稿点

这版是待审稿版本。公开 GitHub 前建议确认：

- Skill 名称是否合适。
- 是否要写入更多你团队内部的声线克隆路径或工具名。
- 是否要补一份示例项目。
- 是否要中英双语 README。
