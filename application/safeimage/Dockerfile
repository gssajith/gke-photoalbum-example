FROM python:3.11-slim-bookworm

LABEL maintainer="Etsuji Nakai"
ENV REFRESHED_AT=2025-07-06

# Install system dependencies
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        build-essential \
        wget \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Create app directory
WORKDIR /opt/safeimage

# Copy requirements first for better caching
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY src/ bin/

# Set executable permissions
RUN chmod +x bin/worker.py

# Create non-root user for security
RUN useradd --create-home --shell /bin/bash appuser && \
    chown -R appuser:appuser /opt/safeimage
USER appuser

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python3 -c "import sys; sys.exit(0)"

CMD ["python3", "bin/worker.py"]
