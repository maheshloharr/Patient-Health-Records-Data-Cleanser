# 🏥 Patient Health Records Data Cleanser

## 📌 Project Overview

This project focuses on **Data Cleaning and Preprocessing** of a Patient Health Records dataset using Python and Jupyter Notebook.

The objective is to identify missing values, apply and compare different imputation techniques, detect outliers using multiple statistical methods, treat extreme values, and prepare a final clean dataset for Machine Learning.

### Project Flow

**Data Loading → Missing Value Analysis → Imputation → Imputation Comparison → Outlier Detection → Winsorization → Final Data Validation**

---

## Project Explanation Video

[Watch Project Explanation Video](https://drive.google.com/file/d/1qvt_h2MLoV6wj-7r3Y9KpB8wJEj_F1l9/view?usp=drive_link)

---

## 🎯 Objectives

- Identify missing values and calculate missing percentages.
- Apply multiple missing-value imputation techniques.
- Compare the results of different imputation methods.
- Detect outliers using Z-Score, IQR, and Percentile methods.
- Apply Winsorization to reduce the effect of extreme values.
- Compare the dataset before and after outlier treatment.
- Create and validate the final clean dataset.
- Prepare the data for future Machine Learning.

---

## 📊 Dataset Information

The dataset contains **50,000 patient records and 9 columns**.

| Column | Description |
|---|---|
| `patient_id` | Unique patient identifier |
| `age` | Age of the patient |
| `gender` | Gender of the patient |
| `region` | Geographical region |
| `bmi` | Body Mass Index |
| `blood_pressure` | Blood pressure measurement |
| `cholesterol` | Cholesterol level |
| `glucose` | Glucose level |
| `disease_risk` | Disease-risk target variable |

### Target Variable

`disease_risk`

- `0` = Lower / No Disease Risk
- `1` = Disease Risk

---

# 🧹 Part A: Handling Missing Values

The project first creates a column-wise missing-value report.

### Initial Missing Values

| Column | Missing Count | Missing % |
|---|---:|---:|
| `patient_id` | 0 | 0.00% |
| `age` | 899 | 1.80% |
| `gender` | 1,000 | 2.00% |
| `region` | 1,250 | 2.50% |
| `bmi` | 1,091 | 2.18% |
| `blood_pressure` | 0 | 0.00% |
| `cholesterol` | 995 | 1.99% |
| `glucose` | 1,244 | 2.49% |
| `disease_risk` | 0 | 0.00% |

---

## 🔧 Imputation Techniques

### 1. Simple Imputation

Simple Imputer is used for individual variables:

- `bmi` → Median Imputation
- `region` → Most Frequent Imputation
- `gender` → Most Frequent Imputation

---

### 2. Random Sample Imputation

Random Sample Imputation is implemented for columns containing missing values.

Missing indicators are also created to track missing observations.

---

### 3. KNN Imputation

K-Nearest Neighbors imputation is applied using:

```text
n_neighbors = 5
weights = distance
```

Categorical variables are temporarily encoded before KNN imputation and converted back afterward.

---

### 4. MICE / Iterative Imputation

MICE is implemented using Scikit-learn's `IterativeImputer`.

```text
max_iter = 10
random_state = 42
```

MICE estimates missing values by using relationships between multiple variables.

---

## 📊 Imputation Comparison

The project compares:

- Original Dataset
- Simple Imputation
- Random Sample Imputation
- KNN Imputation
- MICE

Both missing-value counts and numerical means are compared.

### Selected Strategy

**MICE is selected as the primary imputation strategy** because it considers relationships between multiple variables and performs iterative multivariate imputation.

KNN is also useful because it estimates missing values based on similar patient records.

Simple imputation is easier and faster but does not consider relationships between multiple variables.

---

# 📉 Part B: Handling Outliers

Three outlier detection techniques are implemented:

1. Z-Score
2. IQR
3. Percentile

The final outlier treatment uses **Winsorization**.

---

## 1️⃣ Z-Score Method

Z-Score outlier detection is applied to:

- `cholesterol`
- `glucose`

Threshold:

```text
|Z| > 3
```

The notebook identified **594 outlier records** using this threshold.

---

## 2️⃣ IQR Method

The IQR method is applied to:

```text
bmi
```

Formula:

```text
IQR = Q3 - Q1

Lower Limit = Q1 - 1.5 × IQR

Upper Limit = Q3 + 1.5 × IQR
```

Results from the project:

```text
Q1 = 23.04
Q3 = 29.15
IQR = 6.11
Lower Limit = 13.88
Upper Limit = 38.32
BMI Outliers = 434
```

---

## 3️⃣ Percentile Method

The Percentile method is applied to:

```text
blood_pressure
```

Limits:

```text
1st Percentile  = 85.00
99th Percentile = 168.37
```

The project identified **500 extreme blood-pressure values** using these limits.

---

# 4️⃣ Winsorization

Winsorization is applied to:

```text
bmi
blood_pressure
cholesterol
glucose
```

Values are capped between the **1st and 99th percentiles**.

This approach preserves all patient records instead of deleting rows containing extreme values.

### Before vs After

| Feature | Before Min | After Min | Before Max | After Max |
|---|---:|---:|---:|---:|
| `bmi` | 8.03 | 15.05 | 55.55 | 37.22 |
| `blood_pressure` | 85.00 | 85.00 | 279.71 | 168.37 |
| `cholesterol` | 47.75 | 104.48 | 421.24 | 281.83 |
| `glucose` | 26.49 | 55.00 | 309.42 | 150.22 |

---

# 📊 Before vs After Outlier Treatment

The dataset shape is preserved after Winsorization.

| Metric | Before Treatment | After Treatment |
|---|---:|---:|
| Rows | 50,000 | 50,000 |
| Columns | 9 | 9 |
| Missing Values | 0 | 0 |
| Duplicate Rows | 0 | 0 |

The main change is in the extreme numerical values rather than the number of records.

---

# 📈 Data Quality Improvement

After outlier treatment, the numerical ranges became more controlled.

| Feature | Before Max | After Max |
|---|---:|---:|
| `bmi` | 55.55 | 37.22 |
| `blood_pressure` | 279.71 | 168.37 |
| `cholesterol` | 421.24 | 281.83 |
| `glucose` | 309.42 | 150.22 |

This reduces the influence of extreme observations while preserving the complete dataset.

---

# ✅ Part C: Final Clean Dataset

The final dataset is created after:

- MICE imputation
- Outlier detection
- Winsorization
- Data-quality validation

### Final Validation

```text
Missing Values      = 0
Duplicate Rows      = 0
Duplicate Patient IDs = 0
Dataset Shape       = (50000, 9)
```

The final dataset contains no missing values and no duplicate rows.

---

# 🔄 Complete Project Workflow

```text
                 Patient Health Records
                         │
                         ▼
                    Data Loading
                         │
                         ▼
               Missing Value Analysis
                         │
                         ▼
               Imputation Techniques
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Simple          KNN            MICE
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                Compare Imputations
                         │
                         ▼
                 Outlier Detection
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Z-Score           IQR        Percentile
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                   Winsorization
                         │
                         ▼
              Final Data Validation
                         │
                         ▼
                 Clean Dataset
                         │
                         ▼
              Machine Learning Ready
```

---

# 🛠️ Technologies Used

- **Python**
- **Pandas**
- **NumPy**
- **Scikit-learn**
- **SciPy**
- **Jupyter Notebook**

---

# 📦 Libraries Used

```python
import pandas as pd
import numpy as np

from sklearn.impute import SimpleImputer
from sklearn.impute import KNNImputer

from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

from scipy.stats import zscore
```

---

# 📁 Repository Structure

```text
Patient-Health-Records-Data-Cleanser/
│
├── Data Cleanser.ipynb
├── patient_health_records_50000.csv
├── patient_health_records_clean.csv
└── README.md
```

---

# 📚 Key Concepts Covered

- Data Preprocessing
- Missing Value Analysis
- Missing Percentage Calculation
- Median Imputation
- Most Frequent Imputation
- Random Sample Imputation
- Missing Indicators
- KNN Imputation
- MICE / Iterative Imputation
- Z-Score Outlier Detection
- IQR Outlier Detection
- Percentile Outlier Detection
- Winsorization
- Data Quality Comparison
- Final Dataset Validation
- Machine Learning Data Preparation

---

# 🔮 Future Scope

The final clean dataset can be used for:

- Exploratory Data Analysis
- Feature Engineering
- Disease Risk Prediction
- Classification Models
- Model Training
- Model Evaluation
- Machine Learning Deployment

---

# 👨‍💻 Author

**Mahesh Lohar**

Data Analyst | Python | SQL | Power BI | Excel | Data Science

---

## ⭐ Project Highlights

- 50,000 Patient Health Records
- Multiple Missing Value Techniques
- KNN and MICE Imputation
- Z-Score, IQR and Percentile Outlier Detection
- Winsorization
- Complete Data Quality Validation
- Machine Learning Ready Dataset

---

## 📌 Conclusion

This project demonstrates a complete practical workflow for cleaning and preprocessing patient health records.

Multiple missing-value imputation techniques were implemented and compared. MICE was selected as the primary imputation strategy, while Winsorization was used to treat extreme numerical values.

The final dataset contains **50,000 records, 9 columns, zero missing values, zero duplicate rows, and zero duplicate patient IDs**, making it suitable for further analysis and Machine Learning.
