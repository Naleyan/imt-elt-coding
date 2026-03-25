# KICKZ EMPIRE — ELT Pipeline

ELT (Extract, Load, Transform) pipeline for the **KICKZ EMPIRE** e-commerce website, built as part of the IMT Data Engineering course.

## 1. Context

KICKZ EMPIRE is a fast-growing e-commerce platform specializing in sneakers and streetwear (Nike, Adidas, Jordan, New Balance, Puma…). The store sells sneakers, hoodies, t-shirts, joggers, and accessories to thousands of customers worldwide.

The e-commerce team has been collecting data for weeks — orders, product catalogs, user registrations, customer reviews, clickstream events — but it all sits as raw files in an S3 data lake in multiple formats (CSV, JSONL, Parquet).
As a result, no one can easily query it in order to make some business analyses. For example these questions remain unanswered: "How much revenue are we generating day by day? Which products are trending?" ,"Which brands and categories perform the best?, etc. 

Our goal is to build an **ELT pipeline** following the **Medallion Architecture** (Bronze → Silver → Gold) in order to answer the business questions. Then We implement the good practices like testing, logging and monitoring for making our code ready for industrialization.



## 🏗️ 2. Architecture

```
S3 (CSV/JSON/parquet)  ──→  🥉 Bronze (raw)  ──→  🥈 Silver (clean)  ──→  🥇 Gold (analytics)
```

| Layer | Schema | Description |
|---|---|---|
| **Bronze** | `bronze_group5` | Raw data — faithful copy of CSV files from S3 |
| **Silver** | `silver_group5` | Cleaned data — internal columns removed, PII masked |
| **Gold** | `gold_group5` | Aggregated data — ready for dashboards |


## 🚀 Setup instructions

```bash
# 1. Clone the repo
git clone <repo-url>
cd imt-elt-coding

# 2. Configure the  virtual environment
python -m venv venv

source venv/bin/activate

pip install -r requirements.txt
```

```bash
# 3. Test the connection

cp .env.example .env  # Configure with your credentials (DB + AWS)

python -m src.database
```

## How to run
```bash
#  Extraction step (Bronze)
 python pipeline.py --step extract   # Run extraction only

#Transform step (Silver)
 python pipeline.py --step transform # Run transformation only

# Aggregations steps (Gold)
python pipeline.py --step gold      # Run Gold layer only

# Full Pipeline
python pipeline.py                  # Run the full pipeline
```

## How to test
```bash
# Run all tests
pytest tests/ -v

# Run with coverage report
pytest tests/ -v --cov=src --cov-report=term-missing
```

## ⚙️ Tech Stack

- **Python 3.10+** : Main language
- **pandas** : Data manipulation
- **boto3** : AWS S3 access
- **SQLAlchemy** : ORM / PostgreSQL connection
- **PostgreSQL** (AWS RDS) : Database
- **pytest** : Testing (TP2)

## Team Members
- **Eva Lansalot**
- **Kamon SOURABIE**
- **Nada Aleian**
