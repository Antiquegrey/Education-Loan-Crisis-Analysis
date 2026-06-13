# Project Explanation: Education Loan Crisis Analysis (India)

This document provides a comprehensive explanation of the project structure, concepts, and files. It is designed to help you understand how the data cleaning, merging, and analysis pipelines work.

---

## 1. Project Overview
This project analyzes the education loan ecosystem in India, focusing on regional disbursements, bank types, growth trends, outstanding loan burdens, and gender gaps in STEM (Science, Technology, Engineering, and Mathematics) enrollment. The goal is to uncover geographical and socioeconomic imbalances in how educational funding is accessed across different states and UTs.

---

## 2. File and Directory Structure
The workspace is organized as follows:

- **`analysis.py`**: The core Python script that automates the data pipeline. It cleans raw data, standardizes state names, merges multiple datasets, performs feature engineering (calculates averages, growth rates, gender splits), and saves 12 visualizations to the `plots/` folder.
- **`dataset/`**: The folder containing all raw inputs and the final output csv:
  - `RS_Session_262_AU_238_A_i.csv`: Raw wide-format table of state-wise disbursements over 3 years.
  - `state_wise_outstanding_loans.csv`: Raw outstanding loan accounts and amounts (as of 2021).
  - `state_wise_stem_enrollment.csv`: Raw STEM enrollment numbers by gender (from 2015-16).
  - `RS_Session_266_AU_2460_C_and_D.csv`: National YoY totals (2019-2024).
  - `bank_type_loans.csv`: YoY loans split by Public, Private, and Small Finance banks.
  - `summary_statistics.csv` (also copied to root): The clean, integrated output dataset.
- **`plots/`**: The output directory for static (`.png`) and interactive (`.html`) charts.
- **`practice_interview_prep.ipynb`**: An interactive Jupyter Notebook designed to help you practice writing the data cleaning and plotting code step-by-step.
- **Documentation/Context Files** (e.g., `parliament_qa.pdf`, `goi.pdf`, `implementation plan.pdf`): PDF files containing context, source parliament questions, and project design guidelines.

---

## 3. Data Cleaning & Integration Pipeline (`analysis.py` Phase 1)
The data cleaning phase resolves three key issues: wide-format columns, spelling inconsistencies, and missing values.

### A. State Name Standardization
Indian state names are frequently spelled differently across public datasets (e.g., `"Andaman & Nicobar Islands"` vs. `"Andaman and Nicobar Islands"` or typos like `"Chhatisgarh"`).
- The pipeline uses a `STATE_STANDARDIZATION` mapping dictionary in Python to map all variations to a single uniform set of names.

### B. Reshaping (Melting) Wide to Long Format
The raw disbursement table (`RS_Session_262_AU_238_A_i.csv`) has years as columns (`2021-22 - Number of Account`, `2021-22 - Amount Disbursed`, etc.).
- The pipeline filters out summary rows (such as "Total") and melts this data so each row represents a single **State-Year** combination.
- The columns are normalized to: `['State/UT', 'Year', 'Accounts_Disbursed', 'Amount_Disbursed_Crores']`.

### C. Aggregating and Merging Datasets
The other datasets are merged on the `State/UT` column:
1. **Outstanding Loans**: Grouped by state, summing up loan accounts and outstanding amounts as of 2021.
2. **STEM Enrollment**: Grouped by state, summing total, male, and female enrollment.
3. **Left Joins**: The long-format disbursement table is joined with the aggregated outstanding loans and STEM enrollment.
4. **NaN Handling**: Any missing records (e.g. states with missing STEM enrollment or outstanding loan info) are filled with `0` to prevent computation errors.

### D. Feature Engineering
The pipeline creates new metrics for analysis:
- **`Average_Loan_Size_Lakhs`**: Calculated as $\frac{\text{Amount Disbursed (Crores)} \times 100}{\text{Accounts Disbursed}}$. A division-by-zero check is implemented to return `0` if accounts disbursed is `0`.
- **`STEM_Enrollment_Female_Pct`**: $\frac{\text{Female STEM Enrollment}}{\text{Total STEM Enrollment}} \times 100$, with division-by-zero checks.
- **`Growth_Rate_2021_to_2023_Pct`**: The state-wise percentage change in loan amount disbursed from `2021-22` to `2023-24`.
- **`Region`**: Each state is categorized into a region (`South`, `North`, `East`, `West`, `Central`, `Northeast`) to analyze geographic trends.

---

## 4. Visualizations (`analysis.py` Phase 2)
The script generates 10 different analyses to answer specific questions:

1. **Analysis 1: Loan Distribution (`analysis_1_loan_distribution.png`)**
   - *What it does*: A histogram with a KDE (Kernel Density Estimate) curve.
   - *Concept*: Shows the distribution of loan amounts across states for 2023-24. It highlights that a few states get extremely high funding while most get very little.
2. **Analysis 2: Top vs. Bottom States (`analysis_2_top_bottom_states.png`)**
   - *What it does*: Side-by-side horizontal bar charts showing the top 10 and bottom 10 states by loan disbursements.
   - *Concept*: Visualizes the extreme skew in funding (e.g., southern states dominate, while smaller UTs and northeastern states receive minimal amounts).
3. **Analysis 3: National YoY Trend (`analysis_3_national_yoy_trend.png`)**
   - *What it does*: A dual-axis chart (bar chart for accounts, line chart for amount disbursed) from 2019 to 2024.
   - *Concept*: Reveals the overall national growth trend in loans.
4. **Analysis 4: Bank Type Comparison (`analysis_4_bank_type_comparison.png`)**
   - *What it does*: Stacked bar charts showing YoY loan accounts and amounts disbursed, grouped by bank type (Public Sector, Private Sector, Small Finance Banks).
   - *Concept*: Shows that Public Sector Banks (PSBs) carry the vast majority of the education loan volume.
5. **Analysis 5: Growth Rate Map & Bar Chart (`analysis_5_growth_rate_bar.png` & `analysis_5_growth_rate_map.html`)**
   - *What it does*: A sorted bar chart and an interactive Plotly choropleth map of India.
   - *Concept*: Shows which states are growing fastest in terms of loan disbursements and which are stagnant.
6. **Analysis 6: Amount vs. Accounts Correlation (`analysis_6_amount_vs_accounts.png`)**
   - *What it does*: A scatter plot with a regression line.
   - *Concept*: Correlates the number of accounts with the total amount disbursed to identify outlier states receiving unusually large average loans.
7. **Analysis 7: STEM Gender Gap (`analysis_7_gender_gap.png`)**
   - *What it does*: A diverging bar chart (green for female-skewed, red for male-skewed).
   - *Concept*: Compares the percentage gap between female and male STEM enrollment. Southern states like Kerala and Tamil Nadu show much higher female STEM representation compared to northern states.
8. **Analysis 8: Loan Access vs. STEM Enrollment (`analysis_8_loan_vs_stem_bubble.png` & `.html`)**
   - *What it does*: A bubble chart where the x-axis is STEM enrollment, the y-axis is loan amount, bubble size is the outstanding loan balance, and color is the region.
   - *Concept*: Identifies underserved states. States with high STEM enrollment but low loan disbursements represent areas where students face high financial barriers to higher education.
9. **Analysis 9: State-wise Year Heatmap (`analysis_9_heatmap.png`)**
   - *What it does*: A matrix heatmap of states against years, shaded by disbursement volume.
   - *Concept*: Compares disbursements across all states and years in a single view, sorted by total volume.
10. **Analysis 10: Regional Distribution Boxplot (`analysis_10_boxplot_region.png`)**
    - *What it does*: Boxplots representing loan distributions across geographic regions (South, North, East, etc.).
    - *Concept*: Conclusively proves that the South region has significantly higher loan access and variance compared to all other regions.
