# Admission Controllers

With RBAC its possible to restrict access to api-resources. For advanced restrictions like:

* Check yaml configuration on creating Pods an don't allow images from unkown private registries
* Don't allow to run containers as root user
* Don't allow specific image versions
* Enforce yaml configs to always have metadata specified

Admission Controllers can be used.
