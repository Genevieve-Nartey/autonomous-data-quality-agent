# Autonomous Data Quality Agent

An agentic AI system that autonomously audits any tabular dataset for data quality issues and produces a prioritised, scored remediation report — with no manual configuration of what to check.

---

## Architecture

```
Raw Dataset (CSV)
        |
        v
   Agent (claude-sonnet-4-5)
        |
        |-- profile_dataset()             -> Shape, types, null counts, stats
        |-- check_missing_values()        -> Severity-classified null analysis
        |-- check_duplicates()            -> Exact + key-column duplicate detection
        |-- check_outliers()              -> Z-score + IQR dual-method detection
        |-- check_format_violations()     -> Regex validation (email, phone, dates)
        |-- check_referential_integrity() -> Foreign key range validation
        |-- generate_dq_report()          -> Scored Markdown report
        |
        v
dq_report_{dataset}.md  +  PNG charts
```

---

## Features

- Fully autonomous — the agent decides which checks to run and in what order based on profiling results
- 7 specialised quality check tools covering all major DQ dimensions
- Dual outlier detection (Z-score parametric + IQR non-parametric) for robust coverage
- Severity classification: CRITICAL / WARNING / INFO per issue
- 0-100 data quality score with visual progress bar
- Prioritised remediation plan with actionable fix recommendations
- Two visualisations: missing value rates chart + outlier box plots

---

## Setup

```bash
git clone https://github.com/Genevieve-Nartey/autonomous-data-quality-agent.git
cd autonomous-data-quality-agent

python -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt
```

Create `.env`:
```
ANTHROPIC_API_KEY=sk-ant-your-key-here
```

---

## Usage

**Run the demo (generates dirty data + runs full audit):**
```bash
python data_quality_agent.py
```

**Audit your own CSV:**
```python
from data_quality_agent import load_dataset, run_dq_agent
from pathlib import Path

load_dataset(Path("your_data.csv"))
result = run_dq_agent(dataset_name="your_dataset")
print(result)
```

**Run in Jupyter / Kaggle:**

Open `autonomous_data_quality_agent.ipynb`. Every line is explained with inline comments and markdown cells.

On Kaggle, add your `ANTHROPIC_API_KEY` via **Add-ons → Secrets** before running.

---

## Quality Checks

| Check | Method | What It Finds |
|-------|--------|---------------|
| Missing values | `isnull().sum()` | Null/NaN counts with CRITICAL/WARNING thresholds |
| Duplicates | `duplicated()` | Exact rows and key-column near-duplicates |
| Outliers | Z-score + IQR | Values beyond 3σ or Tukey fences |
| Format violations | Compiled regex | Malformed emails, phones, dates |
| Referential integrity | Range check | Foreign key values outside valid domain |

---

## Output Files

| File | Description |
|------|-------------|
| `dq_report_{dataset}.md` | Full scored remediation report |
| `dq_missing_values.png` | Colour-coded missing value rate bar chart |
| `dq_outliers.png` | Box plots for columns with outliers |

---

## Project Structure

```
autonomous-data-quality-agent/
    data_quality_agent.py                  # Production script
    autonomous_data_quality_agent.ipynb    # Annotated Kaggle notebook
    requirements.txt
    .env
    README.md
```

---

## Key Concepts Demonstrated

| Concept | Implementation |
|---------|---------------|
| Autonomous workflow | Agent profiles data first, then selects appropriate checks |
| Dual outlier methods | Z-score (normal distribution) + IQR (distribution-free) |
| Module-level regex compilation | `re.compile()` at import time for performance |
| Data quality scoring | Weighted severity score across all detected issues |
| Deliberate dirty data | Systematic injection of 7 distinct DQ problem types |

---

## Licence

MIT
