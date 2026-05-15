---
name: obsidian-cli
description: Interact with Obsidian vaults using the Obsidian CLI to read, create, search, and manage notes, tasks, properties, and more. Also supports plugin and theme development with commands to reload plugins, run JavaScript, capture errors, take screenshots, and inspect the DOM. Use when the user asks to interact with their Obsidian vault, manage notes, search vault content, perform vault operations from the command line, or develop and debug Obsidian plugins and themes.
---

# Obsidian CLI

Use the `obsidian` CLI to interact with a running Obsidian instance.

> **⚠️ Important**: Obsidian must be **open and running** on your desktop. The CLI communicates with the running Obsidian application process — it does not work on headless servers, containers, or systems without a graphical desktop environment.

## DeepSeek TUI setup

This skill is designed for [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui). When installed, it lives at:

```
~/.deepseek/skills/obsidian-cli/SKILL.md
```

After installation, restart DeepSeek TUI or start a new session. The skill will appear in the `## Skills` section of the system prompt. Use it by referencing `obsidian-cli` in your conversation.

For full installation instructions, see the [README](https://github.com/deepseek-obsidian-skills).

## Prerequisites

Before using this skill, ensure:

1. **Obsidian Desktop** is installed and **currently open**
2. The Obsidian CLI (`obsidian`) is available in your PATH
3. You have a vault open in Obsidian

### Installing the Obsidian CLI

**macOS / Linux:**
```bash
# The CLI is bundled with Obsidian. Add it to your PATH:
export PATH="/Applications/Obsidian.app/Contents/Resources:$PATH"   # macOS
export PATH="/usr/local/share/obsidian:$PATH"                        # Linux (AppImage)
```

**Windows:**
```powershell
# Add Obsidian install directory to PATH:
$env:Path += ";C:\Users\<user>\AppData\Local\Obsidian\Obsidian.exe"
```

### Verify installation

```bash
obsidian --version
```

If you see a version number, you're ready.

## Command reference

Run `obsidian help` to see all available commands. This is always up to date. Full docs: https://help.obsidian.md/cli

## Syntax

**Parameters** take a value with `=`. Quote values with spaces:

```bash
obsidian create name="My Note" content="Hello world"
```

**Flags** are boolean switches with no value:

```bash
obsidian create name="My Note" silent overwrite
```

For multiline content use `\n` for newline and `\t` for tab.

## File targeting

Many commands accept `file` or `path` to target a file. Without either, the active file is used.

- `file=<name>` — resolves like a wikilink (name only, no path or extension needed)
- `path=<path>` — exact path from vault root, e.g. `folder/note.md`

## Vault targeting

Commands target the most recently focused vault by default. Use `vault=<name>` as the first parameter to target a specific vault:

```bash
obsidian vault="My Vault" search query="test"
```

## Common patterns

```bash
obsidian read file="My Note"
obsidian create name="New Note" content="# Hello" template="Template" silent
obsidian append file="My Note" content="New line"
obsidian search query="search term" limit=10
obsidian daily:read
obsidian daily:append content="- [ ] New task"
obsidian property:set name="status" value="done" file="My Note"
obsidian tasks daily todo
obsidian tags sort=count counts
obsidian backlinks file="My Note"
```

Use `--copy` on any command to copy output to clipboard. Use `silent` to prevent files from opening. Use `total` on list commands to get a count.

## Plugin development

### Develop/test cycle

After making code changes to a plugin or theme, follow this workflow:

1. **Reload** the plugin to pick up changes:
   ```bash
   obsidian plugin:reload id=my-plugin
   ```
2. **Check for errors** — if errors appear, fix and repeat from step 1:
   ```bash
   obsidian dev:errors
   ```
3. **Verify visually** with a screenshot or DOM inspection:
   ```bash
   obsidian dev:screenshot path=screenshot.png
   obsidian dev:dom selector=".workspace-leaf" text
   ```
4. **Check console output** for warnings or unexpected logs:
   ```bash
   obsidian dev:console level=error
   ```

### Additional developer commands

Run JavaScript in the app context:

```bash
obsidian eval code="app.vault.getFiles().length"
```

Inspect CSS values:

```bash
obsidian dev:css selector=".workspace-leaf" prop=background-color
```

Toggle mobile emulation:

```bash
obsidian dev:mobile on
```

Run `obsidian help` to see additional developer commands including CDP and debugger controls.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `obsidian: command not found` | The CLI isn't in your PATH. See installation instructions above |
| Command hangs or times out | Obsidian may not be running. Open Obsidian and make sure it has focus |
| `No vault found` | Ensure you have a vault open in Obsidian, or specify with `vault=` |
| Permission errors on Linux | The AppImage may need to be extracted: `./Obsidian.AppImage --appimage-extract` |
