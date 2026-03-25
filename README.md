# KICKZ EMPIRE — ELT Pipeline

ELT (Extract, Load, Transform) pipeline for the **KICKZ EMPIRE** e-commerce website, built as part of the IMT Data Engineering course.

## Context

KICKZ EMPIRE is a fast-growing e-commerce platform specializing in sneakers and streetwear (Nike, Adidas, Jordan, New Balance, Puma…). The store sells sneakers, hoodies, t-shirts, joggers, and accessories to thousands of customers worldwide.

The e-commerce team has been collecting data for weeks — orders, product catalogs, user registrations, customer reviews, clickstream events — but it all sits as raw files in an S3 data lake in multiple formats (CSV, JSONL, Parquet).
As a result, no one can easily query it in order to make some business analyses. For example these questions remain unanswered: "How much revenue are we generating day by day? Which products are trending?" ,"Which brands and categories perform the best?, etc. 

Our goal is to build an **ELT pipeline** following the **Medallion Architecture** (Bronze → Silver → Gold) in order to answer the business questions. Then We implement the good practices like testing, logging and monitoring for making our code ready for industrialization.



## 🏗️ Architecture

```
S3 (CSV/JSON/parquet)  ──→  🥉 Bronze (raw)  ──→  🥈 Silver (clean)  ──→  🥇 Gold (analytics)
```


| Layer | Schema | Description |
|---|---|---|
| **Bronze** | `bronze_group5` | Raw data — faithful copy of CSV files from S3 |
| **Silver** | `silver_group5` | Cleaned data — internal columns removed, PII masked |
| **Gold** | `gold_group5` | Aggregated data — ready for dashboards |

## 📁 Project Structure

```
├── docs/
│   ├── DATA_PRESENTATION.md    # KICKZ EMPIRE data presentation
│   └── tp1/
│       └── INSTRUCTIONS.md     # Step-by-step TP1 instructions
├── src/
│   ├── __init__.py
│   ├── database.py             # PostgreSQL connection (AWS RDS)
│   ├── extract.py              # Extract: S3 (CSV) → Bronze
│   ├── transform.py            # Transform: Bronze → Silver
│   └── gold.py                 # Gold: Silver → Gold (aggregations)
├── pipeline.py                 # ELT orchestrator
├── tests/                      # Tests (TP2)
├── .env.example                # Environment variables template
├── .gitignore
├── requirements.txt
└── README.md
```

## 🚀 Quick Start

```bash
# 1. Setup
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env  # Configure with your credentials (DB + AWS)

# 2. Test the connection
python -m src.database

# 3. Run the pipeline (reads from S3 automatically)
python pipeline.py
```

## 📊 Datasets

| Dataset | Format | Source (S3) | Bronze Table |
|---|---|---|---|
| Product Catalog | CSV | `raw/catalog/products.csv` | `products` |
| Users | CSV | `raw/users/users.csv` | `users` |
| Orders | CSV | `raw/orders/orders.csv` | `orders` |
| Order Line Items | CSV | `raw/order_line_items/order_line_items.csv` | `order_line_items` |

## 📚 Documentation

- [Data Presentation](docs/DATA_PRESENTATION.md)
- [TP1 Instructions](docs/tp1/INSTRUCTIONS.md)

## ⚙️ Tech Stack

- **Python 3.10+** : Main language
- **pandas** : Data manipulation
- **boto3** : AWS S3 access
- **SQLAlchemy** : ORM / PostgreSQL connection
- **PostgreSQL** (AWS RDS) : Database
- **pytest** : Testing (TP2)
