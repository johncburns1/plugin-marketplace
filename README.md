# Jack's Agent Plugins Marketplace

A marketplace for Claude Code agent plugins including skills, commands, and tools for software engineering and product development.

## What are Agent Skills?

Agent Skills are modular capabilities that extend Claude's functionality. Each Skill packages instructions, metadata, and optional resources that Claude uses automatically when relevant.

## Available Plugins

### Jack Flow

Jack's personal workflow — attention-optimized agentic SDLC.

**Skills included**:

- **[Agentic SDLC](./plugins/jack-flow/skills/agentic-sdlc/README.md)** - Attention-optimized delivery workflow: decompose work into small, independently deployable PRs, run many concurrent agent workstreams against main, and prioritize the human review queue

## Installation

### Using Claude Code

```bash
# Add the marketplace
/plugin marketplace add johncburns1/plugin-marketplace

# Install the jack-flow plugin
/plugin install jack-flow@jacks-agent-plugins

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
- **Version**: `3.0.0`
- **Owner**: John C Burns
- **License**: MIT

### Current Plugins

1. **jack-flow** - Jack's personal workflow (1 skill)

## Resources

- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
- [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)

---

**Last Updated**: 2026-10-29
