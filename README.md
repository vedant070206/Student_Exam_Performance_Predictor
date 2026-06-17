# 🎓 Student Exam Performance Predictor

[![Project Status](https://img.shields.io/badge/status-production-success.svg)](https://github.com/vedant070206/Student_Exam_Performance_Predictor)
[![Python](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/flask-2.3-lightgrey.svg)](https://flask.palletsprojects.com/)
[![Scikit-Learn](https://img.shields.io/badge/scikit--learn-1.4-orange.svg)](https://scikit-learn.org/)
[![CatBoost](https://img.shields.io/badge/catboost-latest-red.svg)](https://catboost.ai/)


## 🚀 Project Purpose

**Student Performance Predictor** is an end-to-end machine learning repository designed to predict student academic outcomes from demographic and test score indicators. The goal is to demonstrate a production-ready ML workflow with clean code organization, model artifact management, and a deployable Flask web interface.

This project is ideal for portfolio presentation because it covers:
- a complete ML lifecycle from raw data ingestion to prediction service
- model training, evaluation, artifact serialization, and inference
- Docker containerization and cloud deployment preparation
- clear architecture for technical reviewers and recruiters

---

## 🌐 Live Demo

Experience the deployed Student Performance Predictor application:

**AWS Elastic Beanstalk Deployment**

🔗 [Live Demo](http://studentexamscorepredictor-env.eba-3m7dcaeb.ap-south-1.elasticbeanstalk.com/predictdata)

### Features Available

* Interactive web interface
* Real-time score prediction
* Automated preprocessing pipeline
* End-to-end machine learning inference workflow

> The application accepts student demographic and academic information and predicts the expected mathematics score using the trained machine learning model.

---

## 📊 Dataset Overview

The dataset is sourced from `notebook/data/stud.csv` and combines student demographic information with academic scores. It includes both categorical variables and numeric exam results to predict a student’s math performance.

Key features include:
- `gender`
- `race_ethnicity`
- `parental_level_of_education`
- `lunch`
- `test_preparation_course`
- `reading_score`
- `writing_score`
- `math_score` (target)

The data is used to train a regression model that estimates student performance while demonstrating preprocessing best practices for mixed data types.

---

## 📌 What This Repository Does

- Reads the student dataset from `notebook/data/stud.csv`
- Splits the dataset into training and testing sets
- Builds preprocessing pipelines for numeric and categorical features
- Evaluates multiple regression models and selects the best candidate
- Serializes the model and preprocessor as artifacts
- Serves predictions through a Flask web application
- Supports container deployment via Docker and cloud deployment on AWS/Azure

---

## 📁 Repository Structure

```text
ML_Project/
├── .ebextensions/                # AWS Elastic Beanstalk configuration (optional)
├── application.py                # Flask web server entrypoint
├── Dockerfile                    # Container build instructions
├── README.md                     # Project documentation
├── requirements.txt              # Python dependencies
├── setup.py                      # Python package setup
├── artifacts/                    # Generated artifacts for pipeline outputs
│   ├── data.csv                  # Raw data snapshot
│   ├── train.csv                 # Train split
│   ├── test.csv                  # Test split
│   ├── model.pkl                 # Serialized best model (after training)
│   └── preprocessor.pkl          # Serialized preprocessing pipeline
├── notebook/                     # Jupyter notebooks and source dataset
│   ├── 1 . EDA STUDENT PERFORMANCE .ipynb
│   ├── 2. MODEL TRAINING.ipynb
│   └── data/stud.csv             # Source dataset for data ingestion
├── src/                          # Core application package
│   ├── __init__.py
│   ├── exception.py              # Custom exception handling
│   ├── logger.py                 # Logging configuration
│   ├── utils.py                  # Serialization and evaluation utilities
│   ├── components/               # Data and model pipeline components
│   │   ├── data_ingestion.py
│   │   ├── data_transformation.py
│   │   └── model_trainer.py
│   └── pipeline/                 # Prediction and training orchestrators
│       ├── predict_pipeline.py
│       └── train_pipeline.py
├── templates/                    # Flask HTML templates
│   ├── home.html
│   └── index.html
├── catboost_info/                # CatBoost training artifacts
└── logs/                         # Application logs
```

---

## 🧰 Technologies Used

| Category | Tools |
| --- | --- |
| Language | Python 3.11 |
| Web | Flask |
| Data | pandas, numpy |
| Machine learning | scikit-learn, CatBoost |
| Preprocessing | sklearn.pipeline, ColumnTransformer, OneHotEncoder |
| Model serialization | dill, pickle |
| Containerization | Docker |
| Cloud | AWS Elastic Beanstalk / ECS, Azure App Service |
| Logging | Python `logging` |

---

## 🧠 Machine Learning Pipeline

### 1. Data ingestion
- `src/components/data_ingestion.py` loads the dataset from `notebook/data/stud.csv`
- saves a raw copy to `artifacts/data.csv`
- splits data into `artifacts/train.csv` and `artifacts/test.csv`

### 2. Data transformation
- `src/components/data_transformation.py` defines separate pipelines for:
  - numeric features: `reading_score`, `writing_score`
  - categorical features: `gender`, `race_ethnicity`, `parental_level_of_education`, `lunch`, `test_preparation_course`
- numeric pipeline uses median imputation and scaling
- categorical pipeline uses most-frequent imputation and one-hot encoding
- the fitted preprocessor is saved as `artifacts/preprocessor.pkl`

### 3. Model training
- `src/components/model_trainer.py` evaluates multiple regressors:
  - Random Forest
  - Decision Tree
  - Gradient Boosting
  - Linear Regression
  - CatBoost Regressor
  - AdaBoost Regressor
- grid search is used for hyperparameter tuning
- best model is selected based on evaluation performance and validated using the test dataset
- the final model is persisted to `artifacts/model.pkl`

### 4. Prediction pipeline
- `src/pipeline/predict_pipeline.py` loads the saved preprocessor and model
- `CustomData` wraps form inputs and converts them into a DataFrame
- predictions are made and returned to the Flask app

### 5. Inference service
- `application.py` exposes a web UI
- users can submit student demographics and scores
- the app returns a predicted student performance score

---

## 📈 Model Performance

After evaluating multiple regression algorithms, the best-performing model was selected and deployed for inference.

### Best Model: Linear Regression

| Metric   | Training Set | Test Set |
| -------- | ------------ | -------- |
| RMSE     | 5.3231       | 5.3940   |
| MAE      | 4.2667       | 4.2148   |
| R² Score | 0.8743       | 0.8804   |

### Performance Summary

* The model explains approximately **88% of the variance** in student math scores.
* Training and testing performance are highly consistent, indicating strong generalization.
* The low gap between training and testing metrics suggests minimal overfitting.
* Average prediction error remains within approximately **4–5 marks**.

### Model Selection

The following algorithms were evaluated during experimentation:

* Linear Regression ✅ (Selected)
* Decision Tree Regressor
* Random Forest Regressor
* Gradient Boosting Regressor
* AdaBoost Regressor
* CatBoost Regressor

Linear Regression achieved the most reliable performance on unseen test data and was selected as the final production model.

---

## 🛠️ How to Run Locally

1. Clone the repository:
   ```bash
   git clone https://github.com/vedant070206/Student_Exam_Performance_Predictor.git
   cd Student_Exam_Performance_Predictor
   ```
2. Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   .venv\Scripts\activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Run the Flask app:
   ```bash
   python application.py
   ```
5. Open the app:
   ```text
   http://localhost:5000
   ```

> If `artifacts/model.pkl` is missing, run the training pipeline from `src/components/data_ingestion.py` or implement a training script to create it.

---

## 🐳 Docker Setup

Build the Docker image:

```bash
docker build -t student-performance-predictor .
```

Run the container:

```bash
docker run -p 5000:5000 student-performance-predictor
```

Then open:

```text
http://localhost:5000
```

### Dockerfile summary
- Base image: `python:3.11-slim`
- Installs dependencies from `requirements.txt`
- Installs `awscli` for optional cloud integration
- Starts the Flask application with `python application.py`

---

## ☁️ AWS Deployment

### Elastic Beanstalk (recommended)
1. Configure AWS CLI:
   ```bash
   aws configure
   ```
2. Initialize Elastic Beanstalk:
   ```bash
   eb init -p docker student-performance-predictor
   ```
3. Create and deploy the environment:
   ```bash
   eb create student-performance-env
   eb deploy
   eb open
   ```

### ECS / ECR deployment
1. Push Docker image to ECR
2. Create an ECS service or Fargate task
3. Expose port `5000`

**AWS notes**
- Use IAM roles instead of plaintext credentials
- Store large model artifacts in S3 if needed
- Use Elastic Beanstalk for fast project demos and ECS for scalable production

---

## ☁️ Azure Deployment

### Azure App Service with Docker
1. Login to Azure CLI:
   ```bash
   az login
   ```
2. Create a resource group and App Service plan:
   ```bash
   az group create --name ml-project-rg --location eastus
   az appservice plan create --name ml-plan --resource-group ml-project-rg --sku B1 --is-linux
   ```
3. Create Azure Container Registry:
   ```bash
   az acr create --resource-group ml-project-rg --name mlprojectacr --sku Basic
   ```
4. Build and push the Docker image:
   ```bash
   az acr build --registry mlprojectacr --image student-performance-predictor:latest .
   ```
5. Deploy to App Service:
   ```bash
   az webapp create --resource-group ml-project-rg --plan ml-plan --name student-performance-app --deployment-container-image-name mlprojectacr.azurecr.io/student-performance-predictor:latest
   az webapp browse --name student-performance-app --resource-group ml-project-rg
   ```

**Azure notes**
- Use Azure Key Vault for secrets
- Configure App Service application settings for environment variables
- Monitor deployment health in Azure Portal

---


## 🚀 Future Enhancements

- Add API endpoints for batch scoring and analytics
- Add model explainability with SHAP or LIME
- Implement a dedicated training script with experiment tracking
- Add automated CI/CD for model training and deployment
- Improve frontend UX and add input validation

---

## 👨‍💻 Author

**Vedant Thakar**

📧 Email: thakar2006@gmail.com

💼 LinkedIn:
https://www.linkedin.com/in/vedant-thakar-4ba561292/

💻 GitHub:
https://github.com/vedant070206

---

