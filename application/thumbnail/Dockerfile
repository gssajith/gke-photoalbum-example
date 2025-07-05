FROM python:3.11-slim-bookworm
LABEL maintainer="Etsuji Nakai"
ENV REFRESHED_AT=2025/07/05

# System packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        wget gcc libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Copy application source
ADD src /opt/thumbnail/bin
RUN chmod u+x /opt/thumbnail/bin/worker.py

# Install Python dependencies
RUN pip install --upgrade pip setuptools wheel && \
    pip install \
        google-cloud-core==2.4.1 \
        google-cloud-pubsub==2.21.1 \
        google-cloud-storage==2.16.0 \
        google-cloud-vision==3.10.0 \
        Flask==3.0.3 \
        Flask-SQLAlchemy==3.1.1 \
        Pillow==10.3.0 \
        PyMySQL==1.1.0 \
        pytz==2024.1

CMD ["python3", "/opt/thumbnail/bin/worker.py"]
