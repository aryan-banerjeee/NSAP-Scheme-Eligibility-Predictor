# NSAP-Scheme-Eligibility-Predictor
## Kosh-AI: NSAP Scheme Eligibility Predictor using IBM watsonx.ai

An AI-powered machine learning solution for automated multi-class classification of National Social Assistance Programme (NSAP) pension schemes using IBM watsonx.ai AutoAI and IBM Cloud Lite Services.

# Project Overview

Kosh-AI is an end-to-end Machine Learning solution developed as part of the Edunet Foundation – IBM SkillsBuild AICTE Internship Program 2026. The project leverages IBM watsonx.ai AutoAI, IBM Watson Machine Learning, and IBM Cloud Lite Services to automate the prediction of the most appropriate National Social Assistance Programme (NSAP) pension scheme using district-level demographic and beneficiary statistics.

The National Social Assistance Programme (NSAP), an initiative of the Government of India, provides financial assistance to elderly individuals, widows, and persons with disabilities belonging to economically vulnerable households. Traditionally, the process of verifying beneficiary records and assigning the appropriate pension scheme requires considerable manual effort and is susceptible to inconsistencies and delays.

Kosh-AI addresses this challenge by utilizing machine learning to analyze demographic patterns and recommend the most suitable NSAP pension scheme. The solution demonstrates the complete lifecycle of an enterprise AI application—from data ingestion and model training to cloud deployment and real-time inference through REST APIs.

# Problem Statement

The National Social Assistance Programme (NSAP) consists of multiple pension schemes, each designed for different beneficiary categories. Manual verification and classification of applicants can be time-consuming, resource-intensive, and may lead to inconsistencies in welfare allocation.

The objective of this project is to develop a machine learning model capable of accurately predicting the appropriate NSAP pension scheme based on district-level demographic and socio-economic information. The solution is intended to support government agencies by providing a faster and more consistent decision-support mechanism for welfare distribution.

# Objectives
1. Develop an AI-powered classification model for NSAP scheme prediction.
2. Automate the beneficiary classification process using machine learning.
3. Reduce manual effort involved in scheme allocation.
4. Improve the consistency and efficiency of welfare distribution.
5. Demonstrate an end-to-end AI deployment workflow using IBM Cloud.
6. Enable real-time predictions through a cloud-hosted REST API.

# Dataset

## Source: AI Kosh – National Social Assistance Programme (NSAP) Dataset

The dataset contains district-level demographic and beneficiary statistics collected under the National Social Assistance Programme.

*Features*

1. Financial Year
2. State Code
3. State Name
4. District Code
5. District Name
6. Total Beneficiaries
7. Total Male Beneficiaries
8. Total Female Beneficiaries
9. Total Transgender Beneficiaries
10. Scheduled Caste (SC) Beneficiaries
11. Scheduled Tribe (ST) Beneficiaries
12. General Category Beneficiaries
13. Other Backward Class (OBC) Beneficiaries
14. Aadhaar Coverage
15. Mobile Number Coverage

*Target Variable*
16. schemecode

# Predicted Classes
Scheme Code	Scheme Name

IGNOAPS	Indira Gandhi National Old Age Pension Scheme
IGNWPS	Indira Gandhi National Widow Pension Scheme
IGNDPS	Indira Gandhi National Disability Pension Scheme

# Technology Stack

Cloud Platform
IBM Cloud Lite
AI & Machine Learning
IBM watsonx.ai Studio
IBM watsonx.ai AutoAI
IBM Watson Machine Learning
Snap Decision Tree Classifier
Programming
Python 3.12
Jupyter Notebook
Libraries
Pandas
NumPy
Requests
IBM watsonx.ai SDK


# Project Workflow

1. Data Collection

The district-wise NSAP dataset was obtained from AI Kosh and uploaded to IBM Cloud Object Storage. The dataset contains aggregated demographic and beneficiary information across districts in India.

2. Data Ingestion

The dataset was securely accessed within IBM watsonx.ai Studio using the IBM Cloud Object Storage integration. Data was loaded into a Pandas DataFrame for further analysis and preprocessing.

3. Automated Machine Learning

IBM watsonx.ai AutoAI was used to automate the machine learning workflow. AutoAI performed:

Data preprocessing
Feature engineering
Categorical encoding
Pipeline generation
Hyperparameter optimization
Cross-validation
Model evaluation

Several machine learning pipelines were automatically generated and evaluated.

4. Model Selection

AutoAI ranked all generated pipelines according to predictive performance.

The best-performing pipeline was:

Pipeline 4

Algorithm:

Snap Decision Tree Classifier

The selected model demonstrated the highest overall performance for multi-class classification of NSAP schemes.

5. Model Registration

The champion model was registered within the IBM watsonx.ai project to enable version control, governance, and deployment readiness.

6. Model Deployment

A dedicated IBM Deployment Space was created to isolate production assets from development resources.

The trained model was promoted to the deployment space and deployed as an Online Deployment, exposing a secure REST API endpoint capable of serving real-time predictions.

7. Real-Time Inference

The deployed model was accessed using authenticated REST API calls. A JSON payload containing district-level demographic information was submitted to the online deployment, and the model returned the predicted NSAP pension scheme along with confidence probabilities.

Example response:

{
  "predictions": [
    {
      "fields": [
        "prediction",
        "probability"
      ],
      "values": [
        [
          "IGNOAPS",
          [0.0, 1.0, 0.0]
        ]
      ]
    }
  ]
}

# Model Performance

AutoAI evaluated multiple candidate pipelines before selecting the optimal model.

Model	Status
Snap Decision Tree Classifier	✅ Selected
Random Forest	Evaluated
Gradient Boosting	Evaluated
Logistic Regression	Evaluated
Decision Tree	Evaluated

The selected Snap Decision Tree pipeline demonstrated the best predictive performance and was deployed as the production model.

# Key Features

1. End-to-end Machine Learning pipeline using IBM watsonx.ai.
2. Automated feature engineering and model selection using AutoAI.
3. Multi-class classification of NSAP pension schemes.
4. Cloud-native deployment using IBM Watson Machine Learning.
5. Secure REST API for real-time predictions.
6. Scalable architecture suitable for integration into government decision-support systems.
7. Enterprise-grade model governance and deployment workflow.

# Sample Prediction
Input
{
  "finyear": "2025-2026",
  "lgdstatecode": 1,
  "statename": "JAMMU AND KASHMIR",
  "lgddistrictcode": 1,
  "districtname": "ANANTNAG",
  "totalbeneficiaries": 8393,
  "totalmale": 5037,
  "totalfemale": 3356,
  "totaltransgender": 0,
  "totalsc": 37,
  "totalst": 232,
  "totalgen": 8039,
  "totalobc": 85,
  "totalaadhaar": 8327,
  "totalmpbilenumber": 7162
}

Output
Predicted Scheme Code:
IGNOAPS

Scheme Name:
Indira Gandhi National Old Age Pension Scheme

Confidence:
[0.0, 1.0, 0.0]
Repository Structure
Kosh-AI-NSAP-Scheme-Predictor
│
├── README.md
├── requirements.txt
├── LICENSE
├── .gitignore
│
├── notebooks/
│   ├── NSAP_Eligibility_Prediction.ipynb
│   └── AutoAI_Local_Notebook.ipynb
│
├── data/
│   └── district_wise_pension_data.csv
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
│   └── Presentation.pdf
│
└── api/
    └── scoring_example.py

Future Enhancements

1. Integration with citizen-facing web applications.
2. Explainable AI (XAI) for transparent prediction explanations.
3. Interactive analytics dashboard using IBM Cognos or Streamlit.
4. Geospatial visualization of welfare scheme distribution.
5. Continuous retraining using updated AI Kosh datasets.
6. Integration with additional government welfare datasets for enhanced decision support.
# Important Note

This solution is trained on district-level aggregated demographic and beneficiary data provided by the AI Kosh dataset. As a result, the model predicts the most appropriate NSAP pension scheme category for a district profile rather than determining the eligibility of individual citizens. The system is intended to function as a decision-support tool to assist welfare administration and streamline scheme classification workflows.

# Acknowledgements
IBM SkillsBuild
Edunet Foundation
AICTE
IBM watsonx.ai
IBM Cloud Lite
AI Kosh (IndiaAI) for providing the NSAP dataset.

# Author

Aryan Banerjee

AICTE – Edunet Foundation IBM SkillsBuild Internship 2026
