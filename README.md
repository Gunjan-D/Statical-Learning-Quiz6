#  Classification Models (Decision Tree, Pruned Tree, Logistic Regression, Majority Vote)

## What this project does
This project builds and tests multiple classification models on a dataset called **GreatUnknown.csv**.  
It cleans the data, splits it into train/test sets, trains models, evaluates them using a confusion matrix, and exports predictions to an Excel file. 

---

## Files in this project
- `testt.Rmd`: Main code (data cleaning, training, evaluation, export).
- `testt.docx`: Report write-up (if you are using it for submission).

---

## Dataset assumptions
- Input file: `GreatUnknown.csv` 
- Target column: `y` (converted to factor for classification). 
- Other columns are features used for prediction.

---

## Steps and logic (high level)

### 1) Load packages
The notebook loads packages used for:
- data handling (`tidyverse`)
- splitting (`caret`, `caTools`)
- decision trees (`rpart`, `rattle`)
- logistic regression (`MASS`)
- exporting results (`writexl`)

### 2) Read and clean data
- Read CSV
- Remove rows with missing values using `na.omit`
- Convert `y` to a factor
- Print how many rows remain after cleaning

### 3) Train/test split
- Uses `createDataPartition` with 75% train and 25% test
- Uses `set.seed` to keep results repeatable 

### 4) Full decision tree (unpruned)
- Trains a full tree using `rpart(..., cp = 0)`
- Predicts test labels
- Creates confusion matrix and metrics (Sensitivity, Specificity, Accuracy) 

### 5) Pruned decision tree (cross validation)
- Uses `caret::train` with 10-fold CV to find best `cp`
- Plots the model
- Uses the best model (pruned tree) for prediction 

### 6) Logistic regression
- Trains `glm(y ~ ., family = binomial)`
- Predicts probabilities on test set
- Converts probabilities to class labels using threshold 0.5
- Computes confusion matrix and metrics 

### 7) Majority vote ensemble
- Combines predictions from:
  - Full Tree (FT)
  - Pruned Tree (PT)
  - Logistic Regression (LR)
- Uses row-wise majority vote
- Computes confusion matrix + metrics for the ensemble
- Exports results to `prediction_results.xlsx` 

---

## Algorithms used

### A) Decision Tree (rpart)
- Splits data based on feature rules to reduce impurity.
- Full tree uses `cp = 0` to allow maximum growth (can overfit). 
### B) Pruned Tree (cross validated)
- Uses k-fold cross validation to choose a `cp` that balances fit vs generalization.
- This usually reduces overfitting compared to the full tree. 

### C) Logistic Regression
- Fits a linear decision boundary in log-odds space.
- Converts probabilities to classes using 0.5 threshold. :contentReference[oaicite:13]{index=13}

### D) Majority Vote
- For each row: take the label predicted by at least 2 out of 3 models.
- Acts like a simple ensemble to improve stability. :contentReference[oaicite:14]{index=14}

---

## Metrics used (from confusion matrix)
Given confusion matrix:

- TP = true positives
- TN = true negatives
- FP = false positives
- FN = false negatives

Metrics:
- **Sensitivity (Recall)** = TP / (TP + FN)
- **Specificity** = TN / (TN + FP)
- **Accuracy** = (TP + TN) / (TP + TN + FP + FN)

---

## How to run

### Option 1: RStudio (recommended)
1. Put `testt.Rmd` and `GreatUnknown.csv` in the same folder.
2. Open `testt.Rmd` in RStudio.
3. Click **Knit** to generate the HTML output.

### Option 2: Run from R console
```r
rmarkdown::render("testt.Rmd")
