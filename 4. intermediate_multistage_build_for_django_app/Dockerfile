# Base build
FROM python:3.11.9-slim as builder

RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    libffi-dev \
    zlib1g-dev \
    libjpeg-dev \
    libmagic-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory in the container
ENV PYTHONUNBUFFERED=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/


# Now multistage build
FROM builder

# Install only necessary runtime dependencies (no build tools)
RUN apt-get update && apt-get install -y \
    libpq-dev \
    libffi-dev \
    zlib1g-dev \
    libjpeg-dev \
    libmagic-dev \
    && rm -rf /var/lib/apt/lists/*  # Clean up apt cache

# Set a working directory for the app
WORKDIR /app
# Copy installed Python packages from the builder stage
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
# Copy application files from the builder stage
COPY --from=builder /code /app

ENV PYTHONUNBUFFERED 1