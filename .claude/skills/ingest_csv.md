# Skill: ingest_csv

## Zweck
CSV-Dateien token-effizient einlesen und Basisinfo ausgeben.

## Wann anwenden
Nutzer gibt eine `.csv` Datei an.

## Code
```python
import pandas as pd

df = pd.read_csv("datei.csv")  # Pfad vom Nutzer übernehmen

print(f"Shape: {df.shape}")
print(f"Speicher: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.dtypes)
```

## Hinweise
- Bei großen Dateien (>100MB) zuerst mit `nrows=1000` testen
- Encoding-Fehler: `encoding='utf-8'` oder `encoding='latin-1'` probieren
- Trennzeichen prüfen: `sep=';'` für deutsche CSVs

## Verbotene Muster:
- Dateiinhalte niemals als String im Code nachbauen, immer direkt einlesen

## Selbstverbesserung
Neue Encoding- oder Trennzeichen-Probleme hier ergänzen.
