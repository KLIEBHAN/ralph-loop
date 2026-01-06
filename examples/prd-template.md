# PRD - [Projektname]

<!--
  RALPH LOOP PRD TEMPLATE

  Dieses Template ist optimiert für die Verwendung mit:
  ralph --prd plans/prd.md "Read plans/prd.md and work on the next open feature."

  WICHTIG: Die Sektion "## Offen" wird vom --prd Flag geparst.
  Nur Zeilen die mit "- " beginnen werden gezählt.
-->

## Workflow

1. **Status:** `git diff` prüfen → bei Änderungen: testen → committen
2. **Feature:** EIN Feature aus "Offen" wählen (höchste Priorität zuerst)
3. **Implementieren:** Code schreiben → linting → tests
4. **Dokumentieren:** Feature nach "Erledigt" verschieben mit Datum
5. **Commit & Push**
6. **Weiter?** Zähle Items in "Offen":
   - Wenn > 0 → nächste Iteration
   - Wenn = 0 → Abschluss-Prüfung durchführen

> **Wichtig:** NUR EIN FEATURE PRO ITERATION. Tests sind Pflicht!

---

## Offen

<!--
  FEATURE BACKLOG
  - Höchste Priorität oben
  - Format: "- Kurzbeschreibung _(Details/Metriken)_"
  - Wenn leer UND --prd gesetzt → Ralph akzeptiert Completion-Signal
-->

- Feature 1 _(Beschreibung oder Metrik)_
- Feature 2

---

## Routine-Checks

<!--
  NICHT-BLOCKIERENDE CHECKS
  - Werden bei jeder Iteration geprüft
  - Blockieren NICHT das Completion-Signal
  - Für kontinuierliche Verbesserung
-->

- [ ] Gibt es offensichtliche Verbesserungen für diese PRD?
- [ ] Fehlt ein wichtiges Feature in der "Offen"-Liste?

---

## Abschluss-Prüfung

<!--
  PRE-COMPLETION CHECKLIST
  - Nur ausführen wenn "Offen" leer ist
  - Verhindert voreilige Completion-Signale
  - Zwingt zur expliziten Verifikation
-->

Bevor du das Completion-Signal ausgibst, beantworte EXPLIZIT:

1. **Offen-Liste leer?** Liste alle Items aus "Offen" auf (sollte: keine)
2. **Tests erfolgreich?** Was war das Ergebnis der Tests?
3. **Alles committed?** Was zeigt `git status`?

Nur wenn ALLE drei Punkte positiv beantwortet → Completion-Signal.

---

## Erledigt

<!--
  ABGESCHLOSSENE FEATURES
  - Format: "- ✅ Beschreibung — YYYY-MM-DD"
  - Neueste zuerst (optional)
-->

- ✅ Beispiel-Feature — 2026-01-01

---

## Notizen

<!--
  KONTEXT FÜR ZUKÜNFTIGE ITERATIONEN
  - Aktueller Status (z.B. "Lint: 638 Fehler")
  - Bekannte Probleme
  - Technische Schulden
-->

Hier Kontext dokumentieren, der für zukünftige Iterationen relevant ist.
