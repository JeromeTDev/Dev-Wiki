

```bash
# =========================================================
# SETUP (Fedora)
# =========================================================

sudo dnf -y install dnf-plugins-core

sudo dnf config-manager addrepo \
https://download.docker.com/linux/fedora/docker-ce.repo

sudo dnf install docker-ce docker-ce-cli containerd.io \
docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker
systemctl status docker

sudo usermod -aG docker $USER
newgrp docker

docker version
docker info


# =========================================================
# BASIC WORKFLOW
# =========================================================

docker run hello-world
docker ps
docker ps -a
docker images


# =========================================================
# RUNNING CONTAINERS
# =========================================================

docker run ubuntu
docker run -it ubuntu bash
docker run -it alpine sh

docker run -d nginx
docker run --name myapp nginx

docker run -p 8080:80 nginx
docker run -v $(pwd):/app ubuntu

docker run --rm ubuntu
docker run -e MY_VAR=test ubuntu


# =========================================================
# CONTAINER MANAGEMENT
# =========================================================

docker start <id>
docker stop <id>
docker restart <id>
docker rm <id>
docker rm -f <id>

docker rename <old> <new>

docker stats
docker top <id>


# =========================================================
# LOGS / DEBUGGING
# =========================================================

docker logs <id>
docker logs -f <id>

docker exec -it <id> bash
docker exec -it <id> sh

docker inspect <id>


# =========================================================
# IMAGES
# =========================================================

docker pull nginx
docker build -t myimage .
docker build -f Dockerfile.dev .

docker images
docker rmi <image>

docker image prune
docker image prune -a


# =========================================================
# VOLUMES
# =========================================================

docker volume ls
docker volume create myvolume
docker volume inspect myvolume
docker volume rm myvolume

docker run -v myvolume:/data ubuntu
docker run -v $(pwd):/app ubuntu


# =========================================================
# NETWORKS
# =========================================================

docker network ls
docker network create mynetwork
docker network inspect mynetwork
docker network rm mynetwork

docker run --network mynetwork nginx


# =========================================================
# DOCKER COMPOSE (WICHTIG)
# =========================================================

docker compose up
docker compose up -d
docker compose down

docker compose restart
docker compose ps
docker compose logs
docker compose logs -f

docker compose build
docker compose pull


# ---------------------------------------------------------
# ⚠️ WICHTIG: Compose FILE LOCATION
# ---------------------------------------------------------

# Standard (im aktuellen Ordner):
docker compose up

# Wenn Datei NICHT im Root liegt:
docker compose -f docker/compose.yml up -d

docker compose -f docker/compose.yml logs -f n8n

docker compose -f docker/compose.yml down


# ---------------------------------------------------------
# 💡 BEST PRACTICE: Alias (sehr empfohlen)
# ---------------------------------------------------------

# fish shell:
alias dcd="docker compose -f docker/compose.yml"

dcd up -d
dcd logs -f n8n
dcd down


# =========================================================
# COMPOSE EXAMPLE
# =========================================================

services:
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: test
    ports:
      - "5432:5432"

  adminer:
    image: adminer
    ports:
      - "8080:8080"


# =========================================================
# CLEANUP
# =========================================================

docker container prune
docker image prune
docker volume prune
docker network prune

docker system prune
docker system prune -a --volumes


# =========================================================
# SAVE / EXPORT
# =========================================================

docker save -o image.tar myimage
docker load -i image.tar

docker export <container> > fs.tar
docker import fs.tar myimage


# =========================================================
# RESOURCE LIMITS
# =========================================================

docker run --memory=512m ubuntu
docker run --cpus=2 ubuntu


# =========================================================
# USEFUL IMAGES
# =========================================================

docker run -it ubuntu bash
docker run -it alpine sh
docker run -p 8080:80 nginx
docker run postgres
docker run redis
docker run adminer


# =========================================================
# REGISTRY
# =========================================================

docker login
docker logout
docker search nginx


# =========================================================
# ADVANCED
# =========================================================

docker cp file container:/tmp
docker cp container:/tmp/file .

docker diff <container>
docker history <image>
docker events


# =========================================================
# LEARNING PATH
# =========================================================

# 1. docker run / ps / logs / exec
# 2. volumes + ports
# 3. Dockerfile
# 4. Docker Compose
# 5. Networks
# 6. Multi-container apps
# 7. DevOps / CI/CD
```
## lazydocker
```bash
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
```
