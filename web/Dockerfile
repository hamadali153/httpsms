# build stage
FROM node:22-alpine AS build

WORKDIR /app

COPY package.json pnpm-lock.yaml ./

# Install pnpm
RUN npm install -g pnpm

RUN pnpm install

COPY . .

# Build-time config (the site is generated statically, so these are baked in).
# Unset args stay out of the environment, so a local web/.env still works.
ARG API_BASE_URL
ARG APP_URL
ARG APP_NAME
ARG APP_ENV
ARG APP_GITHUB_URL
ARG APP_DOCUMENTATION_URL
ARG APP_DOWNLOAD_URL
ARG FIREBASE_API_KEY
ARG FIREBASE_AUTH_DOMAIN
ARG FIREBASE_PROJECT_ID
ARG FIREBASE_STORAGE_BUCKET
ARG FIREBASE_MESSAGING_SENDER_ID
ARG FIREBASE_APP_ID
ARG FIREBASE_MEASUREMENT_ID
ARG CLOUDFLARE_TURNSTILE_SITE_KEY
ARG PUSHER_KEY
ARG PUSHER_CLUSTER
ARG CHECKOUT_URL
ARG ENTERPRISE_CHECKOUT_URL
ARG GITHUB_SHA

RUN pnpm run generate

# production stage
FROM nginx:stable-alpine AS production
COPY --from=build /app/.output/public /usr/share/nginx/html

# Copy the nginx configuration file
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
