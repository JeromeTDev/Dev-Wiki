# Server Migration: n8n & PostgreSQL (Docker Named Volumes)

Dieses Dokument beschreibt die Strategie zur Migration einer lokalen n8n-Instanz mit PostgreSQL-Backend auf einen produktiven Server. Das Setup nutzt **Docker Named Volumes**, um Datenkonsistenz und Performance zu garantieren.

---

## 💡 Architektur-Entscheidung: Named Volumes vs. Bind Mounts

Im Produktionseinsatz werden **Named Volumes** (`postgres_data`) gegenüber relativen Pfaden (`./db_data`) bevorzugt:

* **Kein Rechte-Chaos (UID/GID):** Docker matcht die Linux-Berechtigungen automatisch zwischen Host und Container. Bei Bind Mounts crasht Postgres häufig mit *Permission Denied*.
* **Code/Daten-Trennung:** Das Projektverzeichnis bleibt austauschbar (*disposable*). Updates am Git-Repository gefährden die produktiven Daten nicht.
* **Sicherheit:** Rohe DB-Dateien dürfen niemals im laufenden Betrieb kopiert werden (Gefahr von Datenkorruption). Backups erfolgen immer logisch via Dump.

---

## 🗺️ Migrations-Leitfaden (Schritt-für-Schritt)

### 1. Backup auf dem Quellsystem (Local / WSL)

Zuerst wird die Datenbank logisch exportiert und der n8n-Verschlüsselungskey aus dem Docker-Systemverzeichnis gesichert.

```bash
# 1. PostgreSQL-Datenbank dumpen
docker compose exec -t n8n-postgres pg_dump -U n8n -d n8n > n8n_db_backup.sql

# 2. n8n-Daten (Encryption Key + Files) sichern
sudo tar -czf n8n_data_backup.tar.gz -C /var/lib/docker/volumes/bdc_n8n_data/_data .
```

---

## ✅ Hinweise

- Der Ordnername `bdc_` ergibt sich aus dem Docker-Projektverzeichnis (Compose Project Name).
- Backups sollten **niemals** durch Kopieren laufender Datenbankdateien erfolgen.
- Für produktive Systeme empfiehlt sich eine regelmäßige automatische Backup-Strategie.

---

## 🎯 Ziel

Die Migration soll sicherstellen, dass:

- Daten konsistent übertragen werden
- der Encryption Key erhalten bleibt
- das System ohne Konfigurationsverlust auf einem neuen Server läuft

