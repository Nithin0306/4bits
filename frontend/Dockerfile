# ============================================
# VITA-CARE Frontend Dockerfile
# Multi-stage build for React + Vite + Nginx
# ============================================

# Stage 1: Build
FROM node:18-alpine AS build

WORKDIR /app

# Copy package files first for layer caching
COPY package.json package-lock.json ./

# Install dependencies (ci for deterministic builds)
RUN npm ci

# Copy source code
COPY . .

# Build production assets
RUN npm run build

# ============================================
# Stage 2: Production
# ============================================
FROM nginx:alpine AS production

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built assets from build stage
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Nginx runs in foreground by default with this image
CMD ["nginx", "-g", "daemon off;"]
