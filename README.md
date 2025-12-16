# Power Outages: Analysis and Cause Prediction Model

**Authors:** Sana Gupta & Aarushi Jain [cite: 4]  
**Project Website:** [View the Analysis Here](https://svgupta-eng.github.io/power-outage-analysis/) [cite: 5]

## Project Overview
This project explores a dataset of major power outages in the U.S. to understand the characteristics and causes of these events. [cite_start]The primary goal is to build a machine learning model capable of predicting the **cause of a power outage** (e.g., severe weather, intentional attack, equipment failure) based on location, time, and climate characteristics[cite: 25, 634].

## Dataset & Data Cleaning
The dataset utilized is `outage.xlsx`[cite: 28]. The data cleaning process involved:
* **Feature Selection:** Retaining relevant columns such as `YEAR`, `MONTH`, `U.S._STATE`, `CLIMATE.REGION`, `POPULATION`, and `CAUSE.CATEGORY` [cite: 38-39].
* **Date Time formatting:** Combining `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into a single datetime object[cite: 50].
* **Feature Engineering:** Created a new `URBANIZATION` column by combining `POPPCT_URBAN` and `POPPCT_UC` to better represent population density[cite: 58].
* **Handling Missing Values:** Coerced invalid numeric entries to `NaN` and identified missing data patterns[cite: 43].

## Exploratory Data Analysis (EDA)
We performed univariate and bivariate analyses to understand the distributions of outages:
* **Trends over time:** Analyzed the frequency of outages per year and month[cite: 139, 174].
* [cite_start]**Geographic Analysis:** Examined outage counts by Climate Region[cite: 227].
* [cite_start]**Cause Analysis:** Investigated the relationship between outage causes and population density[cite: 196].

### Key Hypothesis Test
We tested whether **Severe Weather** outages have a longer duration on average than outages caused by other factors.
* [cite_start]**Null Hypothesis:** The mean duration of severe weather outages equals the mean duration of other causes[cite: 526].
* [cite_start]**Result:** With a p-value of **0.0**, we rejected the null hypothesis, suggesting severe weather outages significantly differ in duration[cite: 570].

## Assessment of Missingness
We conducted permutation tests to determine if the missingness in the `OUTAGE.DURATION` column was dependent on other columns.
* [cite_start]**Dependent on Cause:** The p-value was **0.0**, indicating missingness depends on `CAUSE.CATEGORY`[cite: 405].
* [cite_start]**Dependent on Year:** The p-value was **1.0**, indicating missingness does not depend on `YEAR`[cite: 444].

## Prediction Model
[cite_start]We framed a multiclass classification problem to predict `CAUSE.CATEGORY`[cite: 632].

### Baseline Model
* [cite_start]**Algorithm:** Logistic Regression[cite: 661].
* [cite_start]**Features:** `YEAR`, `URBANIZATION`, and `CLIMATE.REGION`[cite: 664].
* [cite_start]**Accuracy:** ~47.2%[cite: 696].

### Final Model
* [cite_start]**Algorithm:** Random Forest Classifier[cite: 703].
* [cite_start]**Features:** `YEAR`, `MONTH`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `POPULATION`, `URBANIZATION`, and `NERC.REGION` [cite: 705-715].
* [cite_start]**Hyperparameters:** Tuned via GridSearchCV (`max_depth=None`, `min_samples_leaf=3`, `n_estimators=300`)[cite: 768].
* [cite_start]**Performance:** The final model achieved a test accuracy of **~69.1%**, a significant improvement over the baseline[cite: 770].

## Fairness Analysis
We evaluated the model's fairness based on the **Urbanization** feature.
* [cite_start]**Groups:** High Urbanization vs. Low Urbanization (split by median) [cite: 775-777].
* **Metric:** Accuracy parity.
* **Results:**
    * Accuracy (High Urbanization): ~63.7%
    * [cite_start]Accuracy (Low Urbanization): ~74.7%[cite: 808].
    * [cite_start]**Permutation Test:** A p-value of **0.05** was observed, suggesting a potential disparity in model performance between highly urbanized and less urbanized areas[cite: 810].

## Installation & Requirements
[cite_start]To reproduce this analysis, ensure you have the following dependencies installed[cite: 6]:

```bash
pip install pandas openpyxl numpy plotly scikit-learn
