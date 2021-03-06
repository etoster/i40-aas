FROM node:lts-alpine as builder

RUN apk update && apk upgrade && \
    apk add --no-cache git openssh bash postgresql postgresql-contrib netcat-openbsd

WORKDIR /wdir

## copy package.json first and proceed to install dependencies to leverage caching as much as possible
##   1. install production dependencies and create a copy of node_modules to be reused in prod stage below
##   2. install dev dependencies
COPY cmd/storage-adapter-mongodb/package.json /wdir
RUN npm install --production \
    && mkdir /production-dependencies/ \
    && cp -R node_modules /production-dependencies/
RUN npm install

# Add & compile sourcecode
COPY cmd/storage-adapter-mongodb/ /wdir
RUN npm run clean \
    && npm run build

###################################
FROM node:lts-alpine as prod

RUN adduser -D aasuser

WORKDIR /wdir

COPY --from=builder /wdir/dist /wdir/dist
COPY --from=builder /production-dependencies/node_modules /wdir/node_modules
COPY cmd/storage-adapter-mongodb/package.json /wdir

USER aasuser
EXPOSE 3000

ENTRYPOINT [ "npm", "start" ]
