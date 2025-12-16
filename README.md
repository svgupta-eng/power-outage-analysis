# Power Outages: Analysis and Cause Prediction Model

**Authors:** Sana Gupta & Aarushi Jain
**Project Website:** [View the Analysis Here](https://svgupta-eng.github.io/power-outage-analysis/)

## Project Overview
This project explores a dataset of major power outages in the U.S. to understand the characteristics and causes of these events. The primary goal is to build a machine learning model capable of predicting the **cause of a power outage** (e.g., severe weather, intentional attack, equipment failure) based on location, time, and climate characteristics.

## Dataset & Data Cleaning
The dataset utilized is `outage.xlsx`. The data cleaning process involved:
* **Feature Selection:** Retaining relevant columns such as `YEAR`, `MONTH`, `U.S._STATE`, `CLIMATE.REGION`, `POPULATION`, and `CAUSE.CATEGORY`.
* **Date Time formatting:** Combining `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into a single datetime object.
* **Feature Engineering:** Created a new `URBANIZATION` column by combining `POPPCT_URBAN` and `POPPCT_UC` to better represent population density.
* **Handling Missing Values:** Coerced invalid numeric entries to `NaN` and identified missing data patterns.

## Exploratory Data Analysis (EDA)
We performed univariate and bivariate analyses to understand the distributions of outages:
* **Trends over time:** Analyzed the frequency of outages per year and month.
* **Geographic Analysis:** Examined outage counts by Climate Region.
* **Cause Analysis:** Investigated the relationship between outage causes and population density.

### Key Hypothesis Test
We tested whether **Severe Weather** outages have a longer duration on average than outages caused by other factors.
* **Null Hypothesis:** The mean duration of severe weather outages equals the mean duration of other causes.
* **Result:** With a p-value of **0.0**, we rejected the null hypothesis, suggesting severe weather outages significantly differ in duration.

## Assessment of Missingness
We conducted permutation tests to determine if the missingness in the `OUTAGE.DURATION` column was dependent on other columns.
* **Dependent on Cause:** The p-value was **0.0**, indicating missingness depends on `CAUSE.CATEGORY`.
* **Dependent on Year:** The p-value was **1.0**, indicating missingness does not depend on `YEAR`.

## Prediction Model
We framed a multiclass classification problem to predict `CAUSE.CATEGORY`.

### Baseline Model
* **Algorithm:** Logistic Regression.
* **Features:** `YEAR`, `URBANIZATION`, and `CLIMATE.REGION`.
* **Accuracy:** ~47.2%.

### Final Model
* **Algorithm:** Random Forest Classifier.
* **Features:** `YEAR`, `MONTH`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `POPULATION`, `URBANIZATION`, and `NERC.REGION`.
* **Hyperparameters:** Tuned via GridSearchCV (`max_depth=None`, `min_samples_leaf=3`, `n_estimators=300`).
* **Performance:** The final model achieved a test accuracy of **~69.1%**, a significant improvement over the baseline.

## Fairness Analysis
We evaluated the model's fairness based on the **Urbanization** feature.
* **Groups:** High Urbanization vs. Low Urbanization (split by median).
* **Metric:** Accuracy parity.
* **Results:**
    * Accuracy (High Urbanization): ~63.7%
    * Accuracy (Low Urbanization): ~74.7%.
    * **Permutation Test:** A p-value of **0.05** was observed, suggesting a potential disparity in model performance between highly urbanized and less urbanized areas.

## Installation & Requirements
To reproduce this analysis, ensure you have the following dependencies installed:

```bash
pip install pandas openpyxl numpy plotly scikit-learn
