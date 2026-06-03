# Skill: categorical_profile

## Zweck
Kategorische Spalten token-effizient analysieren.

## Wann anwenden
Nutzer fragt nach Text-Spalten, Kategorien, Kardinalität, Häufigkeiten.

## Code
```python
cat_cols = df.select_dtypes(['object', 'category'])

# Kardinalitäts-Überblick
kardinalitaet = pd.DataFrame({
    'unique': cat_cols.nunique(),
    'unique_pct': (cat_cols.nunique() / len(df) * 100).round(2),
    'top_wert': cat_cols.mode().iloc[0],
    'top_haeufigkeit': [df[col].value_counts(normalize=True).iloc[0].round(3) 
                        for col in cat_cols.columns]
})
print(kardinalitaet)

# Top-Werte nur für Spalten mit < 20 unique values
for col in cat_cols.columns:
    if cat_cols[col].nunique() <= 20:
        print(f"\n{col}:")
        print(df[col].value_counts(normalize=True).round(3))
```

## Bewertungsregeln
- unique_pct > 80% → wahrscheinlich ID-Spalte, für ML droppen
- unique_pct < 5% → guter Kandidat für category-dtype
- top_haeufigkeit > 95% → Spalte fast konstant, wahrscheinlich nutzlos

## Selbstverbesserung
Neue Muster bei domänenspezifischen Kategorien hier ergänzen.
