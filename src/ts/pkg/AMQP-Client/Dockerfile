FROM node:lts-alpine AS builder

WORKDIR /pkg/amqp-client

## copy package.json first and install dependencies to leverage caching
COPY package.json /pkg/amqp-client

## install dependencies
RUN npm install

## add & transpile sourcecode
COPY ./ /pkg/amqp-client

ENTRYPOINT [ "npm", "test" ]

