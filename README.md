<div align="center">

```
██████╗ ██████╗ ██╗ ██████╗███████╗██████╗  █████╗ ██████╗  █████╗ ██████╗
██╔══██╗██╔══██╗██║██╔════╝██╔════╝██╔══██╗██╔══██╗██╔══██╗██╔══██╗██╔══██╗
██████╔╝██████╔╝██║██║     █████╗  ██████╔╝███████║██║  ██║███████║██████╔╝
██╔═══╝ ██╔══██╗██║██║     ██╔══╝  ██╔══██╗██╔══██║██║  ██║██╔══██║██╔══██╗
██║     ██║  ██║██║╚██████╗███████╗██║  ██║██║  ██║██████╔╝██║  ██║██║  ██║
╚═╝     ╚═╝  ╚═╝╚═╝ ╚═════╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝
```

**E-commerce price intelligence pipeline — scrape · detect · forecast · alert**

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Apache Airflow](https://img.shields.io/badge/Airflow-2.7-017CEE?style=flat-square&logo=apacheairflow&logoColor=white)](https://airflow.apache.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker&logoColor=white)](https://docker.com)
[![Prophet](https://img.shields.io/badge/Prophet-Forecasting-FF6B6B?style=flat-square)](https://facebook.github.io/prophet)

</div>

---

## Overview

PriceRadar is an automated data pipeline that tracks product prices across Amazon and Flipkart, detects anomalies using statistical models, generates 7-day forecasts with Facebook Prophet, and fires real-time email alerts when a user-defined threshold is crossed.

```
Scrapy Spiders ──► Staging Table ──► ETL Pipeline ──► PostgreSQL
                                                           │
                                                    Analytics Layer
                                                    (MA · Z-score · Prophet)
                                                           │
                                                       FastAPI ──► Email Alerts
                                                           │
                                                    ╔══════════╗
                                                    ║  Airflow ║  ← orchestrates all stages
                                                    ╚══════════╝
```

---

## Features

| # | Feature | Description |
|---|---------|-------------|
| 01 | **Scheduled scraping** | Scrapy spiders with rotating user-agents, randomized delays, and retry logic |
| 02 | **Cross-platform dedup** | Fuzzy-matches the same product across Amazon and Flipkart into one record |
| 03 | **Anomaly detection** | Z-score flags price drops relative to each product's own rolling variance |
| 04 | **7-day forecast** | Per-product Prophet model with 95% confidence intervals, retrained on each run |
| 05 | **Alert system** | POST a target price → get an email the moment it's crossed |
| 06 | **REST API** | 9 endpoints, auto-documented Swagger UI at `/docs` |
| 07 | **One-command deploy** | `docker-compose up` starts Airflow + PostgreSQL + API together |

---

## Tech stack

```
Layer           Tool              Why
──────────────────────────────────────────────────────────────
Scraping        Scrapy            Async, extensible, middleware support
Storage         PostgreSQL 15     Relational, great for time-series queries
Orchestration   Apache Airflow    DAG-based, each stage independently retryable
Cleaning        Pandas            Vectorized transforms, fast dedup logic
Forecasting     Prophet           Handles sale-event spikes + missing data natively
API             FastAPI           Async, Pydantic validation, auto Swagger docs
Containers      Docker Compose    Single-command local deployment
Migrations      Alembic           Schema versioning
```

---

## Architecture

Every stage runs as an Airflow DAG on a configurable cron schedule:

```
┌─────────────────────────────────────────────────────────────────┐
│                        Apache Airflow                           │
│                                                                 │
│  ┌──────────┐    ┌─────────┐    ┌──────────┐    ┌──────────┐  │
│  │ scrape   │───►│   etl   │───►│ forecast │───►│  alert   │  │
│  │  DAG     │    │  DAG    │    │   DAG    │    │  check   │  │
│  └──────────┘    └─────────┘    └──────────┘    └──────────┘  │
│       │               │               │               │        │
└───────┼───────────────┼───────────────┼───────────────┼────────┘
        │               │               │               │
   raw scrapes     price_history    forecasts      email fired
   staging table   (cleaned)        table          via SMTP
```

Each stage is independently retryable. A failed forecast job doesn't block the scraper from running on the next cycle.

---

## Database schema

```sql
-- Core tables (simplified)

products (
  id          UUID PRIMARY KEY,
  name        TEXT,
  url         TEXT,
  platform    VARCHAR(20),      -- 'amazon' | 'flipkart'
  created_at  TIMESTAMPTZ
)

price_history (
  id            BIGSERIAL PRIMARY KEY,
  product_id    UUID REFERENCES products(id),
  price         NUMERIC(10,2),
  availability  BOOLEAN,
  scraped_at    TIMESTAMPTZ
)

alerts (
  id            UUID PRIMARY KEY,
  product_id    UUID REFERENCES products(id),
  user_email    TEXT,
  target_price  NUMERIC(10,2),
  is_active     BOOLEAN DEFAULT TRUE,
  triggered_at  TIMESTAMPTZ
)

forecasts (
  id               BIGSERIAL PRIMARY KEY,
  product_id       UUID REFERENCES products(id),
  forecast_date    DATE,
  predicted_price  NUMERIC(10,2),
  conf_low         NUMERIC(10,2),
  conf_high        NUMERIC(10,2)
)
```

---

## API endpoints

```
GET    /products                     search with ?q=&platform=
GET    /products/{id}/history        full price timeline
GET    /products/{id}/forecast       7-day Prophet prediction + confidence intervals
GET    /products/{id}/analytics      moving avg · Z-score · drop %
POST   /alerts                       register a price threshold alert
GET    /alerts                       list active alerts
DELETE /alerts/{id}                  remove an alert
GET    /deals/today                  biggest drops vs 30-day baseline
GET    /dashboard                    aggregate stats across all tracked products
```

Auto-generated Swagger UI available at `http://localhost:8000/docs` when running locally.

---

## Quickstart

**Prerequisites:** Docker + Docker Compose

```bash
# 1. Clone
git clone https://github.com/harshiiika/PriceRadar.git
cd PriceRadar

# 2. Configure
cp .env.example .env
# → fill in SMTP credentials for email alerts

# 3. Start everything
docker-compose up -d

# Services:
# API + Swagger docs  →  http://localhost:8000/docs
# Airflow UI          →  http://localhost:8080  (admin / admin)
# PostgreSQL          →  localhost:5432
```

**Add a product and set an alert:**

```bash
# Track a product
curl -X POST http://localhost:8000/products \
  -H "Content-Type: application/json" \
  -d '{"url": "https://amazon.in/dp/B09XYZ123", "platform": "amazon"}'

# Alert when it drops below ₹999
curl -X POST http://localhost:8000/alerts \
  -d '{"product_id": "...", "target_price": 999, "email": "you@gmail.com"}'

# Check today's best deals
curl http://localhost:8000/deals/today
```

---

## Repo structure

```
PriceRadar/
├── scrapers/
│   ├── spiders/
│   │   ├── amazon.py           # Amazon product spider
│   │   └── flipkart.py         # Flipkart product spider
│   ├── middlewares.py          # UA rotation, retry logic
│   └── selectors.json          # CSS selectors (centralised, not hardcoded)
│
├── pipeline/
│   ├── etl.py                  # Pandas cleaning & normalisation
│   ├── dedup.py                # Cross-platform product matching
│   └── analytics.py           # Moving avg, Z-score, drop %
│
├── ml/
│   └── forecast.py            # Prophet training & prediction per product
│
├── api/
│   ├── main.py                 # FastAPI app entry point
│   ├── routers/               # products.py · alerts.py · deals.py
│   └── schemas.py             # Pydantic request/response models
│
├── airflow/
│   └── dags/
│       ├── scrape_dag.py
│       ├── etl_dag.py
│       └── forecast_dag.py
│
├── db/
│   └── migrations/            # Alembic migration scripts
│
├── docker-compose.yml
├── .env.example
└── requirements.txt
```

---

## Design decisions

**Why store raw HTML alongside parsed data?**
When a site updates its DOM, selectors break — but historical data shouldn't be lost. Storing raw HTML in the staging table means any past scrape can be re-parsed without re-fetching. Selector configs live in `selectors.json`, not inside the spiders, so a DOM change is a one-line fix.

**Why Prophet over ARIMA?**
E-commerce prices spike around sale events (Diwali, Big Billion Days, Prime Day). Prophet handles these as holiday regressors and manages missing data natively — both problems ARIMA needs manual preprocessing for. It also returns confidence intervals out of the box.

**Why Z-score over a fixed threshold for anomaly detection?**
A ₹200 drop means nothing on a ₹50,000 laptop but signals a major deal on a ₹300 cable. Z-score normalises by each product's own variance, making anomaly scores comparable across price ranges.

**Why Airflow for a project that runs on one machine?**
The DAG abstraction forces each stage (scrape → clean → forecast → alert) to be independently retryable and observable. When it's time to scale — Celery workers, cloud composer, whatever — pipeline logic stays unchanged.

---

## Environment variables

```bash
# PostgreSQL
POSTGRES_USER=priceradar
POSTGRES_PASSWORD=yourpassword
POSTGRES_DB=priceradar

# Email alerts
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=you@gmail.com
SMTP_PASS=your_app_password

# Scraping
SCRAPE_INTERVAL_HOURS=6
REQUEST_DELAY_MIN=2
REQUEST_DELAY_MAX=8
```

---

<div align="center">

Built with Python · Scrapy · Airflow · PostgreSQL · FastAPI · Prophet · Docker

</div>
