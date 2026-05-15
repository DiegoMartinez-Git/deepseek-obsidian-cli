# Contributing to DeepSeek Obsidian Skills

Thanks for your interest in contributing.

## How to contribute

### Adding a new skill

1. Create a directory under `skills/<skill-name>/`
2. Add a `SKILL.md` with YAML frontmatter and the skill content
3. Optionally add a `references/` directory with companion files
4. Update the README skills table
5. Add an entry to CHANGELOG.md

### Skill format

Each skill must follow the [Agent Skills specification](https://agentskills.io/specification):

```markdown
---
name: skill-name
description: What the skill does, when to use it, what it enables.
---

# Skill Title

Skill instructions and command reference...
```

### Improving existing skills

- **Bug fixes**: Open an issue first, then a PR with the fix
- **New commands**: Document any new Obsidian CLI commands discovered
- **Documentation**: Improvements to README, references, or inline docs are always welcome

### DeepSeek-specific guidelines

- Skills are installed to `~/.deepseek/skills/<skill-name>/SKILL.md`
- The `skill-installer` skill handles automatic installation from GitHub repos
- Keep skill descriptions concise — they're shown in the skill listing
- Test skills with DeepSeek TUI version 0.8.x or later

## Pull request process

1. Fork the repo
2. Create a feature branch (`feature/my-skill`)
3. Make your changes
4. Test locally by copying the skill to `~/.deepseek/skills/`
5. Submit a PR with a clear description

## Code of Conduct

- Be respectful and constructive
- Focus on the technical merits of contributions
- Follow the existing style and conventions

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
