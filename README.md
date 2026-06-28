# Log Platform

A lightweight log ingestion and monitoring platform built with FastAPI, Elasticsearch, Kibana, and Docker. This project demonstrates how to collect application logs, store them in Elasticsearch, and visualize them in Kibana for real-time troubleshooting and analysis.

## Overview

Log Platform provides a simple but practical logging pipeline for development and demo environments:

- The backend accepts log events through a REST API.
- Logs are indexed in Elasticsearch for fast search and filtering.
- Kibana offers an interactive dashboard for exploring log data.
- An optional Python-based generator can simulate log traffic.

## Architecture

```text
Application / Services -> FastAPI Backend -> Elasticsearch -> Kibana
                           ^
                           |
                    Sample log generator
```

### Components

- Backend: FastAPI service that receives logs and stores them in Elasticsearch.
- Elasticsearch: Search and analytics engine used for log storage and querying.
- Kibana: Visualization layer for dashboards and log exploration.
- Log Generator: Optional Python service that sends mock logs to the backend.

## Tech Stack

- FastAPI
- Elasticsearch 8.x
- Kibana 8.x
- Docker Compose
- Python 3.11

## Project Structure

```text
.
├── backend/
│   ├── app/
│   │   └── main.py
│   ├── Dockerfile
│   └── requirements.txt
├── log-generator/
│   ├── main.py
│   ├── Dockerfile
│   └── requirements.txt
├── docker-compose.yml
└── README.md
```

## Prerequisites

Make sure the following are installed on your machine:

- Docker
- Docker Compose
- Python 3.11 (only if you want to run the log generator locally)

## Quick Start

1. Clone the repository:

```bash
git clone https://github.com/your-username/Log-Platform.git
cd Log-Platform
```

2. Start the stack:

```bash
docker compose up --build -d
```

3. Wait a few moments for Elasticsearch and Kibana to initialize.

4. Open the services:

- Backend API docs: http://localhost:8000/docs
- Kibana: http://localhost:5601
- Elasticsearch: http://localhost:9200

## Sending Your First Log

You can send a sample log using the API:

```bash
curl -X POST "http://localhost:8000/logs" \
  -H "Content-Type: application/json" \
  -d '{
    "service": "auth-service",
    "level": "ERROR",
    "message": "Login failed for user",
    "meta": {
      "user_id": 123,
      "ip": "1.1.1.1"
    }
  }'
```

You can also search logs:

```bash
curl "http://localhost:8000/logs/search?query=login&limit=10"
```

## Kibana Setup

After the services are running:

1. Open Kibana at http://localhost:5601.
2. Create an index pattern for the Elasticsearch index.
3. Use the default index name: `logs-0001`.
4. Set the time field to `timestamp`.

You can then build visualizations such as:

- Logs by level
- Logs by service
- Error trend over time
- Recent error events

## Optional: Run the Log Generator

The repository includes a Python-based generator that simulates logs. Run it locally with:

```bash
BACKEND_URL=http://localhost:8000/logs python log-generator/main.py
```

This will continuously send mock logs to the backend.

## API Endpoints

### Health Check

```http
GET /health
```

### Ingest a Log

```http
POST /logs
```

### Search Logs

```http
GET /logs/search?query=login&limit=20
```

## Environment Variables

The backend uses the following environment variables:

- `ELASTIC_HOST`: Elasticsearch host (default: `elasticsearch`)
- `ELASTIC_PORT`: Elasticsearch port (default: `9200`)
- `ELASTIC_SCHEME`: Elasticsearch scheme (default: `http`)
- `LOG_INDEX`: Elasticsearch index name (default: `logs-0001`)

The generator uses:

- `BACKEND_URL`: Target log ingestion endpoint (default: `http://backend:8000/logs`)
- `INTERVAL`: Delay between generated logs in seconds (default: `5`)

## Troubleshooting

If something does not start correctly:

```bash
docker compose ps
docker compose logs -f backend elasticsearch kibana
```

Common issues:

- Elasticsearch takes a few moments to become ready.
- Kibana may show no data until logs are ingested.
- If the backend cannot connect to Elasticsearch, verify the Docker network and environment variables.

## License

This project is intended for learning, experimentation, and demonstration purposes.

