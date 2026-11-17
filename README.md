# lakehouse-analytics
Data Engineering bootcamp project: building a Dota 2 Lakehouse platform with Apache Spark, Apache Iceberg, PostgreSQL, MongoDB and analytical pipelines

# 1. Project Overview
This project focuses on building a data platform for Dota 2 match analytics. It uses a Lakehouse architecture to process structured match data and game events, transform raw data into analytical datasets, and support machine learning and statistical analysis.

# 2. Architecture
<img width="1548" height="770" alt="изображение" src="https://github.com/user-attachments/assets/d9055a12-cb41-4df6-8643-ae51fa4e943b" />
RAW - Unprocessed data from Postgres and MongoDB
STAGE- Preprocessed data containing key information for solving cases.Cleaning, normalization, deduplication, and preparation
MART - A view in which all data is processed with results for subsequent visualization

# 3. Tech Stack
Languages: Python, SQL
Processing: Apache Spark, PySpark
Storage: Apache Iceberg, MinIO
Databases: PostgreSQL, MongoDB
Query Engine: Trino
Orchestration: Apache Airflow
Analytics / ML: Pandas, NumPy, Scikit-learn, PCA/SVD

# 4. Project Cases

## Case 1
