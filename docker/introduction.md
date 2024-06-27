# Introduction

### Docker purpose

#### Without Docker

* Is the Application compatible with the version of OS we were planning to use?
* You have to check compatibility between services, libaries and dependencies on the OS
* Architecture of application changes over time and you have to upgrade to newer versions or to change the database
* every time something changed we have to go trough the process of checking compatibility

#### With Docker

* no more problems with compatibility
* Docker allows us to modify or change components without affecting the other components

### Linux Kernel

* All linux operating systems are based on the linux kernel
  * it's the software above the kernel, which makes these OS different (Debian, Ubunutu etc.)
* Docker container also are based on the linux kernel
  * That means that you can run on a Linux system with docker any kind of OS which is based on linux kernel
  * That also means, that a docker container can't be run on a Windows and other OS can't be run on top
    * Windows runs a Linux container on a linux virtual machine on Windows
* But the main purpose is not to run OS on Docker

### Containers

* Main purpose of Docker is to package and containerize applications, to ship and run them anywhere
* Containers are isolated environments
  * They can have their own processes, network interfaces etc.

### Images

* Images are read-only templates for creating one or more containers
* Containers are running instances of images that are isolated and have their own environments
* Images are built from a base image using a set of instructions:
  * FROM - definition of base image
  * MAINTAINER - Who maintains this image
  * RUN - A command to execute on top of the base image
  * ADD - ADD a file or directory to the new image
  * ENV - Create an environment variable
  * ENTRYPOINT - Command to run when container is launched from this image
  * CMD - Default command or parameter passed to entrypoint

#### Example

`FROM java:8`

`ADD my-service-fat.jar app.jar`

`ENTRYPOINT ["java", "-jar", "/app.jar"]`
