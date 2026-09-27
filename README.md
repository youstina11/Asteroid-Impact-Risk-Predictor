## ☄️ Asteroid Hazard Prediction Using Machine Learning

An end-to-end Machine Learning system for identifying Potentially Hazardous Asteroids (PHAs) from astronomical and orbital features.

⸻

🌌 Project Overview

Asteroids constantly move through our Solar System, and some of them are classified as Potentially Hazardous Asteroids (PHAs) based on their orbital characteristics and proximity to Earth.

This project uses Machine Learning to analyze asteroid properties and predict whether an asteroid is classified as:

* 🟢 Non-PHA
* 🔴 PHA (Potentially Hazardous Asteroid)

The project goes beyond simply training a classifier. It includes data quality analysis, scientific validation, missing-value reconstruction, feature engineering, feature selection, class-imbalance handling, model optimization, threshold tuning, model persistence, and Streamlit deployment.

⚠️ Important: This model predicts the dataset’s PHA classification. It is not an actual Earth-impact probability predictor.

⸻
## Dataset 

Dataset Link : [Asteroid Dataset] (https://www.kaggle.com/datasets/shamimhasan8/asteroid-dataset?utm_source=chatgpt.com#)
Certificated Source : [NASA] (https://ssd.jpl.nasa.gov/tools/sbdb_query.html)

🎯 Objectives

The main objectives of this project are to:

* Analyze asteroid and orbital data.
* Identify data-quality issues and missing values.
* Reconstruct missing asteroid measurements using Machine Learning and scientific relationships.
* Engineer meaningful astronomical features.
* Handle class imbalance.
* Compare multiple classification algorithms.
* Optimize classification performance.
* Tune the probability threshold for PHA detection.
* Save the complete ML pipeline.
* Deploy the final model as an interactive web application.

⸻

🧠 Machine Learning Pipeline

Raw Asteroid Dataset
        │
        ▼
Data Quality Audit
        │
        ├── Missing Values
        ├── Duplicates
        ├── Outliers
        └── Invalid Scientific Values
        │
        ▼
Data Cleaning
        │
        ▼
Missing Diameter Reconstruction
        │
        ▼
Scientific Albedo Reconstruction
        │
        ▼
Train / Test Split
        │
        ▼
Feature Engineering
        │
        ▼
Feature Selection
        │
        ▼
Class Imbalance Handling
        │
        ▼
Model Training & Hyperparameter Tuning
        │
        ├── XGBoost
        ├── Logistic Regression
        └── Random Forest
        │
        ▼
Threshold Tuning
        │
        ▼
Final Model
        │
        ▼
Saved ML Artifacts
        │
        ▼
Streamlit Web Application

⸻

📊 Dataset

The project uses an asteroid dataset containing astronomical and orbital characteristics.

Main Information

Category	Examples
Physical Properties	Diameter, Albedo
Orbital Properties	Semi-major axis, Eccentricity, Inclination
Distance Features	Perihelion, Aphelion
Earth Proximity	MOID, MOID in Lunar Distance
Classification	PHA
Other Information	NEO and related asteroid attributes

Target

PHA
├── Y → Potentially Hazardous Asteroid
└── N → Non-Potentially Hazardous Asteroid

⸻

🔍 Exploratory Data Analysis

The dataset was investigated from several perspectives:

Data Quality

* Data types
* Missing values
* Missing-value percentages
* Duplicate records
* Unique values
* Invalid scientific values

Distribution Analysis

The project analyzes distributions of:

* Asteroid diameter
* Absolute magnitude H
* Albedo
* Eccentricity
* Inclination
* Semi-major axis
* Perihelion distance
* Aphelion distance
* MOID

Correlation Analysis

A correlation matrix was used to investigate relationships between numerical asteroid and orbital features.

⸻

🧪 Scientific Data Validation

Before training the models, the dataset was checked for physically invalid values.

Examples include:

* Negative diameter
* Negative albedo
* Negative eccentricity
* Non-positive semi-major axis
* Non-positive perihelion distance

This step helps prevent unrealistic measurements from affecting the ML pipeline.

⸻

🛠️ Missing Value Reconstruction

One of the main challenges in the dataset was missing asteroid measurements.

Instead of simply dropping all incomplete rows, the project uses a more advanced approach.

1️⃣ Diameter Prediction Model

A separate XGBoost Regression model is trained to estimate missing asteroid diameter values.

Known Diameter
      │
      ▼
Train Regression Model
      │
      ▼
Learn Relationships
      │
      ▼
Predict Missing Diameter

The predicted diameter is then used in the main pipeline.

⸻

2️⃣ Albedo Reconstruction

Missing albedo values are reconstructed using the relationship between:

* Absolute magnitude H
* Diameter
* Albedo

This combines domain knowledge with Machine Learning instead of relying only on statistical imputation.

⸻

⚙️ Feature Engineering

The project creates additional features designed to represent useful relationships between asteroid characteristics.

Examples include:

Orbital Features

* Orbital span: ad - q
* Trigonometric representation of inclination:
    * sin(i)
    * cos(i)

Physical Features

* Diameter³ as a volume proxy
* Diameter / Albedo relationships
* Relative uncertainty features

Interaction Features

Examples:

Diameter × Albedo
Eccentricity × Inclination

Log Transformations

Log transformations are also applied to selected highly-skewed numerical variables.

⸻

🎯 Feature Selection

An Extra Trees Classifier is used to estimate feature importance.

Features with very low importance are removed from the final training dataset.

All Features
      │
      ▼
Extra Trees Feature Importance
      │
      ▼
Remove Low-Importance Features
      │
      ▼
Selected Features

This helps reduce unnecessary information and creates a more focused model.

⸻

⚖️ Class Imbalance

PHA classification is an imbalanced classification problem.

Therefore, the project calculates the class imbalance ratio and uses techniques such as:

* Class weighting
* scale_pos_weight
* SMOTE where appropriate
* Stratified splitting

The goal is to prevent the model from simply favoring the majority class.

⸻

🤖 Models

Three main classification approaches were investigated:

1. XGBoost

A gradient-boosting model suitable for nonlinear relationships and structured/tabular data.

2. Logistic Regression

Used as a simpler and more interpretable baseline model.

3. Random Forest

An ensemble of decision trees capable of modeling nonlinear relationships and interactions between features.

⸻

🎚️ Probability Threshold Tuning

Instead of automatically using the standard:

Threshold = 0.50

the project evaluates different probability thresholds.

The final deployed configuration uses:

FINAL_THRESHOLD = 0.30

Therefore:

P(PHA) >= 0.30  →  PHA
P(PHA) <  0.30  →  Non-PHA

Why?

For a potentially hazardous asteroid classification task, missing a positive case (False Negative) can be particularly important.

Lowering the threshold makes the classifier more sensitive to potential PHA cases, while creating a trade-off with False Positives.

⸻

🏆 Final Model

The deployed notebook configuration uses:

FINAL_MODEL = best_rf_model
FINAL_THRESHOLD = 0.30

So the current deployed classifier is based on:

Random Forest + optimized probability threshold

The threshold is treated separately from the model because changing the decision threshold changes the balance between precision and recall.

⸻

📈 Evaluation Metrics

The project evaluates classification performance using metrics such as:

* Accuracy
* Precision
* Recall
* F1-score
* ROC-AUC
* Confusion Matrix

For an imbalanced PHA classification problem, Recall, Precision, F1-score and ROC-AUC provide more useful information than accuracy alone.

⸻

💾 Model Persistence

The trained components are saved using joblib, including:

Final Model
Threshold
Encoder
Categorical Columns
Selected Features
Diameter Regression Model

This allows the exact trained pipeline to be reused during deployment without retraining the models.

⸻

🌐 Deployment

The final ML pipeline is deployed using Streamlit.

Application Workflow

User Input
    │
    ▼
Input Validation
    │
    ▼
Missing Diameter Prediction
    │
    ▼
Albedo Reconstruction
    │
    ▼
Feature Engineering
    │
    ▼
Selected Features
    │
    ▼
Random Forest
    │
    ▼
Probability
    │
    ▼
Threshold = 0.30
    │
    ├── PHA
    └── Non-PHA

⸻

🖥️ Application

The Streamlit application allows users to provide asteroid characteristics and receive a classification.

Example Output

Asteroid Classification
────────────────────────
PHA Probability: 0.64
Prediction:
🔴 Potentially Hazardous Asteroid

⸻

🧰 Technologies Used

Programming

* Python

Data Analysis

* Pandas
* NumPy

Visualization

* Matplotlib
* Seaborn

Machine Learning

* Scikit-learn
* XGBoost
* Imbalanced-learn

Model Persistence

* Joblib

Deployment

* Streamlit
* Pyngrok

⸻

📁 Project Structure

Asteroid-Hazard-Prediction/
│
├── 📓 Final_Project_Organized_&_Deployed.ipynb
│
├── 📄 app.py
│
├── 📦 model.pkl
├── 📦 threshold.pkl
├── 📦 encoder.pkl
├── 📦 diameter_model.pkl
│
├── 📄 requirements.txt
│
└── 📄 README.md

File names may vary depending on the final repository structure.

⸻

🚀 How to Run

1. Clone the Repository

git clone YOUR_REPOSITORY_URL
cd Asteroid-Hazard-Prediction

2. Install Dependencies

pip install -r requirements.txt

3. Run the Streamlit Application

streamlit run app.py

The application will then open in your browser.

⸻

📦 Main Dependencies

pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
imbalanced-learn
joblib
streamlit
pyngrok

⸻

🔬 Key Machine Learning Concepts Demonstrated

This project demonstrates practical understanding of:

* Exploratory Data Analysis
* Data Cleaning
* Missing Data Handling
* Regression
* Classification
* Feature Engineering
* Feature Selection
* Ensemble Learning
* XGBoost
* Random Forest
* Logistic Regression
* Class Imbalance
* SMOTE
* Hyperparameter Tuning
* Cross-Validation
* ROC-AUC
* Precision & Recall
* Threshold Optimization
* Model Serialization
* ML Deployment

⸻

⚠️ Limitations

This project has several important limitations:

1. The model predicts the dataset’s PHA classification, not actual asteroid impact probability.
2. ML predictions depend on the quality and representativeness of the training dataset.
3. A probability generated by the classifier should not be interpreted as a physical probability of an asteroid impacting Earth.
4. Threshold selection involves a precision-recall trade-off.
5. Scientific asteroid-risk assessment requires validated astronomical models and official observational data.

⸻

🔮 Future Improvements

Possible extensions include:

* Integrating official NASA/JPL APIs.
* Using larger and more recent asteroid datasets.
* Calibrating model probabilities.
* Performing systematic error analysis.
* Comparing additional boosting algorithms.
* Adding explainable AI using SHAP.
* Creating interactive orbital visualizations.
* Monitoring model performance as new asteroid observations become available.
* Building a more comprehensive asteroid-risk scoring system based on scientifically validated criteria.

⸻

👩‍💻 Author

Youstina Salah Nathan

Computer Science | Artificial Intelligence & Machine Learning

Areas of Interest

* Artificial Intelligence
* Machine Learning
* Data Science
* Data Analytics
* AI Applications

⸻

⭐ Project Highlights

From raw astronomical data → scientific validation → ML-powered prediction → optimized classification → deployed application.

This project demonstrates an end-to-end approach to building a practical Machine Learning system rather than focusing only on model training.
