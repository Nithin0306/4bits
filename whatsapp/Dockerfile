# Use node 18 slim image
FROM node:18-slim

# Set working directory
WORKDIR /app

# Install dependencies required for Puppeteer and Chromium
# We install chromium directly and tell Puppeteer to use it
RUN apt-get update && apt-get install -y \
    chromium \
    libnss3 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libdrm2 \
    libxkbcommon0 \
    libxcomposite1 \
    libxdamage1 \
    libxfixes3 \
    libxrandr2 \
    libgbm1 \
    libasound2 \
    libpango-1.0-0 \
    libcairo2 \
    fonts-liberation \
    fonts-noto-color-emoji \
    && rm -rf /var/lib/apt/lists/*

# Set Puppeteer environment variables
# Skip downloading chromium since we installed it manually
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
# Point Puppeteer to the installed chromium executable
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium

# Copy package files
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci

# Copy the rest of the application code
COPY . .

# Create directory for authentication session data
RUN mkdir -p /app/.wwebjs_auth

# Start the application
CMD ["npm", "run", "main"]
