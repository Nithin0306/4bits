# Use python 3.11 slim image for smaller size
FROM python:3.11-slim

# Set environment variables
# Prevent Python from writing pyc files
ENV PYTHONDONTWRITEBYTECODE=1
# Ensure python output is sent directly to terminal without buffering
ENV PYTHONUNBUFFERED=1

# Set working directory
WORKDIR /app

# Install system dependencies
# gcc is often required for compiling certain python packages
# Clean up apt cache to reduce image size
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements file first to leverage Docker layer caching
COPY requirements.txt .

# Install python dependencies without cache to save space
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Expose port 8000
EXPOSE 8000

# Healthcheck to verify the service is running
# Uses python's built-in library to avoid extra dependencies like curl
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/').read()" || exit 1

# Start the application using Uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
