# DeepSeek Obsidian Skills

> 为 [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui) 打包和文档化的 Obsidian 代理技能。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DeepSeek TUI](https://img.shields.io/badge/DeepSeek%20TUI-%3E%3D0.8.x-blue)](https://github.com/deepseek-tui/deepseek-tui)
[![Obsidian](https://img.shields.io/badge/Obsidian-%3E%3D1.5-purple)](https://obsidian.md)

[English](README.md) | [Español](README.es.md) | [中文](README.zh.md)

这些技能让 DeepSeek TUI 能够与你的 Obsidian 仓库交互——读取、创建、搜索、编辑笔记，管理任务，检查属性，甚至开发插件——全部在终端中完成。

---

## 目录

- [什么是 DeepSeek 技能？](#什么是-deepseek-技能)
- [前提条件](#前提条件)
- [安装](#安装)
- [可用技能](#可用技能)
- [验证](#验证)
- [使用示例](#使用示例)
- [限制](#限制)
- [建议与最佳实践](#建议与最佳实践)
- [故障排除](#故障排除)
- [常见问题](#常见问题)
- [贡献](#贡献)
- [许可证](#许可证)
- [鸣谢](#鸣谢)

---

## 什么是 DeepSeek 技能？

技能是扩展 DeepSeek TUI 能力的 markdown 文件。安装后，它们会被注入到系统提示中，为 AI 代理提供关于特定领域工具、约定和工作流程的精确知识。

技能存放于：

```
~/.deepseek/skills/<技能名称>/SKILL.md
```

DeepSeek TUI 在启动时自动发现它们。你可以通过在对话中提及技能名称来调用它，或者当你的请求与其描述匹配时自动激活。

> 了解更多：[DeepSeek TUI 技能文档](https://github.com/deepseek-tui/deepseek-tui)

---

## 前提条件

### 必需

| 要求 | 最低版本 | 备注 |
|------|----------|------|
| **DeepSeek TUI** | 0.8.x 或更高 | 技能支持在 0.8.x 中添加 |
| **Obsidian Desktop** | 1.5.0 或更高 | 使用技能时必须**打开并运行** |
| **Obsidian CLI** | 随 Obsidian 附带 | `obsidian` 二进制文件必须在你的 PATH 中 |

### 可选

| 软件包 | 用途 |
|--------|------|
| Node.js 18+ | 部分 `obsidian dev:*` 命令需要 |
| Git | 配合 CLI 使用，便于仓库版本管理 |

### 操作系统支持

| 操作系统 | 支持情况 | 备注 |
|----------|----------|------|
| **macOS** | ✅ 完全支持 | CLI 路径：`/Applications/Obsidian.app/Contents/Resources/` |
| **Windows** | ✅ 完全支持 | CLI 随安装程序附带 |
| **Linux** | ✅ 完全支持 | AppImage 可能需要解压才能访问 CLI |
| **无头/服务器** | ❌ 不支持 | Obsidian 需要图形桌面环境 |

---

## 安装

### 方法 1：通过 skill-installer（推荐）

如果你在 DeepSeek TUI 中启用了 `skill-installer` 技能：

```
/skill install github:DiegoMartinez-Git/deepseek-obsidian-cli
```

或在对话中：

> "从 GitHub 安装 deepseek-obsidian-cli 技能"

这会克隆仓库并将技能复制到 `~/.deepseek/skills/`。

### 方法 2：手动 — 克隆仓库

```bash
# 克隆到临时位置
git clone https://github.com/DiegoMartinez-Git/deepseek-obsidian-cli.git /tmp/deepseek-obsidian-cli

# 复制你需要的技能
cp -r /tmp/deepseek-obsidian-cli/skills/obsidian-cli ~/.deepseek/skills/obsidian-cli

# 清理
rm -rf /tmp/deepseek-obsidian-cli
```

### 方法 3：手动 — 单文件

如果你只需要 `obsidian-cli`：

```bash
mkdir -p ~/.deepseek/skills/obsidian-cli/references
curl -o ~/.deepseek/skills/obsidian-cli/SKILL.md \
  https://raw.githubusercontent.com/DiegoMartinez-Git/deepseek-obsidian-cli/main/skills/obsidian-cli/SKILL.md
curl -o ~/.deepseek/skills/obsidian-cli/references/COMMANDS.md \
  https://raw.githubusercontent.com/DiegoMartinez-Git/deepseek-obsidian-cli/main/skills/obsidian-cli/references/COMMANDS.md
```

### 设置 Obsidian CLI

安装技能后，确保 `obsidian` 命令可访问：

**macOS：**
```bash
echo 'export PATH="/Applications/Obsidian.app/Contents/Resources:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Linux（AppImage）：**
```bash
# 解压 AppImage（一次性操作）
/path/to/Obsidian-*.AppImage --appimage-extract
# CLI 位于 squashfs-root/usr/bin/obsidian
sudo cp squashfs-root/usr/bin/obsidian /usr/local/bin/
obsidian --version
```

**Windows（PowerShell）：**
```powershell
[Environment]::SetEnvironmentVariable(
  "Path",
  "$env:Path;C:\Users\$env:USERNAME\AppData\Local\Programs\Obsidian",
  "User"
)
```

---

## 可用技能

| 技能 | 描述 | 要求 |
|------|------|------|
| [obsidian-cli](skills/obsidian-cli) | 读取、创建、搜索和管理笔记；每日笔记、任务、属性、标签、反向链接；插件和主题开发，支持热重载、DOM 检查、截图和 JS 执行 | Obsidian Desktop 运行中 |

更多技能即将推出——欢迎贡献。

---

## 验证

安装后，验证一切正常：

### 1. 检查技能文件是否存在

```bash
ls -la ~/.deepseek/skills/obsidian-cli/SKILL.md
# 应显示文件大小 > 1KB
```

### 2. 检查 Obsidian CLI 是否可访问

```bash
obsidian --version
# 应打印版本号
```

### 3. 快速冒烟测试

在 Obsidian 打开且有活跃仓库的情况下：

```bash
obsidian search query="test" limit=1
# 应返回结果或 "No results found"（两者都是有效的）
```

### 4. 在 DeepSeek TUI 中验证

启动新的 DeepSeek TUI 会话。技能应出现在系统提示的 `## Skills` 下。询问：

> "你有哪些可用的技能？"

`obsidian-cli` 应被列出。

---

## 使用示例

安装后，只需自然地与 DeepSeek TUI 对话。技能会自动激活：

### 笔记管理

> "读取我名为 Project Alpha 的笔记"
> → 运行：`obsidian read file="Project Alpha"`

> "创建一篇标题为 Meeting Notes 的新笔记，内容包含今天的日期"
> → 运行：`obsidian create name="Meeting Notes" content="..."`

### 每日笔记和任务

> "我今天有哪些待办任务？"
> → 运行：`obsidian tasks daily todo`

> "在我的每日笔记中添加一个任务：review the PR"
> → 运行：`obsidian daily:append content="- [ ] review the PR"`

### 搜索和导航

> "在我的 Work 仓库中找到所有提到 budget 的笔记"
> → 运行：`obsidian vault="Work" search query="budget"`

> "我最常用的标签是什么？"
> → 运行：`obsidian tags sort=count counts`

### 插件开发

> "重新加载我名为 my-theme 的插件并检查错误"
> → 运行：`obsidian plugin:reload id=my-theme` 然后 `obsidian dev:errors`

> "截取我插件 UI 的截图"
> → 运行：`obsidian dev:screenshot path=screenshot.png`

---

## 限制

### 🔴 严重 — 不支持无头环境

Obsidian 是一个桌面应用程序（基于 Electron）。CLI 与**正在运行的 Obsidian 进程**通信。这意味着：

- ❌ 不支持服务器、容器或无头虚拟机
- ❌ 不支持没有 X 转发的 SSH
- ❌ 不支持 CI/CD 流水线
- ❌ 不支持没有桌面环境的树莓派

如果你需要无头仓库管理，请考虑：
- 直接编辑仓库目录中的 `.md` 文件
- 使用 Git 进行仓库版本管理
- Obsidian Sync API（需要订阅）

### 🟡 平台特定问题

- **Linux**：AppImage 需要 `--appimage-extract` 才能访问 CLI；Wayland 可能导致某些 `dev:*` 命令出现渲染问题
- **macOS**：Gatekeeper 可能在首次运行时阻止 CLI；在系统偏好设置 > 安全性中允许
- **Windows**：MS Store 版本和直接安装版本的 CLI 路径不同

### 🟡 有时需要 Obsidian 获得焦点

某些命令（特别是 `dev:screenshot`）需要 Obsidian 是活动窗口。如果其他应用获得焦点，结果可能不一致。

### 🟡 插件开发命令需要插件

`obsidian dev:errors`、`dev:console` 和 `dev:dom` 仅适用于**你正在开发**的插件。你无法检查第三方插件的内部。

### 🟢 无 API 速率限制

与云端 API 不同，Obsidian CLI 没有速率限制——它是本地的。但是，过多的 `search` 或 `dev:dom` 调用可能影响 Obsidian 的响应速度。

---

## 建议与最佳实践

### 每日笔记管理

- 批量创建笔记时使用 `silent` 标志避免 Obsidian 窗口切换
- 对于任务记录，优先使用 `daily:append` 而非 `daily:read` + 手动编辑
- 使用 `\n` 分隔符将多个追加操作合并为一个命令

### 插件开发

- 始终遵循循环：**重新加载 → 错误 → 截图 → 控制台**
- 将截图保存在仓库内的 `screenshots/` 文件夹中以方便参考
- 发布前使用 `dev:console level=error` 捕获隐藏问题
- `eval` 命令功能强大——将复杂 JS 包装在单行中，或使用 `.js` 文件并加载它

### 安全

- `obsidian eval code="..."` 命令在 Obsidian 应用上下文中运行任意 JavaScript——仅使用你信任的代码
- DeepSeek TUI 可能会建议 `eval` 命令；在敏感仓库中执行前务必审查
- 保持 Obsidian 安装更新以获取安全补丁

### 性能

- 使用 `limit=` 限制 `search` 以避免转录中出现数千条结果
- 当你只需要计数时使用 `total` 标志
- 关闭未使用的仓库以减少内存使用

### 仓库组织

- 标准化 frontmatter 属性（`status`、`type`、`tags`），使 `property:set` 和 `property:get` 更有用
- 使用一致的笔记命名以获得更好的 `file=` 解析
- 使用 wikilinks 链接笔记——`backlinks` 和 `outgoing-links` 依赖它们

---

## 故障排除

### `obsidian: command not found`

CLI 不在你的 PATH 中。检查你的 PATH 配置：

```bash
echo $PATH | tr ':' '\n' | grep -i obsidian
```

如果没有显示，请重新添加（参见[设置 Obsidian CLI](#设置-obsidian-cli)）。

### 命令挂起或超时

这通常意味着 Obsidian 未运行或未响应：

1. 验证 Obsidian 是否打开（检查任务栏/Dock）
2. 尝试点击 Obsidian 窗口使其获得焦点
3. 如果问题仍然存在，重启 Obsidian

### `No vault found` 或 `No active file`

可能原因：
- Obsidian 中没有打开仓库（先打开一个）
- 需要明确指定仓库：`vault="My Vault"`
- 活动文件已关闭；明确使用 `file=` 或 `path=`

### Linux 上的权限错误（AppImage）

```bash
# 如果 AppImage 解压失败：
chmod +x Obsidian-*.AppImage
./Obsidian-*.AppImage --appimage-extract

# 如果 /usr/local/bin/obsidian 提示权限被拒绝：
sudo chmod +x /usr/local/bin/obsidian
```

### 技能未出现在 DeepSeek TUI 中

1. 验证文件存在于 `~/.deepseek/skills/obsidian-cli/SKILL.md`
2. 检查文件是否有有效的 YAML frontmatter，包含 `name` 和 `description`
3. 完全重启 DeepSeek TUI（不仅仅是 `/compact`）
4. 检查 `~/.deepseek/config.toml` 是否在禁用列表中包含 `obsidian-cli`

### macOS Gatekeeper 阻止 CLI

```bash
# 如果运行 obsidian 时出现安全警告：
xattr -d com.apple.quarantine /Applications/Obsidian.app
```

---

## 常见问题

**问：我可以在多个仓库上使用吗？**
答：可以。在任何命令中使用 `vault="仓库名称"` 作为第一个参数。

**问：能否与 Obsidian Sync 一起使用？**
答：CLI 操作本地文件。同步的仓库可以工作，但 CLI 不直接与 Obsidian Sync 的 API 交互。

**问：我可以安排自动创建笔记吗？**
答：可以，但 Obsidian 必须在计划时间运行。谨慎使用 cron/systemd 定时器——如果 Obsidian 未打开，它们会静默失败。

**问：我可以贡献额外的 Obsidian 技能吗？**
答：当然可以。请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 贡献

欢迎贡献。请参阅 [CONTRIBUTING.md](CONTRIBUTING.md) 了解指南。

## 许可证

MIT —— 详见 [LICENSE](LICENSE)。

## 鸣谢

- [Agent Skills 规范](https://agentskills.io) 由 Anthropic 提供
- [Obsidian](https://obsidian.md) 由 Obsidian 团队开发
- [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui)
