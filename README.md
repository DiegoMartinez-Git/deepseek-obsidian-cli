# DeepSeek Obsidian Skills

> Agent Skills for Obsidian, packaged and documented for [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui).

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DeepSeek TUI](https://img.shields.io/badge/DeepSeek%20TUI-%3E%3D0.8.x-blue)](https://github.com/deepseek-tui/deepseek-tui)
[![Obsidian](https://img.shields.io/badge/Obsidian-%3E%3D1.5-purple)](https://obsidian.md)

[English](README.md) | [Español](README.es.md) | [中文](README.zh.md)

These skills let DeepSeek TUI interact with your Obsidian vault — read, create, search, edit notes, manage tasks, inspect properties, and even develop plugins — all from the terminal.

---

## Table of Contents

- [What are DeepSeek Skills?](#what-are-deepseek-skills)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Available Skills](#available-skills)
- [Verification](#verification)
- [Usage Examples](#usage-examples)
- [Limitations](#limitations)
- [Recommendations & Best Practices](#recommendations--best-practices)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)
- [Credits](#credits)

---

## What are DeepSeek Skills?

Skills are markdown files that extend DeepSeek TUI's capabilities. When installed, they're injected into the system prompt, giving the AI agent precise knowledge of domain-specific tools, conventions, and workflows.

Skills live at:

```
~/.deepseek/skills/<skill-name>/SKILL.md
```

DeepSeek TUI auto-discovers them on startup. You invoke a skill by mentioning its name in conversation, or it activates automatically when your request matches its description.

> Learn more: [DeepSeek TUI Skills Documentation](https://github.com/deepseek-tui/deepseek-tui)

---

## Prerequisites

### Required

| Requirement | Minimum Version | Notes |
|-------------|----------------|-------|
| **DeepSeek TUI** | 0.8.x or later | Skills support was added in 0.8.x |
| **Obsidian Desktop** | 1.5.0 or later | Must be **open and running** when using the skill |
| **Obsidian CLI** | Bundled with Obsidian | The `obsidian` binary must be in your PATH |

### Optional

| Package | Purpose |
|---------|---------|
| Node.js 18+ | Required for some `obsidian dev:*` commands |
| Git | Useful for vault versioning alongside CLI usage |

### Operating System Support

| OS | Supported | Notes |
|----|-----------|-------|
| **macOS** | ✅ Full support | CLI path: `/Applications/Obsidian.app/Contents/Resources/` |
| **Windows** | ✅ Full support | CLI bundled with the installer |
| **Linux** | ✅ Full support | AppImage may require extraction for CLI access |
| **Headless / Server** | ❌ Not supported | Obsidian requires a graphical desktop environment |

---

## Installation

### Method 1: Via skill-installer (recommended)

If you have the `skill-installer` skill enabled in DeepSeek TUI:

```
/skill install github:DiegoMartinez-Git/deepseek-obsidian-cli
```

Or from within a conversation:

> "Install the deepseek-obsidian-cli skills from GitHub"

This clones the repo and copies the skills into `~/.deepseek/skills/`.

### Method 2: Manual — clone the repo

```bash
# Clone to a temporary location
git clone https://github.com/DiegoMartinez-Git/deepseek-obsidian-cli.git /tmp/deepseek-obsidian-cli

# Copy the skill(s) you want
cp -r /tmp/deepseek-obsidian-cli/skills/obsidian-cli ~/.deepseek/skills/obsidian-cli

# Clean up
rm -rf /tmp/deepseek-obsidian-cli
```

### Method 3: Manual — single file

If you only need `obsidian-cli`:

```bash
mkdir -p ~/.deepseek/skills/obsidian-cli/references
curl -o ~/.deepseek/skills/obsidian-cli/SKILL.md \
  https://raw.githubusercontent.com/DiegoMartinez-Git/deepseek-obsidian-cli/main/skills/obsidian-cli/SKILL.md
curl -o ~/.deepseek/skills/obsidian-cli/references/COMMANDS.md \
  https://raw.githubusercontent.com/DiegoMartinez-Git/deepseek-obsidian-cli/main/skills/obsidian-cli/references/COMMANDS.md
```

### Set up the Obsidian CLI

After installing the skill, ensure the `obsidian` command is accessible:

**macOS:**
```bash
echo 'export PATH="/Applications/Obsidian.app/Contents/Resources:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Linux (AppImage):**
```bash
# Extract the AppImage (one-time)
/path/to/Obsidian-*.AppImage --appimage-extract
# The CLI will be at squashfs-root/usr/bin/obsidian
sudo cp squashfs-root/usr/bin/obsidian /usr/local/bin/
obsidian --version
```

**Windows (PowerShell):**
```powershell
[Environment]::SetEnvironmentVariable(
  "Path",
  "$env:Path;C:\Users\$env:USERNAME\AppData\Local\Programs\Obsidian",
  "User"
)
```

---

## Available Skills

| Skill | Description | Requires |
|-------|-------------|----------|
| [obsidian-cli](skills/obsidian-cli) | Read, create, search, and manage notes; daily notes, tasks, properties, tags, backlinks; plugin & theme development with hot-reload, DOM inspection, screenshots, and JS execution | Obsidian Desktop running |

More skills coming soon — contributions welcome.

---

## Verification

After installation, verify everything works:

### 1. Check the skill file exists

```bash
ls -la ~/.deepseek/skills/obsidian-cli/SKILL.md
# Should show the file with size > 1KB
```

### 2. Check Obsidian CLI is accessible

```bash
obsidian --version
# Should print a version number
```

### 3. Quick smoke test

With Obsidian open and a vault active:

```bash
obsidian search query="test" limit=1
# Should return results or "No results found" (both are valid)
```

### 4. Verify in DeepSeek TUI

Start a new DeepSeek TUI session. The skill should appear under `## Skills` in the system prompt. Ask:

> "What skills do you have available?"

`obsidian-cli` should be listed.

---

## Usage Examples

Once installed, just talk naturally to DeepSeek TUI. The skill activates automatically:

### Note management

> "Read my note called Project Alpha"
> → Runs: `obsidian read file="Project Alpha"`

> "Create a new note titled Meeting Notes with today's date in the content"
> → Runs: `obsidian create name="Meeting Notes" content="..."`

### Daily notes & tasks

> "What tasks do I have pending today?"
> → Runs: `obsidian tasks daily todo`

> "Add a task to my daily note: review the PR"
> → Runs: `obsidian daily:append content="- [ ] review the PR"`

### Search & navigation

> "Find all notes mentioning the word budget in my Work vault"
> → Runs: `obsidian vault="Work" search query="budget"`

> "What are my most used tags?"
> → Runs: `obsidian tags sort=count counts`

### Plugin development

> "Reload my plugin called my-theme and check for errors"
> → Runs: `obsidian plugin:reload id=my-theme` then `obsidian dev:errors`

> "Take a screenshot of my plugin's UI"
> → Runs: `obsidian dev:screenshot path=screenshot.png`

---

## Limitations

### 🔴 Critical — No headless support

Obsidian is a desktop application (Electron-based). The CLI communicates with a **running Obsidian process**. This means:

- ❌ Does NOT work on servers, containers, or headless VMs
- ❌ Does NOT work over SSH without X forwarding
- ❌ Does NOT work in CI/CD pipelines
- ❌ Does NOT work on Raspberry Pi without a desktop environment

If you need headless vault management, consider:
- Directly editing `.md` files in the vault directory
- Using Git for vault versioning
- Obsidian Sync API (requires subscription)

### 🟡 Platform-specific quirks

- **Linux**: AppImage requires `--appimage-extract` for CLI access; Wayland may cause rendering issues with some `dev:*` commands
- **macOS**: Gatekeeper may block the CLI on first run; allow it in System Preferences > Security
- **Windows**: CLI path differs between MS Store and direct installer versions

### 🟡 Requires Obsidian to be focused (sometimes)

Some commands (especially `dev:screenshot`) need Obsidian to be the active window. If another app has focus, results may be inconsistent.

### 🟡 Plugin development commands require the plugin

`obsidian dev:errors`, `dev:console`, and `dev:dom` only work for plugins **you are developing**. You cannot inspect third-party plugin internals.

### 🟢 No API rate limits

Unlike cloud APIs, the Obsidian CLI has no rate limits — it's local. However, excessive `search` or `dev:dom` calls can impact Obsidian's responsiveness.

---

## Recommendations & Best Practices

### For daily note management

- Use `silent` flag to avoid Obsidian window switches when bulk-creating notes
- Prefer `daily:append` over `daily:read` + manual edit for task logging
- Batch multiple appends into one command with `\n` separator

### For plugin development

- Always follow the cycle: **reload → errors → screenshot → console**
- Keep screenshots in a `screenshots/` folder inside your vault for easy reference
- Use `dev:console level=error` before releasing to catch hidden issues
- The `eval` command is powerful — wrap complex JS in a single line, or use a `.js` file and load it

### Security

- The `obsidian eval code="..."` command runs arbitrary JavaScript in the Obsidian app context — only use it with code you trust
- DeepSeek TUI may suggest `eval` commands; always review before executing in sensitive vaults
- Keep your Obsidian installation updated for security patches

### Performance

- Limit `search` with `limit=` to avoid thousands of results in the transcript
- Use `total` flag when you only need counts
- Close unused vaults to reduce memory usage

### Vault organization

- Standardize frontmatter properties (`status`, `type`, `tags`) to make `property:set` and `property:get` more useful
- Use consistent note naming for better `file=` resolution
- Link notes with wikilinks — `backlinks` and `outgoing-links` depend on them

---

## Troubleshooting

### `obsidian: command not found`

The CLI isn't in your PATH. Check your PATH configuration:

```bash
echo $PATH | tr ':' '\n' | grep -i obsidian
```

If nothing shows up, re-add it (see [Set up the Obsidian CLI](#set-up-the-obsidian-cli)).

### Commands hang or time out

This usually means Obsidian isn't running, or not responding:

1. Verify Obsidian is open (check your taskbar/dock)
2. Try clicking on the Obsidian window to give it focus
3. If the problem persists, restart Obsidian

### `No vault found` or `No active file`

Either:
- No vault is open in Obsidian (open one first)
- You need to specify the vault explicitly: `vault="My Vault"`
- The active file was closed; use `file=` or `path=` explicitly

### Permission errors on Linux (AppImage)

```bash
# If AppImage extraction fails:
chmod +x Obsidian-*.AppImage
./Obsidian-*.AppImage --appimage-extract

# If /usr/local/bin/obsidian gives permission denied:
sudo chmod +x /usr/local/bin/obsidian
```

### Skill doesn't appear in DeepSeek TUI

1. Verify the file exists at `~/.deepseek/skills/obsidian-cli/SKILL.md`
2. Check the file has valid YAML frontmatter with `name` and `description`
3. Restart DeepSeek TUI completely (not just `/compact`)
4. Check `~/.deepseek/config.toml` doesn't have `obsidian-cli` in a disabled list

### macOS Gatekeeper blocks the CLI

```bash
# If you get a security warning when running obsidian:
xattr -d com.apple.quarantine /Applications/Obsidian.app
```

---

## FAQ

**Q: Can I use this on multiple vaults?**
A: Yes. Use `vault="Vault Name"` as the first parameter on any command.

**Q: Does this work with Obsidian Sync?**
A: The CLI operates on local files. Synced vaults work, but the CLI doesn't directly interact with Obsidian Sync's API.

**Q: Can I schedule automated note creation?**
A: Yes, but Obsidian must be running at the scheduled time. Use cron/systemd timers cautiously — they'll fail silently if Obsidian isn't open.

**Q: Can I contribute additional Obsidian skills?**
A: Absolutely. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT — see [LICENSE](LICENSE) for details.

## Credits

- [Agent Skills specification](https://agentskills.io) by Anthropic
- [Obsidian](https://obsidian.md) by the Obsidian team
- [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui)
