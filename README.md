# Kosh-AI: NSAP Scheme Demand Predictor using IBM watsonx.ai

> **An AI-powered machine learning solution for automated, district-level classification of National Social Assistance Programme (NSAP) pension schemes using IBM watsonx.ai AutoAI and IBM Cloud Lite Services.**

![Python](https://img.shields.io/badge/Python-3.12-blue)
![IBM watsonx.ai](https://img.shields.io/badge/IBM-watsonx.ai-052FAD)
![AutoAI](https://img.shields.io/badge/IBM-AutoAI-052FAD)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Deployed-success)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Dataset](#dataset)
- [Technology Stack](#technology-stack)
- [Solution Architecture](#solution-architecture)
- [Project Workflow](#project-workflow)
- [Model Performance](#model-performance)
- [Sample Prediction](#sample-prediction)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Important Note on Scope](#important-note-on-scope)
- [Future Enhancements](#future-enhancements)
- [Acknowledgements](#acknowledgements)
- [Author](#author)
- [License](#license)

---

## Project Overview

Kosh-AI is an end-to-end Machine Learning solution developed as part of the **Edunet Foundation – IBM SkillsBuild AICTE Internship Program 2026**. The project leverages **IBM watsonx.ai AutoAI**, **IBM Watson Machine Learning**, and **IBM Cloud Lite Services** to automate the prediction of the most likely **National Social Assistance Programme (NSAP)** pension scheme category using district-level demographic and beneficiary statistics.

The National Social Assistance Programme (NSAP), an initiative of the Government of India, provides financial assistance to elderly individuals, widows, and persons with disabilities belonging to economically vulnerable households. Traditionally, verifying beneficiary records and allocating them across the correct pension scheme requires considerable manual effort at the district administration level and is susceptible to inconsistency and delay.

Kosh-AI addresses this by using machine learning to analyze demographic composition at the district level and classify the dominant NSAP scheme pattern for that district. The solution demonstrates the complete lifecycle of an enterprise AI application — from data ingestion and automated model training to cloud deployment and real-time inference through a REST API.

---

## Problem Statement

The National Social Assistance Programme (NSAP) consists of multiple pension schemes, each designed for different beneficiary categories. Manual verification and classification of records at scale can be time-consuming, resource-intensive, and prone to inconsistency across districts and states.

This project develops a machine learning model capable of predicting the most likely NSAP pension scheme category for a district, based on that district's demographic and socio-economic profile — supporting government agencies with a faster, more consistent decision-support mechanism for welfare planning and resource allocation.

---

## Objectives

* Develop an AI-powered classification model for NSAP scheme prediction.
* Automate district-level scheme-pattern classification using machine learning.
* Reduce manual effort involved in welfare planning and scheme allocation review.
* Improve the consistency and efficiency of welfare distribution decision-support.
* Demonstrate an end-to-end AI deployment workflow using IBM Cloud.
* Enable real-time predictions through a cloud-hosted REST API.

---

## Dataset

**Source:** AI Kosh (IndiaAI) – National Social Assistance Programme (NSAP) Dataset
**Granularity:** District-level, aggregated per scheme, Financial Year 2025–2026
**Size:** 2,156 records across 726 districts, 36 states/UTs, 3 scheme categories

### Features

| Column | Description |
|---|---|
| `finyear` | Financial year of the record |
| `lgdstatecode` | LGD state code |
| `statename` | State/UT name |
| `lgddistrictcode` | LGD district code |
| `districtname` | District name |
| `totalbeneficiaries` | Total beneficiaries under the scheme |
| `totalmale` / `totalfemale` / `totaltransgender` | Beneficiaries by gender |
| `totalsc` / `totalst` / `totalgen` / `totalobc` | Beneficiaries by social category |
| `totalaadhaar` | Beneficiaries with Aadhaar linked |
| `totalmobilenumber` | Beneficiaries with mobile number linked |

### Target Variable

```
schemecode
```

### Predicted Classes

| Scheme Code | Scheme Name |
|---|---|
| IGNOAPS | Indira Gandhi National Old Age Pension Scheme |
| IGNWPS | Indira Gandhi National Widow Pension Scheme |
| IGNDPS | Indira Gandhi National Disability Pension Scheme |

---

## Technology Stack

**Cloud Platform**
- IBM Cloud Lite

**AI & Machine Learning**
- IBM watsonx.ai Studio
- IBM watsonx.ai AutoAI
- IBM Watson Machine Learning
- Snap Random Forest Classifier (SnapML, `BatchedTreeEnsembleClassifier`)

**Programming**
- Python 3.12
- Jupyter Notebook

**Libraries**
- `ibm-watsonx-ai`
- `autoai-libs`
- `lale`
- `scikit-learn`
- `snapml`
- `xgboost`, `lightgbm` (evaluated as candidate estimators)
- `pandas`, `numpy`, `matplotlib`

**Data Source**
- AI Kosh Dataset (IndiaAI)

---

## Solution Architecture

```text
                     AI Kosh Dataset
                            │
                            ▼
                 IBM Cloud Object Storage
                            │
                            ▼
               IBM watsonx.ai Studio Project
                            │
                            ▼
                  Data Preparation & Analysis
                            │
                            ▼
               IBM watsonx.ai AutoAI Training
             (multiclass, target = schemecode)
                            │
                            ▼
          Multiple ML Pipeline Evaluation
   (Snap Random Forest, Batched Extra Trees, LightGBM)
                            │
                            ▼
          Best Model Selection — Pipeline_5
             Snap Random Forest Classifier
                            │
                            ▼
        Incremental Training (partial_fit, batched)
                            │
                            ▼
                  Model Registration
                            │
                            ▼
                IBM Deployment Space
                            │
                            ▼
               Online Model Deployment
                            │
                            ▼
                  REST API Endpoint
                            │
                            ▼
              Real-Time Prediction Service
```

---

## Project Workflow

### 1. Data Collection
The district-wise NSAP dataset was obtained from AI Kosh and uploaded to IBM Cloud Object Storage as a project data asset.

### 2. Data Ingestion
The dataset was accessed within IBM watsonx.ai Studio via the Cloud Object Storage integration (`DataConnection` / `data_asset_id`) and loaded for AutoAI training.

### 3. Automated Machine Learning
IBM watsonx.ai AutoAI was configured as a **multiclass** experiment (`prediction_column='schemecode'`, `scoring='accuracy'`, `holdout_size=0.1`) and automated:

- Data preprocessing and cleaning (duplicate removal)
- Categorical encoding
- Feature selection
- Pipeline generation across multiple batched ensemble estimators (Random Forest, Extra Trees, LightGBM — all SnapML-accelerated)
- Hyperparameter optimization
- Cross-validation and holdout evaluation

### 4. Model Selection
AutoAI ranked all generated pipelines by holdout accuracy via `pipeline_optimizer.summary()`.

The best-performing pipeline was:

**Pipeline_5 — Snap Random Forest Classifier** (`BatchedTreeEnsembleClassifier`)

Feature importances for the winning pipeline were extracted using `get_pipeline_details()['features_importance']`.

### 5. Incremental Training
A dedicated notebook resumed Pipeline_5 as a `lale`-wrapped estimator and continued training via batch-wise `partial_fit()` calls over the full dataset (batch size = 2,156 rows), with learning-curve tracking across batches. This supports future retraining as new financial-year data becomes available, without discarding the AutoAI-selected pipeline structure.

### 6. Model Registration
The trained pipeline was registered in the IBM watsonx.ai project repository (`client.repository.store_model`) for version control and deployment readiness.

### 7. Model Deployment
A dedicated **IBM Deployment Space** was created to separate production assets from the development project. The model was promoted to this space and deployed as an **Online Deployment**, exposing a secure REST scoring endpoint.

### 8. Real-Time Inference
The deployed endpoint was tested with a live scoring request. The model returned a prediction and full class-probability vector in real time — confirmed below in [Sample Prediction](#sample-prediction).

---

## Model Performance

AutoAI evaluated multiple batched ensemble pipelines under `include_batched_ensemble_estimators` before selecting the champion model:

| Model | Status |
|---|---|
| Snap Random Forest Classifier (Pipeline_5) | ✅ Selected |
| Batched Extra Trees Classifier | Evaluated |
| Batched LightGBM Classifier | Evaluated |

The selected Snap Random Forest pipeline was deployed as the production model and validated via incremental training and holdout evaluation.

---

## Sample Prediction

### Live API Test Result

A scoring request was sent to the deployed online endpoint. The service returned:

```json
[
  {
    "fields": ["prediction", "probability"],
    "values": [
      [
        "IGNDPS",
        [0.9867446884372569, 0.013255311562742558, 0]
      ]
    ]
  }
]
```

### Interpretation

```text
Predicted Scheme Code:
IGNDPS

Scheme Name:
Indira Gandhi National Disability Pension Scheme

Confidence:
98.67%
```

---

## Repository Structure

```
Kosh-AI-NSAP-Scheme-Predictor/
│
├── README.md
├── requirements.txt
├── LICENSE
├── .gitignore
│
├── notebooks/
│   ├── Kosh-AI_NSAP_Predictor_Notebook.ipynb        # AutoAI experiment, pipeline comparison, deployment
│   └── P5_Snap_Random_Forest_Incremental.ipynb      # Incremental training (partial_fit) on Pipeline_5
│
├── data/
│   └── Social_Welfare_Schemes.csv
│
├── screenshots/
│   ├── dataset.png
│   ├── autoai_leaderboard.png
│   ├── deployment.png
│   ├── api_prediction.png
│   └── architecture.png
│
├── docs/
│   ├── Project_Report.pdf
│   ├── MODEL_CARD.md
│   └── Presentation.pdf
│
└── api/
    └── scoring_example.py
```

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/<your-username>/Kosh-AI-NSAP-Scheme-Predictor.git
cd Kosh-AI-NSAP-Scheme-Predictor

# Install dependencies
pip install -r requirements.txt

# Set required environment variables (never commit real values)
export IBM_CLOUD_API_KEY=your_api_key
export WML_PROJECT_ID=your_project_id
export WML_SPACE_ID=your_space_id
export WML_DEPLOYMENT_ID=your_deployment_id

# Run a sample scoring request
python api/scoring_example.py
```

> API credentials are never hardcoded in notebooks or scripts. See `.env.example` for the required variable names.

---

## Important Note on Scope

This solution is trained on **district-level aggregated demographic and beneficiary data** from the AI Kosh dataset, not individual applicant records. As a result, the model predicts the **most likely NSAP scheme pattern for a district profile**, not the eligibility of any individual citizen. NSAP eligibility remains a statutory, rule-based determination (age, income, disability certification, marital status) made by the relevant government authority. Kosh-AI is intended purely as a **decision-support and planning tool** for welfare administration — helping anticipate district-level demand and resource needs, not to make or override individual eligibility decisions.

---

## Future Enhancements

* Multi-year time-series modeling as additional NSAP financial-year data becomes available.
* Explainable AI (SHAP/LIME) for transparent, auditable predictions.
* Fairness/bias audit across gender and social-category (SC/ST/OBC) subgroups.
* Interactive analytics dashboard using IBM Cognos or Streamlit.
* Geospatial visualization of scheme distribution across districts.
* CI/CD integration (GitHub Actions) for automated retraining and champion/challenger deployment gating.
* Lightweight FastAPI wrapper around the Watson ML deployment for cleaner request/response handling and logging.
* Integration with additional government welfare datasets (e.g., PM-KISAN, ration card digitization) for broader decision support.

---

## Acknowledgements

* **IBM SkillsBuild**
* **Edunet Foundation**
* **AICTE**
* **IBM watsonx.ai**
* **IBM Cloud Lite**
* **AI Kosh (IndiaAI)** for providing the NSAP dataset.

---

## Author

**Aryan Banerjee**
AICTE – Edunet Foundation IBM SkillsBuild Internship 2026

---

## License

This project is released under the **MIT License**. See the `LICENSE` file for details.
