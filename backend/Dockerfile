# ---- Base image ----
FROM python:3.11-slim

# Prevent Python from writing .pyc files and force stdout flush
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Set working directory inside container
WORKDIR /app

# Install system dependencies (needed for psycopg2/Postgres)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    curl \
 && rm -rf /var/lib/apt/lists/*

# Copy requirements first (for caching)
COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

# Copy backend source code into container
COPY . /app

# Expose port for Uvicorn
EXPOSE 8000

# Healthcheck (optional but nice)
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD curl -fsS http://localhost:8000/health || exit 1

# Run the FastAPI app (main.py has "app = FastAPI()")
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
