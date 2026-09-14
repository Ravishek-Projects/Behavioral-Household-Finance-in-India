# Indian Household Financial Behavior Analysis

**Author:** Ravishek Kumar
**[🚀 Link to Notebook](https://colab.research.google.com/drive/1-oOEQ55-W7z1qUZ8HF2BCItwdlgS-Ybh#scrollTo=1_GiQG6kOhzV)**

This repository contains an end-to-end data pipeline and econometric analysis exploring the financial behavior of Indian households. Using extensive microdata from the Centre for Monitoring Indian Economy (CMIE), this project investigates consumption smoothing, the "co-holding puzzle" (simultaneously holding high-cost debt and illiquid savings), and the gap between financial intentions and actual behaviors.

## Project Structure

This repository is structured around two primary Jupyter Notebooks:

### 1. `Indian_Household_Data_Cleaning.ipynb`
This notebook handles the massive scale and complexity of the raw CMIE datasets (*Aspirations of India, Consumption Pyramids, Household Income, and People of India*). It executes an 8-stage data engineering pipeline:
- **Schema Standardization:** Scans monthly files to safely resolve and cast mixed data types.
- **Handling Non-Response & Attrition:** Flags non-responders and shifting families rather than invisibly dropping them.
- **Time-Granularity Alignment:** Safely collapses monthly variations in wave-based datasets to prevent fan-out duplicates.
- **Variable Coding & Winsorization:** Normalizes categorical flags and winsorizes extreme financial outliers at the 99th percentile to preserve sample integrity.
- **Derived Indicators:** Computes complex metrics like Income Volatility (`INC_CV`) and Intention-Behavior Gaps.
- **Out-of-Core Merge:** Utilizes **DuckDB** to execute a highly memory-efficient SQL join across millions of rows, producing a clean `master_panel.parquet` without overwhelming pandas/RAM.

### 2. `Final_Analysis.ipynb`
This notebook transitions from data engineering to advanced econometric analysis on the finalized master panel. It explores:
- **Sanity Checks & Benchmarking:** Validates sample asset participation rates against external macroeconomic baselines (e.g., AIDIS).
- **Marginal Propensity to Consume (MPC):** Regresses monthly expenditure on income using Weighted Least Squares (WLS) with cluster-robust standard errors.
- **Permanent Income Hypothesis (PIH):** Decomposes household income into permanent and transitory components, using Wald tests to evaluate how consumption responds to each.
- **The Co-Holding Puzzle:** Analyzes the predictors of holding high-cost debt alongside illiquid savings, aggressively correcting for selection bias and mechanical artifacts in formal employment.
- **Intention-Behavior Gap:** Contrasts follow-through rates on voluntary savings (Gold) versus automated savings (Provident Funds) to test whether formal employment instills genuine behavioral discipline or merely benefits from mechanical payroll deductions.

## Requirements

To run the notebooks, you will need the following Python libraries installed:
```bash
pip install pandas numpy statsmodels pyarrow seaborn matplotlib duckdb
```

## Usage

1. Place the raw CMIE `.csv` files into their respective subdirectories (`Aspirations_of_india/`, `consumption_pyramids/`, `household_income/`, `people_of_india/`).
2. Run `Indian_Household_Data_Cleaning.ipynb` from start to finish. This will produce a `Cleaned_Output/master_panel.parquet` file.
3. Run `Final_Analysis.ipynb` to execute the regressions and view the econometric outputs.

