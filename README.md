# router

Simple Traefik router. Good for local development.

# Usage

## TLS

The first step is to copy your HTTPS certificate and key into the `certs/` directory. Name your certificate `fullchain.pem` and your key `privkey.pem`. That's the default names from Let's Encrypt.

## Using this router

Just start the router itself with the following command on the root of this repository:

    $ docker-compose up -d

Now you can configure your containers to use it. Here's an example:

```yaml
version: '3.1'

services:
  my_service:
    image: my/service
    labels:
      - "traefik.enable=true"
      - "traefik.frontend.entryPoints=http"
      # assuming you redirect *.local.yourdomain to 127.0.0.1:
      - "traefik.frontend.rule=Host:my_service.local.yourdomain"
    networks:
      - router
    restart: always

networks:
  router:
    external:
      name: router
```

When you start the `my_service` container Traefik will automatically detect it and redirect all requests of `my_service.localhost` to the container.
