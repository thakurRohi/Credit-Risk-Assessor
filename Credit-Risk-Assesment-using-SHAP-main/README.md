# Credit Risk Assessment using SHAP

A full-stack credit risk prediction application that evaluates whether a loan applicant is likely to default, using a trained machine learning model and a simple web interface. The project combines a Python backend, a browser-based front end, and an interactive notebook for model exploration and explainability.

This repository demonstrates how to:

- prepare and analyze loan application data
- train a credit-risk classifier
- save a model and decision threshold for reuse
- expose predictions through a FastAPI service
- build a lightweight UI for real-time assessment
- use SHAP to interpret feature contribution and explain model behavior

---

## Project Overview

The application predicts the probability that a borrower will default on a loan. It accepts a set of applicant and loan attributes from the user, sends them to a trained model, and returns:

- the predicted default probability
- a binary classification result
- the threshold used for the decision
- a human-readable risk label

The app is designed for practical demonstration and deployment, with a small static frontend and a REST API backend that can be hosted in a cloud environment such as Render.

---

## Why This Project Matters

Credit risk assessment is one of the most common and impactful use cases in financial analytics. Loan decisions require a balance between:

- financial safety
- customer experience
- risk transparency
- regulatory and business explainability

This project provides a compact example of how machine learning can support underwriting decisions while helping users understand the model's reasoning through SHAP-based explanations.

---

## Key Features

- Credit risk prediction using a trained classifier
- FastAPI backend for serving predictions
- Static HTML/CSS/JavaScript frontend
- Input validation with Pydantic models
- Real-time API interaction from the browser
- Model threshold tuning for risk classification
- SHAP visual analysis in the notebook for explainability
- Render deployment configuration for cloud hosting

---

## Tech Stack

### Backend
- Python 3.11
- FastAPI
- Pydantic
- Pandas
- scikit-learn
- XGBoost
- joblib
- Uvicorn

### Frontend
- HTML
- CSS
- JavaScript
- Browser-based fetch requests to the API

### Machine Learning / Explainability
- XGBoost classifier
- SHAP for feature importance and model explanation
- Jupyter Notebook for exploratory analysis and model training workflow

---

## Project Structure

```text
Credit-Risk-Assesment-using-SHAP-main/
├── README.md
├── Credit_Risk.ipynb
├── credit_risk_dataset.csv
├── credit_risk_model.pkl
├── best_threshold.pkl
├── main.py
├── requirements.txt
├── runtime.txt
├── render.yaml
├── static/
│   ├── index.html
│   ├── script.js
│   └── style.css
├── __pycache__/
└── .gitignore
```

### File Descriptions

- `Credit_Risk.ipynb` — notebook used for data exploration, training, validation, and SHAP interpretation.
- `credit_risk_dataset.csv` — sample dataset containing borrower and loan information.
- `credit_risk_model.pkl` — serialized trained model used in the API.
- `best_threshold.pkl` — saved threshold used to convert probability to a decision.
- `main.py` — FastAPI application and prediction logic.
- `static/` — frontend files for the user-facing form and risk visualization.
- `requirements.txt` — Python dependencies required to run the project.
- `render.yaml` — deployment configuration for Render.
- `runtime.txt` — Python runtime version for hosting.

---

## Dataset and Problem Type

This project addresses a binary classification problem: whether a borrower will default.

The input fields include applicant and loan characteristics such as:

- age
- annual income
- home ownership status
- employment length
- loan intent
- loan grade
- loan amount
- interest rate
- loan-to-income ratio
- prior default history
- credit history length

These features are used by the model to estimate the probability of default.

---

## Machine Learning Workflow

The notebook in this project follows a typical ML lifecycle:

1. Load the dataset
2. Explore the dataset and inspect distributions
3. Check for missing values, inconsistencies, and outliers
4. Prepare features for modeling
5. Train an XGBoost classifier
6. Evaluate model performance
7. Tune or inspect the decision threshold
8. Save the model and threshold for deployment
9. Use SHAP to explain feature impact and interpret predictions

The SHAP section is especially useful because it answers two important questions:

- Which features most strongly influence the model's outputs?
- Why did the model classify a specific borrower as high or low risk?

---

## SHAP Explainability

SHAP (SHapley Additive exPlanations) is used to explain the model's output in a transparent way. This is useful in financial applications because it helps users and stakeholders understand:

- what drives a loan decision
- which risk factors matter most globally
- which factors contributed to an individual application being labeled high risk

The notebook includes SHAP-based global and local interpretability workflows, which are particularly valuable for model trust and auditing.

---

## Application Behavior

The system has two main layers:

### 1. Backend API
The FastAPI service exposes a POST endpoint at `/predict`.

It accepts a JSON payload matching a loan application schema and returns:

```json
{
  "default_probability": 0.412,
  "default_prediction": 0,
  "threshold": 0.5,
  "Result": "Low Risk"
}
```

Where:

- `default_probability` is the model's risk score in the range [0, 1]
- `default_prediction` is 1 for high risk and 0 for low risk
- `threshold` is the cutoff used to decide the label
- `Result` gives a readable risk outcome

### 2. Frontend UI
The frontend `static/index.html` and `static/script.js` present a loan application form. Users enter data, click the submit button, and receive a visual risk assessment showing:

- probability percentage
- gauge visualization
- high/low risk badge
- threshold value
- result summary

---

## API Contract

### Request Body

The API expects the following fields:

```json
{
  "person_age": 30,
  "person_income": 600000,
  "person_home_ownership": "RENT",
  "person_emp_length": 5.0,
  "loan_intent": "PERSONAL",
  "loan_grade": "B",
  "loan_amnt": 120000,
  "loan_int_rate": 11.5,
  "loan_percent_income": 0.2,
  "cb_person_default_on_file": "N",
  "cb_person_cred_hist_length": 7
}
```

### Response

```json
{
  "default_probability": 0.35,
  "default_prediction": 0,
  "threshold": 0.5,
  "Result": "Low Risk"
}
```

---

## Local Setup

### Prerequisites

Make sure you have:

- Python 3.11 installed
- pip package manager
- a terminal or command prompt

### 1. Clone or open the project

```bash
cd Credit-Risk-Assesment-using-SHAP-main
```

### 2. Create a virtual environment

On Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

On macOS/Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the application

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

Then open the browser at:

```text
http://localhost:8000
```

The frontend is served by the FastAPI app and the static files are mounted directly from the `static` directory.

---

## Deployment Guide (Render)

This project includes a `render.yaml` file for deployment to Render.

### Render configuration

The deployment file defines a Python web service with:

- build command: `pip install -r requirements.txt`
- start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- Python runtime: configured via `runtime.txt`

### Deploy steps

1. Push this repository to GitHub.
2. Log in to Render.
3. Create a new Web Service.
4. Connect the repository.
5. Let Render read `render.yaml`.
6. Deploy the application.

Once deployed, Render will serve the same FastAPI app and frontend that work locally.

---

## Running the Model Notebook

The notebook file `Credit_Risk.ipynb` is designed for interactive exploration and model development. It is useful if you want to:

- inspect the dataset
- validate feature distributions
- train a custom model
- compare model performance
- visualize SHAP explanations

You can open the notebook in Jupyter or VS Code with notebook support and run the cells in order.

---

## Production Notes and Limitations

This project is a demonstration application and is not a production-grade lending system. Some important considerations:

- the model should be validated with domain-specific business rules
- fairness and bias assessment should be performed before use in real lending decisions
- regulatory requirements and compliance checks may be required
- the model artifact should be retrained and validated periodically
- data quality and feature drift should be monitored in production

This app is best understood as a prototype or educational implementation.

---

## Troubleshooting

### App does not start

Check whether:

- Python dependencies were installed successfully
- the model files exist in the root project directory
- the app is being started from the correct folder

### Frontend cannot reach backend

Ensure:

- the API service is running
- the frontend is being opened through the same origin or the correct local URL
- no port mismatch exists between the UI and backend

### Dependency issues

Try reinstalling the project requirements:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

---

## Summary

This project brings together machine learning, web development, and explainability into a single practical example. It shows how a trained credit risk model can be wrapped in a user-friendly interface and served as a real-time application, while also exposing the model's decision logic through SHAP analysis.

It is a useful project for learning how models move from notebook experimentation to deployed application workflows.

---

## License

This project is intended for educational and demonstration purposes. If you plan to use it for production or commercial deployment, review licensing and compliance requirements before using the model in a real lending environment.
