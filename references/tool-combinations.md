# Tool Combinations Guide

## Available Tools

| Tool | Description |
|------|-------------|
| `Read` | Read file contents |
| `Write` | Create or overwrite files |
| `Edit` | Make precise edits to existing files |
| `Glob` | Find files by pattern matching |
| `Grep` | Search file contents with regex |
| `Bash` | Execute shell commands |
| `Task` | Spawn sub-agents (not recommended for sub-agents) |
| `AskUser` | Ask user questions |
| `TodoWrite` | Manage task lists |
| `WebFetch` | Fetch web content |
| `WebSearch` | Search the web |

## Common Combinations

### Read-Only Research
```yaml
tools: Read, Grep, Glob, Bash
```
**Best for**: Code analysis, documentation review, codebase exploration, security audits

**Use cases**:
- Code reviewers that report issues but don't fix them
- Architecture analyzers
- Dependency auditors

### Code Modification
```yaml
tools: Read, Write, Edit, Grep, Glob, Bash
```
**Best for**: Implementing features, fixing bugs, refactoring

**Use cases**:
- Bug fixers
- Feature implementers
- Refactoring specialists

### Minimal Read Access
```yaml
tools: Read, Grep, Glob
```
**Best for**: Security audits, code review (report-only)

**Use cases**:
- Security scanners (no command execution)
- Static analysis reviewers
- Documentation readers

### Full Access
```yaml
# Omit the tools field entirely
```
**Best for**: General-purpose assistants, complex workflows

**Use cases**:
- Project managers
- Full-stack developers
- DevOps automation

### Web-Enabled Research
```yaml
tools: Read, Grep, Glob, WebFetch, WebSearch
```
**Best for**: Research tasks requiring external information

**Use cases**:
- Documentation researchers
- Dependency version checkers
- API documentation fetchers

### File Operations Only
```yaml
tools: Read, Write, Edit, Glob
```
**Best for**: File manipulation without command execution

**Use cases**:
- Configuration editors
- Documentation writers
- Template generators

## Tool Selection Guidelines

### Principle of Least Privilege

Grant only the tools necessary for the task:

1. **Start minimal**: Begin with the smallest tool set
2. **Add as needed**: Expand only when required
3. **Justify each tool**: Ask "Why does this sub-agent need X?"

### Tool Risks

| Tool | Risk Level | Consideration |
|------|------------|---------------|
| `Read` | Low | Can access any readable file |
| `Grep` | Low | Can search all files |
| `Glob` | Low | File discovery only |
| `Write` | Medium | Can overwrite files |
| `Edit` | Medium | Can modify existing files |
| `Bash` | High | Can execute any command |
| `Task` | High | Can spawn other sub-agents |

### Recommendations by Sub-agent Type

| Type | Recommended Tools |
|------|-------------------|
| Code Reviewer | `Read, Grep, Glob, Bash` |
| Bug Fixer | `Read, Write, Edit, Grep, Glob, Bash` |
| Test Runner | `Read, Edit, Bash, Grep, Glob` |
| Security Auditor | `Read, Grep, Glob` |
| Doc Writer | `Read, Write, Edit, Glob` |
| Data Analyst | `Read, Bash, Write` |
