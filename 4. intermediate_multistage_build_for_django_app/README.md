# Instructions 1

## Create `requirements.txt`
```
Django>=5.1
psycopg2
```

## Create `Dockerfile` in your project root directory
```
# syntax=docker/dockerfile:1
FROM python:3.11

# Set the working directory in the container
ENV PYTHONUNBUFFERED=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```

## Create a file called `docker-compose.yml` in your root directory
```
services:
  dbbaru:
    image: postgres
    volumes:
      - ./data/db:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    environment:
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    depends_on:
      - dbbaru
```

## Create django project in container
```
docker-compose run web django-admin startproject djangodockertest .
```

## Change django project Settings.py database
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'dbbaru',
        'PORT': 5432, #default port you don't need to mention in docker-compose
    }
}
```

## Run makemigrations and migrate
```
docker-compose run web python manage.py makemigrations

docker-compose run web python manage.py migrate       
```

## Starting the container
```
docker-compose up
```

## Akses
Buka browser dan akses http://localhost:8000/

## Check image size
```
docker image ls
```

## Hasil

| REPOSITORY                                           | TAG    | IMAGE ID      | CREATED         | SIZE   |
| ---------------------------------------------------- | ------ | ------------- | --------------- | ------ |
| intermediate_multistage_build_for_django_app-web     | latest | b0df8ea09898  | 21 minutes ago  | 1.11GB |


# Instruction 2

Ikuti step yang ada di instruction 1, namun ada beberapa perubahan di bagian ini

## Create `Dockerfile` in your project root directory
```
# syntax=docker/dockerfile:1
FROM python:3.11.9-slim

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
```

## Hasil
| REPOSITORY                                           | TAG    | IMAGE ID      | CREATED         | SIZE   |
| ---------------------------------------------------- | ------ | ------------- | --------------- | ------ |
| intermediate_multistage_build_for_django_app-web     | latest | 0c70777dc5f2  | 58 seconds ago  | 624MB  |


# Instruction 3

Ikuti step yang ada di instruction 1, namun ada beberapa perubahan di bagian ini

## Create `Dockerfile` in your project root directory
```
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
```

## Hasil
| REPOSITORY                                           | TAG    | IMAGE ID      | CREATED         | SIZE   |
| ---------------------------------------------------- | ------ | ------------- | --------------- | ------ |
| intermediate_multistage_build_for_django_app-web     | latest | 9a7cb2dbf370  | 14 seconds ago  | 356MB  |


# Kesimpulan

| No | REPOSITORY   | TAG    | IMAGE ID      | CREATED         | SIZE   | Keterangan                |
|----| ------------ | ------ | ------------- | --------------- | ------ | ------------------------- |
| 1  | django_app   | latest | b0df8ea09898  | 21 minutes ago  | 1.11GB | python:3.11               |
| 2  | django_app   | latest | 0c70777dc5f2  | 58 seconds ago  | 624MB  | python:3.11.9-slim        |
| 3  | django_app   | latest | 9a7cb2dbf370  | 14 seconds ago  | 356MB  | python:3.11.9-slim multi-stage |
