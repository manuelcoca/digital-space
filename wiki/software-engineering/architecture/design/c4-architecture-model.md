-   UML and other architecture model system are often overkill and irrelevant in today's agile environments
-   Aims to help architectures describe and communicate software architecture in agile environments
-   Suggests 4 abstraction levels:
    1.  **Software system:** Software system and other dependencies
    2.  **Container:** Applications and data stores
    3.  **Component:** Set of functions, modules, interfaces etc.
    4.  **Code:** Single classes, functions, interfaces etc.

## Notations

-   C4 model is notation independent
-   Custom colors, shapes and icons are allowed
-   Diagram should have a legend if custom elements are used
-   Recommended stylings:
    ![[c4-notations-relationships.png]]

![[c4-notations-elements.png]]

## Abstractions

### Software system

-   Something a single software development team is building, owns, has responsibility for
-   Often a system that is deployed together
    ![[c4-software-system-elements.png]]

### Container

-   Something that needs to be running in order for the overall software system to work:
    -   Web application
    -   Desktop application
    -   Mobile application
    -   Console application
    -   Severless function
    -   Database
    -   Blob storage
    -   File system
    -   etc.
-   Deployment is a separate concern, because it will likely vary across different stages. Clustering, load balancers, replication, failover is better captured via one or more deployment diagrams.
    ![[c4-container-elements.png]]

### Component

-   Not separately deployable units
-   Only create when they add value, and consider automating their creation for long-lived documentation
    ![[c4-components-elements.png]]

### Code

-   Can use UML class diagrams, entity relationsship models etc.
    ![[c4-code-example.png]]

## Additional diagrams

### Dynamic diagrams

-   Useful to show how elements in the static model collaborate at runtime to implement a user story, use case, feature, etc.
-   Based on UML sequence diagram, but more freedom
    ![[c4-dynamic-diagram.png]]

### Deployment diagram

-   Based on UML deployment diagram
-   Illustrate how instances of software systems and/or containers in the static model are deployed on to the infrastructure within a given deployment environment
    ![[c4-deployment-diagram-1.png]]
    ![[c4-deployment-diagram-2.png]]
    ![[c4-deployment-diagram-3.png]]

## Microservices

-   Are services owned by multiple teams?
    -   No -> Software system covers all systems, container covers all services
    -   Yes -> Software system covers systems and container services that are owned by a specific team
-   Are services implementation detail of a single software system or seperate software systems?

## Tools

-   **Miro template:** https://miro.com/miroverse/c4-architecture/
-   **C4InterFlow:** https://github.com/SlavaVedernikov/C4InterFlow
