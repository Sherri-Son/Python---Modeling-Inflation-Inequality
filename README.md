# The Unequal Burden: Modeling Inflation Inequality with Machine Learning Techniques

**Authors:** Sakib Mowla & Sherri Son  
**Affiliation:** Department of Computer and Information Science, Fordham University  
**Contact:** {smowla & cs215}@fordham.edu

---

## Overview

This project investigates how inflation during the COVID-19 era (2020–2024) disproportionately impacted lower-income U.S. households. While the national CPI peaked at 8% in 2022, households in the lowest income quintile experienced effective inflation rates of 10–11%—driven by surging costs in food, energy, and transportation—while wealthier households were partially insulated by spending more on services with modest price growth.

By integrating Bureau of Labor Statistics (BLS) **Consumer Price Index (CPI)** data with the **Consumer Expenditure Survey (CEX)**, the study calculates household-level effective inflation rates across 58,456 U.S. households and applies regression, classification, and clustering techniques to quantify who bears the greatest inflation burden and why.

---

## Key Findings

- **1.46 percentage point gap** in effective inflation between the poorest (Q1) and wealthiest (Q5) income quintiles, expanding to 2–3 points during peak inflation in mid-2022.
- **16% of households** ("High-Inflation Group") faced average rates of **9.72%**, nearly double the rate of the affluent majority.
- **Consumption patterns matter more than demographics**: K-Means clustering revealed that high transportation and energy spending shares—not age or family type alone—drove the greatest inflation exposure.
- **Homeownership does not uniformly protect against inflation**: Q1 homeowners actually faced *higher* inflation than Q1 renters, challenging the assumption that fixed-rate mortgages shield low-income households.
- Regression models (Random Forest, XGBoost) achieved **R² > 0.70**, with food, energy, and housing expenditure shares as the strongest predictors.
- Classification models categorized households into low/medium/high inflation burden groups at **>80% accuracy**.

---

## Repository Structure

```
├── Data_Preprocessing_Nov_25th_data_mining.ipynb   # Data loading, cleaning, EDA, and train/test/val split
├── Final_Presentation_Analysis_1___4_.ipynb         # Regression & classification modeling (Ridge, Lasso, RF, XGBoost, SVM)
├── Final_Presentation_Analysis_2__5_.ipynb          # Post-clustering deep-dive analysis & visualizations
├── post_clustering_analysis__1_.ipynb               # Cluster persona development, geographic & spending pattern analysis
└── README.md
```

### Notebook Descriptions

| Notebook | Purpose |
|----------|---------|
| `Data_Preprocessing_Nov_25th_data_mining.ipynb` | Loads the merged CPI×CEX dataset, handles missing values (drops `Family_Type` at 38% missing; removes 876 rows with missing `Region`), computes summary statistics, performs EDA including inflation-by-quintile analysis, and splits data into 70/20/10 train/test/validation sets. |
| `Final_Presentation_Analysis_1___4_.ipynb` | Implements regression models (Ridge, Lasso, Random Forest, XGBoost) to predict effective inflation rates, and classification models (Random Forest, XGBoost, SVM, ensemble voting) to categorize households into low/medium/high burden groups. Includes SHAP explainability analysis. |
| `Final_Presentation_Analysis_2__5_.ipynb` | Post-clustering storytelling: names cluster personas, analyzes geographic distribution of vulnerable households, visualizes spending patterns, and creates policy-relevant summary graphics. |
| `post_clustering_analysis__1_.ipynb` | K-Means clustering (k=4 via Silhouette score), PCA and t-SNE dimensionality reduction visualizations, and detailed cluster profiling. |

---

## Data

**Sources (Bureau of Labor Statistics):**
- **Consumer Price Index (CPI):** Monthly category-level price changes, 2020–2024.
- **Consumer Expenditure Survey (CEX):** Household-level expenditure shares across income quintiles and demographic groups, 2020–2023.

**Merged Dataset:** 58,456 households × 22 features including:
- **Demographics:** income quintile, family size, number of earners/children, age of household head, education, employment status, marital status
- **Geography:** region, urban/rural classification, MSA size
- **Expenditure shares (8 categories):** food at home, food away from home, housing, energy, transportation, healthcare, education, apparel
- **Target variable:** Effective Inflation Rate (household-specific, weighted by spending shares × CPI category changes)

> **Note:** The raw input CSV (`Modified_final_inflation_data 2.csv`) is not included in this repository. The notebooks were originally run in Google Colab with data loaded from Google Drive.

---

## Methodology

### Exploratory Data Analysis
- Missing value assessment and handling
- Inflation rate distribution by income quintile and product category
- Homeownership vs. renter inflation comparison across quintiles
- Urban/rural inflation gap analysis

### Regression
- **Models:** Ridge Regression, Lasso Regression, Random Forest, XGBoost
- **Target:** Effective Inflation Rate (continuous)
- **Evaluation:** R², MAE, RMSE
- **Interpretability:** SHAP values for feature importance

### Classification
- **Models:** Random Forest (200 trees), XGBoost (200 estimators), SVM (RBF kernel), Ensemble Voting Classifier
- **Target classes:** Low (<5%), Medium (5–10%), High (>10%) inflation burden
- **Evaluation:** Accuracy, confusion matrices, ROC curves, precision-recall metrics
- **Class balancing:** Balanced class weights

### Clustering
- **Algorithm:** K-Means (k=4, selected by Silhouette score = 0.145)
- **Features:** Demographic attributes + expenditure shares + effective inflation rate (standardized)
- **Visualization:** PCA (2D) and t-SNE (3D)
- **Result:** 3 interpretable clusters + 1 outlier cluster (0.5%)

---

## Requirements

The notebooks were developed in **Google Colab** (Python 3). Key dependencies:

```
numpy
pandas
matplotlib
seaborn
plotly
scipy
scikit-learn
xgboost
shap
```

Install all dependencies:
```bash
pip install numpy pandas matplotlib seaborn plotly scipy scikit-learn xgboost shap
```

---

## How to Run

1. Upload the dataset (`Modified_final_inflation_data 2.csv`) to your Google Drive or update the file path in each notebook.
2. Run the notebooks in order:
   - `Data_Preprocessing_Nov_25th_data_mining.ipynb` → produces train/test/validation splits
   - `Final_Presentation_Analysis_1___4_.ipynb` → regression & classification results
   - `post_clustering_analysis__1_.ipynb` → clustering analysis
   - `Final_Presentation_Analysis_2__5_.ipynb` → post-clustering visualizations
3. Intermediate outputs (e.g., `train_data_with_clusters.csv`) are generated by earlier notebooks and consumed by later ones.

---

## Limitations & Future Work

- **Cross-sectional design** prevents tracking how households adapt spending over time.
- **Binary/threshold classification** (e.g., 6.8% "safe" vs. 7.0% "high risk") introduces arbitrary cutoffs—future work should explore continuous regression or multi-class frameworks.
- **62% recall for high-risk class** means 38% of vulnerable households go undetected.
- **Future directions:** longitudinal panel analysis, granular sub-category CPI data, and integration of real wage/income growth data.

---

## References

1. Broda, C., & Romalis, J. (2009). *The Welfare Implications of Rising Commodity Prices.* NBER Working Paper No. 14437.
2. Kaplan, G., & Schulhofer-Wohl, S. (2017). Inflation at the Household Level. *Journal of Monetary Economics*, 91, 19–38.
3. Jaravel, X. (2021). Inflation Inequality: Measurement and Implications. *Brookings Papers on Economic Activity*, Fall 2021.
4. Bureau of Labor Statistics (2020–2024). *Consumer Price Index (CPI).* U.S. Department of Labor.
5. Bureau of Labor Statistics (2020–2023). *Consumer Expenditure Survey (CEX).* U.S. Department of Labor.
6. Luhby, T. (2014). The Wealth Gap is Getting Worse. *CNN Money*.
7. NAACP. (2009). *Discriminatory Lending Practices and Their Impact on African American Communities.* NAACP Economic Department Report.
8. Altig, D., et al. (2024). Inflation's Impact on American Households. NBER Working Paper No. 32482.
9. Coibion, O., Gorodnichenko, Y., & Weber, M. (2022). Monetary Policy Communications and Inflation Expectations. *Brookings Papers on Economic Activity*, Spring 2022.

---

## License

This project was developed as part of coursework at Fordham University. Please contact the authors for permissions regarding reuse or citation.
