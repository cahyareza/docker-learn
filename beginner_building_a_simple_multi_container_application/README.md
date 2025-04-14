# Instructions

## Create `requirements.txt`
```
Django>=5.1
psycopg2
```

## Create `Dockerfile` in your project root directory
```
# syntax=docker/dockerfile:1
FROM python:3.11.9-slim

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
  db:
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
      - db
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
        'HOST': 'db',
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