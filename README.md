# Data Cleaning Process

## Overview
This document outlines the steps taken to clean the dataset and prepare it for further analysis. The main focus was handling missing values, removing dupliates and choosing an appropriate imputation method.

## Steps Followed

### Step 1: Checked Data Distribution
- Analyzed whether data was normally distributed.
- Found that the data was not normally distributed.
- Decided against using mean imputation since it is sensitive to skewed distributions.

### Step 2: Identified Missing Values
  ```python
  print(df.isnull().sum())
  ```
- Identified that several numerical and categorical columns had missing values (e.g., `car`, `Bar`, `CoffeeHouse`, etc.).


### Step 3: Dropped the Duplicate Rows And Removed a Column
  ```python
  df.drop_duplicates()
  ```
-Found "car" column has more that 99% missing values and omitted it.
  

### Step 4: Converted Possible Categorical Columns to Numerical
Before applying imputation, few categorical features were converted into numerical format:

  ```python
  df['gender'] = df['gender'].map({'Male': 1, 'Female': 0})
  df["time"] = df["time"].str.replace("PM", "").str.replace("AM", "").astype(int) + \
             df["time"].str.contains("PM").astype(int) * 12
  ```


### Step 5: Compared Median and KNN Imputation
- Evaluated both median imputation and KNN imputation.
- Found that both methods resulted in the same accuracy:
  - **Median Imputation Accuracy**: 0.7482
  - **KNN Imputation Accuracy**: 0.7482
- Reason: Could be High redundancy in the data, leading to similar results for both methods.

### Step 6: Chose Median Imputation for Missing Numerical Data
- Since median imputation is computationally less expensive than KNN, it was chosen.
- Used the following method:
  ```python
  from sklearn.impute import SimpleImputer
  
  num_cols = df.select_dtypes(include=["int64", "float64"]).columns
  
  imputer = SimpleImputer(strategy="median")
  df[num_cols] = imputer.fit_transform(df[num_cols])
  ```
  

## Checkout
- [x]  I have read all the contributor guidelines for the repo.


