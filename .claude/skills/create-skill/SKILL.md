/---
description: Create a new Claude Code skill with correct directory structure and conventions. Use when the user wants to scaffold a new skill.
---

# create-skill

Create a new skill at `.claude/skills/<skill-name>/SKILL.md`.

The skill name is: $ARGUMENTS

## Instructions

When creating a new skill, follow this structure precisely.

### Step 1 — Determine scope

- **Project skill** (default): `.claude/commands/<skill-name>.md`
- **Global skill** (if `--global` is passed): `~/.claude/commands/<skill-name>.md`

Global skills apply to all projects. Project skills apply only to the current repo.

### Step 2 — Create the file

The filename (without `.md`) becomes the `/slash-command` name. Use lowercase letters, numbers, and hyphens only.

Only the `.md` file is required. The directory structure is:

```
<scope-path>/commands/
└── <skill-name>.md     # Required — main instructions with frontmatter
```

### Step 3 — Write the file with correct front matter

The file MUST start with YAML front matter between `---` markers:

```yaml
---
description: What this skill does and when to use it.
argument-hint: [optional-arg]
---
```

#### Recommended fields

| Field | Description |
|-------|-------------|
| `description` | What the skill does. Shown in `/help`. Keep under 60 characters. Start with a verb. |
| `argument-hint` | Hint shown during autocomplete, e.g. `[file-path]` or `[issue-number]` |
| `allowed-tools` | Tools Claude can use, e.g. `Read, Grep, Bash(git:*)` |
| `model` | Model to use: `haiku`, `sonnet`, or `opus` |

### Step 4 — Write the skill body

After the front matter, write the skill instructions in markdown. Follow these conventions:

1. **Start with a `## Usage` section** showing invocation syntax and referencing `$ARGUMENTS`
2. **Use numbered `### Step N — Title` sections** for multi-step workflows
3. **Keep the file under 500 lines** — move detailed reference material to separate files

#### Available substitution variables

| Variable | Description |
|----------|-------------|
| `$ARGUMENTS` | All arguments passed to the skill |
| `$1`, `$2`, etc. | Specific argument by position (1-based) |

#### Dynamic context injection

Use `!`command`` to run shell commands whose output is injected before Claude sees the skill:

```markdown
Current branch: !`git branch --show-current`
```

### Step 5 — Confirm the result

After creating the skill, report:
- The full path to the created `.md` file
- The invocation command (e.g. `/my-skill`)
- Whether it is global or project-scoped

### Quick reference — Minimal example

```markdown
---
description: Greet the user warmly
argument-hint: [name]
---

## Usage

Invoked as: `/greet [name]`

Say hello to $ARGUMENTS if provided, otherwise just say hello.
```
