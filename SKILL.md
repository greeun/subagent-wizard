---
name: subagent-wizard
description: Interactive wizard for creating Claude Code sub-agents through step-by-step guidance. Use when users want to create a new sub-agent, design custom AI assistants, need help with sub-agent configuration, want sub-agent templates, or ask about agent creation workflow. Covers sub-agent concepts, configuration options, and systematic creation process.
license: Apache-2.0
---

# Sub-agent Creation Wizard

An interactive guide for creating effective Claude Code sub-agents.

## About Sub-agents

Sub-agents are specialized AI assistants spawned by Claude Code to handle specific tasks with customized prompts and tool access. They execute as separate processes with focused capabilities.

**What Sub-agents Provide:**
1. **Specialized expertise** - Domain-specific knowledge and behavior
2. **Tool restrictions** - Controlled access to file and system tools
3. **Custom prompts** - Tailored instructions for specific workflows
4. **Automatic delegation** - Claude can proactively delegate matching tasks

## Core Principles

### Description is Critical

The `description` field determines when Claude delegates tasks to the sub-agent. Include:
- **Primary function**: What the sub-agent does
- **Trigger scenarios**: When to use it
- **"Use proactively"**: For automatic delegation

### Tool Selection Strategy

Grant only necessary tools:
- **Minimal access** reduces risk and focuses behavior
- **Full access** (omit `tools`) for general-purpose agents
- Common patterns in [references/tool-combinations.md](references/tool-combinations.md)

### Prompt Efficiency

Keep system prompts concise:
- Claude is already smart - add only unique context
- Focus on role, actions, and output format
- Avoid generic advice Claude already knows

## When to Use This Wizard

- User wants to create a sub-agent but doesn't know the format
- User needs help choosing tools and configuration
- User wants templates for specific sub-agent types
- User is unsure about description wording

## Wizard Workflow

Guide users through these 5 phases:

```
Phase 1: Purpose Discovery
    ↓
Phase 2: Configuration Design
    ↓
Phase 3: System Prompt Writing
    ↓
Phase 4: Scope Selection
    ↓
Phase 5: Validation & Creation
```

---

## Phase 1: Purpose Discovery

**Goal**: Understand what the sub-agent should do.

Ask these questions (one at a time):

1. "What specific task or domain should this sub-agent handle?"
2. "Give 2-3 examples of requests that should trigger this sub-agent."
3. "Should Claude delegate to this sub-agent automatically, or only when explicitly requested?"
4. "What does a successful output look like?"

**Output**: Clear purpose statement and delegation strategy.

---

## Phase 2: Configuration Design

**Goal**: Define the sub-agent's technical configuration.

### 2.1 Name

Rules:
- Lowercase letters, digits, hyphens only
- Max 64 characters
- Format: `role-specialty` or `domain-action` (e.g., `code-reviewer`, `data-analyst`)

Ask: "What role + specialty describes your sub-agent?" → Generate 3 name suggestions.

### 2.2 Description

The description determines when Claude delegates tasks. Critical for automatic delegation.

**Formula**: `[What it does]. [When to use]. [Proactive trigger]`

**Templates**:
```yaml
# For automatic delegation
description: Expert [role] for [domain]. Use PROACTIVELY when [scenario].

# For explicit invocation only
description: [Role] specialist for [domain]. Use when user explicitly requests [task].
```

Generate 3 description options based on Phase 1 answers.

### 2.3 Tool Selection

Present tool categories:

| Category | Tools | Best For |
|----------|-------|----------|
| Read-only | `Read, Grep, Glob, Bash` | Code analysis, audits |
| Code modification | `Read, Write, Edit, Grep, Glob, Bash` | Bug fixes, features |
| Minimal | `Read, Grep, Glob` | Security reviews |
| Full access | (omit field) | General-purpose |

Ask: "What level of file system access does your sub-agent need?"

See [references/tool-combinations.md](references/tool-combinations.md) for details.

### 2.4 Model Selection

| Model | When to Use |
|-------|-------------|
| `inherit` | Use parent's model (default) |
| `sonnet` | Balance of speed and quality |
| `opus` | Complex reasoning tasks |
| `haiku` | Fast, simple tasks |

Ask: "Does this sub-agent need fast responses (haiku), balanced (sonnet), or high reasoning (opus)?"

### 2.5 Permission Mode

| Mode | Description |
|------|-------------|
| `default` | Normal permission flow |
| `acceptEdits` | Auto-accept file edits |
| `bypassPermissions` | Skip all permission prompts |
| `plan` | Plan-only mode (no execution) |

Ask: "Should the sub-agent auto-accept edits, or require confirmation?"

---

## Phase 3: System Prompt Writing

**Goal**: Write the sub-agent's system prompt.

### Structure Template

```markdown
You are an expert [role/specialty].

When invoked:

1. [First action to take]
2. [Second action]
3. [Third action]

Key responsibilities:

- [Responsibility 1]
- [Responsibility 2]
- [Responsibility 3]

Guidelines:

- [Guideline 1]
- [Guideline 2]

Output format:

- [What to include]
- [How to structure]
```

### Writing Guidelines

1. **Define role clearly**: Start with "You are a/an [specific expert role]"
2. **List initial actions**: What to do immediately when invoked
3. **Specify responsibilities**: Core tasks the sub-agent handles
4. **Include constraints**: What NOT to do
5. **Define output format**: How to structure responses

### Prompt Length

- **Short** (50-100 words): Simple, focused tasks
- **Medium** (100-200 words): Standard complexity
- **Long** (200-400 words): Complex workflows with checklists

Ask: "Based on your requirements, here's a draft system prompt. Would you like to refine it?"

---

## Phase 4: Scope Selection

**Goal**: Decide where to store the sub-agent.

| Location | Scope | Priority |
|----------|-------|----------|
| `.claude/agents/` | Project-specific | Higher (overrides user) |
| `~/.claude/agents/` | User-wide (all projects) | Lower |

Ask: "Should this sub-agent be available only in this project, or across all your projects?"

---

## Phase 5: Validation & Creation

**Goal**: Verify and create the sub-agent.

### Pre-Creation Checklist

- [ ] Name follows `lowercase-with-hyphens` format
- [ ] Description includes function AND trigger scenarios
- [ ] Tools match required access level
- [ ] System prompt has clear role and actions
- [ ] Location matches intended scope

### Test Scenarios

Create 3 test cases:

1. **Should trigger**: Request that matches the sub-agent's purpose
2. **Edge case**: Unusual but valid request
3. **Should NOT trigger**: Similar request outside scope

### Create the Sub-agent

**Step 1: Ensure directory exists**
```bash
mkdir -p [location]/agents
```

**Step 2: Create the file**
Write to `[location]/agents/[name].md`:

```markdown
---
name: [name]
description: [description]
tools: [tools]
model: [model]
---

[system prompt]
```

**Step 3: Verify**
- Check file created correctly
- Test with sample request

---

## Quick Reference

### Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Vague description | Sub-agent never triggers | Add specific scenarios and keywords |
| Too many tools | Unfocused behavior | Grant minimal necessary tools |
| Generic prompt | No differentiation | Add domain-specific instructions |
| Missing "proactively" | No auto-delegation | Add "Use PROACTIVELY when..." |

### Sub-agent Types

For templates by type, see [references/type-templates.md](references/type-templates.md):
- Code Reviewer
- Debugger
- Test Runner
- Data Analyst
- Documentation Writer
- Security Auditor

---

## Resources

### References
- **Tool combinations**: [references/tool-combinations.md](references/tool-combinations.md)
- **Type templates**: [references/type-templates.md](references/type-templates.md)
- **Description examples**: [references/description-examples.md](references/description-examples.md)

### Assets
- **Subagent template**: [assets/subagent-template.md](assets/subagent-template.md)
