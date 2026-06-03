# 🚀 zrok Cheat Sheet

## 📌 Was ist zrok?
zrok erstellt einen sicheren Tunnel, um lokale Dienste (z. B. Webserver, n8n, Frontend) über einen öffentlichen Link erreichbar zu machen.

---

## ⚙️ Voraussetzungen

- zrok installiert
- Account (zrok.io)
- Auth einmal durchführen

---

## 🔑 Login / Setup

```bash
zrok enable <TOKEN>
```

👉 Token bekommst du nach Registrierung auf zrok.io

---

## 🌍 Lokalen Service öffentlich machen

### Beispiel: Frontend auf Port 3000

```bash
zrok share public http://localhost:3000
```

👉 Ergebnis:
- öffentlicher Link wird erzeugt
- extern erreichbar (z. B. für Demo)

---

### Beispiel: n8n (Standard-Port 5678)

```bash
zrok share public http://localhost:5678
```

---

## 🔒 Privat (mit Zugriffsbeschränkung)

```bash
zrok share private http://localhost:3000
```

👉 nur für dich / Team sichtbar

---

## 📡 TCP Service (z. B. Datenbank)

```bash
zrok share public tcp://localhost:5432
```

⚠️ Vorsicht: DB öffentlich → nur für Tests!

---

## 🛑 Stoppen

```bash
CTRL + C
```

👉 beendet den Tunnel

---

## 📋 Wichtige Hinweise

- ❗ Nur für Demo / Entwicklung nutzen
- ❗ Nicht für produktive Systeme
- ❗ Zugang ggf. mit Passwort sichern (je nach Modus)
- ❗ Firmenrichtlinien beachten!

---

## ✅ Typischer Use Case (dein Projekt)

```
Frontend lokal läuft → zrok → Link teilen → Chef schaut rein
```

---

## 💡 Pro Tipp

- Für Präsentationen:
  - lokales System starten
  - zrok aktivieren
  - Link im Call teilen

👉 Kein Deployment nötig ✅
