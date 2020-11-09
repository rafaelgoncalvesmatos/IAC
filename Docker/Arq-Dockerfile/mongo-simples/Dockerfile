FROM mongo:latest AS base
LABEL maintainer="Rafael Goncalves"

WORKDIR /app

COPY config/mongod.conf /etc

EXPOSE 27017
# VOLUME [ "mongodb:/data/db" ]