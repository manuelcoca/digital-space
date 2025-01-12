# Database Management with pgAdmin4

pgAdmin4 is a open-source database management tool, a complete rewrite of pgAdmin3 enhancing it with modern UI and more features. It can run standalone via an Electron desktop runtime or be deployed on a web server.

### **Run pgadmin4 in Docker Container**

#### Step 1: Download pgadmin4 Image

```sh
docker pull dpage/pgadmin4
```

#### Step 2: Create Docker Container

```sh
docker run --name test-pgadmin -p 15432:80 -e "PGADMIN_DEFAULT_EMAIL=my_email@test.com" -e "PGADMIN_DEFAULT_PASSWORD=my_password" -d dpage/pgadmin4
```

* **“test-pgadmin”** is the name of the container.
* **"-p 15432:80”** option maps port 15432 from the container to port 80 on the host.
* **“PGADMIN\_DEFAULT\_EMAIL”** will be the login to access pgAdmin.
* **“PGADMIN\_DEFAULT\_PASSWORD”** will be the password to access pgAdmin.

pgAdmin UI can be accessed at[ http://localhost:15432](https://localhost:15432/).
