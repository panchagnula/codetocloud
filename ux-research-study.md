# Azure Container Apps CLI research study

## App overview

You'll be working with the app in this repository. It consists of two projects.

- `aca-frontend` - A Node.js app that serves the web frontend UI for a music store. It calls a web API to retrieve albums data.
- `aca-node` - The web API that serves album data at `/albums`.

```mermaid
flowchart LR
    b([browser]) 
    -- "http(s)://{frontend-url}" --> 
    w(aca-frontend)
    -- "http(s)://{api-url}" --> 
    a(aca-node)
```

There are other projects in the repository that implement the same albums API in other languages.

## Container images

For this study, you won't be working directly with the code; we've created Docker images for both the frontend and web API apps.

These images are in a public repository and do not require authentication.

- `albums.azurecr.io/aca-frontend:1.0`
    * Exposed port: 3000
    * Environment variables:
        * `API_BASE_URL` - The base URL of the web API.
        * `BACKGROUND_COLOR` (optional) - The background color of the UI.
- `albums.azurecr.io/aca-node:1.0`
    * Exposed port: 3000

### Run the app locally (optional)

If you have Docker Desktop installed on your machine, you can run the app locally to see how it works.

You can use Docker Compose to run both services at once.

1. Create a file named `docker-compose.yml`.

    ```yaml
    version: "3"
      
    services:
      frontend:
        image: albums.azurecr.io/aca-frontend:1.0
        environment:
          - API_BASE_URL=http://api:3000
          - BACKGROUND_COLOR=blue
        ports:
          - "3000:3000"
        depends_on:
          - api

      api:
        image: albums.azurecr.io/aca-node:1.0
        environment:
          - API_BASE_URL=http://api:3000
        expose:
          - "3000"
    ```

1. Open a terminal to the location you created the file and run the following command:

    ```bash
    docker-compose up
    ```

1. Open your browser to http://localhost:3000.

1. When finished, use `Ctrl-C` to stop the app.

