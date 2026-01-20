# Podman CLI Cheat Sheet

## Images
```bash
podman images
podman pull IMAGE
podman build -t NAME .
podman rmi IMAGE
podman inspect IMAGE
```

## Containers
```bash
podman ps
podman ps -a
podman run IMAGE
podman run -it IMAGE bash
podman run -d --name NAME IMAGE
podman start CONTAINER
podman stop CONTAINER
podman restart CONTAINER
podman rm CONTAINER
podman inspect CONTAINER
podman logs CONTAINER
```

## Exec & Attach
```bash
podman exec -it CONTAINER bash
podman attach CONTAINER
```

## Volumes
```bash
podman volume ls
podman volume create NAME
podman volume inspect NAME
podman volume rm NAME
```

## Networks
```bash
podman network ls
podman network create NAME
podman network inspect NAME
podman network rm NAME
```

## Pods (K8s-style grouping)
```bash
podman pod ls
podman pod create --name NAME
podman pod start NAME
podman pod stop NAME
podman pod rm NAME
podman run --pod NAME IMAGE
```

## System & Cleanup
```bash
podman info
podman stats
podman system prune
podman system df
```

## Kubernetes
```bash
podman generate kube CONTAINER > pod.yaml
podman play kube pod.yaml
```

## Rootless helpers
```bash
podman unshare
loginctl enable-linger $USER
```

## Compose
```bash
podman-compose up
podman-compose down
```

## Tips
- Dockerfiles work natively
- Rootless by default
- No daemon required
- OCI compliant
