# Stage 1: Build the React application
FROM node:18-alpine AS builder

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json (or yarn.lock)
COPY package.json package-lock.json* ./
# If you are using yarn, uncomment the next line and comment out the npm ci line
# COPY package.json yarn.lock ./

# Install dependencies
# Use npm ci for cleaner installs in CI/CD environments, otherwise npm install
RUN npm ci
# If you are using yarn, uncomment the next line and comment out the npm ci line
# RUN yarn install --frozen-lockfile

# Copy the rest of the application code
COPY . .

# Build the application for production
RUN npm run build
# If you are using yarn, uncomment the next line and comment out the npm run build line
# RUN yarn build

# Stage 2: Serve the application with Nginx
FROM nginx:stable-alpine

# Copy the build output from the builder stage to Nginx's web root directory
COPY --from=builder /app/dist /usr/share/nginx/html

# (Optional but Recommended) Copy a custom Nginx configuration file
# This helps with routing for Single Page Applications (SPAs)
# Create a file named 'nginx.conf' in your 'frontend' directory and then uncomment the line below.
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 (Nginx default port)
EXPOSE 80

# Start Nginx when the container launches
CMD ["nginx", "-g", "daemon off;"]
