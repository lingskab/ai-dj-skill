# AI DJ — Codex Skill

先设计 Set 骨架，再根据工作情境决定延续或插入歌曲。

AI DJ 让本地 Codex 结合必要的屏幕线索、音乐偏好和 Apple Music 目录，按用户指定时长设计连贯的音乐路线，并通过 macOS Music.app 原生队列播放。音频衔接由 Apple Music AutoMix 执行。

**当前是实验性的 Skill 指令包。** 不附带独立播放器或后台守护进程。目录访问、Computer Use 和定时能力取决于宿主工具与用户授权。

## 安装

将仓库放入 Codex 能发现的 `skills/ai-dj` 目录，例如：

```sh
git clone https://github.com/lingskab/ai-dj-skill.git ~/.codex/skills/ai-dj
```

如果目标已存在，请先检查并备份自己的版本，不要覆盖个人配置。按当前客户端要求重新加载技能。

本地 Music 控制优先复用 [epheterson/applemusic-mcp](https://github.com/epheterson/applemusic-mcp)。安装 uv 后，可注册固定版本：

```sh
codex mcp add appleMusic -- uvx --from applemusic-mcp==0.21.0 applemusic-mcp serve --level music
```

这只配置Music本地控制。完整目录API需要另行授权；也可以通过已登录Music.app的Computer Use搜索、操作。不要默认启用full权限或读取浏览器会话令牌。具体授权流程以依赖项目当前说明为准。

## 使用

```text
$ai-dj 根据当前工作情境，开始一场45分钟的演出。
$ai-dj 需要提振一点，看看适合继续还是插入下一首。
$ai-dj 停止巡检，音乐继续播放。
```

每次启动先确定目标和情绪/能量/速度路线，不预设曲风配额；逐曲核算时长，再核实歌曲版本和邻接关系。运行中默认延续；明确指令或持续情境变化才调整下一首。插曲同时检查当前→新曲→原下一首，不跳过当前歌曲，不以重启歌单模拟无缝更新。

## 需要什么

- 支持AutoMix的macOS Music.app（以macOS26+为起点，实际设置和输出设备仍须核实）。
- Apple Music订阅及当前地区可播放的歌曲。
- 本地Codex和可用的Computer Use；使用MCP时需安装并配置依赖。
- 持续情境检查需要宿主提供本地调度工具；电脑保持唤醒、解锁、应用运行。

读取屏幕会把必要可见内容提供给Agent用于情境判断；仅保存非敏感摘要，不保存原始私人屏幕内容。可以直接提供手动情境来代替屏幕读取。单次页面不能证明疲劳或情绪。

## 质量边界

本方法考虑能量走势、律动、速度、和声、人声密度与乐句结构。实际拿不到的BPM/调性标记为未知，不虚构已测属性。AutoMix是过渡执行器，不能保证任意顺序达到专业Set效果。

验证分开记录：工具调用成功、真实播放、队列位置、自然执行、AutoMix开关、音频听感、全场耐久。不同机器需重新验证；用户确认的插入结果不当作全自主控制的普遍保证。

## 文件

- [SKILL.md](SKILL.md)：Agent执行入口。
- [编排方法](references/methodology.md)：Set骨架、相邻兼容和实时决策。
- [运行约定](references/runtime.md)：本地工具、权限、原生队列和调度边界。

## 许可

Copyright © 2026 lingskab. 本仓库原创Skill文本与文档采用 [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)，允许在署名、标注修改并遵守许可的前提下作非商业分享和改编。商业使用需另获许可。

这是源码公开的非商业许可项目，不是OSI定义的开源许可项目。音乐、Apple服务、Codex及第三方工具不受本仓库许可授权；各自遵循其权利和条款。此仓库不分发音乐、音频、令牌、个人曲库或第三方依赖代码。

与Apple、OpenAI及依赖项目作者无隶属或背书关系。
