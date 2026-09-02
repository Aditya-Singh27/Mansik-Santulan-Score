# Mansik Santulan — Student Mental Health Score Predictor

[![Python](https://img.shields.io/badge/Python-3.10%20%7C%203.11%20%7C%203.12-blue?logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.111+-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Scikit-Learn](https://img.shields.io/badge/scikit--learn-1.3+-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end Machine Learning web application designed to analyze and predict student mental health wellness scores (on a scale of 0 to 10) based on daily digital habits, academic demands, physical activity, sleep patterns, and self-reported stress levels.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Project Architecture](#-project-architecture)
- [Repository Structure](#-repository-structure)
- [Dataset & Features](#-dataset--features)
- [Installation & Setup](#-installation--setup)
- [Running the Application](#-running-the-application)
- [API Documentation](#-api-documentation)
- [Machine Learning Pipeline](#-machine-learning-pipeline)
- [Score Interpretation](#-score-interpretation)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🌟 Overview

Students face unique stressors balancing academics, social connectivity, screen time, and self-care. **Mansik Santulan** evaluates lifestyle indicators to estimate a predictive mental wellness score. 

The project encompasses:
1. **Data Preprocessing & Exploratory Analysis**: Handling skewness, encoding ordinal/nominal categories, and feature scaling.
2. **Model Training**: A `RandomForestRegressor` ensemble packaged inside a Scikit-Learn `Pipeline`.
3. **Backend API**: A high-performance **FastAPI** service with schema validation using **Pydantic**.
4. **Frontend UI**: A modern, interactive web dashboard with real-time validation and dynamic SVG gauge score visualization.

---

## ✨ Key Features

- **End-to-End Scikit-Learn Pipeline**: Integrated data transformation pipeline handling numeric scaling, log transforms for skewed features, ordinal encoding, and one-hot encoding.
- **FastAPI REST API**: Asynchronous, lightweight backend delivering low-latency inference with full CORS support.
- **Type-Safe Validation**: Pydantic models enforcing range and categorical constraints on incoming requests.
- **Interactive UI**:
  - Live SVG arc gauge displaying predicted wellness scores.
  - Qualitative wellness band categorization (e.g., *Optimal Balance*, *Moderate Strain*, *High Risk*).
  - Responsive layout with dark glassmorphism styling and seamless client-side validation.
- **Auto-Generated API Documentation**: Swagger UI (`/docs`) and ReDoc (`/redoc`) available out of the box.

---

## 🏗 Project Architecture

```mermaid
flowchart LR
    User["Student / User"] -->|Inputs Lifestyle Data| WebUI["Web Frontend\n(HTML5 / CSS3 / JS)"]
    WebUI -->|POST /predict (JSON)| API["FastAPI Backend\n(main.py)"]
    API -->|Validates Input| Schema["Pydantic Schema\n(StudentData)"]
    Schema -->|Pandas DataFrame| Pipeline["Scikit-Learn Pipeline\n(Mental_Health_Model.pkl)"]
    Pipeline -->|Preprocess & Inference| RF["Random Forest Regressor"]
    RF -->|Predicted Score (0-10)| API
    API -->|JSON Response| WebUI
    WebUI -->|Updates Gauge & Insights| User
```

---

## 📂 Repository Structure

```text
Mental-Health-Score/
├── Mental_Health_Model.pkl                        # Serialized Scikit-Learn ML pipeline
├── Student Social Media And Mental Health Impact.csv # Dataset used for training and evaluation
├── index.html                                     # Frontend interface
├── style.css                                      # UI styling and design system
├── script.js                                      # Client-side form handling and SVG gauge rendering
├── main.py                                        # FastAPI backend server with /predict endpoint
├── preprocessing.ipynb                            # Data preprocessing and model experimentation notebook
├── ML_Project.ipynb                               # End-to-end ML training notebook
├── requirements.txt                               # Project dependencies
└── README.md                                      # Project documentation
```

---

## 📊 Dataset & Features

The model is trained on student demographic, behavioral, and psychological data:

| Feature Name | Type | Allowed Values / Range | Description |
| :--- | :--- | :--- | :--- |
| `Age` | Integer | `10 – 100` | Age of the student |
| `Gender` | Categorical | `Male`, `Female` | Self-identified gender |
| `Country` | Categorical | Top 10 countries / `Other` | Country of residence |
| `Academic_Level` | Categorical | `High School`, `Undergraduate`, `Graduate` | Current level of study |
| `Most_Used_Platform` | Categorical | `Instagram`, `YouTube`, `Snapchat`, `Facebook`, etc. | Primary social media platform |
| `Purpose_Of_Use` | Categorical | `Entertainment`, `Education`, `Networking`, `News` | Primary reason for social media usage |
| `Avg_Daily_Usage_Hours` | Float | `0.0 – 24.0` | Average daily social media screen time (hours) |
| `Daily_Unlocks` | Integer | `≥ 0` | Daily smartphone unlock count |
| `Study_Hours` | Float | `0.0 – 24.0` | Average daily study duration (hours) |
| `Physical_Activity_Hours` | Float | `0.0 – 24.0` | Daily physical exercise / sports (hours) |
| `Sleep_Hours_Per_Night` | Float | `0.0 – 24.0` | Nightly sleep duration (hours) |
| `Stress_Level` | Ordinal | `Low`, `Medium`, `High`, `Very High` | Self-reported stress level |
| **`Mental_Health_Score`** | **Float** | **`0.0 – 10.0`** | **Target Output (Wellness Score)** |

---

## ⚙️ Installation & Setup

### Prerequisites

- **Python 3.10+** (Tested on Python 3.10, 3.11, and 3.12)
- **pip** and `venv` (standard Python package manager)

### 1. Clone the Repository

```bash
git clone https://github.com/Aditya-Singh27/Mansik-Santulan-Score.git
cd Mansik-Santulan-Score
```

### 2. Create a Virtual Environment

- **On Windows (PowerShell):**
  ```powershell
  python -m venv venv
  .\venv\Scripts\Activate.ps1
  ```

- **On macOS / Linux:**
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 🚀 Running the Application

### 1. Start the FastAPI Backend

Run the server with Uvicorn:

```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

The API will be available at `http://127.0.0.1:8000`.

### 2. Launch the Frontend

You can open `index.html` directly in any modern browser:

- Double-click [index.html](file:///d:/Mental-Health-Score/index.html) in your file explorer, **or**
- Use Python's built-in HTTP server:
  ```bash
  python -m http.server 3000
  ```
  Then visit `http://localhost:3000` in your web browser.

---

## 🔌 API Documentation

### Interactive Swagger Docs

Once the backend is running, explore and test the endpoints interactively:
- **Swagger UI:** [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- **ReDoc:** [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---

### Endpoints

#### 1. Root Greeting
- **Method:** `GET /`
- **Response:**
  ```json
  ["Welcome to Mental Health Score Predictor"]
  ```

#### 2. Predict Mental Health Score
- **Method:** `POST /predict`
- **Headers:** `Content-Type: application/json`

**Sample Request Body:**
```json
{
  "age": 21,
  "gender": "Female",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Entertainment",
  "avg_daily_usage_hours": 4.5,
  "daily_unlocks": 120,
  "study_hours": 5.0,
  "physical_activity_hours": 1.5,
  "sleep_hours_per_night": 7.5,
  "stress_level": "Medium"
}
```

**Sample Response Body:**
```json
{
  "predicted_mental_health_score": 7.24
}
```

---

## 🤖 Machine Learning Pipeline

The pipeline is packaged into `Mental_Health_Model.pkl` using Scikit-Learn's `ColumnTransformer` and `Pipeline`:

```text
Raw Student Data
 │
 ├── [Study_Hours] ───────────────> Log1p Transform ──> StandardScaler
 ├── [Age, Usage, Unlocks, etc.] ─> StandardScaler
 ├── [Stress_Level] ──────────────> OrdinalEncoder (['Low', 'Medium', 'High', 'Very High'])
 └── [Gender, Country, Platform] ─> OneHotEncoder (handle_unknown='ignore')
 │
 └──> Combined Features ──────────> RandomForestRegressor (random_state=42)
                                     └──> Predicted Score (0.00 – 10.00)
```

---

## 🎯 Score Interpretation

| Score Range | Classification | Indicator |
| :--- | :--- | :--- |
| **8.0 – 10.0** | 🟢 **Optimal Balance** | Habits, sleep, and activity strongly support positive mental well-being. |
| **6.0 – 7.9** | 🟡 **Moderate Balance** | Stable overall lifestyle, with minor areas for routine optimization. |
| **4.0 – 5.9** | 🟠 **Elevated Strain** | High screen time, low sleep, or stress may be impacting well-being. |
| **0.0 – 3.9** | 🔴 **High Vulnerability** | Multiple compounding risk factors present (sleep deficit, high stress, low activity). |

---

## ⚠️ Disclaimer

> [!IMPORTANT]
> **Mansik Santulan** is an academic machine learning demonstration and predictive analytics tool. **It does not provide medical diagnoses or clinical evaluations.** If you or someone you know is experiencing mental health distress, please consult a qualified healthcare professional or contact a local support helpline.

---

## 📜 License

This project is licensed under the [MIT License](LICENSE). Feel free to use, modify, and distribute it for educational and personal projects.
