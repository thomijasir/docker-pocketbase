# PocketBase Docker Project

This project provides a Dockerized setup for running [PocketBase](https://pocketbase.io/), an open-source backend in a single file.

## Author

*   Thomi Jasir

## Prerequisites

*   [Docker](https://www.docker.com/get-started)
*   [Docker Compose](https://docs.docker.com/compose/install/)

## Configuration

The main configuration is handled within `docker-compose.yml` and `Dockerfile`:

*   **PocketBase Version (`PB_VERSION`)**: Set in `docker-compose.yml` (under `build.args`) and `Dockerfile` (as `ARG`).
    *   Current version: `0.28.1`
*   **Port (`PB_PORT`)**: The port PocketBase listens on inside the container and the port exposed on the host.
    *   Defined in `docker-compose.yml` (under `build.args` and `ports`) and `Dockerfile` (as `EXPOSE` and in `CMD`).
    *   Current port: `3546`

## How to Run

1.  **Clone the repository (if you haven't already):**
    ```bash
    git clone <your-repository-url>
    cd docker-pocketbase
    ```

2.  **Build and run the container:**
    Use Docker Compose to build the image and start the container in detached mode:
    ```bash
    docker-compose up --build -d
    ```

3.  **Access PocketBase:**
    Once the container is running, you can access the PocketBase Admin UI at:
    [http://localhost:3546/_/](http://localhost:3546/_/)

    The API will be available at `http://localhost:3546/api/`.

## Data Persistence

Data is persisted using Docker volumes mapped to local directories:

*   `./pb_data:/pb/pb_data`: Stores the main PocketBase database (`data.db`) and other application data.
*   `./pb_migrations:/pb/pb_migrations`: Stores database migration files. It's recommended to commit these to your version control.
*   `./pb_hooks:/pb/pb_hooks`: Stores your custom Go hook files. It's recommended to commit these if you have custom hooks.

The `pb_data` directory is ignored by Git (see `.gitignore`).

## Stopping the Container

To stop the container:
```bash
docker-compose down
```
To stop and remove volumes (including your `pb_data` if it's an anonymous volume, but here it's a bind mount so your local `./pb_data` directory persists):
```bash
docker-compose down -v
```
However, since `./pb_data` is a bind mount, your data will remain in the local directory even after `docker-compose down -v`.

## Customizing

*   **PocketBase Version**: Modify `PB_VERSION` in `docker-compose.yml` and `Dockerfile`.
*   **Port**: Modify the port in `docker-compose.yml` (ports section and build args) and `Dockerfile` (`EXPOSE` and `CMD`).
*   **Hooks**: Add your Go hook files to the `./pb_hooks` directory.
*   **Migrations**: PocketBase will automatically create migration files in `./pb_migrations` as you change your collections in the Admin UI.
