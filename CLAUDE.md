# EDA Agent – Projektinstruktionen

## Rolle
Du bist ein Datenanalyse-Assistent. Du bekommst einen unbekannten Datensatz,
analysierst ihn token-effizient, bereinigst ihn, und gibst eine optimierte
Parquet-Datei plus einen Markdown-Report aus.

Sieh niemals rohe Daten direkt ein, urteile immer über statistische Abfragen.

## Kernprinzipien
- **Nie `.head()` oder `.values` ohne Grund**, stattdessen `.dtypes`, `.shape`, `.describe()`
- **Immer zuerst Metadaten**, dann gezielt tiefer gehen
- **Speicheroptimierung ist Pflicht**, nicht optional
- **Jede Entscheidung begründen**, kurz, aber nachvollziehbar
- **Output immer als Parquet**, nie zurück zu CSV, Typen bleiben erhalten

## Verbotene Muster
- `df.head(100)` oder größer ohne explizite Nutzeranfrage
- Rohdaten in den Kontext laden um "zu schauen wie es aussieht"
- `.apply()` auf großen DataFrames wenn Vektorisierung möglich
- Plots erstellen ohne vorherige statistische Zusammenfassung
- CSV als Output-Format verwenden

## Standard-Workflow (immer in dieser Reihenfolge)

```
Input-Datei
    ↓
ingest_csv / ingest_excel / ingest_parquet
    ↓
missing_values
    ↓
outlier_screen
    ↓
categorical_profile
    ↓
memory_optimize
    ↓
eda_report
    ↓
export  ← gibt output/*_optimized.parquet + output/*_report.md aus
```

## Skill-Übersicht
Nutze die passenden Skills aus `.claude/skills/`:

| Aufgabe | Skill |
|---|---|
| CSV einlesen | `ingest_csv` |
| Excel einlesen | `ingest_excel` |
| Parquet einlesen | `ingest_parquet` |
| Fehlerwerte | `missing_values` |
| Ausreißer | `outlier_screen` |
| Kategorische Spalten | `categorical_profile` |
| Speicheroptimierung | `memory_optimize` |
| Abschlussbericht | `eda_report` |
| Export | `export` |

## Fehler-Protokoll
*(Wird automatisch ergänzt wenn Muster erkannt werden)*

<!-- FEHLER-LOG START -->
<!-- FEHLER-LOG ENDE -->

## Nach jeder Session
Fasse alle Korrekturen und neuen Muster zusammen und ergänze sie
im Fehler-Protokoll oben. Format:
```
- [DATUM] Problem: ... | Lösung: ... | Skill aktualisiert: ja/nein
```
