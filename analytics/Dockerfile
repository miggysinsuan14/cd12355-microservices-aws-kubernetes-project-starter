FROM python:3.8-slim

# Set the working directory
WORKDIR /app

# Install system dependencies for PostgreSQL
RUN apt update -y && apt install -y build-essential libpq-dev

# Copy requirements and install Python dependencies
COPY requirements.txt .
RUN pip install --upgrade pip setuptools wheel && \
    pip install -r requirements.txt

# Copy the application code
COPY . .

# Expose the application port
EXPOSE 5153

# Run the application
CMD ["python", "app.py"]