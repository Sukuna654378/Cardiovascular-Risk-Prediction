# Data Science Research Project — 10-Year CHD Risk Prediction

---

## 1. Provisional Article Title
**Development and External Validation of a 10-Year Coronary Heart Disease Risk Prediction Model Using Routine Non-Invasive Markers: A Comparative Study of Data Science Methods**

---

## 2. Research Question
Do machine learning approaches (ElasticNet penalized logistic regression, Random Forest, XGBoost) improve the discrimination, calibration, and clinical utility of 10-year coronary heart disease (CHD) risk prediction compared with a standard logistic regression baseline, and does their predictive performance hold up under external validation on NHANES across biological sexes?

---

## 3. Hypothesis
We expect to find that:
1. **Discrimination Limit:** XGBoost and Random Forest will achieve discrimination (AUC-ROC) comparable to or slightly higher than ElasticNet, but without clinically significant net benefit.
2. **Calibration Deficit:** The baseline calibration of tree-based models will be poorer than that of logistic regression, requiring post-hoc probability recalibration.
3. **Transportability Degradation:** External validation on NHANES will lead to degraded calibration due to population shift, which can be partially restored via Platt scaling.
4. **Sex Heterogeneity:** Predictive performance and calibration will vary significantly across biological sexes, reflecting the historical tendency of Framingham-based tools to overestimate risk in women.

---

## 4. Dataset Overview

| Item | Detail |
| :--- | :--- |
| **Primary Dataset** | Cardiovascular Risk Prediction Dataset (public subset of the Framingham Heart Study) |
| **Source Link** | [Kaggle Dataset Repository](https://www.kaggle.com/code/rushikeshmuley/coronary-heart-disease-prediction-ipynb) |
| **License** | Public domain academic and educational use (ODbL) |
| **Size & Target** | 3,390 patients, 17 variables; 511 positive events (15.07% class imbalance) |
| **Target Variable** | `TenYearCHD` (1 = developed coronary heart disease within 10 years, 0 = no CHD) |
| **External Validation** | NHANES Linked Mortality Files / CVD Cohort (CDC/NCHS, Public Domain) [CDC NHANES](https://www.cdc.gov/nchs/nhanes/) |

---

## 5. Methods

### Model Architecture
1. **ElasticNet Logistic Regression:** Interpretable, regularized baseline model (Reference).
2. **Random Forest Classifier:** Non-linear bagging ensemble challenger.
3. **XGBoost Classifier:** Non-linear gradient boosting challenger (state-of-the-art tabular benchmark).

### Preprocessing & Pipeline Integrity
* **Anti-Leakage Pipeline:** All scaling and $k$-NN imputation (for `glucose`, `totChol`, `BMI`) are computed strictly inside cross-validation training folds.
* **Feature Engineering:** Derived clinical markers ($PP = \text{sysBP} - \text{diaBP}$, $MAP = \text{diaBP} + \frac{1}{3}PP$, Atherogenic Index = $\text{totChol} / \text{BMI}$, Smoking Load).
* **Imbalance Handling:** Handled via cost-sensitive learning (`class_weight='balanced'` / `scale_pos_weight = 5.63`).
* **Validation Strategy:** 
  * *Internal Validation:* Repeated Stratified 5-Fold Cross-Validation on 100% of the Framingham cohort (maximizing positive event sample size, $N=511$).
  * *External Validation:* Frozen model evaluation on the harmonized NHANES cohort.
* **Reproducibility:** Fixed random seed (`random_state = 42`).

---

## 6. Evaluation Metrics

| Dimension | Metrics & Tools |
| :--- | :--- |
| **Discrimination** | AUC-ROC, Precision-Recall AUC (PR-AUC; prioritized for 15.1% minority class) |
| **Calibration** | Calibration curves (observed vs. predicted), Calibration Slope, Intercept, Brier Score |
| **Clinical Utility** | Decision Curve Analysis (DCA) over actionable decision thresholds (10%–30%) |
| **Interpretability** | SHAP (SHapley Additive exPlanations) global feature importance & local waterfall plots |
| **Fairness & Transportability** | Sex-stratified performance metrics (Men vs. Women) and post-hoc Platt scaling recalibration |

---

## 7. Expected Contribution
To rigorously demonstrate that complex data science methods do not provide clinically significant net benefit over well-calibrated logistic regression in 10-year CHD risk prediction, while identifying specific conditions (decision thresholds, sex stratification, and external transportability) under which non-linear algorithms add measurable value.

---

## 8. Risks and Plan B

| Identified Risk | Probability | Actionable Plan B |
| :--- | :---: | :--- |
| **No model outperforms logistic regression** | High | Honestly report this null result: it is clinically valid, publishable, and aligns with recent empirical literature. |
| **Harmonizing NHANES outcome variables** | Medium | Define a clear mortality/diagnosis proxy (CDC NDI Linked Mortality CVD outcome) and restrict to matching clinical covariates. |
| **Insufficient power for subgroup analyses** | High | Present sex-stratified results descriptively using bootstrapped 95% confidence intervals. |
| **Absence of HDL variable in dataset** | Certain | Benchmark against custom logistic regression on shared variables rather than full published FRS equations; document explicitly as a limitation. |
| **Uncalibrated probability outputs from trees** | High | Apply Platt Scaling (sigmoid) or Isotonic Regression on internal validation folds prior to external transport. |
| **Non-reproducible pipeline execution** | Low | Git version control, pinned `requirements.txt`, fixed random seeds, and mandatory "Restart & Run All" verification prior to commits. |

---

## 9. Members, Roles & Responsibilities

| Member | Role | Key Responsibilities |
| :--- | :--- | :--- |
| **Naoual Houssni** | Lead Author (Data Science) | Study protocol, $k$-NN preprocessing pipeline, comparative model training, calibration curves, DCA, SHAP analysis, drafting Methods & Results sections, GitHub repository maintenance. |
| **Ilyass El Bekkali** | Co-Author (Methodology & Validation) | NHANES external validation pipeline, outcome proxy matching, sex fairness analysis, TRIPOD+AI compliance check, drafting Introduction & Discussion sections, code reproducibility verification. |

* **Cross-Cutting Responsibilities:** Joint peer review of code and manuscript, collaborative validation of figures and tables, and joint manuscript submission.
