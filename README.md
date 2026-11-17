# lakehouse-analytics
Data Engineering bootcamp project: building a Dota 2 Lakehouse platform with Apache Spark, Apache Iceberg, PostgreSQL, MongoDB and analytical pipelines

# 1. Project Overview
This project focuses on building a data platform for Dota 2 match analytics. It uses a Lakehouse architecture to process structured match data and game events, transform raw data into analytical datasets, and support machine learning and statistical analysis.

# 2. Architecture
<img width="1548" height="770" alt="изображение" src="https://github.com/user-attachments/assets/d9055a12-cb41-4df6-8643-ae51fa4e943b" />
RAW - Unprocessed data from Postgres and MongoDB.
STAGE- Preprocessed data containing key information for solving cases.Cleaning, normalization, deduplication, and preparation.
MART - A view in which all data is processed with results for subsequent visualization.

# 3. Tech Stack
### Languages: Python, SQL
### Processing: Apache Spark, PySpark
### Storage: Apache Iceberg, MinIO
### Databases: PostgreSQL, MongoDB
### Query Engine: Trino
### Orchestration: Apache Airflow
### Analytics / ML: Pandas, NumPy, Scikit-learn, PCA/SVD

# 4. Project Cases
## Business goal:
To optimize the workflow of Dota 2 specialists and highlight key aspects of the game.

## Case 1
First Tower

Examine the correlation between the timing of the first tower's destruction and the attacking side's probability of winning.
Identify the first tower to be destroyed, the side it belonged to, and the time at which the event occurred. Compare this early advantage with the final match outcome and subsequent map control.

## Case 2
Duration Prediction

Build a regression model to predict match duration based on early-game events.
Make predictions using only information available within the first 15 minutes. Compare a baseline model against your regression model across different time intervals.

## Case 3
Counter-picks

Analyze pairs of opposing heroes and their win rates.
Evaluate hero pairings while accounting for the side (Radiant/Dire), game patch, role, and sample size. Do not draw conclusions based on win rates without considering confidence intervals.

# File structure

dota2-data-engineering/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
│
├── docs/
│   ├── architecture.png
│   ├── data_model.png
│   └── project_overview.md
│
├── architecture/
│   ├── lakehouse_schema.md
│   └── data_flow.md
│
├── cases/
│   │
│   ├── first tower/
│   │   ├── README.md
│   │   ├── notebooks/
│   │   ├── sql/
│   │   └── src/
│   │
│   ├── match_duration_prediction/
│   │   ├── README.md
│   │   ├── notebooks/
│   │   ├── sql/
│   │   └── src/
│   │
│   └── hero_counter_picks/
│       ├── README.md
│       ├── notebooks/
│       ├── sql/
│       └── src/
│
├── dags/
│   └── README.md
│
├── tests/
│   └── README.md
│
└── requirements.txt
