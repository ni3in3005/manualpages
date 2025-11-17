# Docker Command Reference with Comments

## Table of Contents
- [Most Frequently Used Commands](#most-frequently-used-commands)
- [Docker Basics](#docker-basics)
- [Images](#images)
- [Containers](#containers)
- [Container Inspection and Logs](#container-inspection-and-logs)
- [Exec and Shell Access](#exec-and-shell-access)
- [Ports, Volumes, and Bind Mounts](#ports-volumes-and-bind-mounts)
- [Networks](#networks)
- [Docker Compose](#docker-compose)
- [Cleanup and Pruning](#cleanup-and-pruning)
- [Docker Registry and Hub](#docker-registry-and-hub)
- [Docker Info and Troubleshooting](#docker-info-and-troubleshooting)

---

## Most Frequently Used Commands
```bash
docker ps                     # List running containers
docker ps -a                  # List all containers
docker images                 # List all local images
docker pull nginx             # Download image from Docker Hub
docker run -d -p 8080:80 nginx  # Run container with port mapping
docker logs -f web            # Follow logs of a container
docker exec -it web bash      # Open shell inside container
docker stop web               # Stop a running container
docker rm web                 # Remove a stopped container
docker system prune -a        # Clean up unused resources
```

---

## Docker Basics
```bash
docker version                               # Show client and server version information
docker info                                  # Show detailed information about Docker engine
docker help                                  # Show general help for Docker
docker <command> --help                      # Show help for a specific Docker sub command
```

---

## Images
```bash
docker images                                # List all local images
docker images -a                             # List all images including intermediate layers
docker pull nginx:latest                     # Download an image from Docker Hub
docker pull nginx:1.27                       # Pull a specific version tag of an image
docker rmi nginx:latest                      # Remove a specific image tag
docker rmi -f nginx:latest                   # Force remove image even if used by stopped containers
docker rmi $(docker images -q)               # Remove all images by id
docker tag nginx:latest myrepo/nginx:prod    # Create a new tag pointing to the same image
docker history nginx:latest                  # Show layer history of an image
docker inspect nginx:latest                  # Show detailed JSON metadata for an image
```

---

## Containers
```bash
docker ps                                     # List running containers
docker ps -a                                  # List all containers including stopped ones
docker run nginx                              # Run container from nginx image in foreground
docker run -d nginx                           # Run container in detached mode
docker run --name web nginx                   # Run container with a custom name
docker run -d -p 8080:80 nginx                # Map host port 8080 to container port 80
docker run -d --restart unless-stopped nginx  # Restart container automatically unless manually stopped
docker stop web                               # Gracefully stop a running container by name
docker stop $(docker ps -q)                   # Stop all running containers
docker start web                              # Start a stopped container
docker restart web                            # Restart a container in one step
docker rm web                                 # Remove a stopped container
docker rm -f web                              # Force remove a running container
docker rm $(docker ps -aq)                    # Remove all containers (stopped and running with -f)
```

---

## Container Inspection and Logs
```bash
docker inspect web                            # Show full JSON details for the container
docker inspect -f '{{ .NetworkSettings.IPAddress }}' web  # Print container IP address only
docker logs web                               # Show container logs
docker logs -f web                            # Follow logs in real time
docker logs --tail 100 web                    # Show last 100 lines of logs
docker stats                                  # Live CPU and memory usage of containers
docker top web                                # Show processes running inside the container
docker diff web                               # Show filesystem changes compared to image
```

---

## Exec and Shell Access
```bash
docker exec web ls /                          # Run a command inside a running container
docker exec -it web /bin/bash                 # Start an interactive bash shell inside container
docker exec -it web sh                        # Use sh shell if bash is not available
docker exec -it web env                       # Show environment variables inside container
docker attach web                             # Attach to main process output of a running container
```

---

## Ports, Volumes, and Bind Mounts
```bash
docker run -d -p 8080:80 nginx                        # Map host port 8080 to container port 80
docker run -d -p 127.0.0.1:8080:80 nginx              # Bind port only on localhost
docker port web                                       # Show port mappings for a container

docker volume ls                                      # List named Docker volumes
docker volume create appdata                          # Create a named volume
docker run -d -v appdata:/var/lib/mysql mysql         # Attach named volume to container path
docker volume inspect appdata                         # Show details of a volume
docker volume rm appdata                              # Remove a volume (must not be in use)

docker run -d -v /host/logs:/var/log/nginx nginx      # Bind mount host directory into container
docker run -d -v $(pwd)/config:/etc/nginx/conf.d nginx # Mount current directory config into container
```

---

## Networks
```bash
docker network ls                                   # List Docker networks
docker network create mynet                         # Create a user defined bridge network
docker network inspect mynet                        # Show details of a network
docker run -d --name web1 --network mynet nginx     # Start container attached to a specific network
docker network connect mynet web                    # Attach an existing container to a network
docker network disconnect mynet web                 # Detach container from a network
docker network rm mynet                             # Remove a user defined network
```

---

## Docker Compose
```bash
docker compose version                             # Show Docker Compose version
docker compose up                                  # Create and start containers from compose file
docker compose up -d                               # Start in detached mode
docker compose down                                # Stop and remove containers, networks, and default volume
docker compose down -v                             # Also remove named volumes declared in the file
docker compose ps                                  # List containers created by compose
docker compose logs                                # Show logs from all services
docker compose logs -f                             # Follow logs for all services
docker compose logs -f web                         # Follow logs for a single service
docker compose restart web                         # Restart a single service
docker compose exec web /bin/bash                  # Open a shell in a running service container
```

---

## Cleanup and Pruning
```bash
docker system df                                   # Show disk usage for images, containers, and volumes
docker system prune                                # Remove unused containers, networks, and dangling images
docker system prune -a                             # Also remove unused images not referenced by containers
docker image prune                                 # Remove dangling images only
docker container prune                             # Remove all stopped containers
docker volume prune                                # Remove unused volumes
docker network prune                               # Remove unused networks
```

---

## Docker Registry and Hub
```bash
docker login                                       # Log in to Docker registry (default is Docker Hub)
docker logout                                      # Log out from registry
docker pull redis:7                                # Download image from registry
docker tag webapp:latest myuser/webapp:1.0.0       # Tag local image for your repository
docker push myuser/webapp:1.0.0                    # Push tagged image to registry
docker search nginx                                # Search Docker Hub for images
```

---

## Docker Info and Troubleshooting
```bash
docker info                                        # Show detailed Docker engine information
docker ps -a                                       # Check running and stopped containers
docker logs web                                    # Check application logs for issues
docker events                                      # Stream real time events from Docker daemon
docker inspect web                                 # Inspect container for configuration mistakes
docker run --rm -it alpine ping google.com         # Quick test container to check network connectivity
docker system df                                   # Check how much space Docker is using
```