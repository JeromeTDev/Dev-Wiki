# Linux CLI Cheat-Sheet

## grep

### Häufig
```bash
grep "text" file.txt
grep -i "error" *.log
grep -r "TODO" .
```

### Optionen, die ich nutze
- `-i` – ignoriert Groß-/Kleinschreibung
- `-r` – rekursiv
- `-n` – Zeilennummer anzeigen
- `-v` – invertiert (alles außer Treffer)

### Typische Kombinationen
```bash
journalctl -u nginx | grep -i fail
ps aux | grep python
```

### Achtung
- Regex aktiv → Sonderzeichen escapen
- Ohne `-r` keine Ordner

---

## find

### Häufig
```bash
find . -name "*.log"
find . -type f -mtime -1
find /var/log -size +100M
```

### Optionen, die ich nutze
- `-name` – Dateiname
- `-type f|d` – Datei oder Ordner
- `-mtime` – Änderungszeit
- `-size` – Dateigröße

### Typische Kombinationen
```bash
find . -name "*.tmp" -delete
find . -type f -exec grep "TODO" {} \;
```

### Achtung
- `-delete` ist final ⚠️
- `-exec` langsam bei vielen Dateien

---

## rm

### Häufig
```bash
rm file.txt
rm -r folder/
```

### Optionen, die ich nutze
- `-r` – rekursiv
- `-f` – ohne Nachfrage

### Typische Kombinationen
```bash
rm -rf node_modules/
```

### Achtung
- Kein Papierkorb
- `rm -rf /` = Ende
- Lieber vorher `ls` oder `echo` prüfen
