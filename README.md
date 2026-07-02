# NSAP-Scheme-Eligibility-Predictor
Kosh-AI: NSAP Scheme Eligibility Predictor using IBM watsonx.ai

An AI-powered machine learning solution for automated classification of National Social Assistance Programme (NSAP) pension schemes using IBM watsonx.ai AutoAI and IBM Cloud Lite Services.

*Project Overview*

Kosh-AI is an end-to-end Machine Learning solution developed as part of the Edunet Foundation – IBM SkillsBuild AICTE Internship Program 2026. The project leverages IBM watsonx.ai AutoAI, IBM Watson Machine Learning, and IBM Cloud Lite Services to automate the prediction of the most appropriate National Social Assistance Programme (NSAP) pension scheme using district-level demographic and beneficiary statistics.

The National Social Assistance Programme (NSAP), an initiative of the Government of India, provides financial assistance to elderly individuals, widows, and persons with disabilities belonging to economically vulnerable households. Traditionally, the process of verifying beneficiary records and assigning the appropriate pension scheme requires considerable manual effort and is susceptible to inconsistencies and delays.

Kosh-AI addresses this challenge by utilizing machine learning to analyze demographic patterns and recommend the most suitable NSAP pension scheme. The solution demonstrates the complete lifecycle of an enterprise AI application—from data ingestion and model training to cloud deployment and real-time inference through REST APIs.

*Problem Statement*

The National Social Assistance Programme (NSAP) consists of multiple pension schemes, each designed for different beneficiary categories. Manual verification and classification of applicants can be time-consuming, resource-intensive, and may lead to inconsistencies in welfare allocation.

The objective of this project is to develop a machine learning model capable of accurately predicting the appropriate NSAP pension scheme based on district-level demographic and socio-economic information. The solution is intended to support government agencies by providing a faster and more consistent decision-support mechanism for welfare distribution.

Objectives
Develop an AI-powered classification model for NSAP scheme prediction.
Automate the beneficiary classification process using machine learning.
Reduce manual effort involved in scheme allocation.
Improve the consistency and efficiency of welfare distribution.
Demonstrate an end-to-end AI deployment workflow using IBM Cloud.
Enable real-time predictions through a cloud-hosted REST API.
Dataset

Source: AI Kosh – National Social Assistance Programme (NSAP) Dataset

The dataset contains district-level demographic and beneficiary statistics collected under the National Social Assistance Programme.

Features
Financial Year
State Code
State Name
District Code
District Name
Total Beneficiaries
Total Male Beneficiaries
Total Female Beneficiaries
Total Transgender Beneficiaries
Scheduled Caste (SC) Beneficiaries
Scheduled Tribe (ST) Beneficiaries
General Category Beneficiaries
Other Backward Class (OBC) Beneficiaries
Aadhaar Coverage
Mobile Number Coverage
Target Variable
schemecode
Predicted Classes
Scheme Code	Scheme Name
IGNOAPS	Indira Gandhi National Old Age Pension Scheme
IGNWPS	Indira Gandhi National Widow Pension Scheme
IGNDPS	Indira Gandhi National Disability Pension Scheme
Technology Stack
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
Data Source
AI Kosh Dataset
