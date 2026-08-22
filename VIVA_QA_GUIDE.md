# Viva Q&A Guide — Bank Customer Churn Prediction

> **Module:** Computational Intelligence 6005
> **Project:** Bank Customer Churn Prediction
> **Pipeline:** Data Cleaning → Outlier Handling → Feature Engineering → Preprocessing → Model Training → Deployment

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Dataset and Features](#2-dataset-and-features)
3. [Missing Values and Duplicates (Notebook 01)](#3-missing-values-and-duplicates-notebook-01)
4. [Outlier Handling (Notebook 02)](#4-outlier-handling-notebook-02)
5. [Feature Binning (Notebook 03)](#5-feature-binning-notebook-03)
6. [Scaling and Encoding (Notebook 04)](#6-scaling-and-encoding-notebook-04)
7. [Model Training and Evaluation (Notebook 05)](#7-model-training-and-evaluation-notebook-05)
8. [Hyperparameter Tuning](#8-hyperparameter-tuning)
9. [Final Results and Model Selection](#9-final-results-and-model-selection)
10. [Deployment (FastAPI App)](#10-deployment-fastapi-app)
11. [General Conceptual Questions](#11-general-conceptual-questions)

---

## 1. Project Overview

**Q: What is the goal of this project?**

The goal is to build a machine learning model that predicts whether a bank customer will **churn (leave the bank)**, indicated by the `Exited` column (1 = churned, 0 = stayed). This is a **binary classification** problem.

---

**Q: Why is churn prediction important for a bank?**

Retaining existing customers is far cheaper than acquiring new ones. By identifying customers at high risk of leaving, the bank can take proactive steps — such as offering better deals or personalised services — to retain them.

---

**Q: What was your overall pipeline?**

The project followed a structured pipeline across 5 Jupyter notebooks:

1. **Notebook 01** – Handle missing and duplicate values
2. **Notebook 02** – Detect and handle outliers (IQR method)
3. **Notebook 03** – Feature binning for `Age` and `Tenure`
4. **Notebook 04** – Scaling, encoding, and train/test splitting
5. **Notebook 05** – Model training, evaluation, hyperparameter tuning, and saving the best model

Finally, a **FastAPI web application** was built so users can enter customer details and get a live churn prediction.

---

## 2. Dataset and Features

**Q: Describe the dataset you used.**

The training dataset (`train.csv`) contains **15,000 rows** and **14 columns**. The target variable is `Exited` (binary: 0 or 1). The churn rate was approximately **20.35%**, meaning the dataset is **class-imbalanced**.

---

**Q: What are the features in the dataset?**

| Feature | Type | Description |
|---|---|---|
| `CreditScore` | Numerical | Customer's credit score (431–850) |
| `Geography` | Nominal | Country: France, Germany, Spain |
| `Gender` | Nominal | Male / Female |
| `Age` | Numerical | Customer age (18–74) |
| `Tenure` | Numerical | Years with the bank (0–10, after outlier removal) |
| `Balance` | Numerical | Bank account balance |
| `NumOfProducts` | Numerical | Number of bank products held (1–4) |
| `HasCrCard` | Binary | Has credit card? (0/1) |
| `IsActiveMember` | Binary | Is active member? (0/1) |
| `EstimatedSalary` | Numerical | Estimated salary |
| `Exited` | Target | Did the customer leave? (0/1) |

---

**Q: Which columns did you drop and why?**

We dropped `id`, `CustomerId`, and `Surname`. These are **identifier columns** — they carry no predictive information about churn behaviour. Including them would cause data leakage or simply add noise.

---

## 3. Missing Values and Duplicates (Notebook 01)

**Q: Did your dataset have missing values?**

No. When we ran `df.isnull().sum()`, all 14 columns returned **0 missing values**. We also checked for blank strings (empty whitespace) using `df.astype(str).apply(lambda x: x.str.strip() == '').sum()` — again, 0 found.

---

**Q: Were there any duplicate rows?**

No. `df.duplicated().sum()` returned **0**, confirming no duplicate rows existed.

---

**Q: Why is it still important to check for these even if there are none?**

It is a standard data quality step. Missing values can silently cause model errors or biased results. Duplicates inflate training data and lead to overfitting. Checking ensures the data is clean before any further processing.

---

**Q: What would you have done if there were missing values?**

- For **numerical** features: impute using the **median** (robust to outliers) or mean
- For **categorical** features: impute using the **mode** (most frequent value)
- If a column had too many missing values (e.g. >50%), consider dropping it entirely

We later used `SimpleImputer` inside our sklearn pipeline as a safety net for real-time inference.

---

## 4. Outlier Handling (Notebook 02)

**Q: How did you identify outliers?**

We used visual methods first:
- **KDE plots (distribution plots)** to check skewness
- **Histograms** to see the spread of values
- **Box plots** to visually identify outliers as points beyond the whiskers

We checked the numerical columns: `CreditScore`, `Age`, `Balance`, `EstimatedSalary`, and `Tenure`.

---

**Q: What method did you use to remove outliers?**

We used the **IQR (Interquartile Range) method**:

```
Lower bound = Q1 - 1.5 x IQR
Upper bound = Q3 + 1.5 x IQR
```

Any value below the lower bound or above the upper bound was considered an outlier and **removed** from the dataset.

---

**Q: What does IQR stand for and how is it calculated?**

IQR = **Interquartile Range** = Q3 minus Q1, where:
- **Q1** = 25th percentile (lower quartile)
- **Q3** = 75th percentile (upper quartile)

It represents the middle 50% of the data, making it robust to extreme values.

---

**Q: What outliers did you find?**

- **CreditScore**: 15 outliers (scores like 431, 434, 437 — below the lower bound of 440)
- **EstimatedSalary**: 2 outliers
- **Tenure**: 1 outlier
- `NumOfProducts` was intentionally **not** treated — values 1–4 are realistic for banking products

---

**Q: Why did you not treat outliers in `NumOfProducts`?**

`NumOfProducts` ranges from 1 to 4. These are all **realistic business values** — a customer can hold 1 to 4 bank products. Removing or capping them would distort real customer behaviour and is not statistically justified.

---

**Q: In the deployment app, why did you clip instead of remove outliers?**

At inference time, we cannot remove data — we must make a prediction for every customer that submits a form. So we **clipped (capped)** the input values to the training IQR boundaries calculated from the training set. This ensures the model never receives extreme values it was not trained on.

---

## 5. Feature Binning (Notebook 03)

**Q: What is feature binning and why did you do it?**

Feature binning (also called discretisation) converts a continuous numerical variable into **categorical groups**. We applied it to `Tenure` and `Age` because:
- The relationship between these features and churn is better captured at a **group level** rather than as exact numbers
- It reduces the impact of minor variations within a group
- It makes the model more robust and interpretable

---

**Q: How did you bin `Tenure`?**

```
Tenure 0 to 4  -->  "New"
Tenure 5 to 8  -->  "Medium"
Tenure 9+      -->  "Long-term"
```

After binning, the original `Tenure` column was **dropped** and replaced with `Tenure_bins`.

---

**Q: How did you bin `Age`?**

```
Age <= 30      -->  "Young"
Age 31 to 45   -->  "Adult"
Age 46 to 60   -->  "Middle-aged"
Age > 60       -->  "Senior"
```

After binning, the original `Age` column was **dropped** and replaced with `Age_bins`.

---

**Q: What type of variable are the binned features — ordinal or nominal?**

They are **ordinal** — there is a natural order (e.g., New < Medium < Long-term for tenure, and Young < Adult < Middle-aged < Senior for age). This is why we later use `OrdinalEncoder` for these features, which preserves their ordering.

---

**Q: Why not just leave Age and Tenure as continuous numbers?**

Continuous values can cause the model to overfit to exact numbers. For example, a 36-year-old and a 37-year-old likely have similar churn risk, but a "Young" and "Senior" customer may behave very differently. Binning captures this **group-level behaviour** more effectively.

---

## 6. Scaling and Encoding (Notebook 04)

**Q: Why is feature scaling necessary?**

Many ML algorithms (especially Logistic Regression and distance-based models) are sensitive to the **scale** of features. For example, `Balance` (0 to ~188,000) would dominate `NumOfProducts` (1 to 4) without scaling. We used **StandardScaler**, which transforms features to have mean=0 and standard deviation=1.

---

**Q: What encoding did you apply to categorical features?**

We applied **three different encoding strategies** based on the nature of each feature:

| Feature Type | Features | Encoding Used | Reason |
|---|---|---|---|
| Numerical | `CreditScore`, `Balance`, `NumOfProducts`, `EstimatedSalary` | `StandardScaler` | Scale to unit variance |
| Nominal | `Gender`, `Geography` | `OneHotEncoder` | No ordinal relationship |
| Ordinal | `Tenure_bins`, `Age_bins` | `OrdinalEncoder` | Has a natural order |
| Binary | `HasCrCard`, `IsActiveMember` | Passthrough | Already 0/1 |

---

**Q: Why use OneHotEncoding for Geography and not OrdinalEncoding?**

`Geography` has 3 values: France, Germany, Spain. There is **no meaningful ranking** between countries — Germany is not "more" than France. If we used OrdinalEncoder, the model would assume an ordering (e.g., 0, 1, 2) that does not exist, introducing a false numerical relationship. OneHotEncoder creates separate binary columns for each country, avoiding this bias.

---

**Q: How did you split the data?**

```python
train_test_split(x, y, test_size=0.2, stratify=y, random_state=42)
```

- **80/20 split**: 80% training, 20% testing
- **`stratify=y`**: ensures both train and test sets have the same class ratio (~20% churned), critical for imbalanced datasets
- **`random_state=42`**: for reproducibility

---

**Q: What was the class distribution in the training set?**

- Class 0 (Not churned): **~79.65%**
- Class 1 (Churned): **~20.35%**

This confirms the dataset is **imbalanced**, which is why we later applied **SMOTE** during model training.

---

**Q: How did you set up the ColumnTransformer?**

We used sklearn's `ColumnTransformer` to apply different transformations to different columns simultaneously:

```python
ColumnTransformer([
    ('num', numerical_transformer, numerical_features),
    ('nom', nominal_transformer, nominal_features),
    ('ord', ordinal_transformer, ordinal_features),
    ('bin', 'passthrough',       binary_features)
])
```

Importantly, the preprocessor is **fitted only on training data** and then used to `transform` the test data, preventing data leakage.

---

## 7. Model Training and Evaluation (Notebook 05)

**Q: What models did you train?**

We trained and evaluated **4 classification models**:
1. **Logistic Regression** — linear baseline model
2. **Decision Tree** — non-linear, interpretable tree model
3. **Random Forest** — ensemble of decision trees
4. **XGBoost** — gradient boosting model

---

**Q: What is SMOTE and why did you use it?**

**SMOTE** = Synthetic Minority Over-sampling Technique. Since only ~20% of customers churned (class imbalance), a naive model could achieve high accuracy by always predicting "not churned". SMOTE generates **synthetic samples** of the minority class (churned customers) in the training data to balance the class distribution. We applied it **inside the pipeline** using `imblearn`'s `ImbPipeline`, ensuring SMOTE is only applied to training folds (not validation or test data).

---

**Q: Why apply SMOTE inside the pipeline and not before splitting?**

If SMOTE is applied before splitting, synthetic samples from the minority class may end up in the test set. This would cause **data leakage** — the model would be evaluated on data derived from the training set, giving overly optimistic results. Applying it inside the pipeline ensures synthetic data only appears in training folds.

---

**Q: What cross-validation strategy did you use?**

**Stratified K-Fold** with 5 folds:

```python
StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
```

Stratified K-Fold ensures each fold has the same class ratio as the full dataset — important for imbalanced data.

---

**Q: Why did you choose F1-score as your primary metric?**

With class imbalance (~20% churn), **accuracy is misleading** — a model that always predicts "not churned" would have ~80% accuracy. F1-score is the **harmonic mean of Precision and Recall**, penalising models that are good at one but poor at the other. It better reflects performance on imbalanced problems.

---

**Q: Explain Precision, Recall, F1, and ROC-AUC in the context of this project.**

| Metric | Formula | What it means here |
|---|---|---|
| **Precision** | TP / (TP + FP) | Of customers predicted to churn, how many actually did? |
| **Recall** | TP / (TP + FN) | Of all customers who actually churned, how many did we catch? |
| **F1-Score** | 2 x (P x R) / (P + R) | Balance between catching churners and avoiding false alarms |
| **ROC-AUC** | Area under ROC curve | Model's ability to distinguish churners from non-churners at all thresholds |

---

**Q: What were the baseline model results?**

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| XGBoost | 0.8707 | 0.6537 | 0.7738 | 0.7087 | 0.9178 |
| Random Forest | 0.8630 | 0.6264 | 0.8082 | 0.7058 | 0.9150 |
| Decision Tree | 0.8383 | 0.5728 | 0.8066 | 0.6698 | 0.9070 |
| Logistic Regression | 0.7567 | 0.4421 | 0.7508 | 0.5565 | 0.8052 |

---

**Q: Why did Logistic Regression perform the worst?**

Logistic Regression is a **linear model** — it assumes a linear decision boundary between classes. Churn prediction involves complex, non-linear relationships (e.g., a customer with high balance AND many products AND is German churns at much higher rates). Non-linear models like trees and boosting handle such interactions naturally.

---

## 8. Hyperparameter Tuning

**Q: What is hyperparameter tuning and why did you do it?**

Hyperparameters are settings that control the learning process (e.g., number of trees, max depth). They are not learned from data — we must set them manually. Tuning finds the **optimal combination** to maximise model performance (F1-score in our case).

---

**Q: What tuning strategy did you use?**

We used a **two-stage approach** for Random Forest and XGBoost:

1. **Stage 1 — RandomizedSearchCV**: Broad sweep over a large parameter space, testing 50 random combinations. Identifies a promising region quickly.
2. **Stage 2 — GridSearchCV**: Exhaustive search over a narrow grid around the best Stage 1 parameters. Fine-tunes to the optimal configuration.

This two-stage approach is more **efficient** than full GridSearchCV on a huge grid, while still finding good parameters.

---

**Q: What is the difference between GridSearchCV and RandomizedSearchCV?**

- **GridSearchCV**: Tests **every combination** of the parameter grid. Guarantees finding the best combination within the grid, but can be very slow with many parameters.
- **RandomizedSearchCV**: Randomly samples `n_iter` combinations from the parameter distribution. **Faster**, covers a wider range, and often finds near-optimal results without exhaustive search.

---

**Q: What hyperparameters did you tune for XGBoost?**

| Parameter | What it controls |
|---|---|
| `n_estimators` | Number of boosting trees |
| `max_depth` | Maximum tree depth (controls complexity) |
| `learning_rate` | Step size for boosting (shrinkage) |
| `subsample` | Fraction of samples per tree (prevents overfitting) |
| `colsample_bytree` | Fraction of features per tree |
| `min_child_weight` | Minimum sum of instance weights in a leaf |
| `gamma` | Minimum loss reduction required for a split |
| `scale_pos_weight` | Handles class imbalance |
| `reg_alpha` / `reg_lambda` | L1 / L2 regularisation |

---

**Q: What were the best XGBoost parameters after tuning?**

```
n_estimators    : 200
max_depth       : 3
learning_rate   : 0.2
subsample       : 0.7
colsample_bytree: 0.6
gamma           : 0.3
min_child_weight: 6
reg_alpha       : 1.0
reg_lambda      : 1.5
scale_pos_weight: 1
```

---

**Q: How do you detect overfitting in your results?**

By comparing **Training F1** vs **Test F1**. If Training F1 is much higher than Test F1, the model has memorised the training data and generalises poorly. For example:

- **Random Forest Tuned**: Train F1 = 0.7960, Test F1 = 0.7071 — possible overfitting (flagged with a warning in the code)
- **XGBoost Tuned**: Train F1 = 0.7270, Test F1 = 0.7158 — well generalised (gap is small)

---

## 9. Final Results and Model Selection

**Q: Which model was selected as the best and why?**

**XGBoost** was selected as the best model based on the highest **Test F1-score** and **ROC-AUC** after tuning:

| Model | Test F1 | ROC-AUC | CV F1 |
|---|---|---|---|
| **XGBoost (Tuned)** | **0.7158** | **0.9157** | 0.6936 |
| Random Forest (Tuned) | 0.7071 | 0.9135 | 0.6927 |

XGBoost also had a **smaller gap** between Training F1 and Test F1, indicating better generalisation.

---

**Q: What does a ROC-AUC of 0.9157 mean?**

A ROC-AUC of **0.9157** means the model has a **91.57% chance** of correctly ranking a random churned customer higher than a random non-churned customer. A score of 0.5 is random guessing; 1.0 is perfect. Our model is **strongly discriminative**.

---

**Q: How was the best model saved?**

Using `joblib`:

```python
joblib.dump(best_tuned_model, '../models/best_churn_model.joblib')
```

`joblib` is preferred over `pickle` for scikit-learn models because it handles **large NumPy arrays** more efficiently.

---

## 10. Deployment (FastAPI App)

**Q: How did you deploy the model?**

We built a **FastAPI** web application (`Frontend/app.py`) with:
- A **POST `/api/predict`** endpoint that accepts customer data in JSON format
- The same **preprocessing pipeline** is reconstructed at startup and fitted on the training data
- The saved model is loaded using `joblib.load()`
- A static **HTML/CSS/JS frontend** is served at the root `/`

---

**Q: Why do you need to re-fit the preprocessor at startup?**

The `ColumnTransformer` preprocessor needs to know the **training data statistics** (e.g., mean and std for StandardScaler, unique categories for OneHotEncoder). Only the model was saved — not the preprocessor — so we reload the training data and re-fit the preprocessor on startup to reproduce the exact same transformation.

---

**Q: How does the inference pipeline work at prediction time?**

1. Customer submits form — JSON data received by FastAPI
2. Raw input validated by Pydantic `CustomerData` model
3. Numerical outliers are **clipped** using IQR limits calculated from training data
4. `Age` and `Tenure` are **binned** into categories (same logic as training)
5. Raw `Age` and `Tenure` columns are **dropped**
6. Features are **reordered** to match training order
7. `preprocessor.transform()` applies scaling and encoding
8. `model.predict_proba()` returns the churn probability
9. Response returned: `{ churn_probability, churn_prediction, message }`

---

**Q: What does the API response look like?**

```json
{
  "success": true,
  "churn_probability": 0.7832,
  "churn_prediction": 1,
  "message": "High risk of churn"
}
```

---

**Q: Why use FastAPI over Flask?**

FastAPI offers:
- **Automatic data validation** via Pydantic models
- **Async support** for better performance
- **Automatic API docs** available at `/docs`
- Better **type safety** with Python type hints
- Generally **faster** performance than Flask

---

## 11. General Conceptual Questions

**Q: What is customer churn?**

Churn refers to when a customer **stops doing business** with a company. In banking, a churned customer (`Exited = 1`) has closed their account or left the bank.

---

**Q: What is the difference between classification and regression?**

- **Classification**: Predicts a **discrete label** (e.g., Churned or Not Churned). Used in this project.
- **Regression**: Predicts a **continuous value** (e.g., predicting the exact account balance).

---

**Q: What is an ensemble method and how does Random Forest use it?**

An ensemble method combines **multiple models** to produce a better result than any single model. **Random Forest** uses **Bagging** (Bootstrap Aggregating): it trains many Decision Trees on random subsets of the data and features, then takes a **majority vote** for classification. This reduces variance and prevents overfitting.

---

**Q: What is boosting and how is XGBoost different from Random Forest?**

- **Random Forest**: Trees are built **independently and in parallel**; results are averaged/voted.
- **XGBoost (Gradient Boosting)**: Trees are built **sequentially**, where each new tree corrects the errors of the previous ones. XGBoost adds regularisation (L1/L2), second-order gradients, and column subsampling — making it often more accurate, but also more sensitive to overfitting if not tuned.

---

**Q: What is regularisation in XGBoost?**

- **`reg_alpha` (L1 regularisation)**: Pushes some feature weights to exactly zero — acts as **feature selection**
- **`reg_lambda` (L2 regularisation)**: Penalises large weights, preventing any single feature from dominating — reduces overfitting

Both help control model complexity.

---

**Q: What is the difference between OrdinalEncoder and OneHotEncoder?**

- **OrdinalEncoder**: Assigns integer values (0, 1, 2...) — preserves **order**. Used when the category has a meaningful rank (e.g., New < Medium < Long-term).
- **OneHotEncoder**: Creates a binary column for each category — **no ordering implied**. Used for nominal data like Geography (France, Germany, Spain) where no rank exists.

---

**Q: What is `scale_pos_weight` in XGBoost?**

This parameter adjusts for **class imbalance**. Setting it to `n_negative / n_positive` gives more weight to the minority class. In our case, with ~80% negative and ~20% positive, a value of ~4 would be typical. We tuned this as a hyperparameter (values 1–5 were tested), and the model selected `1` — indicating that SMOTE had already effectively balanced the classes before XGBoost training.

---

**Q: How confident are you in your model's real-world performance?**

The model achieves:
- **Test ROC-AUC of 0.9157** — excellent discrimination ability
- **Test F1 of 0.7158** — good balance of precision and recall
- No major overfitting (Train F1 is close to Test F1 for XGBoost)

However, real-world performance also depends on how representative the training data is. The model should be **monitored over time** for data drift and periodically retrained on fresh data.

---

*Good luck with your viva!*
