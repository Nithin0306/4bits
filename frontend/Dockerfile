# Stage 1: Build the application
# Use node 18 alpine image for smaller size
FROM node:18-alpine AS build

# Set working directory
WORKDIR /app

# Copy package files first to leverage Docker layer caching
COPY package.json package-lock.json ./

# Install dependencies using clean install for reproducibility
RUN npm ci

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the application
# Use nginx alpine image for production
FROM nginx:alpine

# Copy built assets from builder stage
# Vite builds to 'dist' by default
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
