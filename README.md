# wAVdio

wAVdio, the wireless audio-video guide, is a mobile-first 
full stack web application for museums that allows 
managing and presenting multi-media content.

This project provides a `docker-compose` file for starting
up the frontend, backend, and database containers together.

# Start wAVdio

```bash
docker-compose up -d
```

# Stop wAVdio

```bash
docker-compose down
```

# Development

The following sections list useful commands for running individual containers,
which might be useful for debugging.

## MongoDB

Run a MongoDB container and run the `mongo` client in the same container:

```bash
$ docker run --name mongo-server --detach --rm mongo
$ docker exec --interactive --tty mongo-server bash
```

Run a MongoDB container and run the `mongo` client from a separate container
in the same network:

```bash
$ docker network create mongo-net
$ docker run --name mongo-server --network mongo-net --detach --rm mongo
$ docker run --name mongo-client --interactive --tty --network mongo-net --rm mongo mongo --host mongo-server 
```

Further information about the MongoDB Docker image can be found on its
Docker Hub page: https://hub.docker.com/_/mongo
