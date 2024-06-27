# Docker Registry

* Central component for storing and distributing Docker images
* By default, public DockerHub is used by Docker Engine
  * That means, when you pull an image and don't declare a registry the default registry docker hub (docker.io) is assumed
* Docker Registry Open-Source available for self-hosting
* Implementation available from different vendors. E.g:
  * Harbor - https://www.goharbor.io (open-source)

### Docker Naming Scheme

<mark style="color:green;">\[registry]</mark><mark style="color:blue;">\[repository]</mark><mark style="color:orange;">:\[tag]</mark>

<mark style="color:green;">mycompany.com</mark><mark style="color:blue;">/myproject/mycontainer</mark><mark style="color:orange;">:v1.1.0-alpine</mark>

### Public Registry

* Anyone can access images in public registries
* There are many public registries for example Google's registry where a lot of kubernetes related images are stored:
  * gcr.io

### Private Registry

* For images built in-house
* To find images in private registries we first need to login:
  * **`$ docker login <registry-name>`**

