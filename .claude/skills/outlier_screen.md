# Skill: outlier_screen

## Zweck
Ausreißer und unplausible Werte token-effizient erkennen.

## Wann anwenden
Nutzer fragt nach Ausreißern, unerwarteten Werten, Plausibilität.

## Code
```python
num_cols = df.select_dtypes('number')

# Überblick
ausreisser = pd.DataFrame({
    'min': num_cols.min(),
    'max': num_cols.max(),
    'negative_werte': (num_cols < 0).sum(),
    'nullen': (num_cols == 0).sum(),
    'mean': num_cols.mean().round(2),
    'std': num_cols.std().round(2)
})
print(ausreisser)

# IQR-basierte Ausreißer-Zählung (ohne Daten zu sichten)
Q1 = num_cols.quantile(0.25)
Q3 = num_cols.quantile(0.75)
IQR = Q3 - Q1
ausreisser_iqr = ((num_cols < (Q1 - 1.5 * IQR)) | 
                   (num_cols > (Q3 + 1.5 * IQR))).sum()
print("\nAusreißer per IQR:")
print(ausreisser_iqr[ausreisser_iqr > 0])
```

## Bewertungsregeln
- Negative Werte bei Alter/Preis/Gewicht → Datenfehler
- IQR-Ausreißer > 5% der Zeilen → genauer untersuchen
- min == max → Spalte konstant, wahrscheinlich nutzlos

## Selbstverbesserung
Domänenspezifische Plausibilitätsgrenzen (z.B. Alter 0-120) hier ergänzen wenn bekannt.
