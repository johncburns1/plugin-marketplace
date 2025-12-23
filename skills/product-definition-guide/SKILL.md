---
name: product-definition-guide
description: Conversational methodology for creating comprehensive product definition documents. Use when the user wants to create a product spec, PRD, or product definition file for a new project or feature.
---

# Product Definition Guide

A conversational framework for creating comprehensive product definition documents that capture vision, requirements, constraints, and success criteria.

## When to Use This Skill

Activate this skill when the user:

- Wants to create a product spec or PRD
- Is starting a new project and needs to define requirements
- Asks to create a PRODUCT.md file
- Needs help organizing product thinking

## Conversational Workflow

### Step 1: Understand the Big Picture

Start with open-ended questions:

- "What are you trying to build?"
- "What problem does this solve?"
- "Who is this for?"

### Step 2: Work Through Sections Systematically

Go through each section in order:

1. Ask clarifying questions
2. Propose initial content based on responses
3. Iterate based on feedback
4. Move to next section

For detailed questions for each section, see [section-guide.md](section-guide.md).

### Step 3: Capture Uncertainty

As you go, note:

- Open questions
- Decisions that need validation
- Areas that need research

### Step 4: Generate Complete Document

Create a well-formatted PRODUCT.md file with all sections. See [template.md](template.md) for the complete structure.

### Step 5: Iterate

After creating the initial document:

- Review with the user
- Refine based on feedback
- Update open questions as decisions are made

## Core Sections

A complete product definition includes 12 sections:

1. **Product Vision** - One-sentence value proposition
2. **Problem Statement** - What, who, why
3. **Target Users** - Primary and secondary users
4. **Product Goals** - 3-5 measurable objectives
5. **Core Features** - Must have / Should have / Nice to have
6. **User Stories** - Key workflows and outcomes
7. **Success Metrics** - How to measure success
8. **Out of Scope** - What we're NOT building
9. **Technical Considerations** - Constraints, dependencies, architecture
10. **Open Questions** - Decisions to be made
11. **Data Models** - Key data structures (if applicable)
12. **Timeline & Milestones** - Phases (NO time estimates)

## Best Practices

### Keep It Conversational

- Ask one section at a time
- Don't overwhelm with too many questions
- Build on previous answers
- Clarify ambiguities as you go

### Be Specific

- Push for concrete examples
- Avoid vague statements
- Quantify where possible (metrics, targets, limits)

### Document What You Don't Know

- Open questions are valuable
- Uncertainties should be explicit
- Decisions to be made later should be noted

### Focus on Value

- Always connect features to user benefits
- Understand the "why" behind requests
- Prioritize based on impact

## Key Formatting Rules

- Use checkboxes for features: `- [ ] Feature name`
- Use checkboxes for open questions: `- [ ] Question?`
- Organize features into Must/Should/Nice tiers
- Add metadata footer: Last Updated date and Status
- Use code blocks for technical schemas

## Conversational Prompts

**Starting**:

- "Let's create a product definition for your project. What are you building?"
- "I'll help you create a comprehensive PRODUCT.md file. Let's start with the vision..."

**For each section**:

- "Now let's define the problem. What specific pain point does this solve?"
- "Who are your primary users? What are their key characteristics?"
- "What features are absolutely essential for the MVP?"

**Handling uncertainty**:

- "That's a great question to add to our 'Open Questions' section"
- "We can document this as something to decide later"

**Finalizing**:

- "I've drafted the PRODUCT.md with all the sections we discussed. Let me know what needs adjusting"

## Output

Always create a file named `PRODUCT.md` in the project root with:

- All 12 sections properly formatted in Markdown
- Checkboxes for features and open questions
- Code blocks for technical schemas
- Footer with Last Updated date and Status

The document should be comprehensive enough that anyone reading it understands:

- What you're building and why
- Who it's for and what they need
- What's in scope and what's not
- How success will be measured
- What questions remain to be answered

## Further Reading

- [template.md](template.md) - Complete PRODUCT.md template structure
- [section-guide.md](section-guide.md) - Detailed questions and examples for each section
