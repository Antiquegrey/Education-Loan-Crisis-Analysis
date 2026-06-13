# India Education Loan & STEM Enrollment Analysis

This project analyzes the education loan ecosystem in India, exploring state-wise disbursements, regional outstanding loan burdens, bank type distributions (public sector vs. private), and their relationship with STEM student enrollments.

The analysis is automated via a clean Python pipeline that aggregates disparate public datasets, standardizes state name variations, handles mathematical edge-cases, and produces 12 analytical plots.

A fully executed Jupyter Notebook `education_loan_analysis.ipynb` is included at the root. It contains step-by-step code execution, markdown comments, and plots pre-rendered inline for native viewing on GitHub.

---

## 📊 Summary of Datasets Included
All datasets are located in the `dataset/` directory:
- **`RS_Session_262_AU_238_A_i.csv`**: State-wise loan disbursements (2021–2024).
- **`state_wise_outstanding_loans.csv`**: Total outstanding loan accounts and amounts per state (as of 2021).
- **`state_wise_stem_enrollment.csv`**: Total STEM enrollment categorized by gender.
- **`RS_Session_266_AU_2460_C_and_D.csv`**: National year-over-year education loan totals (2019–2024).
- **`bank_type_loans.csv`**: YoY loans categorized by bank types (PSB vs. Private vs. Small Finance).
- **`summary_statistics.csv`**: The clean, integrated dataset containing merged loan and STEM variables per state.

---

## 📈 Key Analyses & Visualizations
The analysis script (`analysis.py`) processes the above datasets and generates the following files in the `plots/` directory:
1. **`analysis_1_loan_distribution.png`**: Distribution of loan amounts across states (2023-24).
2. **`analysis_2_top_bottom_states.png`**: Top 10 and bottom 10 states by loan disbursements.
3. **`analysis_3_national_yoy_trend.png`**: Dual-axis line and bar chart showing national YoY loan volume trends.
4. **`analysis_4_bank_type_comparison.png`**: Stacked bar charts showing loans by bank categories.
5. **`analysis_5_growth_rate_bar.png` / `_map.html`**: Disbursement growth rates per state (Bar chart and interactive Choropleth Map of India).
6. **`analysis_6_amount_vs_accounts.png`**: Correlation between loan accounts and total disbursement amount.
7. **`analysis_7_gender_gap.png`**: Diverging bar chart showing the STEM enrollment gender gap per state.
8. **`analysis_8_loan_vs_stem_bubble.png` / `_bubble.html`**: Disbursement vs. STEM enrollment bubble chart (size represents outstanding loans).
9. **`analysis_9_heatmap.png`**: A full-year state disbursement heatmap.
10. **`analysis_10_boxplot_region.png`**: Regional distribution boxplot showing loan access variance.

---

## 📓 Interactive Jupyter Notebook
A complete Jupyter Notebook, [education_loan_analysis.ipynb](file:///Users/vijay/Desktop/resumes/data(volvo)/Education%20Loan%20Crisis%20Analysis%7C/education_loan_analysis.ipynb), is provided in the root directory. You can view it directly on GitHub to see all steps, code execution outputs, and visualizations rendered inline.

---

## 🚀 How to Run the Analysis

### 1. Prerequisites
Ensure you have Python 3 installed. If you are using the provided virtual environment `venv`, activate it first:

```bash
# On macOS / Linux:
source venv/bin/activate
```

Install the dependencies if running outside of the virtual environment:
```bash
pip install pandas numpy matplotlib seaborn plotly
```

### 2. Run the Script
To run the clean data pipeline and regenerate all charts:
```bash
python analysis.py
```
This will:
- Clean and merge the datasets.
- Save the clean, merged dataset to `summary_statistics.csv`.
- Recreate all plots inside the `plots/` folder.
