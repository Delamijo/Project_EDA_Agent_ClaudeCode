# Skill: ingest_parquet

## Zweck
Parquet-Dateien token-effizient einlesen.

## Wann anwenden
Nutzer gibt eine `.parquet` Datei an.

## Code
```python
import pandas as pd

# Schema prüfen ohne Daten zu laden
import pyarrow.parquet as pq
schema = pq.read_schema("datei.parquet")
print(schema)

# Laden
df = pd.read_parquet("datei.parquet")

print(f"Shape: {df.shape}")
print(f"Speicher: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.dtypes)
```

## Hinweise
- Parquet hat bereits eingebettete Typen, dtypes oft schon optimal
- Spaltenweise lesen wenn nur Teilmenge benötigt: `columns=['a','b']`
- `pyarrow` bevorzugen gegenüber `fastparquet`

## Verbotene Muster:
- Dateiinhalte niemals als String im Code nachbauen, immer direkt einlesen

## Selbstverbesserung
Neue Parquet-Eigenheiten hier ergänzen.
