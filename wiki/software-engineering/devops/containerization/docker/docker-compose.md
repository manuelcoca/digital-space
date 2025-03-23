-   Tool for defining and running multi-container Docker applications
-   Uses YAML files to configure application services
-   Simplifies development workflow with single commands

## Key Features

-   **Service Definition**

    -   Define all services in a single file
    -   Configure networks, volumes, and environment variables

-   **Single Commands for Environment Management**

    -   `docker-compose up`: Start all services
    -   `docker-compose down`: Stop all services
    -   `docker-compose build`: Rebuild services

-   **Environment Variables**

    -   Use .env files for configuration
    -   Override settings per environment

-   **Service Dependencies**
    -   Specify dependencies between services
    -   Automatic startup order management
