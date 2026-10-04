# Podman Cheat Sheet (Fedora)

Die Beispiele verwenden Podman rootless als normaler Benutzer.

## Installation

```bash
sudo dnf install podman
podman --version
podman info
podman run --rm docker.io/library/hello-world
```

Für Compose wird zusätzlich ein Anbieter benötigt:

```bash
sudo dnf install podman-compose
podman compose version
```

## Überblick

```bash
podman ps                       # Laufende Container
podman ps -a                    # Alle Container
podman images                   # Lokale Images
podman volume ls                # Volumes
podman network ls               # Netzwerke
podman pod ps                   # Pods
podman system df                # Speicherverbrauch
```

## Container starten

```bash
podman run --rm -it docker.io/library/alpine sh
podman run -d --name web -p 8080:80 docker.io/library/nginx:alpine
podman run --rm -e MY_VAR=test docker.io/library/alpine env
podman run --rm -v "$PWD:/app:Z" docker.io/library/alpine ls /app
podman run --rm -v appdata:/data docker.io/library/alpine sh -c 'echo test > /data/test.txt'
```

- `-d`: Im Hintergrund starten
- `-it`: Interaktive Sitzung
- `--rm`: Container nach dem Beenden entfernen
- `-p 8080:80`: Host-Port 8080 auf Container-Port 80 weiterleiten
- `-v "$PWD:/app:Z"`: Aktuelles Verzeichnis einbinden; `:Z` setzt unter Fedora ein privates SELinux-Label
- `:z`: SELinux-Label für ein Verzeichnis, das mehrere Container gemeinsam verwenden

## Container verwalten und prüfen

```bash
podman start web
podman stop web
podman restart web
podman rm web                    # Gestoppten Container entfernen
podman rm -f web                 # Laufenden Container zwangsweise entfernen
podman logs web                  # Bisherige Logs
podman logs -f --tail 100 web    # Letzte 100 Zeilen und neue Logs
podman inspect web               # Details als JSON
podman exec -it web sh           # Shell im Container öffnen
podman stats                     # Ressourcennutzung
podman top web                   # Prozesse im Container
podman cp ./config.yml web:/tmp/config.yml
podman cp web:/tmp/output.txt ./output.txt
```

Schlanke Images enthalten oft kein `bash`; verwende dann `sh`.

## Images bauen

Beispiel für ein `Containerfile`:

```dockerfile
FROM docker.io/library/alpine:3
WORKDIR /app
COPY . .
CMD ["sh"]
```

```bash
podman build -t myapp:1.0 .
podman build -f Containerfile -t myapp:1.0 .
podman images
podman run --rm -it myapp:1.0
podman tag myapp:1.0 registry.example.com/team/myapp:1.0
podman push registry.example.com/team/myapp:1.0
podman rmi myapp:1.0
```

## Images offline übertragen

Auf Rechner A:

```bash
podman build -t myapp:1.0 .
podman save -o myapp-1.0.tar myapp:1.0
```

Die TAR-Datei übertragen und auf Rechner B laden:

```bash
podman load -i myapp-1.0.tar
podman images
podman run --rm myapp:1.0
```

`save` und `load` übertragen Images samt Tags und Layern. Volume-Daten sind **nicht** im Image-Archiv enthalten.

## Volumes und Netzwerke

```bash
podman volume create appdata
podman volume inspect appdata
podman run --rm -v appdata:/data docker.io/library/alpine ls /data
podman volume export -o appdata.tar appdata  # Volume separat sichern
podman volume rm appdata

podman network create appnet
podman network inspect appnet
podman run -d --name web --network appnet docker.io/library/nginx:alpine
podman network rm appnet
```

## Compose

Die Befehle im Verzeichnis der `compose.yaml` ausführen:

```bash
podman compose up -d           # Dienste starten
podman compose ps              # Status
podman compose logs -f         # Alle Logs live anzeigen
podman compose logs -f grafana # Logs eines Dienstes
podman compose build           # Images bauen
podman compose pull            # Images herunterladen
podman compose restart         # Dienste neu starten
podman compose down            # Container und Netzwerke entfernen
```

**Achtung:** `podman compose down -v` entfernt zusätzlich Volumes und möglicherweise Anwendungsdaten.

## Pods

```bash
podman pod create --name monitoring -p 3000:3000
podman run -d --pod monitoring --name grafana docker.io/grafana/grafana

podman pod ps
podman pod inspect monitoring
podman pod stop monitoring
podman pod rm monitoring
```

Container im selben Pod teilen sich das Netzwerk und erreichen sich über `localhost`. Host-Ports beim Erstellen des Pods angeben.

## Docker-kompatible API

Socket aktivieren:

```bash
systemctl --user enable --now podman.socket
```

Für Docker-API-Clients die Adresse setzen:

```bash
# Bash oder Zsh
export DOCKER_HOST="unix://$XDG_RUNTIME_DIR/podman/podman.sock"
```

```fish
# Fish
set -gx DOCKER_HOST "unix://$XDG_RUNTIME_DIR/podman/podman.sock"
```

Das installiert keinen `docker`-Befehl. Für normale `podman`-Befehle wird der Socket nicht benötigt.

## Aufräumen

```bash
podman container prune        # Gestoppte Container
podman image prune            # Ungenutzte Images
podman network prune          # Ungenutzte Netzwerke
podman volume prune           # Ungenutzte Volumes; Datenverlust möglich
podman system prune           # Ungenutzte Ressourcen
podman system prune -a        # Auch ungenutzte Images
```

## Schnellablauf: Grafana, Loki, Alloy

```bash
podman compose up -d
podman compose ps
podman compose logs -f alloy
podman compose down
```

Bei Compose ist `alloy` hier der **Dienstname**. Für `podman logs -f alloy` müsste der Container tatsächlich `alloy` heißen.
