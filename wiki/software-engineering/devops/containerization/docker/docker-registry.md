-   Central storage for Docker images
-   Docker Engine uses public Docker Hub by default
-   Available as open-source for self-hosting
-   Alternative implementations:
    -   Harbor - [https://www.goharbor.io](https://www.goharbor.io/) (open-source)

## Docker Naming Scheme

-   Format: `[registry][repository]:[tag]`
-   Example: `mycompany.com/myproject/mycontainer:v1.1.0-alpine`

## Registry Types

### Public Registry

-   Open access to all users
-   Examples:
    -   Docker Hub (docker.io)
    -   Google Container Registry (gcr.io)

### Private Registry

-   For proprietary/in-house images
-   Requires authentication:
    -   `docker login <registry-name>`
