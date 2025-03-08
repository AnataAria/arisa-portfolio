FROM node:20-alpine AS base

FROM base AS installer
WORKDIR /installer
COPY package.json yarn.lock ./
RUN yarn install && yarn cache clean
COPY . .
ARG PUBLIC_API_ENDPOINT
ENV NEXT_PUBLIC_API_ENDPOINT=$PUBLIC_API_ENDPOINT
RUN yarn build

FROM base AS runner
WORKDIR /app
RUN addgroup --system --gid 1001 ariportfolio
RUN adduser --system -uid 1001 ariportfolio
USER ariportfolio

COPY --from=installer --chown=ariportfolio:ariportfolio /installer/.next/standalone ./
COPY --from=installer --chown=ariportfolio:ariportfolio /installer/.next/static ./.next/static
COPY --from=installer --chown=ariportfolio:ariportfolio /installer/public ./public

ENTRYPOINT [ "node", "server.js" ]
