#syntax=docker/dockerfile:1.4

# pin versions
FROM ghcr.io/onlishop/docker-base:8.2 AS base-image
FROM ghcr.io/onlishop/onlishop-cli:latest-php-8.2 AS onlishop-cli

# build

FROM onlishop-cli AS build

ARG ONLISHOP_PACKAGES_TOKEN

ADD . /src
WORKDIR /src

RUN --mount=type=secret,id=composer_auth,dst=/src/auth.json \
    --mount=type=cache,target=/root/.composer \
    --mount=type=cache,target=/root/.npm \
    /usr/local/bin/entrypoint.sh onlishop-cli project ci /src

# build final image

FROM base-image

COPY --from=build --chown=www-data --link /src /var/www/html
