# Sub-agent Description Examples

## Why Description Matters

The description is the **only signal** Claude uses to decide when to delegate tasks to a sub-agent.

```
User Request → Claude scans all sub-agent descriptions → Match? → Delegates task
                              ↑
                    THIS IS THE ONLY GATE
```

## The Formula

```
[What it does]. [When to use]. [Proactive trigger (optional)]
```

## Good vs Bad Examples

### Code Reviewer

**Bad** (too vague):
```yaml
description: Reviews code
```

**Bad** (missing trigger):
```yaml
description: Expert code reviewer analyzing code quality, security, and maintainability
```

**Good** (with proactive trigger):
```yaml
description: Expert code reviewer for quality and security. Use PROACTIVELY after writing or modifying code.
```

### Debugger

**Bad**:
```yaml
description: Helps debug issues
```

**Good**:
```yaml
description: Debugging specialist for errors, test failures, and unexpected behavior. Use PROACTIVELY when encountering any issues.
```

### Data Analyst

**Bad**:
```yaml
description: Analyzes data
```

**Good**:
```yaml
description: Data analysis expert for SQL queries, BigQuery operations, and data insights. Use when analyzing data, writing queries, or generating reports.
```

### Security Auditor

**Bad**:
```yaml
description: Security reviewer
```

**Good**:
```yaml
description: Security specialist for reviewing code vulnerabilities. Use PROACTIVELY when reviewing authentication, authorization, or data handling code.
```

## Trigger Keywords

### Automatic Delegation Triggers

Include these phrases for auto-delegation:
- "Use PROACTIVELY when..."
- "MUST BE USED when..."
- "Use immediately after..."
- "Use automatically for..."

### Action Keywords

Include verbs that match user requests:
- review, analyze, fix, debug, test
- write, create, generate, build
- check, validate, verify, audit

### Domain Keywords

Include terms users mention:
- code, tests, errors, bugs
- API, database, security
- documentation, deployment

## Proactive vs Explicit

### Proactive Sub-agents

Claude delegates automatically based on context.

```yaml
# Triggers after any code change
description: Code reviewer. Use PROACTIVELY after code modifications.

# Triggers on any error
description: Debugger. Use PROACTIVELY when errors occur.
```

### Explicit Sub-agents

Only invoked when user specifically requests.

```yaml
# Only when user asks for docs
description: Documentation writer for creating technical docs. Use when user requests documentation.

# Only when explicitly asked
description: Data migrator for database migrations. Use only when user explicitly requests migration.
```

## Description Checklist

- [ ] Under 200 characters (concise)
- [ ] Includes primary function
- [ ] Has trigger scenario
- [ ] Includes "PROACTIVELY" if auto-delegation desired
- [ ] Uses natural language (not technical jargon)
- [ ] Includes relevant domain keywords
