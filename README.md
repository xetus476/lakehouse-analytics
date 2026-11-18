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

## Начало работы (Getting Started)

### 1. Предварительные требования (Prerequisites)

Перед запуском проекта убедитесь, что установлены следующие инструменты:

| Инструмент | Назначение |
|---|---|
| Python 3.10+ | Основной язык программирования |
| Git | Система контроля версий |
| Docker & Docker Compose | Запуск инфраструктурных сервисов (при наличии соответствующей конфигурации) |
| Apache Spark | Распределённая обработка данных |
| PostgreSQL | Реляционный источник данных |
| MongoDB | NoSQL-источник данных |
| MinIO | Объектное хранилище с поддержкой S3 |
| Trino | SQL-движок для выполнения запросов |
| Apache Iceberg | Открытый формат табличного хранения данных |
| Apache Airflow | Оркестрация рабочих процессов |

> **Примечание:** Некоторые сервисы могут быть необязательными в зависимости от используемого конвейера обработки данных или аналитического кейса. Конкретные требования указаны в README соответствующего кейса.

### 2. Клонирование репозитория

```bash
git clone https://github.com/YOUR_USERNAME/dota2-data-engineering.git

cd dota2-data-engineering
```

### 3. Создание виртуального окружения

Создайте и активируйте виртуальное окружение Python.

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Linux / macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Установка зависимостей

Установите необходимые Python-библиотеки:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 5. Настройка переменных окружения

Создайте локальный файл с переменными окружения.

**Windows:**

```powershell
Copy-Item .env.example .env
```

**Linux / macOS:**

```bash
cp .env.example .env
```

Откройте файл `.env` и укажите параметры подключения к локальным базам данных, настройки хранилища и конфигурацию необходимых сервисов.

**Важно:** Никогда не добавляйте файл `.env` в Git и не публикуйте пароли или ключи доступа.

### 6. Запуск инфраструктурных сервисов

Если в проекте предусмотрена конфигурация Docker Compose, запустите необходимые сервисы:

```bash
docker compose up -d
```

Проверьте состояние запущенных контейнеров:

```bash
docker compose ps
```
После запуска перейдите к ноутбуку выбранного аналитического кейса.

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
│   │   └── notebooks/
│   │
│   ├── match_duration_prediction/
│   │   ├── README.md
│   │   └── notebooks/
│   │
│   └── hero_counter_picks/
│       ├── README.md
│       └── notebooks/
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

## Getting Started

### 1. Prerequisites

Before running the project, make sure you have installed:

| Tool | Purpose |
|---|---|
| Python 3.10+ | Main programming language |
| Git | Version control |
| Docker & Docker Compose | Running infrastructure services (if configured) |
| Apache Spark | Distributed data processing |
| PostgreSQL | Relational data source |
| MongoDB | NoSQL data source |
| MinIO | S3-compatible object storage |
| Trino | SQL query engine |
| Apache Iceberg | Open table format |
| Apache Airflow | Workflow orchestration |

> **Note:** Some services may be optional depending on the pipeline or use case you want to run. Check the corresponding case README for specific requirements.

### 2. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/dota2-data-engineering.git

cd dota2-data-engineering
```

### 3. Create a virtual environment

Create and activate a Python virtual environment:

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Linux / macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Install dependencies

Install the required Python packages:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 5. Configure environment variables

Create your local environment file:

**Windows:**

```powershell
Copy-Item .env.example .env
```

**Linux / macOS:**

```bash
cp .env.example .env
```

Edit `.env` and provide your local database credentials, storage settings, and service configuration.

**Important:** Never commit your `.env` file or expose passwords and access keys.

### 6. Start infrastructure services

If Docker Compose configuration is available, start the required services:

```bash
docker compose up -d
```

Check running containers:

```bash
docker compose ps
```
Then navigate to the selected use case notebook.

# File structure

```dota2-data-engineering/\
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
│   │   └──  notebooks/\
│   │\
│   ├── match_duration_prediction/\
│   │   ├── README.md\
│   │   └── notebooks/\
│   │\
│   └── hero_counter_picks/\
│       ├── README.md\
│       └── notebooks/\
```
