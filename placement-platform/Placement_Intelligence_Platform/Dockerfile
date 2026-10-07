# Base stage
FROM python:3.12-slim AS base

# Install curl for healthcheck probes
RUN apt-get update && apt-get install -y --no-install-recommends curl && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Run as non-root
RUN adduser --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

# API stage
FROM base AS api

EXPOSE 8000

HEALTHCHECK --interval=10s --timeout=5s --start-period=15s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]

# Worker stage
FROM base AS worker

ARG WORKER_CMD=python -m app.mq.consumers.ingestion_worker
CMD ["sh", "-c", "${WORKER_CMD}"]
