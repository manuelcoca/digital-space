## Docker Filesystem

-   All Docker data stored in a dedicated filesystem path

![Docker filesystem](../../../../../_assets/docker_filesystem.png)

## Image Layers

-   Docker images consist of ordered read-only layers
-   Each Dockerfile command creates a new image layer
-   Example layers from a Dockerfile:
    ```docker
    FROM Ubuntu
    RUN apt-get update && apt-get -y install python
    RUN pip install flask
    COPY . /opt/source-code
    ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
    ```

![Storage layers](../../../../../_assets/storage_layers.png)

## Container Layer

-   When a container launches, Docker adds a writable layer on top of image layers
-   All runtime changes stored in this writable layer:
    -   New files
    -   Modified files
    -   Deleted files
-   Data in this layer is deleted when container is removed
