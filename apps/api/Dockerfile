# syntax=docker/dockerfile:1

############################
# Base image
############################
FROM node:22-slim AS base

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
ENV CI=true

RUN corepack enable


############################
# Build Go shared library
############################
FROM golang:1.24 AS go-build
WORKDIR /build

COPY apps/api/sharedLibs/go-html-to-md ./go-html-to-md

WORKDIR /build/go-html-to-md

RUN go mod download
RUN go build -o libhtml-to-markdown.so -buildmode=c-shared html-to-markdown.go


############################
# Build Node application
############################
FROM base AS build

WORKDIR /repo

RUN apt-get update && apt-get install -y \
    curl \
    build-essential \
    pkg-config \
    python3 \
    git \
    && rm -rf /var/lib/apt/lists/*


# Install Rust (required for native modules)
ENV RUSTUP_HOME=/usr/local/rustup
ENV CARGO_HOME=/usr/local/cargo
ENV PATH=/usr/local/cargo/bin:$PATH

RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --no-modify-path \
    && chmod -R a+w $RUSTUP_HOME $CARGO_HOME


# Copy full repository
COPY . .


############################
# Install dependencies
############################
WORKDIR /repo/apps/api

RUN pnpm install --frozen-lockfile


############################
# Build API
############################
RUN pnpm run build

RUN pnpm prune --prod --ignore-scripts


############################
# Runtime image
############################
FROM node:22-slim AS runtime

WORKDIR /app

RUN apt-get update && apt-get install -y \
    git \
    procps \
    && rm -rf /var/lib/apt/lists/*


############################
# Copy built API
############################
COPY --from=build /repo/apps/api/node_modules ./node_modules
COPY --from=build /repo/apps/api/dist ./dist
COPY --from=build /repo/apps/api/native ./native


############################
# Copy Go library
############################
RUN mkdir -p ./sharedLibs/go-html-to-md

COPY --from=go-build /build/go-html-to-md/libhtml-to-markdown.so \
    ./sharedLibs/go-html-to-md/


EXPOSE 8080

CMD ["node", "dist/src/harness.js", "--start-docker"]
