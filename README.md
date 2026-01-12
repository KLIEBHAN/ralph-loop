# Ralph Loop

External loop for AI coding agents with **fresh context each iteration**.

This implements the [original Ralph Wiggum technique](https://ghuntley.com/ralph/) by Geoffrey Huntley.

## What is Ralph?

Ralph is a simple bash loop that repeatedly runs an AI coding agent with the same prompt. Each iteration:

1. Starts a **fresh** AI session (no accumulated context)
2. AI sees previous work only through **files and git history**
3. Continues working until a completion promise is detected
4. Automatically iterates until the task is genuinely complete

## Supported CLIs

- [Claude Code](https://claude.com/code) (Anthropic)
- [Codex CLI](https://github.com/openai/codex) (OpenAI)
- [OpenCode](https://github.com/anomalyco/opencode) (Open Source)

## Installation

```bash
# Download
curl -o ~/.local/bin/ralph https://raw.githubusercontent.com/KLIEBHAN/ralph-loop/master/ralph
chmod +x ~/.local/bin/ralph

# Or clone and symlink
git clone https://github.com/KLIEBHAN/ralph-loop.git
ln -s $(pwd)/ralph-loop/ralph ~/.local/bin/ralph
```

### Dependencies

- **Claude:** requires `jq` (`brew install jq`)
- **Codex:** no additional dependencies
- **OpenCode:** requires `jq` (`brew install jq`)

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `RALPH_OPENCODE_MODEL` | Model for OpenCode CLI | `google/gemini-claude-opus-4-5-thinking` |

## Usage

```bash
ralph [OPTIONS] "Your prompt here"
```

### Options

| Option                  | Short | Description                                             | Default    |
| ----------------------- | ----- | ------------------------------------------------------- | ---------- |
| `--cli <claude\|codex\|opencode>` |       | AI CLI to use                                           | `claude`   |
| `--max-iterations`      | `-n`  | Max iterations before stopping                          | `10`       |
| `--promise`             | `-p`  | Completion promise text                                 | `COMPLETE` |
| `--prd <file>`          |       | PRD file to validate (rejects completion if items open) |            |
| `--help`                | `-h`  | Show help                                               |            |

### Examples

```bash
# Simple task with Claude
ralph "Build a REST API for todos"

# Use Codex instead
ralph --cli codex "Fix the authentication bug"

# Use OpenCode
ralph --cli opencode "Refactor the database layer"

# Custom iteration limit and promise
ralph -n 20 -p "ALL TESTS PASSING" "Implement user registration with tests"

# PRD-based workflow with validation (recommended)
ralph --prd plans/prd.md "Read plans/prd.md and work on the next open feature."

# Complex multi-phase task
ralph -n 50 "
Phase 1: Set up database schema
Phase 2: Implement CRUD endpoints
Phase 3: Add authentication
Phase 4: Write integration tests

Output <promise>COMPLETE</promise> when all phases done.
"
```

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│  ralph "Build feature X"                                │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│  Iteration 1 (fresh context)                            │
│  ├─ AI reads prompt + "Ralph iteration 1/10"            │
│  ├─ Checks files & git history                          │
│  ├─ Works on task                                       │
│  └─ No <promise>COMPLETE</promise> found                │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│  Iteration 2 (fresh context)                            │
│  ├─ AI reads prompt + "Ralph iteration 2/10"            │
│  ├─ Sees previous work in files                         │
│  ├─ Continues from where iteration 1 left off           │
│  └─ Outputs <promise>COMPLETE</promise>                 │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│  ✅ Loop complete after 2 iterations                    │
└─────────────────────────────────────────────────────────┘
```

## Writing Good Prompts

### Clear Completion Criteria

```markdown
Build a REST API for todos.

Requirements:

- CRUD endpoints for todos
- Input validation
- Error handling
- Tests with >80% coverage

Output <promise>COMPLETE</promise> when ALL requirements are met.
```

### Incremental Goals

```markdown
Implement user authentication:

Phase 1: Database schema for users
Phase 2: Registration endpoint with validation
Phase 3: Login endpoint with JWT
Phase 4: Protected route middleware
Phase 5: Integration tests

Output <promise>COMPLETE</promise> when all phases done.
```

### PRD-Based Workflow

For ongoing projects, use a PRD file with `--prd` validation:

```bash
ralph --cli opencode --prd plans/prd.md -n 50 "Bitte lese dich in plans/prd.md ein und arbeite den Workflow ab"
```

**How `--prd` works:**

- When the AI outputs the completion signal, Ralph parses the PRD file
- Counts items in the `## Offen` section (lines starting with `- `)
- If items remain → **rejects completion** and continues iterating
- Only accepts completion when `## Offen` is empty

This prevents premature completion signals—the script validates against ground truth.

#### Example PRD (`plans/prd.md`)

```markdown
# PRD

## Workflow (must do!!!!)

1. **Status:** Wichtig: `git diff` prüfen → bei Änderungen: genau prüfen, passende Tests (`npm test`) und commiten/pushen
2. **Feature:** EIN Feature aus "Offen" wählen (höchste Priorität zuerst - bewerte selbst)
3. **Implementieren:** Code schreiben → echte Lösung, keine Shortcuts
4. **Clean up**: Code nochmals prüfen, aufräumen und sonstiges Cleanup
5. **Quality Gates:**
   - `npm run lint` — KEINE neuen `eslint-disable` ohne Genehmigung!
   - `npm test` — Tests mit echtem Mehrwert (keine Coverage-Padding)
   - `npm run build` — Build muss durchlaufen
6. **Dokumentieren:** Feature nach "Erledigt" verschieben mit Datum - sauber dokumentieren für den nächsten Agenten
7. **Commit & Push**

---

## Offen

### Performance-Optimierungen

### Code Quality

- **Code smells beseitigen:** Schlechte Patterns korrigieren, Lesbarkeit verbessern (aussagekräftige Bezeichner, kleine fokussierte Funktionen, Kommentare für das "Warum"). Bestehende Logik nicht brechen.

### Test Coverage prüfen und wenn nötig erhöhen

- Test Coverage auf mindestens 35% bringen
- Fokus auf Services mit Business-Logik, nicht auf Coverage-Padding

### Bug hunt

- Scanne die Codebase nach Bugs (Null/Undefined, async races, error handling, parsing, state consistency, security). Pro Bug: Ort, Symptom, Root Cause, Fix, Test. Priorisiere P0-P2. Nur Bugs fixen, die vollständig verstanden sind.

### Dokumentation verbessern

- Prüfe die Dokumentation und vergleiche mit den Code
- Verbessere die Dokumentation wo es sinnvoll ist und einen Mehrwert bringt
- Wenn du eindeutige Fehler oder Inkonsistenzen bemerkst, fixe diese

### Lesbarkeit des Codes

- Bitte optimiere diesen Code für maximale Lesbarkeit und Verständlichkeit. Achte auf aussagekräftige, konsistente Bezeichner, vermeide unnötige Komplexität und erstelle gegebenenfalls kleine, fokussierte Funktionen. Ergänze kurze, sinnvolle Kommentare, die das ‚Warum' statt das ‚Wie' hervorheben. Erkläre abschließend, wie deine Änderungen die Verständlichkeit und Wartbarkeit verbessern.

### Komplexität reduzieren

- Bitte suche nach unnötiger Komplexität und Redundanz. Erkläre mir dann bitte genau, wie dies zustande kommt und was die wahrscheinlichsten Ursachen sind.
- Versuche es dann besser zu lösen und zu vereinheitlichen.

---

## Routine-Checks

<!-- Bei JEDER Iteration prüfen und ggf Änderungen durchführen wenn notwendig -->

- [ ] Gibt es offensichtliche Verbesserungen für diese PRD?
- [ ] Fehlt ein wichtiges Feature in der "Offen"-Liste?
```

See also [`examples/prd-template.md`](examples/prd-template.md) for a simpler template.

## Philosophy

> "Ralph is deterministically bad in an undeterministic world."
> — Geoffrey Huntley

- **Iteration > Perfection:** Don't aim for perfect on first try
- **Failures Are Data:** Use them to refine your prompts
- **Fresh Context:** Each iteration starts clean, avoiding context pollution
- **Persistence Wins:** Keep iterating until genuinely complete

## Credits

- Original technique: [Geoffrey Huntley](https://ghuntley.com/ralph/)
- Ralph Orchestrator: [mikeyobrien/ralph-orchestrator](https://github.com/mikeyobrien/ralph-orchestrator)

## License

MIT
