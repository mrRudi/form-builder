FROM node:18-alpine AS base

RUN apk update
RUN apk add --no-cache libc6-compat
RUN npm i -g npm@latest
RUN npm i -g pnpm


FROM base AS deps
WORKDIR /app

COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile --prod


FROM base AS builder
WORKDIR /app
COPY . .
COPY --from=deps /app/node_modules ./node_modules

ENV NEXT_TELEMETRY_DISABLED 1

RUN pnpm build
# Remove packages in devDependencies
RUN pnpm prune --prod



FROM base AS runner
WORKDIR /app

ENV NODE_ENV production
ENV NEXT_TELEMETRY_DISABLED 1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/public ./public
COPY --from=builder /app/next.config.js ./

USER nextjs
EXPOSE ${FRONT_SERVICE_PORT}
ENV PORT ${FRONT_SERVICE_PORT}

CMD ["node", "./server.js"]
