# lakehouse-analytics

Проект Data Engineering Bootcamp: построение Lakehouse-платформы для аналитики матчей Dota 2 с использованием Apache Spark, Apache Iceberg, PostgreSQL, MongoDB и аналитических конвейеров.

# 1. Обзор проекта

Проект посвящён созданию платформы обработки данных для аналитики матчей Dota 2.

В основе проекта лежит архитектура Lakehouse, позволяющая обрабатывать структурированные данные о матчах и игровые события, преобразовывать исходные данные в аналитические наборы данных, а также использовать их для машинного обучения и статистического анализа.

# 2. Архитектура

<img width="1548" height="770" alt="изображение" src="https://github.com/user-attachments/assets/d9055a12-cb41-4df6-8643-ae51fa4e943b" />

- **RAW** — исходные данные из PostgreSQL и MongoDB без предварительной обработки.
- **STAGE** — предварительно обработанные данные, содержащие ключевую информацию для решения аналитических задач. Включает очистку, нормализацию, удаление дубликатов и подготовку данных.
- **MART** — слой витрин данных, в котором данные окончательно обработаны и подготовлены для последующей визуализации и аналитики.

# 3. Технологический стек

### Языки программирования
- Python
- SQL

### Обработка данных
- Apache Spark
- PySpark

### Хранение данных
- Apache Iceberg
- MinIO

### Базы данных
- PostgreSQL
- MongoDB

### Движок выполнения запросов
- Trino

### Оркестрация
- Apache Airflow

### Аналитика и машинное обучение
- Pandas
- NumPy
- Scikit-learn
- PCA / SVD

# 4. Аналитические задачи проекта

## Бизнес-цель

Оптимизировать рабочие процессы специалистов по Dota 2 и выявить ключевые закономерности игрового процесса на основе анализа данных.

## Кейс 1. Первая башня (First Tower)

Исследовать взаимосвязь между временем уничтожения первой башни и вероятностью победы атакующей стороны.

Необходимо:
- Определить первую уничтоженную башню.
- Установить, какой стороне принадлежала башня (Radiant или Dire).
- Определить время уничтожения башни.
- Сопоставить полученное раннее преимущество с итогом матча.
- Исследовать дальнейший контроль карты после уничтожения первой башни.

## Кейс 2. Предсказание продолжительности матча (Duration Prediction)

Разработать регрессионную модель для предсказания продолжительности матча на основе событий ранней стадии игры.

Предсказания должны строиться исключительно на информации, доступной в первые 15 минут матча.

Необходимо:
- Сформировать признаки на основе событий ранней стадии игры.
- Использовать только данные, доступные в пределах первых 15 минут.
- Построить базовую модель (Baseline).
- Разработать регрессионную модель.
- Сравнить качество предсказаний базовой и регрессионной моделей на различных временных интервалах.

## Кейс 3. Контр-пики героев (Counter-picks)

Проанализировать пары героев противоположных команд и оценить их показатели побед.

Необходимо:
- Проанализировать пары противостоящих героев.
- Рассчитать винрейт для различных сочетаний героев.
- Учитывать игровую сторону (Radiant / Dire).
- Учитывать версию игрового патча.
- Учитывать позиции и роли героев.
- Принимать во внимание размер выборки.
- Рассчитывать доверительные интервалы для оценки статистической неопределённости.

Не следует делать выводы исключительно на основе винрейта без учёта доверительных интервалов и объёма выборки.

# 5. Структура проекта

```text
dota2-data-engineering/
│
├── README.md
├── .env.example
│
├── architecture_for_use_cases/
│   ├── lakehouse_schema.md
│   ├── ER-model
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
```

# lakehouse-analytics
Data Engineering bootcamp project: building a Dota 2 Lakehouse platform with Apache Spark, Apache Iceberg, PostgreSQL, MongoDB and analytical pipelines

# 1. Project Overview
This project focuses on building a data platform for Dota 2 match analytics. It uses a Lakehouse architecture to process structured match data and game events, transform raw data into analytical datasets, and support machine learning and statistical analysis.

# 2. Architecture
<img width="1548" height="770" alt="изображение" src="https://github.com/user-attachments/assets/d9055a12-cb41-4df6-8643-ae51fa4e943b" />
RAW - Unprocessed data from Postgres and MongoDB.\
STAGE- Preprocessed data containing key information for solving cases.Cleaning, normalization, deduplication, and preparation.\
MART - A view in which all data is processed with results for subsequent visualization.\

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

dota2-data-engineering/\
│\
├── README.md\
├── .env.example\
│\
├── architecture_for_use_cases/\
│   ├── lakehouse_schema.md\
│   ├── ER-model\
│   └── data_flow.md\
│\
├── cases/\
│   │\
│   ├── first tower/\
│   │   ├── README.md\
│   │   ├── notebooks/\
│   │   ├── sql/\
│   │   └── src/\
│   │\
│   ├── match_duration_prediction/\
│   │   ├── README.md\
│   │   ├── notebooks/\
│   │   ├── sql/\
│   │   └── src/\
│   │\
│   └── hero_counter_picks/\
│       ├── README.md\
│       ├── notebooks/\
│       ├── sql/\
│       └── src/\
