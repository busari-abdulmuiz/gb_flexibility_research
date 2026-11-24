# CLAUDE.md

## This is a learning project

The goal is for me to learn by doing, not to get finished code.

- **Guide and explain.** Explain concepts, suggest approaches, point to the relevant pandas/DuckDB functions, and ask questions that help me reason through the problem.
- **Don't write full solutions unless I ask for them.** Short snippets (a few lines) are fine for illustrating an API or idea. Leave the complete script, function, or notebook cell for me to write.
- **Review rather than rewrite.** When I share code, point out bugs, edge cases, and better idioms, and explain *why*. Let me make the fix.
- **Say what to check.** After suggesting an approach, tell me how I can verify it worked (expected row counts, dtypes, a sanity-check plot).
- If I explicitly ask for full code ("just write it", "give me the solution"), that's fine. Do it, then briefly explain how it works.

## Environment

- Always use the project virtualenv at `.venv` (Python 3.13). Run things with `.venv/bin/python`, `.venv/bin/pytest`, `.venv/bin/jupyter`, or activate it with `source .venv/bin/activate`. Never use the system Python.
- Dependencies are listed in `requirements.txt` (pandas, duckdb, pyarrow, openpyxl, matplotlib, pyyaml, requests, pytest, jupyter). Ask before adding new packages.

## Configuration

- All settings live in `config.yaml`: paths, file names, REPD quarters, and project parameters (baseline/target year, the CP2030 flex range).
- Paths in `config.yaml` are relative to the repo root. Load them from the config instead of hardcoding paths or constants in code.

## Data rules

- **Never edit, overwrite, rename, or delete anything in `data/raw/`.** Treat it as read-only source data. Read from it only.
- Cleaned or derived outputs go in `data/processed/`. The DuckDB file is `data/processed/flex.duckdb`.
- Hand-maintained reference tables live in `data/reference/` (e.g. `roadmap_table1.csv`).
- `data/raw/**` and `data/processed/*` are gitignored. Don't commit data files.

## Repo layout

```
config.yaml          project settings
data/raw/repd/       REPD quarterly CSV extracts (read-only)
data/raw/fes/        FES data (read-only, not yet populated)
data/reference/      reference/target tables
data/processed/      derived outputs + DuckDB
notebooks/           exploration notebooks
sql/                 SQL queries
src/common/          shared helpers (e.g. config loading)
src/tracker/         (planned)
src/simulator/       (planned)
src/battery/         (planned)
tests/               pytest tests
```

## Current stage: Stage 1: Exploring the REPD CSV

We're exploring the Renewable Energy Planning Database (REPD) extract in `data/raw/repd/`. The current file is `REPD_Publication_Q3_2026 (2).csv`. The name contains spaces and parentheses, so quote it in the shell.

Focus for this stage:
- Load the CSV and understand its structure: columns, dtypes, row count, encoding/header quirks.
- Check data quality: missing values, inconsistent categories, capacity units, date formats, duplicates.
- Understand the key dimensions: technology type, development/operational status, installed capacity (MW), region, and key dates.
- Note findings and open questions as we go (in a notebook in `notebooks/`).

Out of scope for now: building the pipeline in `src/`, DuckDB loading, and the tracker/simulator/battery modules. If a question drifts into a later stage, mention that and keep the focus on exploration.

Known things to watch:
- The `repd.quarters` list in `config.yaml` (2024-Q4 … 2025-Q3) doesn't yet match the file on disk (Q3 2026). Raise this with me rather than silently changing it.
