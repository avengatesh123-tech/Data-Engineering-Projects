# Kafka Order Streaming

A minimal end-to-end example demonstrating Apache Kafka producer and consumer patterns using `confluent-kafka` (Python), with a single-node Kafka broker running in KRaft mode (no Zookeeper) via Docker Compose.

## Project Structure

| File | Description |
|---|---|
| `docker-compose.yaml` | Single-node Kafka broker configuration (KRaft mode) |
| `producer.py` | Publishes a sample order event to the `orders` topic |
| `tracker.py` | Consumes and displays order events from the `orders` topic |

## Prerequisites

- Docker and Docker Compose
- Python 3.8 or higher

## Setup

### 1. Start the Kafka broker

```bash
docker compose up -d
```

### 2. Install dependencies

```bash
pip3 install confluent-kafka
```

## Usage

### Produce an event

```bash
python3 producer.py
```

### Consume events

```bash
python3 tracker.py
```

## Kafka CLI Reference

**List all topics**
```bash
docker exec -it kafka kafka-topics --list --bootstrap-server localhost:9092
```

**Describe a topic (partitions, replication, etc.)**
```bash
docker exec -it kafka kafka-topics --bootstrap-server localhost:9092 --describe --topic orders
```

**View all events in a topic from the beginning**
```bash
docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic orders --from-beginning
```

## Notes

- Kafka is exposed on `localhost:9092`.
- Data is persisted in the `kafka_kraft` Docker volume.
- To reset the environment completely, run `docker compose down -v`.