```markdown
# Diabetes Prediction using Machine Learning

This repository contains an end-to-end machine learning project to predict diabetes based on diagnostic measurements. The analysis investigates different data imputation strategies, evaluates multiple classification models, and provides recommendations for clinical deployment.

## 📌 Table of Contents
- [Project Overview](#project-overview)
- [Dataset Description](#dataset-description)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Data Preprocessing & Feature Engineering](#data-preprocessing--feature-engineering)
- [Model Evaluation & Validation](#model-evaluation--validation)
- [Key Results](#key-results)
- [Deployment Recommendations](#deployment-recommendations)
- [How to Run](#how-to-run)

---

## 🔍 Project Overview
The goal of this project is to build a robust binary classifier to predict the onset of diabetes. Since medical datasets often contain missing values masked as zeros (e.g., zero blood pressure or insulin), this project compares how handling these missing values with different subset selections (including/excluding heavily missing features) impacts model robustness and validation scores.

## 📊 Dataset Description
The dataset used is the widely studied **Pima Indians Diabetes Database**. It consists of 768 patient records with 9 clinical features:
- **Pregnancies**: Number of times pregnant
- **Glucose**: Plasma glucose concentration (2 hours in an oral glucose tolerance test)
- **BloodPressure**: Diastolic blood pressure (mm Hg)
- **SkinThickness**: Triceps skin fold thickness (mm)
- **Insulin**: 2-Hour serum insulin (mu U/mL)
- **BMI**: Body mass index (weight in kg / (height in m)^2)
- **DiabetesPedigreeFunction**: A score indicating the hereditary risk based on family history
- **Age**: Age in years
- **Outcome**: Class variable (0 for non-diabetic, 1 for diabetic)

---

## 📈 Exploratory Data Analysis (EDA)
- **Zero-Value Anomaly**: Columns like `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, and `BMI` contained invalid `0` values. These were logically treated as missing values (`NaN`).
- **Missingness Extent**:
  - **Insulin**: Missing in **48.7%** (374/768) of patients.
  - **SkinThickness**: Missing in **29.6%** (227/768) of patients.
- **Imbalance**: The target variable `Outcome` is imbalanced, with 500 non-diabetic and 268 diabetic cases.
- **Feature Correlations**: `Glucose` (0.49) and `BMI` (0.31) showed the strongest positive correlation with diabetes outcome.

---

## 🛠️ Data Preprocessing & Feature Engineering
To prevent data leakage, preprocessing steps are contained within a pipeline and applied dynamically across validation folds:
1. **Indicator Columns**: Added missing-value indicators (`_missing`) for columns with high sparsity (`Insulin`, `SkinThickness`).
2. **Imputation**: Replaced zeros with `NaN` and imputed values using the **Median** calculated from the training folds.
3. **Feature Scaling**: Scaled all continuous variables using `StandardScaler`.
4. **Feature Sets Tested**:
   - `all_features` (All 8 original predictors)
   - `features_without_insulin` (Dropped `Insulin`)
   - `features_without_insulin_skinThickness` (Dropped both `Insulin` and `SkinThickness`)

---

## 🧪 Model Evaluation & Validation
We evaluated four classifiers using **Stratified 5-Fold Cross-Validation** to guarantee stable performance estimates:
- Logistic Regression
- Random Forest Classifier
- Decision Tree Classifier
- K-Nearest Neighbors (KNN)

Models were evaluated across multiple metrics including **ROC AUC**, **F1-Score**, **Accuracy**, **Precision**, and **Recall**.

---

## 🏆 Key Results

| Model Configuration | Accuracy | Mean ROC AUC | Mean F1-Score | ROC AUC SD |
| :--- | :---: | :---: | :---: | :---: |
| **Logistic Regression (No Insulin/SkinThickness)** | **77.08%** | **83.80%** | 63.48% | **0.0199** |
| **Random Forest (All Features)** | 76.82% | 83.50% | **64.49%** | 0.0215 |

Both the Random Forest (utilizing all features with imputation) and the simplified Logistic Regression model yielded highly competitive performance.

---

## 💡 Deployment Recommendations
We highly recommend deploying the **Logistic Regression model trained on features without Insulin & SkinThickness** for clinical environments:

1. **Clinical Feasibility**: Discarding `Insulin` (48.7% missing) and `SkinThickness` (29.6% missing) avoids the need for expensive or delayed lab tests, making predictions immediate and cheaper to obtain.
2. **Superior Generalization & Stability**: It achieves the highest **ROC AUC (83.80%)** and the lowest Standard Deviation (**0.0199**), indicating highly reliable performance across unseen patient profiles.

```
