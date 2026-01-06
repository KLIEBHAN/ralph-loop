# PRD - [Projektname]

## Workflow

1. **Status:** `git diff` prüfen → bei Änderungen: testen & committen
2. **Feature:** EIN Feature aus "Offen" wählen (höchste Priorität zuerst)
3. **Implementieren:** Code schreiben → `npm run lint` → `npm test`
4. **Dokumentieren:** Feature nach "Erledigt" verschieben mit Datum
5. **Commit & Push**

> **Wichtig:** NUR EIN FEATURE PRO ITERATION. Tests sind Pflicht!

---

## Offen

<!-- Höchste Priorität oben -->

- Verbessere Test Coverage für Angular-Komponenten _(aktuell ~27%)_
- Behebe 10-20 Lint-Fehler _(aktuell 638)_

---

## Dauerhaft

<!-- Diese Items werden NIE erledigt → Endlosloop bleibt aktiv -->

- Optimiere diese PRD-Datei bei Bedarf
- Füge neue sinnvolle Features zur "Offen"-Liste hinzu

---

## Erledigt

- ✅ Lint-Fehler behoben (14 Stück, 652→638) — 2026-01-06
- ✅ Lint-Fehler behoben (12 Stück, 664→652) — 2026-01-06
- ✅ Lint-Fehler behoben (26 Stück, 792→773) — 2026-01-06
- ✅ PRD-Struktur verbessert — 2026-01-06

---

## Notizen

**Lint-Status:** 638 Fehler verbleibend

**Häufigste Fehlertypen:**

1. `explicit-function-return-type` — große Komponenten
2. `no-explicit-any` — Domain-Typen erforderlich
