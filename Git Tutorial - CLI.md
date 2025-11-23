## 1. Lokales Repo erstellen und online Hochladen
```bash
cd /pfad/zu/deinem/ordner
git init
git add .
git commit -m "Initial commit"
gh repo create mein-repo --public --source=. --remote=origin --push
```

## 2. Workflow für Repo mit Schreibrechten
```bash
# 0. Repo klonen
git clone https://github.com/orgName/mein-repo.git
cd mein-repo

# 1. Lokale Kopie aktuell halten
git fetch
git checkout main
git pull

# 2. Feature-Branch erstellen
git checkout -b feature/<issue-nummer>-kurze-beschreibung

# 3. Änderungen durchführen
git add .
git commit -m "feat: Beschreibung der Änderung"

# 4. Branch pushen
git push -u origin feature/<issue-nummer>-kurze-beschreibung

# 5. Pull Request erstellen
gh pr create --fill --reviewer Nick

```

## 3. Workflow ohne Schreibrechte (Fork)
```bash
# 0. Fork klonen
git clone https://github.com/deinName/dasRepo.git
cd dasRepo

# 1. Upstream setzen
git remote add upstream https://github.com/originalAutor/dasRepo.git

# 2. Main aktuell halten
git fetch upstream
git checkout main
git pull upstream main

# 3. Feature-Branch
git checkout -b feature/<issue-nummer>-kurze-beschreibung

# 4. Arbeiten & committen
git add .
git commit -m "Beschreibung der Änderung"

# 5. Branch zum Fork pushen
git push origin feature/<issue-nummer>-kurze-beschreibung

# 6. PR erstellen
# GitHub UI zeigt automatisch "Compare & Pull Request"
# oder per CLI:
gh pr create
# -> fragt Titel, Beschreibung usw. ab
--fill # = Titel+Beschreibung aus den Commits nehmen
```
