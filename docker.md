# Docker Commands Cheatsheet

This file contains commonly used **Docker commands** that are helpful for beginners and during DevOps training.

---

## 🔹 Docker Basics

```bash
docker --version          # Check Docker version
docker info               # Show system-wide information
docker help               # Get help with Docker commands
```

---

## 🔹 Working with Images

```bash
docker images             # List all downloaded images
docker search <name>      # Search for images on Docker Hub
docker pull <image>       # Download image from Docker Hub
docker rmi <image_id>     # Remove image
```

---

## 🔹 Working with Containers

```bash
docker ps                 # List running containers
docker ps -a              # List all containers (including stopped ones)
docker run <image>        # Run a container from image
docker run -it <image>    # Run container in interactive mode
docker run -d <image>     # Run container in detached mode
docker start <container>  # Start a stopped container
docker stop <container>   # Stop a running container
docker restart <container> # Restart a container
docker rm <container_id>  # Remove a container
```

---

## 🔹 Container Access

```bash
docker exec -it <container_id> bash   # Open bash shell inside container
docker attach <container_id>          # Attach to running container
docker logs <container_id>            # View container logs
```

---

## 🔹 Building & Tagging Images

```bash
docker build -t <name>:<tag> .   # Build image from Dockerfile in current directory
docker tag <image_id> <repo>:<tag>   # Tag an image
docker push <repo>:<tag>             # Push image to Docker Hub
```

---

## 🔹 Volumes & Storage

```bash
docker volume ls                 # List all volumes
docker volume create <name>      # Create a new volume
docker run -v <volume>:/path <image>  # Mount volume inside container
docker volume rm <name>          # Remove volume
```

---

## 🔹 Networks

```bash
docker network ls                 # List networks
docker network create <name>      # Create custom network
docker run --network=<name> <image>   # Run container on a specific network
docker network rm <name>          # Remove network
```

---

## 🔹 Cleanup Commands

```bash
docker system prune               # Remove unused containers, networks, images
docker container prune            # Remove stopped containers
docker image prune                # Remove unused images
docker volume prune               # Remove unused volumes
```

---


