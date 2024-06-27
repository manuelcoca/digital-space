# Architecture

### Container Ecosystem Layers

<figure><img src="../.gitbook/assets/Screenshot 2023-05-24 at 18.03.08.png" alt=""><figcaption></figcaption></figure>

### Docker Engine

<div align="left">

<figure><img src="../.gitbook/assets/Screenshot 2023-05-24 at 18.03.43.png" alt=""><figcaption></figcaption></figure>

</div>

### Docker Daemon

* Core component for building, managing and running containers
* Uses Linux namespaces and control groups (cgroups) to isolate containers from each other and the host
* Provides dedicated network stack to each container
  * Depending on configuration containers can communicate with each other, the host and/or the rest of the world
* By default only reachable via Unix socket on the local machine

### Docker Client

* Client for communicating with Docker daemon
* Available for most OS (Windows, MacOs, Linux)
* On Linux by default uses local Unix socket to connect to Docker daemon
* Docker CLI documentation:
  * [https://docs.docker.com/engine/reference/commandline/docker/](https://docs.docker.com/engine/reference/commandline/docker/)&#x20;

### Docker Registry

*
