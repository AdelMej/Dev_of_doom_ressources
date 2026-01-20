# Podman Cheat Sheet

## Basics

-   `podman --version`
-   `podman info`
-   `podman search nginx`
-   `podman pull nginx:latest`
-   `podman images`
-   `podman ps`
-   `podman ps -a`

## Running Containers

-   `podman run -d --name web -p 8080:80 nginx`
-   `podman run -it --rm ubuntu bash`
-   `podman logs web`
-   `podman logs -f web`
-   `podman exec -it web bash`

## Managing Containers

-   `podman stop web`
-   `podman start web`
-   `podman restart web`
-   `podman rm web`
-   `podman rm -a`
-   `podman rmi -a`

## Volumes & Storage

-   `podman volume create data`
-   `podman volume inspect data`
-   `podman run -d -v /host/path:/container/path nginx`

## Networks

-   `podman network create mynet`
-   `podman run -d --network=mynet nginx`
-   `podman network ls`

## Podman Compose

-   `podman compose up -d`
-   `podman compose down`
-   `podman compose up -d --build`
-   `podman compose logs -f`

## Pods

-   `podman pod create --name mypod -p 8080:80`
-   `podman run -d --pod mypod nginx`
-   `podman pod inspect mypod`
-   `podman pod ps`
