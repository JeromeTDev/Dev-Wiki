# Libvirt / KVM unter Fedora

## Übersicht

Unter Linux ist **KVM/QEMU + libvirt** meist die bessere Alternative zu VirtualBox.

```text
virt-manager
    ↓
libvirt
    ↓
QEMU
    ↓
KVM
    ↓
Linux Kernel / CPU Virtualisierung
```

- **KVM** → Virtualisierung direkt im Linux-Kernel
- **QEMU** → stellt virtuelle Hardware bereit
- **libvirt** → Verwaltungsschicht für VMs
- **virt-manager** → grafische Oberfläche für libvirt

---

## Installation unter Fedora

```bash
sudo dnf install virt-manager libvirt qemu-kvm
```

Libvirt starten:

```bash
sudo systemctl enable --now libvirtd
```

GUI starten:

```bash
virt-manager
```

---

## Prüfen, ob KVM geladen ist

```bash
lsmod | grep kvm
```

Bei AMD:

```text
kvm_amd
kvm
```

Bei Intel:

```text
kvm_intel
kvm
```

CPU-Virtualisierung prüfen:

```bash
lscpu | grep Virtualization
```

Beispiel:

```text
Virtualization: AMD-V
```

---

## Neue VM erstellen

```text
virt-manager
→ Neue virtuelle Maschine
→ ISO auswählen
→ CPU / RAM festlegen
→ virtuelle Festplatte erstellen
→ Installation starten
```

Für eine Fedora-Test-VM z. B.:

```text
CPU:   2–4
RAM:   4–8 GB
Disk:  30–40 GB
```

---

## VMs über CLI anzeigen

```bash
virsh list
```

Alle VMs:

```bash
virsh list --all
```

VM starten:

```bash
virsh start fedora-test
```

VM herunterfahren:

```bash
virsh shutdown fedora-test
```

VM hart ausschalten:

```bash
virsh destroy fedora-test
```

---

## Snapshots

Snapshot erstellen:

```bash
virsh snapshot-create-as fedora-test clean-install
```

Snapshots anzeigen:

```bash
virsh snapshot-list fedora-test
```

Snapshot wiederherstellen:

```bash
virsh snapshot-revert fedora-test clean-install
```

Sehr praktisch zum Testen von Ansible:

```text
Fedora installieren
       ↓
Snapshot "clean-install"
       ↓
Ansible Playbook testen
       ↓
Fehler?
       ↓
Snapshot zurücksetzen
       ↓
erneut testen
```

---

## IP-Adresse der VM herausfinden

```bash
virsh domifaddr fedora-test
```

Danach z. B.:

```bash
ssh user@192.168.122.123
```

---

## Verwendung mit Ansible

Beispiel `inventory.ini`:

```ini
[fedora]
192.168.122.123 ansible_user=woodz
```

Verbindung testen:

```bash
ansible all -i inventory.ini -m ping
```

Playbook ausführen:

```bash
ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
```

---

## Warum KVM statt VirtualBox?

### KVM

```text
+ Bestandteil von Linux
+ sehr gute Performance
+ keine externen VirtualBox-Kernelmodule
+ libvirt / virsh gut automatisierbar
+ gut für Linux- und DevOps-Labs
```

### VirtualBox

```text
+ einfache GUI
+ plattformübergreifend
- benötigt vboxdrv Kernelmodul
- kann nach Kernelupdates Probleme machen
```

Für einen Linux-Host und Ansible-Testumgebungen ist **KVM/libvirt** meistens die sinnvollere Wahl.
