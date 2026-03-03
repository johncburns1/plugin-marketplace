# Retro

**Version**: 1.0.0
**Last Updated**: 2026-03-03

## Description

Development session retrospective skill. Analyzes the completed session across all workflow steps and creates a GitHub issue in the plugin marketplace with specific, actionable improvement suggestions.

## When to Use

Used by the `retro-agent` in step 6 of the dev workflow. Runs autonomously after every session — no human input required.

## Output

A GitHub issue in `johncburns1/plugin-marketplace` containing:

- What went well / what didn't
- Root causes of friction
- Specific improvement suggestions targeted at named skills and agents
- Session metrics (plan amendments, validation cycles, code review findings)

## Continuous Improvement Loop

Each session's retro feeds back into the marketplace, improving the skills and agents for the next session.
