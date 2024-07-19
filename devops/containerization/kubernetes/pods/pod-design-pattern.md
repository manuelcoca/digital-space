---
description: >-
  With pod design patterns you think about closely coupled groups of containers
  that cooperate to provide a service
---

# Pod Design Pattern

## Sidecar Pattern

* Sidecar containers extend and enhance the "main" container
* Sidecar container can be developed and tested once and reused across numerous apps

<div align="left">

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-05-24 at 17.32.32.png" alt=""><figcaption></figcaption></figure>

</div>

### Advantages

* Seperate resource allocation per process
* Clean boundaries and seperation of responsibility
* Improved reuse

## Ambassador Pattern

* Ambassador containers proxy communication to and from a main container
* Ambassadors are possible because containers on the same machine share the same localhost network interface
* Examples
  * Cache Proxy
  * ServiceMesh Proxy

<div align="left">

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-05-24 at 17.34.35.png" alt=""><figcaption></figcaption></figure>

</div>

### Advantages

* Think and program in terms of the application connecting to a single server on localhost
* Test the application standalone by running without an ambassador on a local machine

## Adapter Pattern

* Adapters present the outside world with a simplified view of an application
* Standardizing output and interfaces across multiple containers

<div align="left">

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-05-24 at 17.37.16.png" alt=""><figcaption></figcaption></figure>

</div>

### Advantages

Uniform interface without requiring modification of the original application
