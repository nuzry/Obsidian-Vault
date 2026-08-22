# Plain English Explainer — What Does Everything Mean?

> This document explains every technical term from the project in simple, everyday language.
> No prior knowledge needed. Read this BEFORE your viva to be confident explaining concepts.

---

## Table of Contents

1. [The Big Picture — What Did We Build?](#1-the-big-picture)
2. [The Dataset — What Is It?](#2-the-dataset)
3. [Missing Values](#3-missing-values)
4. [Duplicate Values](#4-duplicate-values)
5. [Outliers and the IQR Method](#5-outliers-and-the-iqr-method)
6. [Feature Binning (Discretisation)](#6-feature-binning)
7. [Scaling (StandardScaler)](#7-scaling)
8. [Encoding — Turning Words Into Numbers](#8-encoding)
9. [Train/Test Split](#9-traintest-split)
10. [Class Imbalance and SMOTE](#10-class-imbalance-and-smote)
11. [Machine Learning Models — What Are They?](#11-machine-learning-models)
12. [Cross-Validation and K-Fold](#12-cross-validation-and-k-fold)
13. [Evaluation Metrics — How Do We Measure Success?](#13-evaluation-metrics)
14. [Hyperparameter Tuning](#14-hyperparameter-tuning)
15. [Overfitting](#15-overfitting)
16. [The API and FastAPI — How We Deployed It](#16-the-api-and-fastapi)
17. [Quick Reference Glossary](#17-quick-reference-glossary)

---

## 1. The Big Picture

### What is Machine Learning?

Machine Learning (ML) is teaching a computer to learn patterns from **examples** (data), so it can make predictions on new, unseen examples — without being given explicit rules.

**Everyday analogy**: You don't teach a child the rule "if it has fur, four legs, and barks, it's a dog." Instead, you show them hundreds of dogs and non-dogs, and they figure out the pattern themselves. That is what ML does with data.

---

### What did we build?

We built a **churn prediction model** for a bank. The bank has records of 15,000 customers, and for each customer, it knows whether they eventually **left the bank** (Exited = 1) or **stayed** (Exited = 0).

We trained a computer to find patterns in customer information (age, balance, credit score, etc.) that predict who is likely to leave. Once trained, the model can look at a NEW customer and say: "This customer has a **72% chance** of churning."

---

### What is "Churn"?

Churn = when a customer **stops using a service**. In banking, churn means a customer closed their account and left the bank. Banks want to predict this early so they can offer deals to keep those customers.

---

### What is Classification?

Classification is a type of ML task where the answer is one of a fixed set of **categories (labels)**:
- Churn prediction: **"Churned" or "Did not churn"** (2 categories = binary classification)
- Email spam detection: "Spam" or "Not spam"
- Medical diagnosis: "Has disease" or "Does not have disease"

It is different from **regression**, which predicts a number (e.g., "What will the house price be?").

---

## 2. The Dataset

### What is a Dataset?

A dataset is a structured table of information — like a spreadsheet. Each **row** is one record (one customer), and each **column** is one piece of information about that customer (one feature).

Our dataset had:
- **15,000 rows** — 15,000 customers
- **14 columns** — 14 pieces of information per customer

---

### What is a Feature?

A feature is one column of information used to make a prediction. Examples:
- `Age` = how old the customer is
- `Balance` = how much money they have in the bank
- `CreditScore` = their credit rating

---

### What is the Target Variable?

The target variable is the thing we are **trying to predict**. In our project:
- `Exited` = Did the customer leave? (1 = Yes, 0 = No)

Everything else is used as **input** to predict this **output**.

---

### Why did we drop `id`, `CustomerId`, and `Surname`?

These columns are just **labels to identify the person** — they don't tell us anything about their behaviour. The customer's surname does not predict whether they will churn. Keeping them would confuse the model.

---

## 3. Missing Values

### What are Missing Values?

A missing value is when a cell in your dataset is **empty** — the information was not recorded.

**Analogy**: Imagine filling out a form and leaving the "Age" box blank. That blank box is a missing value.

In Python/pandas, missing values show up as `NaN` (Not a Number).

---

### Why are they a problem?

Most machine learning algorithms cannot work with empty cells — they need actual numbers or text. If you try to train a model with missing values, it will either crash or give wrong results.

---

### What did we find?

We ran `df.isnull().sum()` and found **0 missing values** in our dataset — every cell had a value. This is clean data.

We also checked for **blank strings** (cells that look filled but just contain spaces) using `df.astype(str).apply(...)` — also 0 found.

---

### What would you do if you HAD missing values?

- For **numbers** (like Age, Balance): fill in the **median** (the middle value when sorted). We use median instead of mean (average) because the average gets pulled by very high or low values.
- For **text** (like Geography, Gender): fill in the **mode** (the most common value).
- If too much data is missing (more than ~50% of a column), just **delete that column**.

---

## 4. Duplicate Values

### What are Duplicate Rows?

A duplicate row is when **the exact same customer record appears more than once** in the dataset. This can happen due to data entry errors or system glitches.

---

### Why are they a problem?

If the same customer appears 3 times, the model will learn from that customer 3 times — giving them 3x more influence than they should have. This is unfair and can bias the model.

---

### What did we find?

We ran `df.duplicated().sum()` and found **0 duplicates** — every row was unique.

---

## 5. Outliers and the IQR Method

### What is an Outlier?

An outlier is a value that is **extremely different from the rest** — it is unusually high or unusually low compared to most values.

**Analogy**: If 30 students in a class score between 50–80 marks and one student scores 1 (or 200, if that were possible), that one score is an **outlier**. It is so different that it might be an error, or it might skew your analysis.

---

### Why do outliers matter?

Outliers can **mess up** your machine learning model by pulling it in the wrong direction. Imagine trying to draw a line through 100 normal points plus one point that is 1000x bigger — that one extreme point drags the line way off.

---

### What is IQR?

IQR stands for **Interquartile Range**. It is a measure of how spread out the middle 50% of your data is.

Think of your data sorted from smallest to largest:
- **Q1 (25th percentile)** = the value where 25% of data falls below it
- **Q3 (75th percentile)** = the value where 75% of data falls below it
- **IQR = Q3 minus Q1** = the range of the middle half of the data

**Simple example**: If your data is [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]:
- Q1 = 25 (roughly)
- Q3 = 75 (roughly)
- IQR = 75 - 25 = 50

---

### How do we use IQR to find outliers?

We define boundaries:
```
Lower boundary = Q1 - (1.5 x IQR)
Upper boundary = Q3 + (1.5 x IQR)
```

Any value **below the lower boundary** or **above the upper boundary** is flagged as an outlier and removed.

The 1.5 multiplier is a widely accepted standard (proposed by statistician John Tukey).

---

### What outliers did we find?

| Column | Outliers Found | Action |
|---|---|---|
| `CreditScore` | 15 rows (scores below 440) | Removed |
| `EstimatedSalary` | 2 rows | Removed |
| `Tenure` | 1 row | Removed |
| `NumOfProducts` | Not checked | Values 1–4 are all realistic |

---

### Why did we NOT remove outliers from `NumOfProducts`?

`NumOfProducts` tells us how many bank products a customer holds (credit card, loan, savings account, etc.). A value of 4 means "they have 4 products" — that is a real thing that can happen. It is not an error. So we did not treat those values as outliers.

---

### Why did we CLIP (not remove) outliers in the app?

When a new customer fills in a form to get a prediction, we **cannot just delete their data** — we need to give them a result. So instead of removing outliers, we **clip** (cap) the values to the training boundaries. For example, if a customer enters a CreditScore of 400 (below our training lower bound of 440), we treat it as 440 for the model. This keeps the model safe from extreme inputs.

---

## 6. Feature Binning

### What is Feature Binning?

Feature binning (also called discretisation) means taking a **continuous number and grouping it into a category**.

**Analogy**: Instead of saying a person is exactly "37 years old", you group them as an "Adult" (age 31–45). Instead of tracking exactly how many years a customer has been with the bank, you say they are "New" (0–4 years), "Medium" (5–8 years), or "Long-term" (9+ years).

---

### Why do this?

1. **Groups capture real patterns better**: A 30-year-old and a 44-year-old might both be in "Adult" and have similar behaviour. The exact age number adds noise.
2. **More robust**: If the model sees a 31-year-old during training, it won't panic when it sees a 32-year-old during testing — they are both "Adult".
3. **More interpretable**: It is easier to explain "seniors churn more" than "for every extra year of age, churn probability increases by 0.003".

---

### What bins did we create?

**Tenure (years with the bank):**

| Original Value | Bin Label |
|---|---|
| 0 to 4 years | "New" |
| 5 to 8 years | "Medium" |
| 9+ years | "Long-term" |

**Age:**

| Original Value | Bin Label |
|---|---|
| 18 to 30 | "Young" |
| 31 to 45 | "Adult" |
| 46 to 60 | "Middle-aged" |
| 61 to 74 | "Senior" |

After creating the bins, we **deleted the original Age and Tenure columns** — they were replaced by the bin labels.

---

### What is Ordinal vs Nominal?

- **Ordinal** = categories that have a **meaningful order**. Example: New < Medium < Long-term. One is "more" than the other.
- **Nominal** = categories with **no meaningful order**. Example: France, Germany, Spain. No country is "more" than another.

Our bins (Age_bins and Tenure_bins) are **ordinal** because there is a natural progression.

---

## 7. Scaling

### What is Feature Scaling?

Scaling means **adjusting all numbers to be on the same size scale**, so no single feature unfairly dominates the others.

**Analogy**: Imagine comparing a person's height in centimetres (170 cm) and their weight in kilograms (70 kg). The height number is much bigger simply because of the unit. If you gave these raw numbers to an algorithm, it would think height matters 2–3x more than weight just because of the units — which is wrong.

---

### What is StandardScaler?

StandardScaler transforms numbers so that:
- The **mean (average) becomes 0**
- The **standard deviation becomes 1**

Formula: `scaled_value = (original_value - mean) / standard_deviation`

After scaling, a CreditScore of 658 (the average) becomes 0.0, a very high score becomes +2.3, and a very low score becomes -2.1. Now all features are on a comparable scale.

---

### Which features did we scale?

Only the true **numerical** features:
- `CreditScore`, `Balance`, `NumOfProducts`, `EstimatedSalary`

Binary features (0/1) like `HasCrCard` were left as-is — they are already on a 0-to-1 scale.

---

## 8. Encoding — Turning Words Into Numbers

### Why do we need encoding?

Machine learning models **only understand numbers**. They cannot read text like "France" or "Female". We need to convert these text categories into numbers.

---

### OneHotEncoder — For Nominal Categories (No Order)

OneHotEncoder creates **a new column for each unique category**, filled with 0 or 1.

**Example for Geography:**
| Geography | France | Germany | Spain |
|---|---|---|---|
| France | 1 | 0 | 0 |
| Germany | 0 | 1 | 0 |
| Spain | 0 | 0 | 1 |

Each row has exactly one "1" showing which country the customer is from.

**Why not just use 0, 1, 2?** Because that would imply France < Germany < Spain (as if Germany is somehow "in between"), which has no meaning. OneHotEncoding treats all categories as **equal and separate**.

---

### OrdinalEncoder — For Ordinal Categories (Has Order)

OrdinalEncoder assigns a number that **preserves the order**:

| Bin | Number |
|---|---|
| New | 0 |
| Medium | 1 |
| Long-term | 2 |

This works for `Tenure_bins` and `Age_bins` because the order matters — "Long-term" customers really have been with the bank longer than "New" ones.

---

### What about Binary Features?

`HasCrCard` (0 or 1) and `IsActiveMember` (0 or 1) are **already numbers**. We just pass them through unchanged. This is called **passthrough** in our pipeline.

---

## 9. Train/Test Split

### What is a Train/Test Split?

We split our 15,000 customers into two groups:
- **Training set (80% = 12,000 customers)**: The model learns from this data.
- **Test set (20% = 3,000 customers)**: The model is evaluated on this data — it has **never seen these customers** during training.

---

### Why do we need a separate test set?

If we test the model on the same data it trained on, it would get a perfect score — because it has already memorised the answers. That tells us nothing about how it will perform on **real new customers**.

**Analogy**: If a student studies an exam paper and then takes the exact same exam, they will score 100% — but they have not really learned anything. We test on a **different exam paper** to measure true understanding.

---

### What is `stratify=y`?

When we split the data, `stratify=y` ensures that **both the training and test sets have the same percentage of churned customers (~20%)**.

Without this, by random chance, the test set might have 30% churners and the training set only 15%. That would make evaluation unfair.

---

## 10. Class Imbalance and SMOTE

### What is Class Imbalance?

In our dataset, only **~20% of customers churned** and **~80% stayed**. This is called class imbalance — the two classes are not equally represented.

**Why is this a problem?** A lazy model could predict "Nobody churns" for every single customer and still get **80% accuracy**. But it would be completely useless — it catches 0 actual churners.

---

### What is SMOTE?

SMOTE = **Synthetic Minority Over-sampling Technique**

SMOTE **generates fake (synthetic) examples** of the minority class (churners) to balance the dataset during training. It creates new churner examples that are slightly different from real ones by interpolating between existing churner records.

**Analogy**: You have 1,000 photos of cats and only 200 of dogs. SMOTE creates 800 more "dog-like" images by slightly modifying the 200 real dog images (changing brightness, rotation, etc.) until you have 1,000 of each.

---

### Why apply SMOTE INSIDE the training pipeline (not before splitting)?

This is a very common viva question. The answer is **data leakage**.

If you apply SMOTE before splitting:
1. SMOTE creates synthetic churners based on ALL your data (train + test combined)
2. You split: some synthetic churners end up in the test set
3. The test set now contains data that was derived from the training set
4. Your model scores look great — but it is cheating

By applying SMOTE only inside the training fold (via `ImbPipeline`), **the test set always contains only original, real customer data**. The evaluation is honest.

---

## 11. Machine Learning Models — What Are They?

### Logistic Regression

Despite the name, Logistic Regression is used for **classification** (not regression). It finds a **straight line (or hyperplane)** that best separates the two classes.

**Analogy**: Drawing a straight line on a graph to separate churners from non-churners. Simple, interpretable, but can only draw straight boundaries.

**Our result**: F1 = 0.5565 — worst performer. Churn patterns are not linearly separable.

---

### Decision Tree

A Decision Tree makes predictions by asking a series of **yes/no questions**, like a flowchart:
- "Is Balance > 100,000?" → Yes → "Is NumOfProducts > 2?" → No → "Predict: Churned"

**Analogy**: A 20 Questions game. The model learns which questions to ask to most efficiently separate churners from non-churners.

**Strength**: Easy to understand and visualise.
**Weakness**: Can memorise the training data (overfitting) if allowed to grow too deep.

**Our result**: F1 = 0.6698

---

### Random Forest

Random Forest is an **ensemble** of many Decision Trees. It:
1. Creates hundreds of trees, each trained on a random subset of data and features
2. Each tree votes on the prediction
3. The **majority vote** wins

**Analogy**: Instead of asking one doctor for a diagnosis, you ask 500 doctors. The majority answer is more reliable than any single opinion.

**Strength**: Much more accurate than a single tree, less overfitting.
**Our result**: Tuned F1 = 0.7071, ROC-AUC = 0.9135

---

### XGBoost (eXtreme Gradient Boosting)

XGBoost is also an ensemble of trees, but instead of building them **independently** (like Random Forest), it builds them **one by one**, where each new tree **learns from the mistakes of the previous ones**.

**Analogy**: A student gets their test back, studies the questions they got wrong, and does better on the next test. Then studies those new mistakes, and improves again. Each round corrects previous errors.

XGBoost is also famous for:
- Being very fast (hence "eXtreme")
- Adding **regularisation** (penalties to prevent overfitting)
- Winning many ML competitions

**Our result (best model)**: Tuned F1 = **0.7158**, ROC-AUC = **0.9157**

---

## 12. Cross-Validation and K-Fold

### What is Cross-Validation?

Cross-validation is a way of **testing your model multiple times** on different portions of the training data, so you get a more reliable estimate of how well it performs.

---

### What is Stratified K-Fold?

We used **5-Fold Stratified Cross-Validation**:

1. Split the training data into 5 equal parts (called "folds")
2. Train on folds 1+2+3+4, test on fold 5 → record score
3. Train on folds 1+2+3+5, test on fold 4 → record score
4. Train on folds 1+2+4+5, test on fold 3 → record score
5. Train on folds 1+3+4+5, test on fold 2 → record score
6. Train on folds 2+3+4+5, test on fold 1 → record score
7. **Average all 5 scores** → this is your cross-validation score

**Stratified** means each fold keeps the same ~20% churn ratio.

**Why do this?** One test split might be lucky (easy customers) or unlucky (hard customers). Averaging 5 different test folds gives a much more **reliable estimate** of real performance.

---

## 13. Evaluation Metrics — How Do We Measure Success?

### Confusion Matrix — The Starting Point

Before understanding the metrics, you need to understand what can go right or wrong:

|  | **Predicted: Not Churned** | **Predicted: Churned** |
|---|---|---|
| **Actually: Not Churned** | True Negative (TN) — Correct! | False Positive (FP) — Wrong, cried wolf |
| **Actually: Churned** | False Negative (FN) — Missed! | True Positive (TP) — Correct! |

In churn prediction, **False Negatives are costly** — we predicted someone stays, but they leave. We lose that customer. **False Positives** are less harmful — we offered a deal to someone who was going to stay anyway.

---

### Accuracy

```
Accuracy = (TP + TN) / Total
```

**Correct predictions out of all predictions.**

Why it is **misleading** here: If 80% of customers don't churn, and we always predict "not churned", accuracy = 80%. But we caught zero actual churners. Useless.

---

### Precision

```
Precision = TP / (TP + FP)
```

**Of all customers we PREDICTED to churn — how many actually did?**

High precision = fewer false alarms. "When we warn you about a churner, we are usually right."

---

### Recall (also called Sensitivity)

```
Recall = TP / (TP + FN)
```

**Of all customers who ACTUALLY churned — how many did we catch?**

High recall = fewer missed churners. "We catch most of the people who are about to leave."

---

### F1-Score

```
F1 = 2 x (Precision x Recall) / (Precision + Recall)
```

F1 is the **harmonic mean** of Precision and Recall. It balances both — it punishes you if either one is very low.

**We used F1 as our main metric** because the dataset is imbalanced and we care about both catching churners (recall) and not spamming non-churners with unwanted offers (precision).

Our best model F1 = **0.7158** (71.58%)

---

### ROC-AUC

ROC = Receiver Operating Characteristic. AUC = Area Under the Curve.

**In plain English**: ROC-AUC measures how well the model can **rank** customers. If we pick one random churner and one random non-churner, ROC-AUC is the probability that the model **scores the churner higher**.

- AUC = 0.5 → random guessing (coin flip)
- AUC = 1.0 → perfect model
- AUC = 0.9157 → our model is correct 91.57% of the time when ranking a random churner vs non-churner

**Our best model ROC-AUC = 0.9157** — this is an excellent score.

---

## 14. Hyperparameter Tuning

### What are Hyperparameters?

Every ML model has **settings you must choose before training**. The model cannot learn these from data — you set them manually. These are called hyperparameters.

**Examples for XGBoost:**
- `n_estimators = 200` → build 200 trees (not 50, not 1000 — you choose)
- `max_depth = 3` → each tree can ask at most 3 questions deep
- `learning_rate = 0.2` → how much each new tree corrects errors (too big = overshooting, too small = very slow learning)

---

### What is GridSearchCV?

Grid Search tries **every possible combination** of hyperparameter values you give it.

**Example**: If you give it 3 possible values for `n_estimators`, 3 for `max_depth`, and 2 for `learning_rate`, it tries 3 x 3 x 2 = **18 combinations** and picks the best one.

It is thorough but **slow** when there are many options.

---

### What is RandomizedSearchCV?

Instead of trying every combination, Randomized Search **randomly picks** N combinations (we used 50) from all possible ones.

**Why?** If you have 10 hyperparameters each with 5 options, GridSearch would need 5^10 = **10 million** combinations. RandomizedSearch just tries 50 randomly selected ones — much faster, and usually finds a very good solution.

---

### Our Two-Stage Tuning Strategy

**Stage 1 — RandomizedSearchCV**: Cast a wide net across many possible values. 50 random combinations tried. Find the "rough neighbourhood" of the best settings.

**Stage 2 — GridSearchCV**: Now zoom in. Take the best settings from Stage 1 and search more carefully around them. Exhaustive search in a narrow range.

This is like: Stage 1 = scan the whole city for a good restaurant, Stage 2 = walk every street in the best neighbourhood to find the exact best one.

---

### What is `scale_pos_weight`?

This XGBoost setting tells the model to **pay more attention to the minority class** (churners). The ratio of non-churners to churners is ~4:1, so a value of 4 would tell XGBoost to count each churner 4 times heavier.

In our case, after tuning, the best value was `1` — meaning SMOTE had already balanced the training data enough that this extra weight was not needed.

---

### What is regularisation?

Regularisation is a technique to **prevent the model from becoming too complex and memorising the training data**.

Think of it as a penalty: the model is allowed to be complex, but it gets penalised for each extra bit of complexity it adds. This forces it to find simpler, more general patterns.

- **L1 regularisation (`reg_alpha`)**: Can make some feature weights exactly zero — effectively **ignoring** features that do not help. Acts like automatic feature selection.
- **L2 regularisation (`reg_lambda`)**: Makes all feature weights **smaller** but non-zero. Spreads out the learning evenly.

---

## 15. Overfitting

### What is Overfitting?

Overfitting is when a model **learns the training data too well** — it memorises specific patterns in the training examples rather than learning general rules. As a result, it performs great on training data but poorly on new, unseen data.

**Analogy**: A student who memorises all the past exam answers word-for-word (instead of understanding the concepts). They ace the practice exams but fail the real exam because the questions are worded slightly differently.

---

### How do we detect it?

By comparing **Training F1** and **Test F1**:
- If Training F1 is much higher than Test F1 → **overfitting**
- If they are similar → the model generalises well

**Our XGBoost (best model)**:
- Training F1 = 0.7270
- Test F1 = 0.7158
- Gap = 0.011 → Very small gap. Well generalised.

**Our Random Forest (after tuning)**:
- Training F1 = 0.7960
- Test F1 = 0.7071
- Gap = 0.089 → Larger gap. Possible overfitting. (Our code even printed a warning about this.)

---

### How do we prevent overfitting?

- **Limit tree depth** (`max_depth`) — shallower trees are simpler
- **Use regularisation** (`reg_alpha`, `reg_lambda`) — penalise complexity
- **Use fewer features** (`colsample_bytree`) — randomly ignore some features
- **Use Cross-Validation** — detect it early during tuning

---

## 16. The API and FastAPI

### What is an API?

API = **Application Programming Interface**. It is a way for two programs to **talk to each other**.

**Analogy**: A waiter in a restaurant. You (the customer/frontend) give your order to the waiter. The waiter takes it to the kitchen (the ML model). The kitchen prepares the food (makes a prediction). The waiter brings it back to you. The waiter is the API.

---

### What is FastAPI?

FastAPI is a **Python framework** for building APIs quickly. It is like a toolkit that handles the communication between the web browser and our ML model.

Our FastAPI app does this:
1. **Receives** customer data from the website form
2. **Processes** it (clipping, binning, scaling, encoding)
3. **Feeds** it to the XGBoost model
4. **Returns** the churn prediction and probability

---

### What is Pydantic?

Pydantic is used for **data validation**. When a customer submits a form, Pydantic checks that:
- `Age` is a number (not text)
- `Geography` is provided
- All required fields are filled in

If anything is wrong, it returns an error message immediately without even touching the ML model. Think of it as **form validation** but done automatically by the framework.

---

### What is joblib?

`joblib` is a Python library used to **save and load** trained ML models to/from a file. After training, we save our XGBoost model:

```python
joblib.dump(model, 'best_churn_model.joblib')
```

When the app starts, we load it back:

```python
model = joblib.load('best_churn_model.joblib')
```

This means we don't need to re-train the model every time someone visits the website — we just load the already-trained model from the file.

---

## 17. Quick Reference Glossary

| Term | Plain English |
|---|---|
| **Churn** | A customer leaving the bank |
| **Binary Classification** | Predicting one of two outcomes (yes/no) |
| **Feature** | One column of information (e.g., Age, Balance) |
| **Target Variable** | What we are predicting (Exited: 0 or 1) |
| **Missing Value** | An empty cell in the data |
| **Duplicate Row** | The same customer record appearing twice |
| **Outlier** | A value that is extremely different from the rest |
| **IQR** | Interquartile Range — spread of the middle 50% of data |
| **Clipping** | Capping values at a boundary instead of removing them |
| **Feature Binning** | Grouping a continuous number into a category |
| **Ordinal** | Categories with a meaningful order (New < Medium < Long-term) |
| **Nominal** | Categories with no meaningful order (France, Germany, Spain) |
| **StandardScaler** | Transforms numbers to have mean=0, std=1 |
| **OneHotEncoder** | Converts nominal categories into binary columns |
| **OrdinalEncoder** | Converts ordinal categories into ordered numbers |
| **Train/Test Split** | Dividing data into learning set and evaluation set |
| **Stratify** | Keeping the same class ratio in both train and test |
| **Class Imbalance** | When one outcome (not churned) vastly outnumbers the other |
| **SMOTE** | Creates fake minority-class samples to balance training data |
| **Data Leakage** | When test data information sneaks into training |
| **Logistic Regression** | Linear model that draws a straight boundary |
| **Decision Tree** | A flowchart of yes/no questions |
| **Random Forest** | Many decision trees voting together (parallel) |
| **XGBoost** | Trees built one by one, each fixing previous mistakes (sequential) |
| **Ensemble** | Combining many models to get a better result |
| **Overfitting** | Model memorises training data, fails on new data |
| **Hyperparameter** | A model setting you choose before training |
| **GridSearchCV** | Tries every combination of hyperparameters |
| **RandomizedSearchCV** | Tries N random combinations — faster |
| **Cross-Validation** | Testing on multiple subsets of data to get reliable scores |
| **Stratified K-Fold** | K-Fold that keeps class ratio in each fold |
| **Accuracy** | % of correct predictions (misleading for imbalanced data) |
| **Precision** | Of predicted churners, how many actually churned? |
| **Recall** | Of actual churners, how many did we catch? |
| **F1-Score** | Balance between Precision and Recall |
| **ROC-AUC** | Probability that model ranks a churner above a non-churner |
| **Regularisation** | Penalty added to prevent overfitting |
| **API** | A way for programs to talk to each other |
| **FastAPI** | Python toolkit to build APIs |
| **joblib** | Library to save and load trained ML models |
| **Pydantic** | Automatically validates input data in FastAPI |
| **Pipeline** | A chain of processing steps applied in order |
| **ColumnTransformer** | Applies different transformations to different columns at once |

---

*Read this document a few times and the viva guide will make much more sense!*
