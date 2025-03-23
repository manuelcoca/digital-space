-   Persist data beyond container lifecycle
-   Directory or file in host filesystem mounted directly to container
-   Operate at host speed

![Volume creation](../../../../../_assets/volume_create.png)

## Volume Initialization

-   Volumes mounted when container is created
-   If container image contains data at mount point, it's copied to new volume ![Volume initialization](../../../../../_assets/docker_volume_initialization.png)

## Mounting Types

-   **Volume Mounting**

    -   Mounts from `/var/lib/docker/volumes`
    -   Docker creates volume automatically if none exists

-   **Bind Mounting**
    -   Mounts any directory from host
    -   Example: Mount host's `/tmp/data` to container's `/usr/src/app/data`

![Volume mounting](../../../../../_assets/volume_mounting.png)
