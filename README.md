# router

Simple Traefik router. Good for local development and simple deployments.

# Usage

## TLS

To configure Let's Encrypt we are usind DNS Challenge.

The included example uses DigitalOcean's API but you can check other providers [here](https://doc.traefik.io/traefik/https/acme/#providers)

Create a `.env` based on `.env.dist`.

## Using this router

### Configure the `web` network

You need to create a `web` network for containers that should be exposed by the router.

    $ docker network create --driver bridge --subnet your:ipv6:subnet/124 --ipv6 web

### Start Router

Start the router itself with the following command:

    $ docker-compose up -d

### Configure your services

Now you can configure your containers to use it. Here's an example:

```yaml
version: '3.5'

services:
  my_service:
    image: my/service
    labels:
      - traefik.enable=true
      - traefik.http.routers.donato.rule=Host(`example.com`)
      - traefik.http.routers.donato.tls=true
      - traefik.http.routers.donato.tls.certResolver=lets-encrypt
      - traefik.http.routers.donato.middlewares=secHeaders@file
      - traefik.port=80
    networks:
      - web

networks:
  web:
    external: true
```

When you start the `my_service` container Traefik will automatically detect it and redirect all requests for `example.com` to the container.
