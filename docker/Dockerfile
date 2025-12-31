# 1. Dependencies Stage
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

# 2. Build Stage
FROM node:20-alpine AS builder
WORKDIR /app
ARG NODE_OPTIONS="--max-old-space-size=4096"
ENV NODE_OPTIONS=$NODE_OPTIONS

# Dynamic Build Arguments
ARG NEXT_PUBLIC_API_SERVER
ARG NEXT_PUBLIC_APP_URL

ENV NEXT_PUBLIC_API_SERVER=$NEXT_PUBLIC_API_SERVER
ENV NEXT_PUBLIC_APP_URL=$NEXT_PUBLIC_APP_URL

COPY --from=deps /app/node_modules ./node_modules
COPY . .

# BuildKit Cache
RUN mkdir -p .next/cache
RUN --mount=type=cache,target=/app/.next/cache \
    npm run build

# Production Runner Stage
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV PORT=3000
ENV HOSTNAME=0.0.0.0

# for other dependencies(etc. libc6)
RUN apk add --no-cache libc6-compat wget curl

# Security: Create non-root user
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 appuser

COPY --from=builder /app/public ./public
COPY --from=builder --chown=appuser:nodejs /app/.next/standalone ./
COPY --from=builder --chown=appuser:nodejs /app/.next/static ./.next/static

USER appuser
EXPOSE 3000
CMD ["node", "server.js"]
