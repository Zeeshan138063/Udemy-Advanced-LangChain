FROM python:3.11-slim

WORKDIR /usr/src/app

RUN apt-get update && apt-get install -y \
    postgresql-client \
    libmagic1 \
    dos2unix \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install them
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy the wait-for-postgres.sh script and ensure it is executable
COPY wait-for-postgres.sh /wait-for-postgres.sh
RUN dos2unix /wait-for-postgres.sh && chmod +x /wait-for-postgres.sh

# Copy the application code
COPY . .

EXPOSE 8000

# Run the wait-for-postgres.sh script before starting the app
CMD ["/wait-for-postgres.sh", "postgres", "uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
