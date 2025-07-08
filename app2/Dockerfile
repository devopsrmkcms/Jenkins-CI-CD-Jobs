# Use an official Python runtime as a parent image (specify a version, not 'latest')
FROM python:3.11-slim

# Set the working directory in the container
WORKDIR /app

# Copy only requirements first for better caching
COPY requirements.txt ./

# Install dependencies in one RUN command and clean up cache
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of your application code
COPY . .

# Expose the port your app runs on
EXPOSE 5000

# Define the default command to run your app
CMD ["python", "app.py"]

