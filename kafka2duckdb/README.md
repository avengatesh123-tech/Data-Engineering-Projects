# Kafka to DuckDB Analytics Pipeline

## Overview

This project implements a streaming analytics pipeline using Apache Kafka, Parquet, and DuckDB.

Synthetic e-commerce order events are continuously generated and published to a Kafka topic. A Kafka consumer reads the incoming events in batches and stores them as Parquet files. DuckDB queries these Parquet files directly to perform analytical workloads without requiring data to be loaded into a separate database.

---

## Pipeline

```text
Producer
    │
    ▼
Apache Kafka
    │
    ▼
Kafka Consumer
    │
    ▼
Parquet Storage
    │
    ▼
DuckDB
    │
    ▼
SQL Analytics
```

---

## How the Pipeline Works

### 1. Data Generation

The producer continuously generates synthetic e-commerce order events. Each event contains information such as customer details, product information, quantity, pricing, payment method, order status, and timestamp.

Every generated event is serialized as JSON and published to the **orders** Kafka topic.

---

### 2. Event Streaming

Apache Kafka acts as the streaming platform responsible for receiving, storing, and delivering events.

Each order message is appended to the **orders** topic, allowing consumers to process events independently.

---

### 3. Batch Consumption

The consumer subscribes to the **orders** topic and continuously listens for new messages.

Instead of writing every message individually, records are collected into batches. Once the configured batch size is reached, the accumulated records are converted into a DataFrame and written as a Parquet file.

Batch processing reduces disk I/O while producing analytics-optimized files.

---

### 4. Parquet Storage

Order events are stored in Parquet format.

Parquet is a columnar storage format that provides:

* Efficient compression
* Faster analytical queries
* Reduced storage requirements
* Direct compatibility with DuckDB

As new batches arrive from Kafka, additional Parquet files are created without modifying previously written data.

---

### 5. Analytics with DuckDB

DuckDB reads the generated Parquet files directly without requiring data import.

Analytical SQL queries can be executed across all Parquet files to calculate metrics such as:

* Total orders
* Revenue
* Product performance
* Customer spending
* Category performance
* City-wise sales
* Payment method distribution
* Order status distribution
* Time-based sales trends

Since DuckDB executes SQL directly on Parquet, analysis remains fast while avoiding unnecessary data movement.

---

## Data Flow

```text
Generate Order
      │
      ▼
Publish to Kafka
      │
      ▼
Consumer Receives Message
      │
      ▼
Accumulate Batch
      │
      ▼
Write Parquet File
      │
      ▼
DuckDB Reads Parquet
      │
      ▼
Execute SQL Analytics
```

---

## Result

The project demonstrates a lightweight streaming analytics workflow where Kafka is responsible for event ingestion, Parquet serves as the analytical storage layer, and DuckDB provides fast SQL-based analysis directly on the generated files.
