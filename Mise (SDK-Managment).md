 ls[mise](https://mise.jdx.dev/) ist ein universeller Tool-Manager (ähnlich wie SDKMAN! oder nvm), der in Rust geschrieben und extrem schnell ist. Wir nutzen ihn, um SDKs (Java, Node, Python etc.) außerhalb des Root-Systems zu verwalten.

## 1. Installation (Fedora)

```bash
# 1. Das offizielle Repo hinzufügen
curl https://mise.jdx.dev/rpm/mise.repo | sudo tee /etc/yum.repos.d/mise.repo

# 2. Über DNF installieren
sudo dnf install -y mise

# 3. In Fish aktivieren
# Das ist wichtig, damit 'mise' die Pfade für Java/Node etc. setzen kann
echo 'mise activate fish | source' >> ~/.config/fish/config.fish

echo 'eval "$(mise activate bash)"' >> ~/.bashrc
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc
```

## 2. Btrfs-Optimierung (Speicherpfad)

Um zu verhindern, dass SDK-Binärdaten deine Home-Snapshots aufblähen, verlinken wir den Speicherort in das NoCoW-Subvolume `@data`.

```bash
# Verzeichnis im NoCoW-Subvolume erstellen
mkdir -p ~/@data/mise

# Standardpfad zu @data verlinken
mkdir -p ~/.local/share
ln -s ~/@data/mise ~/.local/share/mise
```

## 3. Grundlegende Befehle

### SDKs installieren & Global setzen

```bash
# Eine Java-Version global installieren
# Installiert das aktuellste Standard-JDK
mise use --global java@25

# Eine Node.js-Version global installieren
mise use --global node@lts
```

### Projektspezifische Versionen (Auto-Switch)

Wenn ein Projekt eine andere Version benötigt (z.B. Java 17), im Projektordner ausführen:

```bash
cd ~/dev/mein-projekt
mise use java@openjdk-17
```

_Dies erstellt eine `.mise.toml` im Ordner. mise schaltet die Version automatisch um, sobald du den Ordner betrittst._


### Deklaratives Setup (Bootstrapping)

Wenn du deine `~/.config/mise/config.toml` in deinen Dotfiles sicherst, kannst du dein gesamtes System mit einem Befehl wiederherstellen:

```bash
# Installiert alle in der globalen config.toml definierten Tools
# Perfekt für Neuinstallationen nach dem Kopieren der Dotfiles
mise install
```




### weitere Befehle

```bash
# --- 1. SUCHEN & FINDEN ---
# Zeige alle verfügbaren Tools (Java, Node, Python, etc.)
mise plugins ls-remote

# Suche nach verfügbaren Versionen für ein spezifisches Tool
mise ls-remote java          # Alle Java Versionen
mise ls-remote node@20       # Nur Node 20.x Versionen
mise ls-remote python | tail # Die neuesten Python Versionen

# --- 2. INSTALLATION ---
# Installiert eine Version global (für das ganze System/User)
mise use --global java@openjdk-21
mise use --global node@lts

# Installiert eine Version nur für das aktuelle Verzeichnis
# (Erstellt oder aktualisiert eine .mise.toml Datei)
mise use java@17

# --- 3. STATUS & INFOS ---
# Liste alle installierten Versionen auf
mise ls

# Zeige an, welche Version gerade aktiv ist
mise current java

# Zeige den Pfad zur aktuellen Version (nützlich für IDEs)
mise where java

# --- 4. WARTUNG & CLEANUP ---
# Entferne eine spezifische Version
mise uninstall java@openjdk-17

# Entferne alle SDK-Versionen, die nirgendwo mehr benutzt werden
# (Hält deinen @data/mise Ordner sauber)
mise prune

# --- 5. UPDATES ---
# Aktualisiert die Liste der verfügbaren Versionen
mise rescan

```

## 4. Warum mise?

- **Geschwindigkeit:** In Rust geschrieben, keine spürbare Verzögerung beim Terminalstart.
    
- **Universell:** Ein Tool für fast alle Sprachen (Java, Python, Node, Go, Rust, etc.).
    
- **Snapshot-Schonend:** Durch den Symlink nach `@data` bleiben stündliche Backups von `~/dev` klein, während die Logik (`.mise.toml`) mitgesichert wird.

|**Kategorie**|**Befehl**|**Beschreibung**|
|---|---|---|
|**Setup**|`mise install`|Installiert alle Tools aus der `config.toml` (Bootstrapping).|
||`mise install -y`|Automatische Installation ohne Bestätigung.|
|**Suche**|`mise ls-remote <tool>`|Zeigt alle verfügbaren Versionen (z.B. `java`).|
||`mise plugins ls-remote`|Zeigt alle unterstützten Tools/Sprachen.|
|**Installation**|`mise use -g <tool>@<v>`|Installiert & setzt Version **global** (User-Ebene).|
||`mise use <tool>@<v>`|Installiert & setzt Version **lokal** (.mise.toml).|
|**Status**|`mise ls`|Liste aller aktuell installierten Versionen.|
||`mise current`|Zeigt aktive Versionen im aktuellen Verzeichnis.|
||`mise where <tool>`|Pfad zur Installation (für IDEs wie IntelliJ/PyCharm).|
|**Wartung**|`mise prune`|Löscht ungenutzte Versionen (spart Platz in `@data`).|
||`mise uninstall <t>@<v>`|Deinstalliert eine spezifische Version.|
||`mise rescan`|Aktualisiert die Liste der verfügbaren Versionen.|


---

