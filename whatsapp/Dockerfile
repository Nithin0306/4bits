# ============================================
# VITA-CARE WhatsApp Service Dockerfile
# Node.js + Puppeteer with Chromium
# ============================================

FROM node:18-slim

# Set working directory
WORKDIR /app

# ============================================
# Install Chromium Dependencies for Puppeteer
# ============================================
RUN apt-get update && apt-get install -y --no-install-recommends \
    chromium \
    # Chromium dependencies
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
    # Fonts for proper rendering
    fonts-liberation \
    fonts-noto-color-emoji \
    # Clean up
    && rm -rf /var/lib/apt/lists/*

# ============================================
# Puppeteer Configuration
# ============================================
# Skip downloading bundled Chromium (we use system Chromium)
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
# Point to system Chromium
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium

# ============================================
# Application Setup
# ============================================
# Copy package files first for layer caching
COPY package.json package-lock.json* yarn.lock* ./

# Install dependencies
RUN npm install

# Copy application code
COPY . .

# Create directory for WhatsApp session data (will be mounted as volume)
RUN mkdir -p /app/.wwebjs_auth

# Run the WhatsApp service
CMD ["npm", "run", "main"]
