## Network Drivers

-   **Bridge (default)**

    -   For container communication on single host
    -   Creates private internal network
    -   Assigns internal IP to containers
    -   Ports must be mapped to access from outside

-   **Host**

    -   Containers use host's network interfaces directly
    -   No separate container IP
    -   No port mapping needed
    -   Port numbers can only be used once

-   **Overlay**

    -   Spans multiple hosts
    -   Used for Docker Swarm mode

-   **None**
    -   No network attachment
    -   No external or container-to-container connections

![Docker default networks](../../../../../_assets/default_docker_networks.PNG)

## Features

-   **Embedded DNS**

    -   Containers can discover each other by name

    ![Docker embedded DNS](../../../../../_assets/embedded_dns.PNG)

-   **Link**

    -   Legacy feature for container connections

    ![Docker link feature](../../../../../_assets/docker_link.png)

## Common Commands

-   List networks: `docker network ls`
-   Create network: `docker network create --driver bridge <name>`
-   Run with network: `docker run --network=<name>`
-   Port mapping: `docker run -p 80:8080`
-   Port range: `docker run -p 8000-9000:8080`
-   Connect/disconnect: `docker network (dis)connect <network-name> <container-name>`
-   Inspect network: `docker network inspect <name>`
