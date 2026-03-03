# Load Claude Code Docs

Fetches the current Claude Code documentation for plugins, subagents, and skills, then produces a structured reference in context — ready to use when authoring new plugin components.

## When to Use

Invoke this skill before:
- Creating a new plugin, agent, or skill
- Editing `plugin.json` and needing to verify schema fields
- Authoring subagent frontmatter and needing to check valid values
- Authoring a skill and needing to verify `$ARGUMENTS`, `context: fork`, or `allowed-tools` syntax

## Invocation

```
/utilities:load-claude-docs
```

No arguments needed.

## What It Does

1. Fetches three Claude Code documentation pages **in parallel**:
   - `https://code.claude.com/docs/en/plugins`
   - `https://code.claude.com/docs/en/sub-agents`
   - `https://code.claude.com/docs/en/skills`

2. Extracts the key technical details from each page:
   - **Plugins**: directory structure, `plugin.json` schema, component types, `${CLAUDE_PLUGIN_ROOT}`
   - **Subagents**: frontmatter fields, tool access, models, permission modes, hooks, memory
   - **Skills**: frontmatter fields, `$ARGUMENTS`, `context: fork`, supporting file patterns

3. Produces a single **Claude Code Reference** block in context with all key info organized by topic

## Output

After running, you'll have a structured reference section in context covering all three APIs — so Claude can use accurate, up-to-date information when helping you build or modify plugin components.
