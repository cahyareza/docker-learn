# syntax=docker/dockerfile:1
FROM python:3.11.9-slim

# Set the working directory in the container
ENV PYTHONUNBUFFERED=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/