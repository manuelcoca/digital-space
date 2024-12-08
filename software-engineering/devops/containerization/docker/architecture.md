# Architecture

### Container Ecosystem Layers

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-05-24 at 18.03.08.png" alt=""><figcaption></figcaption></figure>

### Docker Engine

<div align="left"><figure><img src="../../../../.gitbook/assets/Screenshot 2023-05-24 at 18.03.43.png" alt=""><figcaption></figcaption></figure></div>

### Docker Daemon

* Core component for building, managing, and running containers.
* Uses Linux namespaces and control groups (cgroups) to isolate containers from each other and the host.
* Provides a dedicated network stack to each container.
* Depending on the configuration, containers can communicate with each other, the host, and/or the rest of the world.
* By default, it is only reachable via a Unix socket on the local machine.

### Docker Client

* Client for communicating with the Docker daemon.
* Available for most operating systems (Windows, macOS, Linux).
* On Linux, by default, uses a local Unix socket to connect to the Docker daemon.
* Docker CLI Documentation: [https://docs.docker.com/engine/reference/commandline/docker/](https://docs.docker.com/engine/reference/commandline/docker/)

### Docker Registry

* A storage and content delivery system for Docker images
* Types of Registries:
  * Public Registries: Docker Hub as the most popular, allowing users to store, manage, and share images publicly. Supports automated builds and webhooks for CI/CD integration.
  * Private Registries: Can be hosted on-premises or via cloud services. Control who can access your images. Examples: [Azure Container Registry,](../cloud/acr-azure-container-registry.md) Docker Trusted Registry or Harbor.
* Functionality:
  * Image Storage: Stores Docker images in layers, which is efficient storage and transfer since only changed layers need to be downloaded or uploaded.
  * Image Management: Tools to manage images, like versioning (tags), image deletion, and garbage collection to remove unused layers.
  * Security: Image signing and scanning for vulnerabilities, ensuring that images are secure before deployment.
  * Access Control: Access management through user accounts or integration with identity providers
