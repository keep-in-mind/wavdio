# wAVdio

wAVdio, the wireless audio-video guide, is a mobile-first full stack web application for museums that allows managing
and presenting multi-media content.

This project provides a `docker-compose` file for starting up the frontend, backend, and database containers together.

# Start wAVdio

```bash
docker-compose up -d
```

# Stop wAVdio

```bash
docker-compose down
```

# Development

The following sections list useful commands for running individual containers, which might be useful for debugging.

## MongoDB

Run a MongoDB container and run the `mongo` client in the same container:

```bash
$ docker run --name mongo --rm --detach mongo
$ docker exec --interactive --tty mongo bash
```

Run a MongoDB container and run the `mongo` client from a separate container in the same network:

```bash
$ docker network create mongo-net
$ docker run --name mongo  --rm --network mongo-net --detach mongo
$ docker run --name mongo-client --rm --interactive --tty --network mongo-net mongo mongo --host mongo 
```

Further information about the MongoDB Docker image can be found on its Docker Hub page: https://hub.docker.com/_/mongo

## wavdio-express

Given a running `mongo` container on the `mongo-net` network, the following command starts the `wavdio-express`
container and forwards its port to `localhost:80`:

```bash
$ docker run --name wavdio-express --rm --network mongo-net --publish 80:3000 xvlcw/wavdio-express node ./bin/server.js --db-uri mongodb://mongo:27017/
```

As soon as the container is running, the API can be tested at http://localhost/api/v2/museum.
