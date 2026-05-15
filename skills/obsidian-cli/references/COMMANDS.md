# Full Command Reference — Obsidian CLI

> Auto-generated from `obsidian help`. Run `obsidian help` for the latest commands.

## Notes

### Read
```bash
obsidian read                    # Read active file
obsidian read file="Note"        # Read by name (wikilink resolution)
obsidian read path="folder/n.md" # Read by vault-relative path
```

### Create
```bash
obsidian create name="Title"                            # Create empty note
obsidian create name="Title" content="# Hello"          # With content
obsidian create name="Title" template="Template"        # From template
obsidian create name="Title" content="..." silent       # Don't open in Obsidian
obsidian create name="Title" overwrite                  # Overwrite if exists
```

### Append
```bash
obsidian append file="Note" content="New line"
obsidian append file="Note" content="Line1\nLine2"     # Multiline with \n
obsidian append file="Note" content="..." silent
```

### Search
```bash
obsidian search query="term"                           # Search entire vault
obsidian search query="term" limit=10                  # Limit results
obsidian search query="term" total                     # Count only
obsidian search path="folder" query="term"             # Search within path
obsidian search query="term" vault="My Vault"          # Specific vault
```

### Delete / move / rename
```bash
obsidian delete file="Note"
obsidian move file="Note" path="archive/"
obsidian rename file="Note" name="New Name"
```

### Open
```bash
obsidian open file="Note"                              # Open in Obsidian
obsidian open path="folder/note.md"                    # Open by path
obsidian open daily                                    # Open today's daily note
obsidian open random                                   # Open a random note
```

---

## Properties

```bash
obsidian property:get name="status" file="Note"
obsidian property:set name="status" value="done" file="Note"
obsidian property:delete name="temp" file="Note"
obsidian property:list file="Note"                     # List all properties
```

---

## Daily Notes

```bash
obsidian daily:read                           # Read today's daily note
obsidian daily:append content="- [ ] Task"    # Append to today
obsidian daily:open                           # Open today
obsidian daily:create                         # Create today if not exists
obsidian daily:read date=2026-01-01           # Read specific date
obsidian daily:append content="..." date=2026-01-01
```

---

## Tasks

```bash
obsidian tasks file="Note"                    # List tasks in a note
obsidian tasks daily                          # List tasks in today's daily note
obsidian tasks daily todo                     # Only incomplete tasks
obsidian tasks daily done                     # Only completed tasks
```

---

## Tags

```bash
obsidian tags                                 # All tags with file counts
obsidian tags sort=count counts               # Sorted by frequency
obsidian tags limit=10                        # Top 10 most used
```

---

## Links and Navigation

```bash
obsidian backlinks file="Note"                # Notes that link to this one
obsidian outgoing-links file="Note"           # Notes this one links to
obsidian links file="Note"                    # All links (in/out)
obsidian unresolved-links                     # Broken links across vault
```

---

## Vault Management

```bash
obsidian vault:list                           # List all known vaults
obsidian vault:info                           # Info about current vault
obsidian vault:open name="My Vault"           # Open a specific vault
```

---

## Templates

```bash
obsidian template:list                        # List available templates
obsidian template:insert name="Template"      # Insert template into active note
```

---

## Aliases

```bash
obsidian aliases file="Note"                  # List aliases of a note
```

---

## Graph

```bash
obsidian graph:local file="Note"              # Local graph for a note
obsidian graph:local file="Note" depth=2      # With depth
```

---

## Plugins

```bash
obsidian plugin:list                          # List installed plugins
obsidian plugin:reload id=my-plugin           # Hot-reload a plugin
obsidian plugin:enable id=my-plugin           # Enable a plugin
obsidian plugin:disable id=my-plugin          # Disable a plugin
obsidian plugin:install id=my-plugin          # Install from community plugins
obsidian plugin:uninstall id=my-plugin        # Remove a plugin
```

---

## Development Commands

```bash
# Reload
obsidian plugin:reload id=my-plugin

# Errors & Console
obsidian dev:errors                           # Show plugin errors
obsidian dev:console                          # All console output
obsidian dev:console level=error              # Only errors
obsidian dev:console level=warn               # Warnings only

# DOM Inspection
obsidian dev:dom                              # Full DOM outline
obsidian dev:dom selector=".workspace"        # Specific selector
obsidian dev:dom selector=".workspace" text   # Text content only

# CSS Inspection
obsidian dev:css selector=".workspace-leaf" prop=background-color

# Screenshots
obsidian dev:screenshot path=screenshot.png
obsidian dev:screenshot path=capture.png full # Full window

# JavaScript execution
obsidian eval code="app.vault.getFiles().length"
obsidian eval code="app.workspace.activeLeaf.view.getViewType()"

# Mobile emulation
obsidian dev:mobile on
obsidian dev:mobile off

# CDP (Chrome DevTools Protocol)
obsidian dev:cdp                               # Open CDP
obsidian dev:cdp port=9222                     # Custom port

# Debugger
obsidian dev:debugger on
obsidian dev:debugger off
```

---

## Global Options

These work on any command:

| Flag | Description |
|------|-------------|
| `--copy` | Copy command output to clipboard |
| `silent` | Don't open/switch to the affected file in Obsidian |
| `total` | Return only the count (on list commands) |
| `overwrite` | Overwrite existing files without confirmation |
| `--wait` | Wait for Obsidian to finish processing before returning |

## Vault Targeting

Use `vault="Name"` as the first parameter on any command to target a specific vault:

```bash
obsidian vault="My Vault" search query="budget"
obsidian vault="Work" daily:read
obsidian vault="Personal" tags
```
