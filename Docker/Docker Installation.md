## 📦 Installation (Fedora)

```bash
sudo dnf -y install dnf-plugins-core                 
# DNF Plugin für Repository-Management installieren

sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo 
# Offizielles Docker-Repo hinzufügen

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin              # Docker Engine + CLI + Plugins installieren

sudo systemctl enable --now docker # Docker Dienst starten + beim Boot aktivieren
systemctl status docker                                 # Status prüfen

sudo usermod -aG docker $USER      # User zur Docker-Gruppe hinzufügen (kein sudo mehr nötig)
newgrp docker                      # Gruppenrechte neu laden (ohne Logout)

docker version                     # Version anzeigen
docker info                        # Systeminformationen anzeigen
```

---

## 🚀 Basic Workflow

```bash
docker run hello-world             # Testcontainer starten (Installation prüfen)
docker ps                          # Laufende Container anzeigen
docker ps -a                       # Alle Container anzeigen
docker images                      # Lokale Images anzeigen
docker volume ls                   # Volumes anzeigen
docker network ls                  # Netzwerke anzeigen
```

---

## 🏃 Container starten & ausführen

```bash
docker run ubuntu                   # Container starten (stoppt direkt wieder)
docker run -it ubuntu bash          # Interaktive Bash im Container
docker run -it alpine sh            # Minimale Shell (Alpine)
docker run -d nginx                 # Container im Hintergrund starten
docker run --name myapp nginx       # Container mit Namen starten
docker run -p 8080:80 nginx         # Port-Mapping: Host 8080 → Container 80
docker run -v $(pwd):/app ubuntu    # Verzeichnis in Container mounten
docker run --rm ubuntu              # Container nach Stop automatisch löschen
docker run -e MY_VAR=test ubuntu    # Environment Variable setzen
```

---

## 🕹️ Container Management

```bash
docker start <container>                      # Gestoppten Container starten
docker stop <container>                       # Container sauber stoppen
docker restart <container>                    # Container neu starten
docker kill <container>                       # Sofort stoppen (hart)
docker rm <container>                         # Container löschen
docker rm -f <container>                      # Container erzwingen löschen
docker rename <old> <new>                     # Container umbenennen
docker stats                                  # Live CPU/RAM Nutzung
docker top <container>                        # Prozesse im Container anzeigen
```

---

## 🔍 Logs & Debugging

```bash
docker logs <container>                    # Logs anzeigen
docker logs -f <container>                 # Logs live verfolgen

docker inspect <container>                 # Detaillierte JSON-Infos

docker exec -it <container> bash           # Shell in laufendem Container öffnen
docker exec -it <container> sh             # Alternative Shell

docker attach <container>                  # Direkt an STDIN/STDOUT hängen
```

---

## 🖼️ Images & Dockerfile

### Beispiel Dockerfile

```Dockerfile
FROM ubuntu                                         # Basisimage
RUN apt update && apt install -y curl               # Paket installieren
WORKDIR /app                                        # Arbeitsverzeichnis setzen
COPY . .                                            # Dateien kopieren
CMD ["bash"]                                        # Startkommando
```

### Image Befehle

```bash
docker pull nginx                              # Image von Docker Hub laden
docker build -t myimage .                      # Image bauen

docker build -f Dockerfile.dev .               # Anderes Dockerfile nutzen

docker tag myimage myrepo/myimage              # Image taggen (für Registry)

docker push myrepo/myimage                     # Image hochladen

docker rmi <image>                             # Image löschen

docker image prune                             # Ungenutzte Images löschen
docker image prune -a                          # Alle ungenutzten löschen
```

---

## 💾 Volumes & Netzwerke
j5
```bash
# Volumes
docker volume create myvolume                           # Volume erstellen
docker volume inspect myvolume                          # Infos anzeigen
docker volume rm myvolume                               # Volume löschen

docker run -v myvolume:/data ubuntu                     # Volume mounten

# Netzwerke
docker network create mynetwork                         # Netzwerk erstellen
docker network inspect mynetwork                        # Details anzeigen
docker network rm mynetwork                             # Netzwerk löschen

docker run --network mynetwork nginx             # Container ins Netzwerk hängen
```

---

## 🐙 Docker Compose

```bash
docker compose up                                       # Services starten
docker compose up -d                                    # Im Hintergrund

docker compose down                                # Container stoppen + löschen
docker compose restart                                  # Neustarten

docker compose logs                                     # Logs anzeigen
docker compose logs -f                                  # Live Logs

docker compose ps                                       # Status anzeigen
docker compose build                                    # Images neu bauen
docker compose pull                                     # Images aktualisieren
```

---

## 🧹 System Cleanup

```bash
docker container prune                             # Gestoppte Container löschen
docker volume prune                                # Unbenutzte Volumes löschen
docker network prune                               # Unbenutzte Netzwerke löschen

docker system prune                                # Alles Unbenutzte löschen
docker system prune -a --volumes                   # Aggressiver Cleanup
```

---

## 📦 Import / Export & Advanced

```bash
docker save -o image.tar myimage                        # Image exportieren
docker load -i image.tar                                # Image importieren

docker export <container> > fs.tar           # Container-Dateisystem exportieren
docker import fs.tar myimage                            # Neues Image aus FS

docker cp file.txt container:/tmp            # Datei in Container kopieren
docker cp container:/tmp/file .                         # Datei rauskopieren

docker diff <container>                                 # Veränderungen anzeigen
docker history <image>                                  # Image Layer anzeigen
docker events                                           # Live Events streamen
```

---

## 🏗️ Multi-Architecture Builds

```bash
docker buildx ls                                        # Builder anzeigen
docker buildx create --use                              # Builder erstellen
docker buildx build --platform linux/amd64 --load -t myimage:amd64 . # Cross-Build
```

---

## 🧠 CLI Notes

```bash
# STRG+p + STRG+q → Container verlassen ohne zu stoppen
# docker system df → Speicherverbrauch anzeigen
# docker system prune → Cleanup
# docker logs → Erste Debug-Anlaufstelle
```
