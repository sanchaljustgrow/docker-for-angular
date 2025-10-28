
FROM node:20-alpine AS builder

WORKDIR /app

ENV CI=true

COPY package*.json ./
RUN npm ci

COPY . .

# If using Ionic/Angular build command from README:
# Build production web assets (adjust command per your repo - ionic build or ng build)
RUN npm run build --if-present
# Some Ionic projects need: npm run build -- --configuration production
# Tailwind build step if required:
# RUN npm run tailwind:build

# Production stage - nginx
FROM nginx:stable-alpine

# Remove default site
RUN rm -rf /usr/share/nginx/html/*

# copy built files from builder
COPY --from=builder /app/www /usr/share/nginx/html
# If build output is in 'dist' or 'build', change the path above:
# COPY --from=builder /app/dist /usr/share/nginx/html

# copy a simple nginx config (optional) - adjust if you need SPA routing fallback to index.html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
