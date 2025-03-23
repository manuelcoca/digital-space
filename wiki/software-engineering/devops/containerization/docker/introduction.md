## Purpose of Docker

-   Solves compatibility problems between applications and operating systems
-   Allows modification of components without affecting others
-   Packages and containerizes applications to run anywhere

## Linux Kernel and Docker

-   Docker containers are based on the Linux kernel
-   Can run any Linux-based OS on a Linux system with Docker
-   Windows runs Linux containers through a Linux VM
-   Docker's main purpose is containerizing applications, not running OS

## Containers

-   Isolated environments with own processes and network interfaces
-   Package applications with dependencies to run consistently anywhere

## Images

-   Read-only templates for creating containers
-   Built from base images using instructions:
    -   FROM - Base image definition
    -   RUN - Command execution
    -   ADD - Add files/directories
    -   ENV - Environment variables
    -   ENTRYPOINT - Command to run at container launch
    -   CMD - Default parameters for entrypoint

### Example Dockerfile

```docker
FROM java:8
ADD my-service-fat.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```
