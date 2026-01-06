# PRD - [Projektname]

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

<!-- Höchste Priorität oben. Wenn leer → Task ist abgeschlossen. -->

- Feature 1
- Feature 2

---

## Routine-Checks

<!-- Bei JEDER Iteration prüfen, blockiert aber NICHT den Abschluss -->

- [ ] Gibt es offensichtliche Verbesserungen für diese PRD?
- [ ] Fehlt ein wichtiges Feature in der "Offen"-Liste?

---

## Abschluss-Prüfung

<!-- NUR ausführen wenn "Offen" leer ist! -->

Bevor du das Completion-Signal ausgibst, beantworte EXPLIZIT:

1. **Offen-Liste leer?** Liste alle Items aus "Offen" auf (sollte: keine)
2. **Tests erfolgreich?** Was war das Ergebnis der Tests?
3. **Alles committed?** Was zeigt `git status`?

Nur wenn ALLE drei Punkte positiv beantwortet → Completion-Signal.

---

## Erledigt

- ✅ Beispiel-Feature — 2026-01-01

---

## Notizen

Kontext für zukünftige Iterationen hier dokumentieren.
