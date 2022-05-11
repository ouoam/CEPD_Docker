## Stage 1 (production base)
# This gets our prod dependencies installed and out of the way
FROM node:14-alpine as base
EXPOSE 3000
ENV NODE_ENV=production
# WORKDIR /opt
COPY package*.json ./
RUN npm install && \
    npm ci --progress=false --no-audit --loglevel=error


## Stage 2 (development)
FROM base as dev
ENV NODE_ENV=development
# ENV PATH=/opt/node_modules/.bin:$PATH
# WORKDIR /opt
RUN npm install --only=development
# WORKDIR /opt/app
COPY . .
CMD ["node", "./src"]

## Stage 3 (copy in source)
# This gets our source code into builder for use in next two stages
# It gets its own stage so we don't have to copy twice
# this stage starts from the first one and skips the last two
FROM base as source
# WORKDIR /opt/app
COPY . .

## Stage 4 (default, production)
# this will run by default if you don't include a target
# it has prod-only dependencies
# Least Privilege: Using node User for production
FROM source as prod
# USER node
CMD ["node", "./src"]
