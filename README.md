# Sub-agent Wizard

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE.txt)

> Interactive wizard for creating Claude Code sub-agents through guided questions.

[한국어 문서](README.ko.md)

## Overview

Sub-agent Wizard is a Claude Code skill that guides users through creating effective sub-agents via a 5-phase interactive workflow. Instead of reading documentation and figuring out the configuration yourself, the wizard asks targeted questions and generates optimized sub-agent files.

## Features

- **5-Phase Guided Workflow**: Step-by-step process from purpose discovery to sub-agent creation
- **Description Optimization**: Guide for writing effective trigger descriptions with auto-delegation support
- **Tool Selection Strategy**: Principle of least privilege with common tool combinations
- **Type-Specific Templates**: Ready-to-use templates for 8 sub-agent types
- **Validation Checklist**: Pre-creation verification to catch common mistakes

## What is a Sub-agent?

Sub-agents are specialized AI assistants that Claude Code can spawn to handle specific tasks. They have:

- **Custom system prompts** - Tailored instructions for specific domains
- **Tool restrictions** - Controlled access to file and system tools
- **Model selection** - Choose haiku, sonnet, or opus based on task complexity
- **Automatic delegation** - Claude can proactively delegate matching tasks

## Installation

### As a Claude Code Skill

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/greeun/subagent-wizard.git ~/.claude/skills/subagent-wizard
```

### Or symlink from local development

```bash
ln -s /path/to/subagent-wizard ~/.claude/skills/subagent-wizard
```

### Verify Installation

Restart Claude Code, then the skill will automatically activate when you mention creating a sub-agent.

## Usage

### Automatic Activation

Simply ask Claude to help create a sub-agent:

```
"I want to create a sub-agent for code review"
"Help me build a custom agent for debugging"
"Create a sub-agent that handles test execution"
"서브에이전트 만들어줘"
```

### Wizard Workflow

The wizard guides you through 5 phases:

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

#### Phase 1: Purpose Discovery

Understanding what the sub-agent should do:
- What task or domain should it handle?
- Example requests that should trigger it
- Automatic vs explicit delegation
- Success criteria

#### Phase 2: Configuration Design

Defining technical configuration:
- **Name**: `lowercase-with-hyphens` format
- **Description**: Function + trigger scenarios + proactive keyword
- **Tools**: Read-only, modification, or full access
- **Model**: haiku (fast), sonnet (balanced), opus (complex)
- **Permission mode**: default, acceptEdits, bypassPermissions, plan

#### Phase 3: System Prompt Writing

Writing effective system prompts:
- Role definition
- Initial actions when invoked
- Responsibilities and constraints
- Output format

#### Phase 4: Scope Selection

Choosing storage location:
- `.claude/agents/` - Project-specific (higher priority)
- `~/.claude/agents/` - User-wide (all projects)

#### Phase 5: Validation & Creation

Final checks and file generation:
- Pre-creation checklist
- Test scenario creation
- File writing and verification

## Sub-agent Types

| Type | Best For | Tools |
|------|----------|-------|
| **Code Reviewer** | Quality and security review | Read, Grep, Glob, Bash |
| **Debugger** | Error analysis and fixing | Read, Edit, Bash, Grep, Glob |
| **Test Runner** | Test execution and fixing | Bash, Read, Edit, Grep, Glob |
| **Data Analyst** | SQL and data insights | Bash, Read, Write |
| **Doc Writer** | Documentation creation | Read, Write, Edit, Glob |
| **Security Auditor** | Vulnerability scanning | Read, Grep, Glob |
| **API Developer** | REST/GraphQL development | Read, Write, Edit, Grep, Glob, Bash |
| **DevOps Engineer** | CI/CD and infrastructure | Read, Write, Edit, Bash, Grep, Glob |

## Project Structure

```
subagent-wizard/
├── SKILL.md                          # Main wizard guide
├── LICENSE.txt                       # Apache 2.0 license
├── README.md                         # This file
├── README.ko.md                      # Korean documentation
├── references/
│   ├── tool-combinations.md          # Tool selection guide
│   ├── type-templates.md             # Templates for each sub-agent type
│   └── description-examples.md       # Description writing examples
└── assets/
    └── subagent-template.md          # Basic sub-agent template
```

## Quick Start Example

Create a code reviewer sub-agent in 3 steps:

### 1. Create directory

```bash
mkdir -p ~/.claude/agents
```

### 2. Create sub-agent file

Write to `~/.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: Expert code reviewer. Use PROACTIVELY after code modifications.
tools: Read, Grep, Glob, Bash
model: inherit
---

You are a senior code reviewer.

When invoked:

1. Run git diff to see changes
2. Review modified files
3. Report issues by priority

Focus on:
- Code readability
- Security vulnerabilities
- Error handling
- Best practices
```

### 3. Test

Ask Claude: "Review my recent code changes"

## Configuration Reference

### Frontmatter Fields

| Field | Required | Values | Description |
|-------|----------|--------|-------------|
| `name` | Yes | `lowercase-hyphens` | Sub-agent identifier |
| `description` | Yes | string | When to use (critical for auto-delegation) |
| `tools` | No | comma-separated | Tool access (omit for full access) |
| `model` | No | `inherit`, `haiku`, `sonnet`, `opus` | AI model selection |
| `permissionMode` | No | `default`, `acceptEdits`, `bypassPermissions`, `plan` | Permission handling |
| `skills` | No | comma-separated | Auto-load skills |

### Tool Categories

| Category | Tools | Use Case |
|----------|-------|----------|
| Read-only | `Read, Grep, Glob, Bash` | Analysis, audits |
| Modification | `Read, Write, Edit, Grep, Glob, Bash` | Bug fixes, features |
| Minimal | `Read, Grep, Glob` | Security reviews |
| Full | (omit field) | General-purpose |

### Auto-delegation Keywords

Include in description for automatic task delegation:

- `Use PROACTIVELY when...`
- `MUST BE USED when...`
- `Use immediately after...`

## Integration with subagent-creator

Sub-agent Wizard complements the existing `subagent-creator` skill:

| Feature | subagent-creator | subagent-wizard |
|---------|------------------|-----------------|
| Approach | Documentation-based | Question-driven |
| Description Help | Basic format | Comprehensive examples |
| Templates | Single template | 8 type-specific templates |
| Tool Selection | Manual choice | Guided recommendation |
| Target Users | Experienced | Beginners to experts |

Use together:
1. **subagent-wizard** → Plan and design the sub-agent interactively
2. **subagent-creator** → Reference for advanced configurations

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Vague description | Never triggers | Add specific scenarios and keywords |
| Too many tools | Unfocused behavior | Grant minimal necessary tools |
| Generic prompt | No differentiation | Add domain-specific instructions |
| Missing "proactively" | No auto-delegation | Add "Use PROACTIVELY when..." |
| Wrong location | Not found | Check `.claude/agents/` vs `~/.claude/agents/` |

## Requirements

- Claude Code CLI
- No additional dependencies

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

Apache License 2.0 - see [LICENSE.txt](LICENSE.txt) for details.

## Related

- [subagent-creator](https://github.com/greeun/subagent-creator) - Sub-agent creation reference
- [skill-wizard](https://github.com/greeun/skill-wizard) - Skill creation wizard
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
