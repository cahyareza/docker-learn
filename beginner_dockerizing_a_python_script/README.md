# Instructions

## Write the Python script
Create a script named process_data.py that reads and processes a CSV file. Here’s an example script:
```
import pandas as pd
df = pd.read_csv('data.csv')
print(df.describe())
```

## Create a requirements.txt file
This file lists the Python libraries the script needs. In this case, we only need pandas:
```
pandas
```

## Write the Dockerfile
This file will define the environment for the Python script:
```
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "process_data.py"]
```

## Build the Docker image
```
docker build -t python-script .
```

## Run the container
```
docker run -v $(pwd)/data:/app/data python-script
```