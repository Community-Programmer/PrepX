# ── Stage 1: Build ────────────────────────────────────────────────────────
FROM node:20-alpine AS builder

WORKDIR /app

# Install dependencies first (better caching)
COPY package.json package-lock.json ./
RUN npm ci

# Copy source and build
COPY . .

# Build args for Vite env variables (injected at build time)
ARG VITE_GOOGLE_CLIENT_ID
ARG VITE_API_URL
ARG VITE_FASTAPI_URL

ENV VITE_GOOGLE_CLIENT_ID=${VITE_GOOGLE_CLIENT_ID}
ENV VITE_API_URL=${VITE_API_URL}
ENV VITE_FASTAPI_URL=${VITE_FASTAPI_URL}

RUN npm run build

# ── Stage 2: Serve with Nginx ─────────────────────────────────────────────
FROM nginx:alpine

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built assets from builder
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
