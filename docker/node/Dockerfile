# ──────────────────────────────────────────────────
# Node.js 22 LTS — Laravel Vite / npm asset pipeline
# ──────────────────────────────────────────────────
FROM node:22-alpine

# Install build tools for native npm modules (e.g. canvas, sharp)
RUN apk add --no-cache \
    python3 \
    make \
    g++ \
    git

# Set working directory
WORKDIR /app

# Expose Vite dev server port
EXPOSE 5173

# Default — override via docker-compose command or `docker compose run`
CMD ["sh", "-c", "npm install && npm run dev"]