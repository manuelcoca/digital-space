# Docker Registry

* Central component for storing and distributing Docker images
* By default, the public Docker Hub is used by Docker Engine.
* This means that when you pull an image and do not declare a registry, the default registry, Docker Hub (docker.io), is assumed.
* Docker Registry is available as an open-source for self-hosting.
* There are implementations from various vendors, for example:
  * Harbor - [https://www.goharbor.io](https://www.goharbor.io/) (open-source)

### Docker Naming Scheme

<mark style="color:green;">\[registry]</mark><mark style="color:blue;">\[repository]</mark><mark style="color:orange;">:\[tag]</mark>

<mark style="color:green;">mycompany.com</mark><mark style="color:blue;">/myproject/mycontainer</mark><mark style="color:orange;">:v1.1.0-alpine</mark>

### Public Registry

* Anyone can access images in public registries.
* There are many public registries; for instance, Google's registry, where many Kubernetes-related images are stored:
  * gcr.io

### Private Registry

* For images built in-house.
* To access images in private registries, we first need to log in:
  * `$ docker login <registry-name>`
