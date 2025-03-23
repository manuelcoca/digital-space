## Container Ecosystem Layers

![Container Ecosystem Layers](../../../../../_assets/docker_container_ecosystem.png)

## Docker Engine

![Docker Engine](../../../../../_assets/docker_engine.png)

## Docker Engine Components

-   **Docker Daemon**

    -   Core component for building, managing, and running containers
    -   Uses Linux namespaces and cgroups to isolate containers
    -   Provides dedicated network stack to each container
    -   By default, only reachable via local Unix socket

-   **Docker Client**

    -   Communicates with Docker daemon
    -   Available for Windows, macOS, and Linux
    -   Uses local Unix socket to connect to daemon (Linux default)
    -   Documentation: [https://docs.docker.com/engine/reference/commandline/docker/](https://docs.docker.com/engine/reference/commandline/docker/)

-   **Docker Registry**
    -   Storage and content delivery system for Docker images
    -   Types:
        -   Public (Docker Hub - most popular)
        -   Private (on-premises or cloud services)
    -   Functionality:
        -   Stores images in layers for efficient storage/transfer
        -   Provides image management tools (versioning, deletion)
        -   Includes security features (signing, vulnerability scanning)
        -   Offers access control via user accounts or identity providers
