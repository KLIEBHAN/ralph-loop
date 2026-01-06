# Ralph Loop Examples

This directory contains templates and examples for using Ralph effectively.

## PRD Template

[`prd-template.md`](prd-template.md) — A Product Requirements Document template optimized for Ralph loops.

### Quick Start

```bash
# 1. Copy template to your project
cp prd-template.md ~/my-project/plans/prd.md

# 2. Edit the PRD: add your features to "Offen"
vim ~/my-project/plans/prd.md

# 3. Run Ralph with PRD validation
cd ~/my-project
ralph --prd plans/prd.md "Read plans/prd.md and work on the next open feature."
```

### Template Structure

| Section               | Purpose                                               |
| --------------------- | ----------------------------------------------------- |
| **Workflow**          | Step-by-step process for each iteration               |
| **Offen**             | Prioritized feature backlog (top = highest priority)  |
| **Routine-Checks**    | Non-blocking checks to run every iteration            |
| **Abschluss-Prüfung** | Pre-completion checklist (prevents premature signals) |
| **Erledigt**          | Completed features with dates                         |
| **Notizen**           | Context for future iterations                         |

### How `--prd` Validation Works

```
┌─────────────────────────────────────────────┐
│  AI outputs <promise>COMPLETE</promise>     │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│  Ralph parses PRD file                      │
│  └─ Counts "- " lines in "## Offen"         │
└─────────────────────────────────────────────┘
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
┌───────────────┐       ┌───────────────┐
│  Items > 0    │       │  Items = 0    │
│  ⚠️ REJECTED  │       │  ✅ ACCEPTED  │
│  → continue   │       │  → exit 0     │
└───────────────┘       └───────────────┘
```

### Customization Tips

**For endless loops** (continuous improvement):

```markdown
## Offen

- Improve test coverage _(target: >80%)_
- Fix lint errors _(current: 638)_

## Routine-Checks

- [ ] Add new improvements to "Offen" if discovered
```

**For finite tasks** (complete and exit):

```markdown
## Offen

- Implement login API
- Add password reset
- Write integration tests

<!-- No Routine-Checks that add new items -->
```

### Language

The template uses German section headers (`Offen`, `Erledigt`, etc.) because the `--prd` flag specifically looks for `## Offen`. If you prefer English, update both the template AND the validation regex in the `ralph` script:

```bash
# In ralph script, change:
sed -n '/^## Offen/,/^## /p'
# To:
sed -n '/^## Open/,/^## /p'
```
