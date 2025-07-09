# ---- Stage 1: Builder ----
FROM python:3.11-slim AS builder

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Set work directory
WORKDIR /app

# Install build dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies system-wide
COPY requirements.txt .
RUN pip install --upgrade pip && \
    pip install -r requirements.txt

# Copy application source code
COPY . .

# ---- Stage 2: Production ----
FROM python:3.11-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Set work directory
WORKDIR /app

# Copy installed dependencies and app from builder stage
COPY --from=builder /usr/local /usr/local
COPY --from=builder /app /app

# Create a non-root user and switch to it
RUN groupadd -r flask && useradd -m -g flask flaskuser
USER flaskuser

# Expose port
EXPOSE 5000

# Healthcheck (optional, requires curl in requirements and app's /health endpoint)
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD curl --fail http://localhost:5000/health || exit 1

# Set default command
ENTRYPOINT ["flask"]
CMD ["run", "--host=0.0.0.0", "--port=5000"]

