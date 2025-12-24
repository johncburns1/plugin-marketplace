# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a marketplace for Claude Code agent plugins containing agent skills that extend Claude's functionality. The repository is content-based (markdown files) with no code compilation, tests, or build process.

## Repository Structure

```
.claude-plugin/
  marketplace.json          # Marketplace metadata and plugin definitions

skills/
  <skill-name>/
    SKILL.md               # Skill definition (YAML frontmatter + instructions)
    README.md              # User-facing documentation
    *.md                   # Supporting documentation files
```

## Key Files

- **marketplace.json** - Defines marketplace metadata and all plugins
- **SKILL.md** - Skill definition with frontmatter (`name`, `description`) and instruction body
- **README.md** - High-level skill documentation for users

## Skill File Format

Each skill directory must contain:

1. **SKILL.md** with YAML frontmatter:
   ```markdown
   ---
   name: skill-name
   description: Brief description. Use when [trigger conditions].
   ---

   # Skill Title
   [Instructions for Claude when this skill is activated]
   ```

2. **README.md** for user-facing documentation

Skills can reference other markdown files in their directory for detailed content.

## Making Changes

### Adding a New Skill

1. Create directory in `skills/` with `SKILL.md` and `README.md`
2. Add skill path to plugin's `skills` array in `marketplace.json`
3. Update root README.md
4. Increment version in `marketplace.json`

### Modifying Skills

- Edit `SKILL.md` for instruction changes
- Edit `README.md` for documentation changes
- Update version in `marketplace.json` for releases

### Adding a New Plugin

1. Create plugin structure
2. Add plugin entry to `plugins` array in `marketplace.json`
3. Update root README.md
4. Increment version

## Installation

Users install via Claude Code:
```bash
/plugin marketplace add johncburns1/plugin-marketplace
/plugin install engineering-skills@jacks-agent-plugins

# Or local testing
claude --plugin-dir /path/to/plugin-marketplace
```

## References

- [Agent Skills Documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
