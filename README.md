# DS4002 Case Study - Charlottesville Housing Uncertainty Analysis

## Before You Begin: How to Use This README

This README is your guide for understanding the project, navigating the repository, and reproducing the full housing uncertainty analysis pipeline. Please read through it carefully before running any scripts.

---

## Quick Start (Read This First)

To reproduce the full workflow, run the notebooks in this exact order:

1. `scrape_meeting_mins.ipynb`
2. `scrape_caar_reports.ipynb`
3. `join_created_csv_files.ipynb`
4. `cleaning_for_eda.ipynb`
5. `exploratory_data_analysis_visualizations.ipynb`
6. `time_series_regression_model.ipynb`

All datasets, cleaned files, and output visualizations will be created automatically in the appropriate folders as you run each notebook.

If you need more help, refer to the rest of this README, which includes the complete project structure, detailed reproduction steps, and troubleshooting tips.

---

## Understanding the Repository

To get started:

**Look at the Folder Structure section** — This shows you exactly where each file lives in the repository. Understanding what data, scripts, and outputs exist will help you follow the workflow smoothly.

**Review the Reproducing the Results section** — This outlines the exact order to run the notebooks, what each notebook does, and what outputs you should expect at every step. If your results differ from what is described, you may have found an issue to troubleshoot.

**Check the Troubleshooting Notes** — The notes at the end highlight common pitfalls such as missing raw data files, path issues, or mismatched dependencies. If something goes wrong, this section will help you resolve it.

By reading these sections first, you will understand how the project is organized, what each component does, and how to replicate the final results with confidence.

---

## Project Background

### Motivation and Context

Housing affordability and market volatility are pressing concerns in college towns like Charlottesville, Virginia. Local policy discussions — captured in Housing Advisory Committee (HAC) meeting minutes — often reflect community uncertainty about the housing market. At the same time, CAAR (Charlottesville Area Association of Realtors) publishes monthly market indicator reports with concrete data on prices, inventory, and sales trends.

By combining sentiment and uncertainty signals extracted from HAC meeting minutes with quantitative housing market data from CAAR reports, we can investigate whether community-level uncertainty corresponds to measurable changes in housing prices or market volatility. If we extract linguistic signals from meeting minutes and align them with market indicators over time, a time series regression model can test whether uncertainty predicts price movement.

### Hypothesis

A time series regression model using uncertainty signals extracted from HAC meeting minutes will explain a statistically significant portion of variance in Charlottesville housing market indicators.

### Research Question

To what extent do uncertainty signals in Charlottesville Housing Advisory Committee meeting minutes correlate with and predict changes in local housing market indicators over time?

---

## Software Requirements

### Languages and Tools

- Python 3.11
- Jupyter Notebook for scraping, cleaning, analysis, and modeling

### Key Packages Used

- `numpy`: numerical operations
- `pandas`: data loading and manipulation
- `matplotlib.pyplot` / `seaborn`: visualizations
- `os` / `re`: file handling and text processing
- `nltk`: natural language processing and tokenization
- `pdfplumber`: extracting text from CAAR PDF reports
- `statsmodels.api`: time series regression modeling

### Environment

Development performed on Jupyter Notebook (macOS). The pipeline runs on macOS, Windows, or Linux with dependencies installed.

### Dependencies

All required packages can be installed with:

```
pip install numpy pandas matplotlib seaborn statsmodels nltk pdfplumber
```

Additionally, download the required NLTK tokenizer:

```python
import nltk
nltk.download('punkt')
```

---

## Folder Structure

```
DS4002-Project1
├── Supplemental Materials
│   ├── DATA
│   │   ├── caar_monthly_reports                         
│   │   │   ├── 25-03-caar-market-indicators.pdf
│   │   │   ├── 25-04-market-report.pdf
│   │   │   ├── 25-05-market-indicator-report.pdf
│   │   │   ├── 25-06-market-indicator-report.pdf
│   │   │   ├── 25-07-market-indicator-report.pdf
│   │   │   ├── 25-08-market-indicator-report.pdf
│   │   │   ├── 25-09-market-indicator-report.pdf
│   │   │   ├── 25-10-market-report.pdf
│   │   │   └── 25-11-market-report.pdf
│   │   │
│   │   └── charlottesville_hac_meeting_mins             
│   │       ├── HAC Minutes 03-2025.txt
│   │       ├── HAC Minutes 04-2025.txt
│   │       ├── HAC Minutes 05-2025.txt
│   │       ├── HAC Minutes 06-2025.txt
│   │       ├── HAC Minutes 07-2025.txt
│   │       ├── HAC Minutes 09-2025.txt
│   │       ├── HAC Minutes 10-2025.txt
│   │       └── HAC Minutes 11-2025.txt
│   ├── OUTPUT
│   │   ├── uncertainty_vs_price_timeseries.png
│   │   ├── correlation_heatmap.png
│   │   └── uncertainty_vs_volatility_scatter.png
│   ├── SCRIPTS
│   │   ├── scrape_meeting_mins.ipynb
│   │   ├── scrape_caar_reports.ipynb
│   │   ├── join_created_csv_files.ipynb
│   │   ├── cleaning_for_eda.ipynb
│   │   ├── exploratory_data_analysis_visualizations.ipynb
│   │   └── time_series_regression_model.ipynb
│   └── ARTICLES
│   │   ├── nlp_in_finance.pdf
│   │   └── what_is_an_arimax_model.pdf
│
├── Case Study Rubric.pdf
├── Hook Document.pdf
├── README.md
└── LICENSE.md
```

---

## Reproducing the Results

### Data locations (already in the repo)

- `Data/caar_monthly_reports/` → raw CAAR PDF market indicator reports
- `Data/charlottesville_hac_meeting_mins/` → raw HAC meeting minutes as .txt files

All scripts expect a joined and cleaned dataset with columns for date, uncertainty signal, and housing market indicators.

### Reproduction steps (run in this order)

**1. `scrape_meeting_mins.ipynb`**
- Reads HAC meeting minute .txt files and extracts uncertainty-related language signals.
- Output → structured uncertainty data passed into the join step.
- Not strictly required if a pre-joined CSV is available, but documents the text extraction process.

**2. `scrape_caar_reports.ipynb`**
- Uses `pdfplumber` to extract housing market indicators (prices, inventory, sales) from CAAR monthly PDF reports.
- Output → structured market data passed into the join step.

**3. `join_created_csv_files.ipynb`**
- Merges scraped HAC uncertainty signals with CAAR market data on a shared date key.
- Output → a single joined CSV for cleaning.
- Prints: row count and date range of the merged dataset.

**4. `cleaning_for_eda.ipynb`**
- Cleans and normalizes the joined dataset: handles missing values, standardizes date formats, removes erroneous rows.
- Output → cleaned dataset ready for analysis.
- Prints: row count, null counts, column summary.

**5. `exploratory_data_analysis_visualizations.ipynb`**
- Produces exploratory visualizations of uncertainty signals and market indicators over time.
- Outputs → `Output/uncertainty_vs_price_timeseries.png`, `Output/correlation_heatmap.png`, `Output/uncertainty_vs_volatility_scatter.png`

**6. `time_series_regression_model.ipynb`**
- Fits a time series regression model using `statsmodels` to test whether uncertainty signals predict housing market indicators.
- Output → regression summary and model diagnostics printed in-notebook.

### How to verify

Review the output plots in `Output/` and the regression summary printed by `time_series_regression_model.ipynb`. Coefficients, p-values, and R² should be consistent across runs given the same raw data files.

### Clean run (optional)

Delete any previously generated intermediate CSV files and `Output/` plots. Re-run all six notebooks in order.

### Important notes

- Raw PDF and .txt files must be present in `Data/` before running any scraping notebooks.
- August HAC minutes (`HAC Minutes 08-2025.txt`) are absent from the dataset; this gap is expected and handled during cleaning.
- Results depend on the specific PDF layouts of CAAR reports — if new report formats are used, `scrape_caar_reports.ipynb` may require updates.

---

## Troubleshooting Tips

If anything goes wrong while running the notebooks, here are common issues and how to fix them.

**FileNotFoundError (most common)**

This usually happens when a notebook cannot locate a raw data file.

How to fix:
- Confirm that all CAAR PDFs are present in `Data/caar_monthly_reports/` and all HAC .txt files are in `Data/charlottesville_hac_meeting_mins/`.
- Check that filenames match exactly as listed in the Folder Structure section.
- Paths are relative to the repository root — make sure you launch Jupyter from the top-level folder.

**Environment or dependency issues**

If imports fail or packages behave unexpectedly:

```
pip install numpy pandas matplotlib seaborn statsmodels nltk pdfplumber
```

Restart the Jupyter kernel and re-run all cells. If that does not work, install packages individually at the top of the relevant notebook.

**NLTK tokenizer missing**

If you see an error like `Resource punkt not found`:

```python
import nltk
nltk.download('punkt')
```

**pdfplumber cannot extract text**

If CAAR report text is not extracted correctly, the PDF may use a non-standard layout or scanned images instead of embedded text. Check that the file opens and displays readable text in a standard PDF viewer. Scanned PDFs will require OCR preprocessing before `pdfplumber` can read them.

**Columns not found (KeyError)**

If a notebook raises a KeyError on a column name, confirm that the join step in `join_created_csv_files.ipynb` completed successfully and that the expected columns are present in the merged CSV.

**Notebook ran out of order**

If outputs look wrong or variables are missing:
- Restart the kernel.
- Run the entire notebook top to bottom without skipping cells.
- Follow the correct sequence listed in the Quick Start section:

```
01 scrape_meeting_mins
02 scrape_caar_reports
03 join_created_csv_files
04 cleaning_for_eda
05 exploratory_data_analysis_visualizations
06 time_series_regression_model
```

**Regression output looks unexpected**

If model coefficients or p-values differ significantly from expected:
- Confirm the cleaning notebook ran successfully and no rows with missing dates or NaN values remain.
- Check that the date column is properly formatted as a datetime type before modeling.
- Large deviations may indicate the join step produced duplicate or misaligned rows — re-run Steps 3 and 4 before re-running the model.
