# Heart Disease Prediction Pipeline

An end-to-end data engineering and predictive machine learning pipeline developed to extract, clean, engineer, integrate, and analyze clinical and lifestyle datasets. This system transforms messy, raw health records into highly organized, model-ready data to support the early detection and predictive understanding of heart disease.

---

## 🛠️ System Architecture & Workflow

The project implements a rigorous data engineering lifecycle that guarantees traceablity, data lineage, and reliable predictive modeling.

1. **Extraction (E)**: Pulls data from standalone local files.
2. **Transformation (T)**: Executes extensive automated quality controls, corrupts datasets deliberately to test robustness, imputes missing values, scales variables, and performs feature engineering.
3. **Integration**: Merges separate demographic and clinical profiles.
4. **Loading (L)**: Systematically structures and version-controls outputs at every milestone phase.
5. **Modeling & Prediction**: Trains and validates multiple machine learning algorithms.

---

## 📁 Repository Directory Structure

All generated outputs are cataloged inside the `DataProjectOutputs/` directory, allowing modular access to intermediate states without executing the entire pipeline from scratch.

```text
DataProjectOutputs/
├── phase1_extraction_simple/       # Raw baseline data profiles
├── phase2_before_cleaning/         # Injected noisy/corrupted datasets
├── phase2_after_cleaning/          # Standardized and imputed datasets
│   ├── heart_dataset_cleaned.csv
│   ├── heart_profiling_after_cleaning.csv
│   ├── medical_dataset_cleaned.csv
│   └── medical_profiling_after_cleaning.csv
├── phase3_feature_engineering/     # Risk metrics and categorical groupings
├── phase3_integration/             # Demographic-aligned composite data
├── phase3_scaling/                 # Normalized feature matrices
├── phase4_feature_selection_fixed/ # Target features isolated for learning
├── phase5_chi_squared/             # Statistical dependency metrics
├── phase6_model_comparison/        # Saved model weights and validation metrics
└── phase7_visual_dashboard/        # Performance plots and metrics dashboards

```

---

## 🔄 Core Pipeline Phases

### 1. Data Extraction & Profiling

Initial ingestion accesses two decoupled CSV datasets sourced via Kaggle:

* **Heart Disease Dataset**: $1,000$ patient records containing $16$ baseline features (age, cholesterol, blood pressure, lifestyle traits, and stress levels).
* **Medical Dataset**: $1,319$ patient records containing $9$ clinical attributes (such as CK-MB, troponin, and blood sugar levels).

### 2. Quality Control & Automated Data Cleaning

To stress-test cleaning routines against real-world data issues, anomalies were algorithmically injected:

* **Missingness Injection**: $10\%$ of all continuous numeric fields were replaced with `NaN`.
* **Categorical Corruption**: $5\%$ of text strings were systematically replaced with `"INVALID"`.

#### Cleaning Remediation Suite:

* **Normalization**: Standardizes schema columns to lower-case snake_case formats while scrubbing tracking white spaces and symbol artifacts.
* **Invalid Scrubber**: Replaces `"INVALID"` categories with standard `NaN` formatting for clean uniform processing.
* **Outlier Management**: Implements an Interquartile Range (IQR) clipping method. Continuous values falling outside $[Q_1 - 1.5 \times IQR, Q_3 + 1.5 \times IQR]$ are clipped safely back within normal boundaries.
* **Imputation Engine**: Continuous properties missing observations are filled using column **medians**, while categorical fields rely on the **most frequent value (mode)**.
* **Categorical Encoding**: Maps string parameters into numerical values via a standard Label Encoder (e.g., `Female` $\rightarrow$ $0$, `Male` $\rightarrow$ $1$).

### 3. Feature Engineering

Constructs highly informative domain-specific indicators to assist classification algorithms:

* `lifestyle_score`: Calculated via cross-analysis of smoking habits and alcohol consumption metrics.
* `risk_score`: A multi-variable compound rating based on cholesterol level, blood pressure, and biological age.
* `age_group`: Groups patients into discrete clinical life intervals (`Middle-Aged`, `Senior`, `Elder`).

### 4. Dataset Integration

To construct an exhaustive view of each patient, the transformed tables are compiled using a structured inner-join merge pattern.

* **Merge Keys**: `age`, `gender`
* **Resulting Shape**: The aligned composite dataset expands to a matrix of $11,002$ rows $\times$ $26$ distinct features.

### 5. Feature Scaling

To ensure distance-based models or weight updates are not biased by large data variations (e.g., Blood Pressure vs Troponin), continuous attributes are evaluated using two distinct frameworks:

* **Standard Scaling ($Z$-Score)**: Centers distributions around a mean of $0$ with a standard deviation of $1$.
* **Min-Max Scaling**: Constrains feature bounds strictly between the interval $[0, 1]$.

---

## 🤖 Predictive Modeling & Machine Learning

The integrated dataset is split into independent training and testing sets to evaluate real-world generalizability.

### Trained Architectures:

1. **Logistic Regression**: Implemented as a linear baseline model to assess statistical log-odds probabilities.
2. **Random Forest Classifier**: Implemented as a non-linear ensemble method to evaluate feature interaction splits and capture subtle health patterns.

### Evaluation Suite:

Model performance is continuously evaluated across multiple visual diagnostics:

* **Confusion Matrices**: Provides exact counts of true positives, true negatives, false positives, and false negatives.
* **ROC Curves (Receiver Operating Characteristic)**: Tracks the true-positive rate against false-positive trade-offs to visualize prediction performance across thresholds.
* **Probability Plots**: Analyzes distribution alignments and predictive confidence intervals.

---

## 📈 Key Visualization Discoveries

* **Age Distributions**: Comparative density plots confirm that the cleaning process smooths out data variations, resulting in a balanced, realistic age distribution profile across medical cohorts.
* **Feature Correlations**: Post-cleaning correlation heatmaps show that multi-collinearity remains minimal throughout the core feature matrix. Strong correlations are restricted strictly to anticipated clinical dependencies (such as Systolic vs Diastolic blood pressure), ensuring that each feature contributes unique predictive value to the machine learning models.
