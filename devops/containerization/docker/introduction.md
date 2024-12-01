# Introduction

### Docker purpose

#### Without Docker

* Is the application compatible with the version of OS we planned to use?
* You have to check compatibility between services, libraries, and dependencies on the OS.
* The architecture of the application changes over time, necessitating upgrades to newer versions or changes to the database.
* Every time something changes, you must go through the process of checking compatibility.

#### With Docker

* No more problems with compatibility.
* Docker allows us to modify or change components without affecting other components.

### Linux Kernel

* All Linux operating systems are based on the Linux kernel.
* It's the software above the kernel that differentiates these OS (Debian, Ubuntu, etc.).
* Docker containers are also based on the Linux kernel.
* This means you can run any kind of OS based on the Linux kernel on a Linux system with Docker.
* That also means a Docker container cannot be run directly on Windows, and other OS cannot run on top of Docker without additional layers.
* Windows runs Linux containers on a Linux virtual machine within Windows.
* However, the main purpose of Docker is not to run OS on Docker.

### Containers

* The main purpose of Docker is to package and containerize applications, to ship and run them anywhere.
* Containers are isolated environments; they can have their own processes, network interfaces, etc.

### Images

* Images are read-only templates for creating one or more containers.
* Containers are running instances of images that are isolated and have their own environments.
* Images are built from a base image using a set of instructions:
  * FROM - Definition of the base image.
  * MAINTAINER - Who maintains this image. (Note: This instruction is deprecated in newer Docker versions; use LABEL instead)
  * RUN - A command to execute on top of the base image.
  * ADD - Add a file or directory to the new image.
  * ENV - Create an environment variable.
  * ENTRYPOINT - Command to run when a container is launched from this image.
  * CMD - Default command or parameters passed to the entrypoint.

#### Example

```docker
FROM java:8
ADD my-service-fat.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```
