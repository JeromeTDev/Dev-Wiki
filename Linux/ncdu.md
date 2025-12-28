# ncdu Cheat Sheet

## Kurzbeschreibung
zeigt die Größe von Dateien und Verzeichnissen übersichtlich im Terminal an und hilft, Speicherfresser zu finden.

## Häufig
```bash
ncdu               # scannt aktuelles Verzeichnis
ncdu /home/user    # scannt ein bestimmtes Verzeichnis
ncdu -x /          # scannt nur eine Partition
``` 

## Optionen, die ich nutze

- `-x` – nur eine Partition
- `-q` – quiet, weniger Output
- `-r` – read-only


## Achtung 

- Keine Löschaktionen ohne Prüfung
- Read-only Modus empfohlen, wenn du nur analysierst