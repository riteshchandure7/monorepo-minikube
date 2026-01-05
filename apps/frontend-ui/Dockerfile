FROM node:20-alpine AS build
WORKDIR /repo

RUN corepack enable

# Copy workspace files
COPY pnpm-lock.yaml package.json pnpm-workspace.yaml ./

# Copy app manifest
COPY apps/frontend-ui/package.json apps/frontend-ui/package.json

# Install ONLY frontend deps (workspace-aware)
RUN pnpm install --filter frontend-ui --frozen-lockfile

# Copy source
COPY apps/frontend-ui apps/frontend-ui

# 🔑 RUN BUILD FROM ROOT WITH FILTER
RUN pnpm --filter frontend-ui build

FROM nginx:alpine
COPY --from=build /repo/apps/frontend-ui/dist /usr/share/nginx/html
