# Use a Node.js image compatible with your version
FROM node:22-bullseye AS builder

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install necessary system dependencies (for SWC and other tools)
RUN apt-get update && apt-get install -y \
    python3 \
    make \
    g++ \
    && rm -rf /var/lib/apt/lists/*

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the app
RUN npm run build

# Use Nginx to serve the app
FROM nginx:1.25-alpine

# Copy the built app
COPY --from=builder /app/dist /usr/share/nginx/html

# Expose the port for the application
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
