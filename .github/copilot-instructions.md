<!-- Copilot/agent instructions tailored to this repo -->
# Copilot instructions — SP500 volatility forecasting

Purpose
- Help AI coding agents be productive: where code lives, workflows, and project-specific patterns.

Big picture
- This is a research-style data-science repo: primary work is in `notebooks/` (analysis + pipeline design). The repo has placeholder folders (`src/`, `scripts/`, `data/`, `images/`, `reports/`) but most executable code is currently inside notebooks.
- Typical flow: ingest raw data into `data/` → feature engineering (notebooks/01_data_and_feature_engineering.ipynb) → modeling (notebooks/02_modeling.ipynb) → export figures to `images/` and write summaries to `reports/`.

Key files & references
- Notebook outlines: [notebooks/01_data_and_feature_engineering.ipynb](notebooks/01_data_and_feature_engineering.ipynb#L1-L200) and [notebooks/02_modeling.ipynb](notebooks/02_modeling.ipynb#L1-L200).
- Project README: [README.md](README.md#L1-L200) contains the high-level problem statement and evaluation goals.
- `src/` and `scripts/` are present but empty — prefer adding reusable, testable modules into `src/` rather than proliferating logic inside notebooks.

Environment & run commands (Windows)
- Project uses a virtualenv at `.venv/` if present. To activate on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

- Install dependencies (check notebooks for imports; `requirements.txt` is currently empty):

```powershell
# install pinned deps if provided
pip install -r requirements.txt
# otherwise inspect notebooks for needed packages (pandas, numpy, scikit-learn, matplotlib, xgboost, shap, etc.)
```

Conventions & patterns specific to this repo
- Time-based splitting: notebooks explicitly require STRICT time splits (Train/Val/Test ranges documented in `01_data_and_feature_engineering.ipynb`). Do not use randomized cross-validation for time-series work — use `TimeSeriesSplit` when tuning.
- Feature engineering: the feature list is authoritative in `01_data_and_feature_engineering.ipynb` (lag returns, rolling vol, skew/kurtosis, regime dummies). Re-implement these as pure functions in `src/features.py` when extracting reusable code.
- Outputs: figures -> `images/`, final write-ups -> `reports/`. Keep intermediate or large datasets in `data/` and do not commit private/raw data.

Integration points & expectations
- No external services or APIs are hard-coded. Notebook instructions mention downloading S&P500 from Yahoo Finance — prefer explicit data-download scripts in `scripts/` or functions in `src/data.py` that save to `data/` for reproducibility.
- There are no tests or CI config present. If you add reusable code in `src/`, also add simple unit tests under `tests/` and a `requirements-dev.txt` if needed.

What an AI agent should do first
1. Inspect `notebooks/` to find the smallest reproducible piece of work to convert into `src/` (start with feature functions from `01_data_and_feature_engineering.ipynb`).
2. Create a module under `src/` (e.g., `src/features.py`) and a tiny runner script in `scripts/` that demonstrates usage on a small synthetic or placeholder dataset saved to `data/`
3. Add dependency pins to `requirements.txt` and document activation in README or this file.

Constraints & cautions
- Do not assume `data/` contains sensitive or complete datasets — data folders in this repo are empty. Always write code that can run with small synthetic data or a clear data-download step.
- Preserve notebook narrative. When refactoring code out of notebooks, keep notebooks as orchestration/analysis cells that import functions from `src/`.

If anything is unclear, ask the maintainer which notebook cells are canonical implementations to port into `src/`.
