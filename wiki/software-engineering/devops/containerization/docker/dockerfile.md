-   Text file containing instructions to build a Docker image
-   Each instruction creates a layer in the image
-   Format: `INSTRUCTION arguments`

## Common Instructions

-   **FROM**: Specifies the base image
-   **RUN**: Executes commands in a new layer
-   **COPY/ADD**: Copies files from build context to the image
-   **WORKDIR**: Sets the working directory
-   **ENV**: Sets environment variables
-   **EXPOSE**: Documents which ports the container listens on
-   **VOLUME**: Creates a mount point for external volumes
-   **CMD**: Provides defaults for an executing container
-   **ENTRYPOINT**: Configures container to run as an executable

## Best Practices

-   Use official images as base
-   Use specific tags instead of `latest`
-   Minimize the number of layers (combine RUN commands)
-   Remove unnecessary dependencies
-   Use .dockerignore file
-   Sort multi-line arguments
-   Build context should only include necessary files

## Multi-stage Builds

-   Use multiple FROM statements
-   Each FROM starts a new build stage
-   Copy artifacts between stages
-   Final image contains only needed artifacts

Example:

```dockerfile
# Build stage
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
