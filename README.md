Resolves [https://github.com/iiitl/Classification-ml/issues/2]

# 📌 Outlier Detection and Handling

This project focuses on detecting and handling outliers in a dataset using **Interquartile Range (IQR), transformations, and categorical grouping** to improve data quality for further analysis.

## 🚀 Steps Followed

### 🔹 1. Checking Normality of Numerical Columns
- To determine whether to use **Interquartile Range (IQR)** or **Z-scores** for outlier detection.
- The data was skewed, so **IQR** was chosen instead of **Z-scores**.

```python
import matplotlib.pyplot as plt
import seaborn as sns

for col in num_cols:
    plt.figure(figsize=(6,4))
    sns.histplot(df[col], kde=True, bins=30)
    plt.title(f"Distribution of {col}")
    plt.show()
```

---

### 🔹 2. Outlier Detection Using IQR
- IQR method was applied to numerical columns to count outliers.

```python
Q1 = df[num_cols].quantile(0.25)
Q3 = df[num_cols].quantile(0.75)
IQR = Q3 - Q1

outliers = ((df[num_cols] < (Q1 - 1.5 * IQR)) | (df[num_cols] > (Q3 + 1.5 * IQR))).sum()
print("Outlier count per column:") 
print(outliers)
```

---

### 🔹 3. Visualizing Outliers
- **Boxplots & Scatter Plots** were used to analyze extreme outliers.

```python
import seaborn as sns
import matplotlib.pyplot as plt

for col in num_cols:
    plt.figure(figsize=(6, 4))
    sns.boxplot(y=df[col])
    plt.title(f"Boxplot for {col}")
    plt.show()
```

---

### 🔹 4. Handling Outliers
✅ **No Changes for `toCoupon_GEQ25min`**
- - **Binary Columns (0/1)** were ignored since they cannot have true outliers.

✅ **`direction_opp` & `direction_same`**
- Tried **Yeo-Johnson Transformation** & **Capping**, but skewness persisted.
- The data might have a meaningful skew, and forcing normalization may remove useful information, so left them as it is.

```python
from scipy.stats import yeojohnson

df["direction_same_transformed"], _ = yeojohnson(df["direction_same"])
df["direction_opp_transformed"], _ = yeojohnson(df["direction_opp"])
```

```python
import numpy as np

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
df["direction_opp_capped"] = np.clip(df["direction_opp"], lower_bound, upper_bound)
from scipy.stats import skew

skew_direction_same = skew(df["direction_same_capped"])
skew_direction_opp = skew(df["direction_opp_capped"])

```

---

### 🔹 5. Handling Categorical Data – Detecting Rare Categories
- Categorical columns do not have traditional outliers, but **rare categories** can cause issues.
- Group low-frequency categories into **"Other"**.

```python
threshold = 100  
rare_occupations = df["occupation"].value_counts()[df["occupation"].value_counts() < threshold].index
df["occupation"] = df["occupation"].replace(rare_occupations, "Other")
```

---

## ✨ Conclusion
- Systematically analysed the dataset to remove the outliers ensuring data quality for futher processing. 

---

## Checkout
- [x] I have read all the contributor guidelines of the repo.
