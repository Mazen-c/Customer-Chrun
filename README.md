# Customer Churn Prediction

A comprehensive machine learning project for predicting customer churn in the telecom industry using the Telco Customer Churn dataset. This project includes data preprocessing, feature engineering, model training, evaluation, and deployment via a FastAPI web service.

## Features

- **Data Preprocessing**: Automated data cleaning and preprocessing pipeline
- **Feature Engineering**: Custom feature building for improved model performance
- **Model Training**: XGBoost classifier with hyperparameter tuning
- **Model Evaluation**: Comprehensive metrics including accuracy, precision, recall, F1-score, and ROC-AUC
- **Experiment Tracking**: MLflow integration for logging experiments and models
- **Web API**: FastAPI-based REST API for real-time predictions
- **Docker Support**: Containerized deployment for easy setup and scalability
- **Interactive UI**: Gradio interface for easy model interaction

## Project Structure

```
├── Data/
│   ├── WA_Fn-UseC_-Telco-Customer-Churn.csv    # Raw dataset
│   └── processed/
│       └── telco_churn_processed.csv           # Preprocessed data
├── Scripts/
│   ├── prepare_processed_data.py               # Data preparation script
│   ├── run_pipeline.py                         # Main pipeline runner
│   ├── test_fastapi.py                         # API testing script
│   ├── test_pipeline_phase1_data_features.py   # Data/features testing
│   └── test_pipeline_phase2_modeling.py        # Modeling testing
├── src/
│   ├── app/
│   │   ├── app.py                              # FastAPI application setup
│   │   └── main.py                             # Main API endpoints
│   ├── data/
│   │   ├── load_data.py                        # Data loading utilities
│   │   └── preprocess.py                       # Data preprocessing
│   ├── features/
│   │   └── build_features.py                   # Feature engineering
│   ├── models/
│   │   ├── evaluate.py                         # Model evaluation
│   │   ├── train.py                            # Model training
│   │   └── tune.py                             # Hyperparameter tuning
│   ├── serving/
│   │   ├── inference.py                        # Model inference
│   │   └── models/                             # Saved models
│   └── utils/
│       ├── utlis.py                            # Utility functions
│       └── validate_data.py                    # Data validation
├── artifacts/
│   └── feature_columns.json                    # Feature metadata
├── mlruns/                                     # MLflow experiment logs
├── dockerfile                                  # Docker configuration
├── requirements.txt                            # Python dependencies
└── README.md                                   # This file
```

## Installation

### Prerequisites

- Python 3.11+
- Docker (optional, for containerized deployment)

### Local Setup

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd customer-churn
   ```

2. Create a virtual environment:
   ```bash
   python -m venv .venv
   .venv\Scripts\activate  # On Windows
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Docker Setup

1. Build the Docker image:
   ```bash
   docker build -t customer-churn .
   ```

2. Run the container:
   ```bash
   docker run -p 8000:8000 customer-churn
   ```

## Usage

### Running the Full Pipeline

Execute the complete data processing and model training pipeline:

```bash
python Scripts/run_pipeline.py
```

### Starting the API Server

Launch the FastAPI server for real-time predictions:

```bash
python -m uvicorn src.app.main:app --host 0.0.0.0 --port 8000
```

The API will be available at `http://localhost:8000`

### API Endpoints

- `GET /`: Health check endpoint
- `POST /predict`: Make predictions on customer data
- `GET /docs`: Interactive API documentation (Swagger UI)

### Example API Usage

```python
import requests

# Example customer data
customer_data = {
    "gender": "Female",
    "SeniorCitizen": 0,
    "Partner": "Yes",
    "Dependents": "No",
    "tenure": 1,
    "PhoneService": "No",
    "MultipleLines": "No phone service",
    "InternetService": "DSL",
    "OnlineSecurity": "No",
    "OnlineBackup": "Yes",
    "DeviceProtection": "No",
    "TechSupport": "No",
    "StreamingTV": "No",
    "StreamingMovies": "No",
    "Contract": "Month-to-month",
    "PaperlessBilling": "Yes",
    "PaymentMethod": "Electronic check",
    "MonthlyCharges": 29.85,
    "TotalCharges": 29.85
}

response = requests.post("http://localhost:8000/predict", json=customer_data)
print(response.json())
```

## Data

This project uses the [Telco Customer Churn dataset](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) from Kaggle. The dataset contains information about telecom customers and their churn status.

### Key Features

- Customer demographics (gender, senior citizen, partner, dependents)
- Service subscriptions (phone, internet, security, etc.)
- Account information (tenure, contract type, billing)
- Churn status (target variable)

## Model Details

### Algorithm

- **XGBoost Classifier**: Gradient boosting framework optimized for speed and performance
- **Hyperparameter Tuning**: Grid search with cross-validation for optimal parameters

### Performance Metrics

The model achieves the following metrics on the test set:
- Accuracy: ~80%
- Precision: ~65%
- Recall: ~55%
- F1-Score: ~60%
- ROC-AUC: ~85%

*Note: Actual metrics may vary based on random seed and data splits.*

### Feature Importance

Top features contributing to churn prediction:
1. Contract type (Month-to-month vs. long-term)
2. Tenure (length of customer relationship)
3. Monthly charges
4. Internet service type
5. Payment method

## Experiment Tracking

All experiments are logged using MLflow, including:
- Model parameters
- Performance metrics
- Model artifacts
- Training time and prediction time

View experiments:
```bash
mlflow ui
```

## Testing

Run the test suite to validate the pipeline:

```bash
# Test data and features pipeline
python Scripts/test_pipeline_phase1_data_features.py

# Test modeling pipeline
python Scripts/test_pipeline_phase2_modeling.py

# Test FastAPI endpoints
python Scripts/test_fastapi.py
```

## Acknowledgments

- Dataset provided by IBM Watson Analytics
- Built with scikit-learn, XGBoost, FastAPI, and MLflow
- Inspired by telecom industry churn analysis best practices