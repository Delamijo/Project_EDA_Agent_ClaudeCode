# Skill: memory_optimize

## Zweck
DataFrame-Speicher durch Typ-Downcast optimieren mit kontrolliertem Informationsverlust.

## Wann anwenden
Nutzer fragt nach Speicheroptimierung, oder Datei ist > 50MB.

## Schritt 1 – Ist-Zustand erfassen
```python
mem_vorher = df.memory_usage(deep=True).sum()
print(f"Speicher aktuell: {mem_vorher / 1024**2:.2f} MB")
print(df.dtypes)
```

## Schritt 2 – User fragen (float-Strategie)
Zeige dem User folgende Auswahl bevor float-Downcast durchgeführt wird:

```
Float-Konvertierung (float64 → float32):
Wie viel Informationsverlust ist akzeptabel?

1. Kein Downcast        - float64 bleibt erhalten, kein Verlust
2. Streng (1e-8)      - Konvertierung nur bei minimalem Verlust
3. Normal (1e-6)      - empfohlen für die meisten Datensätze
4. Aggressiv (1e-4)     - maximale Ersparnis, spürbare Abweichungen möglich
```

Warte auf Nutzerantwort bevor du weitermachst. Setze "user_wahl" auf die gegebene Zahl.

## Schritt 3 – Optimierung durchführen

```python
# Schwellenwert je nach User-Wahl
schwelle = {
    "1": None,    # kein float downcast
    "2": 1e-8,
    "3": 1e-6,
    "4": 1e-4
}.get(user_wahl)

# Kategorische Konvertierung
for col in df.select_dtypes('object'):
    if df[col].nunique() / len(df) < 0.5:
        df[col] = df[col].astype('category')

# Integer downcast (kein Informationsverlust)
for col in df.select_dtypes('int64'):
    df[col] = pd.to_numeric(df[col], downcast='integer')

# Float downcast (nur wenn User nicht "1" gewählt hat)
if schwelle is not None:
    for col in df.select_dtypes('float64'):
        diff = (df[col] - df[col].astype('float32')).abs().max()
        if diff < schwelle:
            df[col] = df[col].astype('float32')
        else:
            print(f"{col}: bleibt float64 (max. Abweichung: {diff:.2e})")

mem_nachher = df.memory_usage(deep=True).sum()
ersparnis = (1 - mem_nachher / mem_vorher) * 100

print(f"Vorher:    {mem_vorher / 1024**2:.2f} MB")
print(f"Nachher:   {mem_nachher / 1024**2:.2f} MB")
print(f"Ersparnis: {ersparnis:.1f}%")
print("\nNeue dtypes:")
print(df.dtypes)
```

## Hinweise
- Immer nach `missing_values` ausführen, NaN in int-Spalten blockiert downcast
- category-dtype spart viel bei Wiederholungswerten, kostet bei hoher Kardinalität
- Integer downcast ist sicher, pandas prüft automatisch ob Werte passen

## Selbstverbesserung
User-Entscheidungsmuster dokumentieren. Wenn ein Datensatz-Typ immer dieselbe
Schwelle braucht (z.B. GPS-Daten immer streng), hier als Regel ergänzen.
