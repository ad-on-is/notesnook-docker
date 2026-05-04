# Build Stage
FROM node:22 AS builder

WORKDIR /app

# Install git & cleanup cache
RUN apt-get update && apt-get install -y --no-install-recommends git \
    && rm -rf /var/lib/apt/lists/*

# Clone Notesnook monorepo
#RUN git clone --depth=1 https://github.com/beardedtek/notesnook.git .
RUN git config --global user.email "you@example.com"
RUN git config --global user.name "Your Name"
RUN git clone --branch v3.3.17 --single-branch https://github.com/streetwriters/notesnook.git .
#RUN git remote add beardedtek https://github.com/beardedtek/notesnook.git
#RUN git fetch beardedtek
#RUN git cherry-pick  -X theirs 027d602

# Install dependencies (monorepo root)
RUN npm ci

# Build just the web app
RUN npm run build:web

# Production Stage
FROM nginx

# Remove default nginx site
RUN rm -rf /usr/share/nginx/html/*

# Copy build output from builder
COPY --from=builder /app/apps/web/build /usr/share/nginx/html

# Custom nginx config for SPA
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]