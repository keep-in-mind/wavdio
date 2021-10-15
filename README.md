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

# Troubleshooting

In case wAVdio container network does not start via `docker-compose`, the following commands show how to manually set up
the container network step by step to find the cause of the problem:

## Docker

First, make sure both the Docker client and server are available and up to date:

```bash
$ docker version
```

## wavdio-mongo

With Docker up and running, execute the following commands to set up the common network for backend and database and
start the `wavdio-mongo` container:

```bash
$ docker network create wavdio-net
$ docker run --name wavdio-mongo --rm --network wavdio-net --detach mongo
```

You can test that the container is running and reachable from within the network by accessing it via the `mongo` client
from another container:

```bash
$ docker run --name mongo-client --rm --interactive --tty --network wavdio-net mongo mongo --host wavdio-mongo 
```

## wavdio-express

Given the running `wavdio-mongo` container on the `wavdio-net` network, the following command starts
the `wavdio-express` container and publishes its port at `localhost:3000`:

```bash
$ docker run --name wavdio-express --rm --network wavdio-net --publish 3000:3000 --detach xvlcw/wavdio-express node ./bin/server.js --db-uri mongodb://wavdio-mongo:27017/
```

You can then test the API at http://localhost:3000/api/v2/museum.

## wavdio-angular

Finally, the following command adds the `wavdio-angular` container to and publishes its port to `localhost:80`:

```bash
$ docker run --name wavdio-angular --rm --network wavdio-net --publish 80:80 --detach xvlcw/wavdio-angular
```

You can then run the app at http://localhost/.
