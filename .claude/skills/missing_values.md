# Skill: missing_values

## Zweck
Fehlerwerte token-effizient identifizieren und bewerten.

## Wann anwenden
Nutzer fragt nach Fehlerwerten, NaN, NULL, fehlenden Daten.

## Code
```python
# Fehlerwerte-Report
missing = pd.DataFrame({
    'absolut': df.isna().sum(),
    'prozent': (df.isna().mean() * 100).round(2)
}).query('absolut > 0').sort_values('prozent', ascending=False)

print(missing if not missing.empty else "Keine fehlenden Werte")
print(f"Betroffene Zeilen: {df.isna().any(axis=1).sum()}")
print(f"Vollständige Zeilen: {df.notna().all(axis=1).sum()}")
```

## Bewertungsregeln
- < 5% fehlend → einfache Imputation (Median/Modus) vertretbar
- 5–20% fehlend → Imputation mit Vorsicht, Muster prüfen
- > 20% fehlend → Spalte ggf. droppen oder gesondert behandeln
- > 50% fehlend → Spalte fast immer droppen

## Entscheidung
- Nach Bewertung, mit Empfehlung den User fragen

## Selbstverbesserung
Neue Muster bei strukturellen Fehlerwerten hier ergänzen. User Entscheidungsmuster einbeziehen.
