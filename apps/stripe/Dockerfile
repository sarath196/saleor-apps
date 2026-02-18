FROM node:22-alpine AS base

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"

RUN corepack enable

FROM base AS builder

WORKDIR /app

RUN apk add --no-cache libc6-compat

ARG SKIP_ENV_VALIDATION=1

ENV NEXT_TELEMETRY_DISABLED=1
ENV SKIP_ENV_VALIDATION=${SKIP_ENV_VALIDATION}

COPY . .

RUN pnpm install --frozen-lockfile
RUN pnpm --filter saleor-app-payment-stripe build

FROM base AS runner

WORKDIR /app

ENV NODE_ENV=production

RUN apk add --no-cache libc6-compat

COPY --from=builder /app ./

EXPOSE 3000

CMD ["pnpm", "--filter", "saleor-app-payment-stripe", "start"]
