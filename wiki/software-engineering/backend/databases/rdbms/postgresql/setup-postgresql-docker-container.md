## Create PostgreSQL Container

### Step 1: Download PostgreSQL Image

```sh
docker pull postgres
```

Image can also be found at Docker Hub:[ https://hub.docker.com/\_/postgres/](https://hub.docker.com/_/postgres/).

### Step 2: Create a PostgreSQL Container

Create a container from the image:

```sh
docker run --name test-container -e POSTGRES_PASSWORD=test-password -d -p 5432:5432 postgres
```

-   **“test-container”** is the name of the container.
-   **“test-password”** is the database password for the “postgres” user.
-   **“-d”** option runs the container in the background.
-   **“-p 5432:5432”** option maps port 5432 from the container to port 5432 on the host.

Manage database with [pgAdmin4](database-management-with-pgadmin4.md).\
