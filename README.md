# Jack's Agent Plugins Marketplace

A marketplace for Claude Code agent plugins including skills, commands, and tools for software engineering and product development.

## What are Agent Skills?

Agent Skills are modular capabilities that extend Claude's functionality. Each Skill packages instructions, metadata, and optional resources that Claude uses automatically when relevant.

## Available Plugins

### Project Planning

Project planning and management skills for defining products, designing system architecture, creating GitHub issues, and organizing engineering work.

**Skills included**:

- **[Product Definition Guide](./skills/product-definition-guide/README.md)** - Conversational methodology for creating comprehensive product specs and PRDs
- **[Architecture Guide](./skills/architecture-guide/README.md)** - Enterprise architect perspective for creating one-page, technology-agnostic system architecture docs
- **[Technical Project Manager](./skills/technical-project-manager/README.md)** - Principal-level PM skill for creating GitHub issues and milestones from product requirements

### Engineering Skills

Full-stack software engineering skills including core principles, Python standards, React frontend, and serverless backend development.

**Skills included**:

- **[Engineering Standards](./skills/engineering-standards/README.md)** - Core engineering principles including simplicity-first philosophy, TDD, and hexagonal architecture
- **[Python Engineering](./skills/python-engineering/README.md)** - Python-specific tooling and practices for Python 3.13+ projects (uv, ruff, mypy, pytest)
- **[Frontend Development](./skills/frontend-development/README.md)** - React + Vite web development with TypeScript, responsive design, hooks + Context patterns
- **[Backend Development](./skills/backend-development/README.md)** - Serverless architecture, API design, authentication/authorization, database design (language-agnostic)

## Installation

### Using Claude Code

```bash
# Add the marketplace
/plugin marketplace add johncburns1/plugin-marketplace

# Install the project planning plugin
/plugin install project-planning@jacks-agent-plugins

# Install the engineering skills plugin
/plugin install engineering-skills@jacks-agent-plugins

# Or install from local directory
claude --plugin-dir /path/to/plugin-marketplace
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

### Adding a New Skill to Existing Plugin

1. Create a new directory in `skills/` with a `SKILL.md` file
2. Add a `README.md` with metadata and high-level description
3. Add the skill path to the plugin's `skills` array in `marketplace.json`
4. Update this README to list the new skill
5. Increment the version in `.claude-plugin/marketplace.json`

### Adding a New Plugin to Marketplace

1. Create the plugin structure (commands, skills, agents, etc.)
2. Add a new plugin entry to the `plugins` array in `marketplace.json`
3. Update this README to describe the new plugin
4. Increment the version in `.claude-plugin/marketplace.json`

## Marketplace Metadata

- **Name**: `jacks-agent-plugins`
- **Version**: `1.0.0`
- **Owner**: John C Burns
- **License**: MIT

### Current Plugins

1. **project-planning** - Project planning and management skills (3 skills)
2. **engineering-skills** - Full-stack software engineering skills (4 skills)

## Resources

- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
- [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)

---

**Last Updated**: 2025-12-28
