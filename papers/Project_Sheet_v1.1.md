# Data Science Research Project — 10-Year CHD Risk Prediction

---

## 1. Provisional Article Title
**Development and Internal Validation of a 10-Year Coronary Heart Disease Risk Prediction Model Using Routine Non-Invasive Markers: A Comparative Study of Data Science Methods**

---

## 2. Research Question
Do machine learning approaches (ElasticNet penalized logistic regression, Random Forest, XGBoost) improve the discrimination, calibration, and clinical utility of 10-year coronary heart disease (CHD) risk prediction compared with a standard logistic regression baseline on the Framingham cohort, and how consistently do they perform across biological sexes?

---

## 3. Hypothesis
We expect to find that:
1. **Discrimination Limit:** XGBoost and Random Forest will achieve discrimination (AUC-ROC) comparable to or slightly higher than ElasticNet, but without clinically significant net benefit.
2. **Calibration Deficit:** The raw probability calibration of tree-based models will be inferior to logistic regression, requiring post-hoc recalibration (Platt scaling or Isotonic regression).
3. **Sex Heterogeneity:** Predictive discrimination (PR-AUC) will be significantly lower in women than in men, and uncalibrated models will exhibit a higher rate of risk overestimation in female participants relative to male participants directly reflecting the historical calibration deficit of Framingham-derived risk markers in lower-incidence female subgroups.

---

## 4. Dataset Overview

| Item | Detail |
| :--- | :--- |
| **Dataset Name** | Cardiovascular Risk Prediction Dataset (public subset of the Framingham Heart Study) |
| **Source Link** | [Kaggle Dataset Repository](https://www.kaggle.com/code/rushikeshmuley/coronary-heart-disease-prediction-ipynb) |
| **License** | Public domain academic and educational use (ODbL) |
| **Size & Target** | 3,390 patients, 17 variables; 511 positive events (15.07% class imbalance) |
| **Target Variable** | `TenYearCHD` (1 = developed coronary heart disease within 10 years, 0 = no CHD) |
| **Data Scope** | Single-cohort epidemiological dataset with routine non-invasive clinical markers |

---

## 5. Methods

### Model Architectures
1. **ElasticNet Logistic Regression:** Regularized, highly interpretable baseline classifier (Reference).
2. **Random Forest Classifier:** Non-linear bagging ensemble challenger.
3. **XGBoost Classifier:** Non-linear gradient boosting challenger (state-of-the-art tabular benchmark).

### Preprocessing & Pipeline Integrity
* **Anti-Leakage Pipeline:** All feature scaling and $k$-NN imputation (for `glucose`, `totChol`, `BMI`) are computed strictly inside cross-validation training folds.
* **Clinical Feature Engineering:** Derived markers ($PP = \text{sysBP} - \text{diaBP}$, $MAP = \text{diaBP} + \frac{1}{3}PP$, Atherogenic Index = $\text{totChol} / \text{BMI}$, Smoking Load).
* **Imbalance Handling:** Cost-sensitive loss functions (`class_weight='balanced'` / `scale_pos_weight = 5.63`).
* **Validation Strategy:** 
  * Stratified 80/20 Train-Test holdout split.
  * Repeated Stratified 5-Fold Cross-Validation on the training split ($80\%$) for model selection and hyperparameter optimization.
  * Final evaluation and threshold selection on the holdout test set ($20\%$).
* **Reproducibility:** Fixed random seed (`random_state = 42`).

---

## 6. Evaluation Metrics

| Dimension | Metrics & Tools |
| :--- | :--- |
| **Discrimination** | AUC-ROC, Precision-Recall AUC (PR-AUC; prioritized due to $15.1\%$ positive class ratio) |
| **Calibration** | Calibration curves (observed vs. predicted risk), Calibration Slope, Intercept, Brier Score |
| **Clinical Utility** | Decision Curve Analysis (DCA) over actionable decision threshold ranges (10%–30%) |
| **Interpretability** | SHAP (SHapley Additive exPlanations) global feature importance & local waterfall plots |
| **Fairness & Subgroups** | Performance and calibration stratified by biological sex (Men vs. Women) |

---

## 7. Expected Contribution
To rigorously demonstrate whether non-linear machine learning algorithms offer any measurable clinical net benefit over a well-calibrated logistic regression baseline for 10-year CHD risk prediction when evaluated under a leak-free cross-validation framework and decision threshold optimization.

---

## 8. Risks and Plan B

| Identified Risk | Probability | Actionable Plan B |
| :--- | :---: | :--- |
| **No model outperforms logistic regression** | High | Report this negative result transparently: it is clinically valid, publishable, and aligns with recent benchmark literature. |
| **Severe class imbalance skews precision** | High | Optimize decision probability thresholds directly on the Precision-Recall curve to maximize F1 / Recall. |
| **Uncalibrated probability outputs from trees** | High | Apply Platt Scaling (sigmoid) or Isotonic Regression fit on internal cross-validation folds. |
| **Absence of HDL variable in dataset** | Certain | Benchmark against custom logistic regression on available variables rather than full published FRS equations; document explicitly as a limitation. |
| **Non-reproducible pipeline execution** | Low | Git version control, pinned `requirements.txt`, fixed random seeds, and mandatory "Restart & Run All" verification prior to commits. |

---

## 9. Members, Roles & Responsibilities

| Member | Role | Key Responsibilities |
| :--- | :--- | :--- |
| **Naoual Houssni** | Lead Author (Data Science) | Preprocessing pipeline architecture, $k$-NN imputation, model training, hyperparameter optimization, calibration curves, DCA execution, drafting Methods & Results sections. |
| **Ilyass El Bekkali** | Co-Author (Methodology & Evaluation) | Holdout test set validation, decision threshold tuning, sex fairness & subgroup stratification, TRIPOD+AI compliance check, drafting Introduction & Discussion sections, code reproducibility verification. |

* **Cross-Cutting Responsibilities:** Joint code review, collaborative validation of figures and tables, and joint manuscript submission.
