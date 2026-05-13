# Rollback auf alten Snapshot (Fedora / Btrfs / Snapper)

## Ausgangslage
Du hast ein laufendes Fedora-System oder ein System, das nicht mehr bootet, und möchtest auf einen älteren Btrfs-Snapshot zurückrollen, z. B. nach einem fehlerhaften Update oder kaputtem Systemzustand.

## Ziel
Das Root-System (`/`) auf einen früheren Snapshot zurücksetzen, während wichtige Daten in `/home/@data` oder anderen getrennten Subvolumes erhalten bleiben.  
System soll danach wieder bootfähig sein.

## Lösung
```bash
# Schritt 1: Snapshot anzeigen
sudo snapper -c root list

# Schritt 2: Rollback auf Snapshot-ID <ID> durchführen
sudo snapper -c root rollback <ID>

# Schritt 3: SELinux neu labeln (wichtig nach Rollback)
sudo touch /.autorelabel

# Schritt 4: System neu starten
sudo reboot

```

## Erklärung
- `snapper list` zeigt alle verfügbaren Snapshots.
- `snapper rollback <ID>` setzt das Root-Subvolume (`/`) auf den gewählten Snapshot zurück.
- Andere Subvolumes wie `/home/@data` oder `/home/.cache` bleiben unverändert.
- SELinux benötigt nach Rollback eine Neuvergabe der Labels (`/.autorelabel`), sonst startet das System nicht korrekt.

## Varianten
### Fall A: Rollback aus laufendem System
```bash
sudo snapper -c root list
sudo snapper -c root rollback <ID>
sudo touch /.autorelabel
sudo reboot

```

### Fall B: Rollback aus nicht startendem System (Live-USB)
```bash
# Live-USB booten und Root mounten
sudo mount -o subvolid=5 /dev/nvme0n1pX /mnt

# Snapshots anzeigen
sudo snapper --root /mnt -c root list

# Rollback durchführen
sudo snapper --root /mnt -c root rollback <ID>

```
- Vorteil: Rettung bei kaputtem System
- Nachteil: erfordert Live-USB, Mount-Pfade korrekt setzen

### Fall C: Rollback über GRUB-Menü (grub-btrfs, optional)
1. Rechner starten
2. **ESC** gedrückt halten → GRUB-Menü öffnen
3. Menüpunkt **„Snapshots“** auswählen
4. Gewünschten Snapshot markieren und booten → System startet **read-only**
5. Nach erfolgreichem Test dauerhaft übernehmen:
```bash
sudo snapper -c root rollback <ID>
sudo touch /.autorelabel
sudo reboot
```
- Vorteil: sehr komfortabel, kein Terminal nötig
- Nachteil: nur möglich, wenn **grub-btrfs installiert und GRUB aktualisiert** ist




## Entscheidungshilfe
- Wenn … → …
- Wenn … → …

## Verwandte Themen
- [[]]
- [[]]
