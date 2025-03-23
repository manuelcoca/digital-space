## Image Commands

-   Create: `docker build -t <image-name> -f <dockerfile> .`
-   List: `docker images`
-   Delete: `docker rmi <image-name>`
-   Tag: `docker tag <source-image> <target-image>`

## Container Commands

-   Create and run: `docker run [options] <image>`
-   Start: `docker start <container-name>`
-   Stop: `docker stop <container-name>`
-   Restart: `docker restart <container-name>`
-   Delete: `docker rm <container-name>`
-   List running: `docker ps`
-   List all: `docker ps -a`
-   Execute command: `docker exec <container> <command>`
-   Copy files: `docker cp <source> <container>:<path>`
-   View logs: `docker logs <container-name>`
-   Rename: `docker rename <old-name> <new-name>`
-   Update config: `docker update <container> [options]`
-   Resource stats: `docker stats`
-   Inspect: `docker inspect <container-name>`

## Registry Commands

-   Push: `docker push <image-name>`
-   Pull: `docker pull <image-name>`
-   Login: `docker login [registry]`
-   Logout: `docker logout [registry]`
-   Search: `docker search <term>`

## Network Commands

-   List networks: `docker network ls`
-   Create: `docker network create --driver bridge <name>`
-   Run with network: `docker run --network=<name>`
-   Port mapping: `docker run -p 80:8080`
-   Port range: `docker run -p 8000-9000:8080`
-   Connect/disconnect: `docker network (dis)connect <network> <container>`
-   Inspect: `docker network inspect <name>`
-   List ports: `docker port <container>`
