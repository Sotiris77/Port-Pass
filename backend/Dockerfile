# backend/Dockerfile
FROM python:3.11-slim
# Make logs unbuffered and avoid .pyc files
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
# Workdir inside the container
WORKDIR /app
# System deps for psycopg2 etc.
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential libpq-dev curl \
 && rm -rf /var/lib/apt/lists/*
# Install Python deps first (better caching)
COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt
# Copy the backend code
COPY . /app
# Expose app port
EXPOSE 8000
# Optional: container-level healthcheck (matches /health)
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD curl -fsS http://localhost:8000/health || exit 1
# Run FastAPI
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
