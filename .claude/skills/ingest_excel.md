# Skill: ingest_excel

## Zweck
Excel-Dateien token-effizient einlesen.

## Wann anwenden
Nutzer gibt eine `.xlsx` oder `.xls` Datei an.

## Code
```python
import pandas as pd

# Sheets prüfen ohne alles zu laden
xl = pd.ExcelFile("datei.xlsx")
print(f"Sheets: {xl.sheet_names}")

# Gezielt ein Sheet laden
df = pd.read_excel("datei.xlsx", sheet_name=0)

print(f"Shape: {df.shape}")
print(f"Speicher: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.dtypes)
```

## Hinweise
- Immer erst Sheet-Namen prüfen bevor alles geladen wird
- `header=1` wenn Spaltenköpfe nicht in Zeile 1 sind
- `skiprows=N` für Dateien mit Metadaten am Anfang

## Selbstverbesserung
Neue Excel-Eigenheiten hier ergänzen.
