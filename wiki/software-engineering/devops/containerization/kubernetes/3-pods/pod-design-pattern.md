## Sidecar Pattern

-   Sidecar containers extend and enhance the "main" container
-   Sidecar container can be developed and tested once and reused across numerous apps ![[k8s_sidecar_pattern.png]]

### Advantages

-   Seperate resource allocation per process
-   Clean boundaries and seperation of responsibility
-   Improved reuse

## Ambassador Pattern

-   Ambassador containers proxy communication to and from a main container
-   Ambassadors are possible because containers on the same machine share the same localhost network interface
-   Examples
    -   Cache Proxy
    -   ServiceMesh Proxy ![[k8s_ambassador_pattern.png]]

### Advantages

-   Think and program in terms of the application connecting to a single server on localhost
-   Test the application standalone by running without an ambassador on a local machine

## Adapter Pattern

-   Adapters present the outside world with a simplified view of an application
-   Standardizing output and interfaces across multiple containers ![[k8s_adapter_pattern.png]]

### Advantages

-   Uniform interface without requiring modification of the original application
