# Build context is the repo root (pnpm workspace needs the root lockfile).
FROM node:20-alpine
RUN apk add --no-cache openssl && corepack enable
WORKDIR /app

COPY pnpm-workspace.yaml pnpm-lock.yaml package.json ./
COPY apps/api/package.json apps/api/package.json
COPY apps/web/package.json apps/web/package.json
RUN pnpm install --frozen-lockfile

COPY apps/api apps/api
RUN pnpm --filter api prisma:generate && pnpm --filter api build

WORKDIR /app/apps/api
ENV NODE_ENV=production
EXPOSE 4000
CMD ["sh", "-c", "npx prisma migrate deploy && node dist/src/main.js"]
