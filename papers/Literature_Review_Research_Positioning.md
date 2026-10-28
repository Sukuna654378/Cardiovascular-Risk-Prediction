# 10-Year Cardiovascular Heart Disease (CHD) Risk Prediction Academic Study

## 1. Academic Search Strategy & Keywords
A systematic literature search was conducted across Google Scholar, Semantic Scholar, arXiv, medRxiv, and IEEE Xplore using English multi-keyword combinations with domain synonyms:
* **Concept 1 (Target Condition):** "Cardiovascular Disease" OR "Coronary Heart Disease" OR "CHD Risk" OR "Cardiovascular Event Prediction"
* **Concept 2 (Data / Cohort):** "Framingham Heart Study" OR "Framingham Cohort" OR "Imbalanced Medical Datasets"
* **Concept 3 (Methodology / ML):** "Data Leakage" OR "Cost-Sensitive Learning" OR "Threshold Optimization" OR "Precision-Recall AUC"

---

## 2. Comparative Literature Matrix (Key Open-Access Papers)

| Reference | Question | Data | Methodology | Key Result | Limitation |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **[Raman et al. (2026)](https://www.ispor.org/heor-resources/presentations-database/presentation-cti/ispor-2026/poster-session-1-4/machine-learning-based-prediction-of-10-year-coronary-heart-disease-risk-and-comparison-with-the-framingham-risk-score)**<br>ISPOR 2026<br>[Open Access] | How do ML models perform on 10-year CHD prediction compared to traditional FRS under strict evaluation? | Framingham Heart Study ($N=4,240$) | Logistic Regression, RF, XGBoost, SVM, SHAP interpretability | Random Forest and FRS achieved top AUROC (~0.70), with age, sysBP, and sex as top predictors. | Did not integrate clinical ratio features (PP, MAP) or tune probability decision thresholds for minority recall. |
| **[medRxiv Study (2025)](https://www.medrxiv.org/content/10.1101/2025.02.07.25321756v1.full-text)**<br>medRxiv Preprint<br>[Open Access] | How accurately do machine learning algorithms differentiate 10-year CVD risk across cohorts? | Framingham cohort & CVD clinical sets ($N>3,000$) | Systematic appraisal of ML risk rating classifiers | Algorithms differentiated 10-year CVD risk with 63%–86% overall accuracy. | Suffers from low sensitivity on raw imbalanced datasets when unhandled by threshold optimization. |
| **[MDPI Applied Sciences (2025)](https://www.researchgate.net/publication/394361214_Machine-Learning_Insights_from_the_Framingham_Heart_Study_Enhancing_Cardiovascular_Risk_Prediction_and_Monitoring)**<br>Appl. Sci., 15(15)<br>[Open Access] | Which risk factors are most significant in ML models predicting mortality from Framingham data? | Framingham Heart Study dataset ($N=4,240$) | Random Forest, XGBoost, feature importance analysis | Identified glucose, diastolic BP, and coronary artery disease as key parameters. | Relied on default 0.50 classification decision thresholds, causing low minority positive recall. |
| **[MDPI Life Review (2025)](https://www.mdpi.com/2075-1729/15/1/94)**<br>MDPI Life, 15(1)<br>[Open Access] | What are the key practical challenges and reporting flaws in ML cardiovascular risk models? | Comprehensive literature sample of CVD risk benchmarks | Systematic review of sample size, transparency, and validation standards | Standardized reporting, leak-free CV, and XAI are essential for clinical adoption. | Highlights widespread lack of external validation and threshold tuning in current literature. |
| **[Tasnim et al. / ETASR (2025)](https://etasr.com/index.php/ETASR/article/view/9152)**<br>ETASR, 15(1)<br>[Open Access] | How do ensemble boosting models perform on cardiovascular event prediction? | Cardiovascular risk dataset ($N>3,000$) | HistGradientBoosting, Random Forest, SHAP explainability | HistGradientBoosting achieved high overall predictive performance. | Relied on standard accuracy metrics without reporting PR-AUC or addressing data leakage in preprocessing. |

---

## 3. Research Positioning Statement
> "Existing works train standard machine learning and transformer models using basic imputation, naive accuracy evaluation, unengineered raw features, and default 0.50 classification thresholds. However, no study has combined strict leak-free k-NN cross-validation with clinical risk interaction features (Pulse Pressure, Mean Arterial Pressure, Atherogenic Index) and Precision-Recall decision threshold tuning in the context of the 3,390-patient Framingham 10-year cardiovascular risk dataset. We propose a leak-free Machine Learning framework that eliminates data leakage, handles heavy class imbalance through cost-sensitive weighting, and optimizes decision thresholds specifically for clinical recall."

### Formula Breakdown:
* **X (Current Literature):** Train standard machine learning models with basic mean/median imputation, naive accuracy evaluation, raw unengineered features, and default 0.50 decision thresholds.
* **Y (Identified Gap / Innovation):** Combine strict leak-free k-NN cross-validation with clinical interaction feature engineering (Pulse Pressure, MAP, Atherogenic Index), cost-sensitive weighting, and Precision-Recall threshold tuning.
* **Z (Clinical Context):** 10-year Coronary Heart Disease (CHD) risk prediction on the 3,390-patient Framingham dataset.
* **Our Proposal:** Deliver a robust, leak-free ML framework that eliminates data leakage, resolves the accuracy paradox, and maximizes clinical sensitivity (Recall) for high-risk patients.
