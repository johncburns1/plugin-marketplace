---
name: load-claude-docs
description: This skill should be used when the user asks to "load Claude Code docs", "read the Claude Code documentation", "fetch Claude Code plugin docs", or needs up-to-date reference on the Claude Code plugins, subagents, or skills APIs before authoring.
---

# Load Claude Code Docs

Fetches the current Claude Code plugin, subagent, and skill documentation and produces a structured reference in context.

## Instructions

Fetch all three pages **in parallel** using `WebFetch`. Use targeted extraction prompts to pull the technical details needed for authoring.

### Step 1: Fetch All Three Pages (in parallel)

**Plugins page**
- URL: `https://code.claude.com/docs/en/plugins`
- Extraction prompt: `Extract all technical details about the plugin directory structure, plugin.json schema (all fields and valid values), component types (commands, agents, skills, hooks), how plugins are installed and distributed, and the ${CLAUDE_PLUGIN_ROOT} variable. List all field names, types, and valid values.`

**Subagents page**
- URL: `https://code.claude.com/docs/en/sub-agents`
- Extraction prompt: `Extract all technical details about subagent markdown file structure, every YAML frontmatter field (name, description, tools, model, permission-mode, color, context, etc.) with all valid values, how agents are triggered, tool access configuration, and any hooks or memory patterns. List all field names, types, and valid values.`

**Skills page**
- URL: `https://code.claude.com/docs/en/skills`
- Extraction prompt: `Extract all technical details about skill file structure, every YAML frontmatter field (name, description, disable-model-invocation, argument-hint, allowed-tools, context, etc.) with all valid values, how skills are invoked, the $ARGUMENTS variable, context: fork behavior, and supporting file patterns. List all field names, types, and valid values.`

### Step 2: Produce Structured Claude Code Reference

After fetching, produce the following structured reference in your response:

---

## Claude Code Reference

### Plugins — `plugin.json` Schema & Directory Structure

[Key facts from the plugins page: directory layout, all plugin.json fields with types and valid values, component types, `${CLAUDE_PLUGIN_ROOT}`, installation/distribution]

### Subagents — Frontmatter Fields & Triggering

[Key facts from the subagents page: all frontmatter fields with valid values, how agents are triggered, tool access, supported models, permission modes, hooks, memory]

### Skills — Frontmatter Fields & Invocation

[Key facts from the skills page: all frontmatter fields with valid values, invocation patterns, `$ARGUMENTS` syntax, `context: fork` behavior, supporting file patterns]

---

This reference is now in context. Use it when creating or modifying plugins, skills, or agents to ensure correctness against the current API.
