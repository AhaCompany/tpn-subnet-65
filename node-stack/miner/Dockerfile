# Use Nodejs image base
FROM node:22.13.0-slim

# Set the working directory inside the container
WORKDIR /app

# Copy package management files
COPY package*.json ./

# Install dependencies, data files from maxmind and ip2location are downloaded later and not during build
RUN npm i -g npm
RUN npm ci --omit=dev

# Install curl for the healthcheck
RUN apt update && apt install -y curl

# Copy application code
COPY app.js ./
COPY routes ./routes

# Expose the port the app runs on
EXPOSE 3001

# Serve the app
CMD ["node", "app.js"]

# Healthcheck call to /score, expect 200
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 CMD curl -f http://localhost:3001/challenge || exit 1
