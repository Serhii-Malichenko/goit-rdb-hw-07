# Homework 5 — Topic 7: Time Series and Semi-Structured Data for ML

## Overview

This project explores PostgreSQL capabilities for working with time-series data and semi-structured JSONB data in machine learning workflows.

The notebook demonstrates time-series feature engineering, JSONB querying, index optimization, and a performance comparison between JSONB and normalized relational data.

## Data Sources

This project uses **synthetic datasets generated directly in PostgreSQL**. No external CSV files or datasets are required.

Two datasets are created:

- **Time-series dataset:** 300 daily records covering 60 days across 5 pickup zones.
- **Event log dataset:** 50,000 synthetic events representing five event types: `page_view`, `click`, `add_to_cart`, `checkout`, and `recommendation_shown`.

The data is generated using PostgreSQL SQL queries (`generate_series()` and conditional expressions), making the notebook self-contained and reproducible.

## Time-Series Features

The time-series section uses daily taxi activity data to demonstrate feature engineering with PostgreSQL window functions.

The following features are implemented:

- **Lag features:** previous-day, 7-day, and 30-day trip counts using `LAG()`.
- **Rolling 7-day average:** average fare over a moving 7-day window.
- **Rolling 30-day volatility:** sample standard deviation of fares over a moving 30-day window using `STDDEV_SAMP()`.
- **Expanding features:** cumulative trip count and average trip count using window functions.

A combined view, `v_t7_hw_taxi_features`, brings the engineered features together.

## JSONB Event Log

The semi-structured data section uses PostgreSQL's `JSONB` data type to store event payloads with different structures.

The notebook demonstrates:

- JSONB containment queries using `@>`.
- Key existence checks using `?` and `?|`.
- JSONPath queries using `jsonb_path_query()` and `jsonb_path_exists()`.
- GIN indexing with `jsonb_path_ops` for JSONB containment queries.
- Expression B-tree indexing for queries on extracted JSONB values.

Query performance is evaluated using `EXPLAIN (ANALYZE, BUFFERS)` before and after index creation.

## JSONB vs. Normalized Data

A separate normalized checkout table is created from the JSONB event log.

The project compares aggregation query performance between the JSONB-based event table and the normalized relational table, considering execution time and buffer usage.

The comparison illustrates the trade-offs between schema flexibility and structured relational data.

## Technologies

- Python
- PostgreSQL 16
- SQL
- SQLAlchemy
- pandas
- Google Colab

## How to Run

1. Open the Jupyter notebook in Google Colab.
2. Run the notebook from the beginning using **Runtime → Run all**.
3. The setup section installs the required dependencies, starts the PostgreSQL server, and establishes the database connection.
4. All tables, synthetic datasets, features, and indexes are created by the notebook.

**Note:** The notebook is designed to run from a fresh runtime without requiring manually prepared datasets or an existing PostgreSQL database.

