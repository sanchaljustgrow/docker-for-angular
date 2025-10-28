# backend-fastify/Dockerfile
FROM node:20-alpine AS builder

WORKDIR /app

# copy package files and install only production deps later (but we install dev to build if needed)
COPY package*.json ./
RUN npm ci --ignore-scripts

# copy source
COPY . .

# if there is a build step (typescript), run it. Adjust if repo uses ts-node or compiled JS.
# Uncomment if you need to build typescript
# RUN npm run build

# final image
FROM node:20-alpine

WORKDIR /app

# create non-root user
RUN addgroup -S app && adduser -S -G app app
USER app

# copy node_modules and app files from builder (keeping ownership okay because user exists in both stages)
COPY --from=builder /app /app

# Expose port (default in README is 8000)
EXPOSE 8000

ENV NODE_ENV=production
# Placeholders for environment variables - to be provided with docker-compose or .env
ENV PORT=8000

# If repo's start script is "npm start" or "node server.js", adjust accordingly
CMD ["npm", "start"]
