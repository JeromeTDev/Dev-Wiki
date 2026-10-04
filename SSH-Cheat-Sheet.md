# SSH Cheat Sheet

## Verbindung zu einem Server

```bash
ssh user@192.168.1.100
```

Mit Hostname:

```bash
ssh user@server01
```

Mit anderem Port:

```bash
ssh -p 2222 user@server01
```

---

## SSH-Key erstellen

Empfohlen: Ed25519

```bash
ssh-keygen -t ed25519
```

Optional mit Kommentar:

```bash
ssh-keygen -t ed25519 -C "mein-laptop"
```

Standard-Speicherort:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

- `id_ed25519` = privater Schlüssel
- `id_ed25519.pub` = öffentlicher Schlüssel

Der private Schlüssel darf niemals weitergegeben werden.

---

## SSH-Key auf Server kopieren

```bash
ssh-copy-id user@192.168.1.100
```

Beispiel:

```bash
ssh-copy-id jerome@192.168.122.84
```

Danach sollte die Anmeldung ohne Passwort funktionieren:

```bash
ssh jerome@192.168.122.84
```

Mit anderem SSH-Port:

```bash
ssh-copy-id -p 2222 user@server01
```

---

## Public Key anzeigen

```bash
cat ~/.ssh/id_ed25519.pub
```

Der Key wird auf dem Zielserver in folgender Datei hinterlegt:

```text
~/.ssh/authorized_keys
```

---

## SSH-Key manuell hinterlegen

Auf dem Server:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Public Key eintragen:

```bash
nano ~/.ssh/authorized_keys
```

Danach:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## Vorhandene SSH-Dateien anzeigen

```bash
ls -la ~/.ssh
```

Typische Dateien:

```text
id_ed25519
id_ed25519.pub
known_hosts
config
authorized_keys
```

---

## SSH-Verbindung testen

```bash
ssh user@server
```

Mit Debug-Ausgabe:

```bash
ssh -v user@server
```

Noch ausführlicher:

```bash
ssh -vvv user@server
```

---

## SSH Config verwenden

Datei:

```text
~/.ssh/config
```

Beispiel:

```sshconfig
Host fedora-vm
    HostName 192.168.122.84
    User jerome
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Danach reicht:

```bash
ssh fedora-vm
```

Statt:

```bash
ssh jerome@192.168.122.84
```

Berechtigungen setzen:

```bash
chmod 600 ~/.ssh/config
```

---

## Mehrere Server in SSH Config

```sshconfig
Host fedora-vm
    HostName 192.168.122.84
    User jerome
    IdentityFile ~/.ssh/id_ed25519

Host ubuntu-server
    HostName 192.168.1.50
    User admin
    IdentityFile ~/.ssh/id_ed25519
```

Verbindung:

```bash
ssh fedora-vm
ssh ubuntu-server
```

---

## SSH Agent

SSH-Agent starten:

```bash
eval "$(ssh-agent -s)"
```

Key hinzufügen:

```bash
ssh-add ~/.ssh/id_ed25519
```

Geladene Keys anzeigen:

```bash
ssh-add -l
```

Alle Keys entfernen:

```bash
ssh-add -D
```

---

## Dateien mit SCP kopieren

Lokale Datei auf Server:

```bash
scp datei.txt user@server:/tmp/
```

Vom Server lokal herunterladen:

```bash
scp user@server:/tmp/datei.txt .
```

Ordner rekursiv kopieren:

```bash
scp -r mein-ordner user@server:/tmp/
```

---

## Dateien mit rsync kopieren

Für größere Datenmengen oft besser als `scp`:

```bash
rsync -av ./ordner/ user@server:/tmp/ordner/
```

Explizit über SSH:

```bash
rsync -av -e ssh ./ordner/ user@server:/tmp/ordner/
```

---

## Remote-Befehl ausführen

```bash
ssh user@server "hostname"
```

Beispiel:

```bash
ssh jerome@192.168.122.84 "uname -a"
```

Mehrere Befehle:

```bash
ssh user@server "hostname && uptime && df -h"
```

---

## Bestimmten SSH-Key verwenden

```bash
ssh -i ~/.ssh/id_ed25519 user@server
```

---

## Host-Key entfernen

Wenn eine VM oder ein Server neu installiert wurde, kann folgende Meldung erscheinen:

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

Alten Host-Key entfernen:

```bash
ssh-keygen -R 192.168.122.84
```

Oder:

```bash
ssh-keygen -R server01
```

---

## SSH-Port prüfen

Port 22 testen:

```bash
nc -zv 192.168.122.84 22
```

---

## SSH-Server prüfen

Auf dem Zielserver:

```bash
systemctl status sshd
```

Starten:

```bash
sudo systemctl start sshd
```

Automatisch starten:

```bash
sudo systemctl enable --now sshd
```

Auf Fedora installieren:

```bash
sudo dnf install openssh-server
sudo systemctl enable --now sshd
```

---

## Firewall für SSH

Fedora / firewalld:

```bash
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload
```

Status:

```bash
sudo firewall-cmd --list-all
```

---

## Passwort-Login deaktivieren

Erst machen, wenn SSH-Key-Login sicher funktioniert.

Datei:

```text
/etc/ssh/sshd_config
```

```text
PasswordAuthentication no
PubkeyAuthentication yes
```

Danach:

```bash
sudo systemctl restart sshd
```

---

## Root-Login deaktivieren

In:

```text
/etc/ssh/sshd_config
```

```text
PermitRootLogin no
```

Danach:

```bash
sudo systemctl restart sshd
```

---

## SSH über Jump Host

Direkt:

```bash
ssh -J user@jump-host user@zielserver
```

Beispiel:

```bash
ssh -J jerome@192.168.1.10 admin@10.0.0.20
```

Mit SSH Config:

```sshconfig
Host jump
    HostName 192.168.1.10
    User jerome

Host internal-server
    HostName 10.0.0.20
    User admin
    ProxyJump jump
```

Danach:

```bash
ssh internal-server
```

---

## SSH Port Forwarding

Lokalen Port auf einen Remote-Service weiterleiten:

```bash
ssh -L 8080:localhost:80 user@server
```

Danach lokal:

```text
http://localhost:8080
```

Beispiel für Grafana:

```bash
ssh -L 3000:localhost:3000 user@server
```

Dann:

```text
http://localhost:3000
```

---

## Verbindung offen halten

In:

```text
~/.ssh/config
```

```sshconfig
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

---

## Typische Fehler

### Permission denied

```text
Permission denied (publickey)
```

Prüfen:

```bash
ssh -v user@server
```

Berechtigungen auf dem Zielserver:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

### Connection refused

```text
ssh: connect to host ... port 22: Connection refused
```

Mögliche Ursachen:

- `sshd` läuft nicht
- Firewall blockiert Port 22
- falscher SSH-Port

Prüfen:

```bash
systemctl status sshd
```

---

### No route to host

```text
ssh: connect to host ... port 22: No route to host
```

Das ist normalerweise ein Netzwerkproblem.

Prüfen:

```bash
ping 192.168.122.84
ip route
ip a
```

---

## Ansible mit SSH-Key

Key auf den Zielserver kopieren:

```bash
ssh-copy-id jerome@192.168.122.84
```

Danach kann Ansible ohne SSH-Passwort arbeiten:

```bash
ansible fedora -i inventory.ini -m ping
```

Mit Passwort:

```bash
ansible fedora -i inventory.ini -m ping --ask-pass
```

Playbook mit `sudo` / `become`:

```bash
ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
```

---

## Nützliche Kurzbefehle

```bash
ssh user@server
ssh-copy-id user@server
ssh-keygen -t ed25519
ssh-add ~/.ssh/id_ed25519
ssh -v user@server
scp file user@server:/tmp/
rsync -av ./dir/ user@server:/tmp/
ssh-keygen -R server
```

---

## Empfohlener Workflow

```bash
# 1. Key erstellen
ssh-keygen -t ed25519

# 2. Key auf Server kopieren
ssh-copy-id user@server

# 3. Verbindung testen
ssh user@server

# 4. SSH Config anlegen
nvim ~/.ssh/config

# 5. Ab jetzt nur noch Hostnamen verwenden
ssh server
```
