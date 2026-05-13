# Btrfs / Snapper Cheat Sheet

## Kurzbeschreibung
Btrfs ist ein Copy-on-Write-Dateisystem mit Subvolumes, Snapshots und integrierter Kompression. Snapper ermöglicht die einfache Verwaltung von Snapshots, Rollbacks und System-Wiederherstellung, auch bei kaputtem System.

## Häufig
```bash
# Subvolumes anzeigen
sudo btrfs subvolume list /

# Subvolume erstellen
sudo btrfs subvolume create /home/woodz/@data

# Snapshot eines Subvolumes erstellen
sudo snapper -c root create --description "pre-update" --type pre

# Snapshots auflisten
sudo snapper -c root list

# Rollback auf Snapshot
sudo snapper -c root rollback <ID>

# Subvolume löschen
sudo btrfs subvolume delete /home/woodz/@cache

# Live-USB: Root mounten für Snapper
sudo mount -o subvolid=5 /dev/nvme0n1pX /mnt
sudo snapper --root /mnt list

``` 

## Optionen, die ich nutze
- `@` → Root
- `@home` → Home-Daten
- `@data` → persistente Entwickler-Daten
- `.cache` → User-Cache (optional Subvolume, nicht gesnapshottet)
- `@games` → Spiele / experimentelle Software
- `var/cache` / `var/tmp` → Systemcache, separate Subvolumes
## Typische Kombinationen

- Pre-Update Snapshot + Post-Update Snapshot:
```bash
sudo snapper -c root create --description "pre-update" --type pre
sudo dnf upgrade
sudo snapper -c root create --description "post-update" --type post
```
-> python3-dnf-plugin-snapper macht dies automatisch 

- Rollback aus laufendem System:
```bash
sudo snapper -c root list
sudo snapper -c root rollback <ID>
sudo touch /.autorelabel
sudo reboot
```

Rollback aus nicht startendem System:
```bash
sudo mount -o subvolid=5 /dev/nvme0n1pX /mnt
sudo snapper --root /mnt -c root list
sudo snapper --root /mnt -c root rollback <ID>
```


Optional: GRUB Snapshot Boot (grub-btrfs):
- ESC → GRUB → Snapshots auswählen → Testen → `snapper rollback`
## Achtung 

SELinux nach Rollback immer neu labeln:
```bash
sudo touch /.autorelabel
``` 
