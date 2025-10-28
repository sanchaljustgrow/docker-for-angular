
FROM node:20-alpine AS builder

WORKDIR /app

ENV CI=true

COPY package*.json ./
RUN npm ci

COPY . .


RUN npm run build --if-present


# Production stage - nginx
FROM nginx:stable-alpine

# Remove default site
RUN rm -rf /usr/share/nginx/html/*

# copy built files from builder
COPY --from=builder /app/dist /usr/share/nginx/html


COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
