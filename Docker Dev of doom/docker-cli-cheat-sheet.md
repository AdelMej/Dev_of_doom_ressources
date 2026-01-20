# Docker CLI Cheat Sheet

## Images
```bash
docker images
docker pull IMAGE
docker build -t NAME .
docker rmi IMAGE
docker inspect IMAGE
```

## Containers
```bash
docker ps
docker ps -a
docker run IMAGE
docker run -it IMAGE bash
docker run -d --name NAME IMAGE
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
docker rm CONTAINER
docker inspect CONTAINER
docker logs CONTAINER
```

## Exec & Attach
```bash
docker exec -it CONTAINER bash
docker attach CONTAINER
```

## Volumes
```bash
docker volume ls
docker volume create NAME
docker volume inspect NAME
docker volume rm NAME
```

## Networks
```bash
docker network ls
docker network create NAME
docker network inspect NAME
docker network rm NAME
```

## System & Cleanup
```bash
docker info
docker stats
docker system prune
docker system df
```

## Docker Compose
```bash
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
```

## Dockerfile Tips
- Same Dockerfile syntax as Podman
- Supports multi-stage builds
- `.dockerignore` supported

## Notes
- Requires Docker daemon
- Typically runs as root
- OCI-compatible images
