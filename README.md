# Docker Web Server

A static website served by Nginx inside a Docker container. The page shows live details about the container it's running in, filled in by Nginx on every request.

Built for the CodeAlpha DevOps Internship (Task 4: Web Server using Docker).

## Files

| File | Purpose |
|---|---|
| `index.html` | The web page, with Nginx server-side includes for the live container details |
| `default.conf` | Nginx configuration: enables server-side includes, disables caching, adds a `/health` endpoint |
| `Dockerfile` | Builds the image from `nginx:alpine`, copies in the page and config, and defines a health check |

## Run it

```
docker build -t my-webserver .
docker run -d -p 8080:80 --name webserver my-webserver
```

Open http://localhost:8080. After about 30 seconds, `docker ps` shows the container as `(healthy)`.

## Container lifecycle

| Command | What happens |
|---|---|
| `docker ps` | List running containers and their health status |
| `docker stop webserver` | Stop the container; the page stops loading |
| `docker start webserver` | Start the same container again (same container ID) |
| `docker logs webserver` | View Nginx's access log |
| `docker stats webserver` | Live CPU and memory usage |
| `docker exec -it webserver sh` | Open a shell inside the container |
| `docker rm -f webserver` | Delete the container (the image stays) |

Creating a new container with `docker run` gives it a new ID, which appears on the page.

## Health check

Docker calls `/health` every 30 seconds. Nginx answers with `{"status":"UP"}`, and these checks are excluded from the access log.