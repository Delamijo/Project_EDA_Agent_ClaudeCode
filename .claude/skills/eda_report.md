# Skill: eda_report

## Zweck
Strukturierte EDA-Zusammenfassung nach abgeschlossener Analyse ausgeben.

## Wann anwenden
Nach Abschluss einer EDA-Session, immer als letzter Schritt.

## Output-Format
```
## EDA-Zusammenfassung: [Dateiname]

### Überblick
- Shape: X Zeilen × Y Spalten
- Speicher: X MB → Y MB (Z% Ersparnis nach Optimierung)

### Datenqualität
- Fehlerwerte: [Spalten mit > 5% missing, sonst "keine kritischen"]
- Ausreißer: [auffällige Spalten per IQR, sonst "keine kritischen"]
- Duplikate: X Zeilen

### Kategorische Spalten
- Hohe Kardinalität (>80%): [Spalten] → wahrscheinlich IDs
- Für category-dtype geeignet: [Spalten]

### Empfohlene nächste Schritte
1. [konkrete Bereinigungsmaßnahme]
2. [konkrete Bereinigungsmaßnahme]
3. ...

### Offene Fragen
- [Was unklar bleibt und Domänenwissen erfordert]

---

## Cleaning Agenda
-- Empfehlungen für Cleaning Agent --

EMPFEHLUNG DROPPEN:
- spalte: [name] | grund: [z.B. unique_pct > 80%]

EMPFEHLUNG IMPUTATION:
- spalte: [name] | methode: [median/modus] | grund: [missing_pct X%, numerisch/kategorisch]

EMPFEHLUNG TYPKORREKTUR:
- spalte: [name] | von: [dtype] | zu: [dtype] | grund: [z.B. Datum falsch eingelesen]

EMPFEHLUNG AUSREISSER:
- spalte: [name] | methode: [IQR-clip] | grund: [X% Ausreißer]
```

### Hinweise
Cleaning Agenda nur mit tatsächlich gefundenen Problemen befüllen
Leere Abschnitte weglassen wenn keine Empfehlung nötig
Offene Fragen ehrlich benennen, nicht raten

## Selbstverbesserung
Wenn wiederkehrende Muster in Zusammenfassungen auftauchen, hier als Vorlage ergänzen.
