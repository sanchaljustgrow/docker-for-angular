# -------------------------------
# Stage 1: Build Angular app
# -------------------------------
FROM node:20-alpine AS build

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci --silent

# Install Angular CLI (only during build)
RUN npm install -g @angular/cli

# Copy all source files
COPY . .

# Build the Angular app (production mode)
# The output path usually becomes /app/dist/<project-name>
RUN ng build --configuration production

# -------------------------------
# Stage 2: Serve with NGINX
# -------------------------------
FROM nginx:stable-alpine


COPY --from=build /app/dist/frontend-angular /usr/share/nginx/html

# Expose HTTP port
EXPOSE 80

# Start NGINX
CMD ["nginx", "-g", "daemon off;"]
