# Skill: export

## Zweck
Optimierten DataFrame als Parquet speichern und Analysebericht als Markdown ausgeben.

## Wann anwenden
Immer als letzter Schritt nach `memory_optimize` und `eda_report`.

## Code

```python
from pathlib import Path
from datetime import datetime

# Pfade ableiten
input_path = Path("input_dateiname")  # vom Nutzer übernehmen
output_dir = Path("output")
output_dir.mkdir(exist_ok=True)

stem = input_path.stem
parquet_path = output_dir / f"{stem}_optimized.parquet"
report_path = output_dir / f"{stem}_report.md"

# Optimierten DataFrame speichern
df.to_parquet(parquet_path, index=False)
print(f"Gespeichert: {parquet_path}")

# Report schreiben
report = f"""# EDA Report: {input_path.name}
Erstellt: {datetime.now().strftime('%Y-%m-%d %H:%M')}

## Überblick
- Shape: {df.shape[0]} Zeilen × {df.shape[1]} Spalten
- Speicher vorher: {mem_vorher / 1024**2:.2f} MB
- Speicher nachher: {mem_nachher / 1024**2:.2f} MB
- Ersparnis: {(1 - mem_nachher / mem_vorher) * 100:.1f}%

## Durchgeführte Schritte
{schritte}

## Finale Datentypen
{df.dtypes.to_string()}

## Fehlerwerte nach Bereinigung
{df.isna().sum()[df.isna().sum() > 0].to_string() or "Keine"}

{cleaning_agenda}

## Output
- Optimierte Datei: `{parquet_path}`
- Dieser Report: `{report_path}`
"""

report_path.write_text(report, encoding='utf-8')
print(f"Report: {report_path}")
```

## Hinweise
- `mem_vorher` und `mem_nachher` müssen aus `memory_optimize` übernommen werden
- `schritte` ist eine Liste der durchgeführten Bereinigungsschritte als Markdown-Bullets
- `cleaning_agenda` wird als fertiger String aus `eda_report` übernommen
- Output-Ordner wird automatisch erstellt falls nicht vorhanden
- Parquet erhält alle Typ-Optimierungen, bei CSV gehen diese verloren

## Selbstverbesserung
Neue Export-Formate oder Report-Abschnitte hier ergänzen wenn sinnvoll.
