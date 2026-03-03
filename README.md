# Jack's Agent Plugins Marketplace

A marketplace for Claude Code agent plugins including skills, commands, and tools for software engineering and product development.

## What are Agent Skills?

Agent Skills are modular capabilities that extend Claude's functionality. Each Skill packages instructions, metadata, and optional resources that Claude uses automatically when relevant.

## Available Plugins

### Project Planning

Project planning and management skills for defining products, designing system architecture, creating GitHub issues, and organizing engineering work.

**Skills included**:

- **[Product Definition Guide](./plugins/project-planning/skills/product-definition-guide/README.md)** - Conversational methodology for creating comprehensive product specs and PRDs
- **[Architecture Guide](./plugins/project-planning/skills/architecture-guide/README.md)** - Enterprise architect perspective for creating one-page, technology-agnostic system architecture docs
- **[Technical Project Manager](./plugins/project-planning/skills/technical-project-manager/README.md)** - Principal-level PM skill for creating GitHub issues and milestones from product requirements

### Engineering Skills

Software engineering skills including core principles and Python standards.

**Skills included**:

- **[Engineering Standards](./plugins/engineering/skills/engineering-standards/README.md)** - Core engineering principles including simplicity-first philosophy, TDD, and hexagonal architecture
- **[Python Engineering](./plugins/engineering/skills/python-engineering/README.md)** - Python-specific tooling and practices for Python 3.13+ projects (uv, ruff, mypy, pytest)

### Dev Workflow

End-to-end development workflow orchestration. Runs a complete plan → review → implement → code-review → retro pipeline with two human touchpoints (plan approval and merge decision). All other steps are fully autonomous.

**Skills included**:

- **[Dev Workflow](./plugins/dev-workflow/skills/dev-workflow/README.md)** - Orchestration command (`/dev-workflow`) that runs the full pipeline
- **[Plan Review](./plugins/dev-workflow/skills/plan-review/README.md)** - Criteria for evaluating plan completeness and interface contract precision
- **[Code Review](./plugins/dev-workflow/skills/code-review/README.md)** - Final code review against the original plan
- **[Retro](./plugins/dev-workflow/skills/retro/README.md)** - Session retrospective that files improvement issues in this marketplace

**Agents included**:

- **plan-reviewer** - Critical plan review (fresh context, engineering-standards)
- **implementation-agent** - Full TDD cycle: writes failing tests, implements to pass, runs all gates, opens PR (isolated worktree)
- **code-reviewer** - Reviews PR against original plan (fresh context)
- **retro-agent** - Files retrospective issue in marketplace (autonomous)

### Utilities

General-purpose utility skills for Claude Code development.

**Skills included**:

- **[Load Claude Code Docs](./plugins/utilities/skills/load-claude-docs/README.md)** - Fetches the current Claude Code plugin, subagent, and skill docs in parallel and produces a structured API reference in context

## Installation

### Using Claude Code

```bash
# Add the marketplace
/plugin marketplace add johncburns1/plugin-marketplace

# Install the project planning plugin
/plugin install project-planning@jacks-agent-plugins

# Install the engineering plugin
/plugin install engineering@jacks-agent-plugins

# Or install from local directory
claude --plugin-dir /path/to/plugin-marketplace
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/johncburns1/plugin-marketplace.git

# Use as a local plugin directory
claude --plugin-dir /path/to/plugin-marketplace
```

## Usage

Once installed, Claude will automatically use these skills when relevant. No manual invocation needed.

## Contributing

### Adding a New Skill to Existing Plugin

1. Create a new directory in `plugins/<plugin-name>/skills/` with `SKILL.md` and `README.md`
2. Add the skill path to the plugin's `skills` array in `.claude-plugin/marketplace.json`
3. Update this README to list the new skill
4. Increment the version in `.claude-plugin/marketplace.json`

### Adding a New Plugin to Marketplace

1. Create `plugins/<plugin-name>/` with the plugin structure (skills, agents, etc.)
2. Add a new plugin entry to the `plugins` array in `.claude-plugin/marketplace.json`
3. Update this README to describe the new plugin
4. Increment the version in `.claude-plugin/marketplace.json`

## Marketplace Metadata

- **Name**: `jacks-agent-plugins`
- **Version**: `1.2.0`
- **Owner**: John C Burns
- **License**: MIT

### Current Plugins

1. **project-planning** - Project planning and management skills (3 skills)
2. **engineering** - Software engineering skills (2 skills)
3. **dev-workflow** - End-to-end development workflow (4 skills, 4 agents)
4. **utilities** - General-purpose utility skills (1 skill)

## Resources

- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
- [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)

---

**Last Updated**: 2026-03-03
