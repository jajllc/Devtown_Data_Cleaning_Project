# Sales Data Cleaning Mini Project

A Python/Pandas project that cleans a messy 185-row sales dataset in Google Colab and exports an analysis-ready CSV, with a full audit trail of every change.

## Overview
`Messy_data.csv` contains orders with duplicate rows, missing values, outliers, numbers stored as text, inconsistent capitalization and mixed date formats. This notebook finds each problem, fixes it with a documented rule, and checks the result.

## Problems Found and How They Were Fixed
| Problem | Fix |
|---|---|
| Extra spaces, inconsistent capitals (12 city spellings, 8 status spellings) | Trimmed, title-cased, mapped known variants → 8 cities, 5 statuses |
| Numbers stored as text ("three", "2 pcs", "18,500", "forty", "29 years") | Custom `to_number()` function; unparseable values ("unknown") set to missing |
| Mixed and invalid dates | ISO format parsed first, the rest read day-first; invalid date set to missing |
| Impossible values (quantity -3, age 185) and placeholder prices (999999, 0.01) | Business rules set them to missing before imputation |
| 5 duplicate rows | Removed after text standardization (185 → 180 rows) |
| Missing values | Median imputation (price uses the same product's median); "Unknown" for text; dates left blank |
| Outliers | IQR fences (prices within each product); flagged and capped, not deleted |

## Results
| Check | Before | After |
|---|---|---|
| Rows | 185 | 180 |
| Duplicate rows | 5 | 0 |
| City spellings | 12 | 8 |
| Status spellings | 8 | 5 |
| Missing values (excluding dates) | 12 blanks + 3 invalid values | 0 |
| Columns | 9 | 10 (`TotalAmount` added) |

## Key Decisions
- **Median, not mean:** extreme values distort means, and a missing price is not zero.
- **Cap, don't delete:** outliers are flagged in `*_outlier` columns and capped to IQR limits, so no orders are lost.
- **Dates stay blank when unknown:** inventing a date would be fabricating data.
- **Everything is traceable:** `*_imputed` and `*_outlier` columns record every change.

## How to Run
1. Open the notebook in Google Colab: [Open in Colab]([YOUR_COLAB_LINK](https://colab.research.google.com/drive/1ZiBqK2J6CkKPI0uivJx0tMqgBx-AOZ-m?usp=sharing))
2. Run all cells in order (**Runtime → Run all**).
3. When prompted, upload `Messy_data.csv`.
4. The cleaned files download automatically at the end.

**Requirements:** Python 3, `pandas`, `numpy` (both preinstalled in Colab).

## Repository Contents
```text
├── Sales_Data_Cleaning_Mini_Project.ipynb    # Notebook with code and explanations
├── Messy_data.csv                            # Raw input data
├── cleaned_sales_dataset.csv                 # Cleaned data (10 columns)
├── cleaned_sales_dataset_with_flags.csv      # Cleaned data plus outlier/imputation flags
└── README.md
```

## Assumptions and Limitations
- Non-ISO dates are assumed to be day-first (02/03/2026 = 2 March 2026).
- Negative quantities are treated as entry errors, not returns.
- Median-filled values are estimates; use the `*_imputed` flags to exclude them when precision matters.
- Capping changes the original value of 4 prices and 1 quantity. The flag columns preserve that history.

## Skills Demonstrated
Pandas, data quality auditing, type conversion, date parsing, missing-value imputation, IQR outlier handling, feature engineering, reproducible notebook workflow.
