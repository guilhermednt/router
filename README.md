# router

Simple Traefik router. Good for local development.

# Usage

First start the HTTP router itself. Run the following command on the route of this repository:

    $ docker-compose up -d

Now you can configure your containers to use it. Here's an example:

```yaml
version: '3.1'

services:
  my_service:
    image: my/service
    labels:
      - "traefik.frontend.entryPoints=http"
      - "traefik.frontend.rule=Host:my_service.localhost" # assuming you redirect *.localhost to 127.0.0.1
    networks:
      - router
    restart: always

networks:
  router:
    external:
      name: router
```

When you start the `my_service` container Traefik will automatically detect it and redirect all requests of `my_service.localhost` to the container.
