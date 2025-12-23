# Jack's Agent Skills Marketplace

A curated collection of Claude Code agent skills for software engineering, including Python development standards, testing practices, and architectural patterns.

## What are Agent Skills?

Agent Skills are modular capabilities that extend Claude's functionality. Each Skill packages instructions, metadata, and optional resources that Claude uses automatically when relevant.

## Available Skills

- **[Engineering Standards](./skills/engineering-standards/README.md)** - Core engineering principles including simplicity-first philosophy, TDD, and hexagonal architecture
- **[Python Engineering](./skills/python-engineering/README.md)** - Python-specific tooling and practices for Python 3.13+ projects (uv, ruff, mypy, pytest)
- **[Product Definition Guide](./skills/product-definition-guide/README.md)** - Conversational methodology for creating comprehensive product specs and PRDs

## Installation

### Using Claude Code

```bash
# Load plugin from local directory
claude --plugin-dir /path/to/plugin-marketplace

# Or install from GitHub
/plugin marketplace add johncburns1/plugin-marketplace
/plugin install jc-agent-skills@plugin-marketplace
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/johncburns1/plugin-marketplace.git

# Copy skills to your project
cp -r plugin-marketplace/skills/* /path/to/your-project/.claude/skills/

# Or copy to global skills directory
cp -r plugin-marketplace/skills/* ~/.claude/skills/
```

## Usage

Once installed, Claude will automatically use these skills when relevant. No manual invocation needed.

## Contributing

To add a new skill:

1. Create a new directory in `skills/` with a `SKILL.md` file
2. Add a `README.md` with metadata and high-level description
3. Update this README to include the new skill
4. Increment the version in `.claude-plugin/plugin.json`

## Plugin Metadata

- **Name**: `jc-agent-skills`
- **Version**: `1.0.0`
- **Author**: John C Burns
- **License**: MIT

## Resources

- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
- [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)

---

**Last Updated**: 2025-12-23
