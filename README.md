# EDA Agent

A token-efficient Exploratory Data Analysis agent built with Claude Code and modular Markdown skills.

## Concept

Most LLM-based data analysis workflows load raw data directly into context — inefficient and expensive. This agent instead judges datasets through statistical queries only (`isna()`, `min/max`, `nunique()`, `describe()`), never reading raw rows unless explicitly necessary.

The agent is built as a set of small, single-purpose skill files that Claude Code loads selectively based on the task at hand. A central `CLAUDE.md` defines principles and accumulates a self-improving error log over time.

## Structure

```
eda-agent/
├── CLAUDE.md                  # Agent instructions & self-improvement log
└── .claude/
    └── skills/
        ├── ingest_csv.md      # CSV ingestion
        ├── ingest_excel.md    # Excel ingestion
        ├── ingest_parquet.md  # Parquet ingestion
        ├── missing_values.md  # Missing value analysis
        ├── outlier_screen.md  # Outlier detection via IQR
        ├── categorical_profile.md  # Cardinality & frequency profiling
        ├── memory_optimize.md # dtype downcast & category conversion
        └── eda_report.md      # Structured summary output
```

## How It Works

Token efficiency is achieved by following a strict rule: no raw data enters the context window. Instead, every judgment is made through aggregations:

| Instead of | The agent uses |
|---|---|
| `df.head(50)` | `df.dtypes`, `df.shape` |
| Viewing all values | `value_counts(normalize=True).head(10)` |
| Manual inspection | `isna().mean()`, `describe()`, IQR bounds |

Modular skills mean Claude Code only loads what's relevant for the current task. An Excel ingestion session won't load the Parquet skill. Each skill file will be kept under ~80 lines.

Self-improvement happens at the end of each session. Claude Code updates `CLAUDE.md` with new patterns, edge cases, and lessons learned, the agent gets more accurate with every dataset it processes.

## Usage

Copy the files into your project root and open it in Claude Code:

```bash
git clone https://github.com/Delamijo/Project_EDA_Agent_ClaudeCode.git
cd eda_agent
# open in VS Code with Claude Code extension
```

Then prompt Claude Code naturally:

```
"Load example.csv and run a full EDA"
"Check for missing values and optimize memory"
"Summarize findings and update CLAUDE.md"
```

## Standard Workflow

1. Ingest (`ingest_csv` / `ingest_excel` / `ingest_parquet`)
2. Missing value analysis (`missing_values`)
3. Outlier screening (`outlier_screen`)
4. Categorical profiling (`categorical_profile`)
5. Memory optimization (`memory_optimize`)
6. Structured report (`eda_report`)

## Self-Improvement Loop

At the end of each session:

```
"Update CLAUDE.md and relevant skills with everything new from this session"
```

Claude Code appends new patterns, edge cases, and fixes to the error log in `CLAUDE.md` and updates the relevant skill files. Over time, the agent accumulates domain knowledge specific to your datasets.

## Tech Stack

- [Claude Code](https://code.claude.com)
- [pandas](https://pandas.pydata.org)
- [pyarrow](https://arrow.apache.org/docs/python/)
- Python 3.10+

## License

MIT

## Future Plans

- Integrate basic Data Visualization
- Cleaning Agent working based on the eda_report and its recommendations