

```bash
# =========================================================
# Docker CLI Cheatsheet
# Fedora / Linux / CLI-first Workflow
# =========================================================

# --- Installation (Fedora) ---
sudo dnf -y install dnf-plugins-core

sudo dnf config-manager --add-repo \
https://download.docker.com/linux/fedora/docker-ce.repo

sudo dnf install docker-ce docker-ce-cli containerd.io \
docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker   # Start Docker daemon
systemctl status docker              # Check Docker status

sudo usermod -aG docker $USER        # Use docker without sudo
newgrp docker                        # Reload groups without logout

docker version                       # Show Docker version
docker info                          # Show Docker system info

# --- Basic Workflow ---
docker run hello-world               # Test Docker installation
docker ps                            # Show running containers
docker ps -a                         # Show all containers
docker images                        # Show downloaded images
docker volume ls                     # List volumes
docker network ls                    # List networks

# --- Running Containers ---
docker run ubuntu                    # Run Ubuntu container
docker run -it ubuntu bash           # Interactive Ubuntu shell
docker run -it alpine sh             # Interactive Alpine shell
docker run -d nginx                  # Run container detached
docker run --name myapp nginx        # Assign container name
docker run -p 8080:80 nginx          # Map ports
docker run -v $(pwd):/app ubuntu     # Mount current directory
docker run --rm ubuntu               # Auto-remove container
docker run -e MY_VAR=test ubuntu     # Set environment variable

# --- Container Management ---
docker start <container>             # Start stopped container
docker stop <container>              # Stop container
docker restart <container>           # Restart container
docker kill <container>              # Force kill container
docker rm <container>                # Remove container
docker rm -f <container>             # Force remove container

docker rename <old> <new>            # Rename container

docker stats                         # Live resource usage
docker top <container>               # Show processes

# --- Logs & Debugging ---
docker logs <container>              # Show logs
docker logs -f <container>           # Follow logs live
docker inspect <container>           # Detailed JSON info
docker exec -it <container> bash     # Open shell in running container
docker exec -it <container> sh       # Alpine shell
docker attach <container>            # Attach terminal

# --- Images ---
docker pull nginx                    # Download image
docker build -t myimage .            # Build image from Dockerfile
docker build -f Dockerfile.dev .     # Use custom Dockerfile
docker tag myimage myrepo/myimage    # Tag image
docker push myrepo/myimage           # Push image to registry

docker rmi <image>                   # Remove image
docker image prune                   # Remove dangling images
docker image prune -a                # Remove unused images

# --- Dockerfile ---
# Example Dockerfile

FROM ubuntu

RUN apt update && apt install -y curl

WORKDIR /app

COPY . .

CMD ["bash"]

# Build image
docker build -t myapp .

# Run image
docker run -it myapp

# --- Volumes ---
docker volume create myvolume        # Create named volume
docker volume ls                     # List volumes
docker volume inspect myvolume       # Inspect volume
docker volume rm myvolume            # Remove volume

docker run -v myvolume:/data ubuntu  # Mount named volume
docker run -v $(pwd):/app ubuntu     # Bind mount current dir

# --- Networks ---
docker network ls                    # List networks
docker network create mynetwork      # Create network
docker network inspect mynetwork     # Inspect network
docker network rm mynetwork          # Remove network

docker run --network mynetwork nginx # Connect container to network

# --- Docker Compose ---
docker compose up                    # Start services
docker compose up -d                 # Detached mode
docker compose down                  # Stop services
docker compose restart               # Restart services
docker compose logs                  # Show logs
docker compose logs -f               # Follow logs
docker compose ps                    # Show compose containers
docker compose build                 # Build compose services
docker compose pull                  # Pull latest images

# --- compose.yaml Example ---
# File: compose.yaml

services:
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: test

  adminer:
    image: adminer
    ports:
      - "8080:8080"

# Start:
docker compose up

# --- Cleanup ---
docker container prune               # Remove stopped containers
docker image prune                   # Remove unused images
docker volume prune                  # Remove unused volumes
docker network prune                 # Remove unused networks

docker system prune                  # Cleanup everything unused
docker system prune -a --volumes     # Aggressive cleanup

# --- Save & Export ---
docker save -o image.tar myimage     # Export image
docker load -i image.tar             # Import image

docker export <container> > fs.tar   # Export container filesystem
docker import fs.tar myimage         # Import filesystem as image

# --- Resource Limits ---
docker run --memory=512m ubuntu      # Limit RAM
docker run --cpus=2 ubuntu           # Limit CPU cores

# --- Useful Images ---
docker run -it ubuntu bash           # Ubuntu shell
docker run -it alpine sh             # Tiny Linux shell
docker run -p 8080:80 nginx          # Web server
docker run postgres                  # PostgreSQL
docker run redis                     # Redis server
docker run adminer                   # DB web UI

# --- Registry / Docker Hub ---
docker login                         # Login to Docker Hub
docker logout                        # Logout
docker search nginx                  # Search images

# --- Multi-Architecture ---
docker buildx ls                     # List buildx builders
docker buildx create --use           # Create builder
docker buildx build --platform linux/amd64 .

# --- Advanced ---
docker cp file.txt container:/tmp    # Copy file into container
docker cp container:/tmp/file .      # Copy file from container

docker diff <container>              # Show filesystem changes
docker history <image>               # Show image layers

docker events                        # Real-time Docker events


# --- Best Learning Path ---
# 1. docker run / ps / logs / exec
# 2. volumes + ports
# 3. Dockerfiles
# 4. Docker Compose
# 5. Networks
# 6. Multi-container setups
# 7. CI/CD + deployment

# --- Great Practice Projects ---
# nginx static website
# Java REST API + PostgreSQL
# Node + Redis
# Fullstack app
# Self-hosted services
# Dev environments
```
## lazydocker
```bash
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
```
