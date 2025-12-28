# Git auf SSH umstellen

## Ausgangslage

Du arbeitest an einem bestehenden Projekt, dessen Git-Remote noch auf HTTPS (`https://github.com/...`) gesetzt ist. Trotz eingerichtetem SSH-Key musst du bei jedem Push oder Pull deinen GitHub-Benutzernamen eingeben.

## Ziel

Das Projekt soll SSH für alle Git-Operationen verwenden, sodass keine wiederholte Eingabe von Username oder Passwort nötig ist.


## ## Lösung

```bash
#1. Prüfe das aktuelle Remote:
git remote -v

#2. Stelle die URL auf SSH um:

git remote set-url origin git@github.com:USERNAME/REPO.git

#3. Teste die SSH-Verbindung:

ssh -T git@github.com

#4. Optional: Alle HTTPS-Remotes automatisch auf SSH umstellen:

git config --global url."git@github.com:".insteadOf "https://github.com/"
```

## Erklärung

- Git verwendet die Remote-URL, um den Authentifizierungsweg zu bestimmen.
- HTTPS erfordert Benutzername + Passwort (oder Token), SSH nutzt den lokal hinterlegten SSH-Key.
- Mit `insteadOf` kann man global jedes HTTPS-Remote automatisch auf SSH mappen.
## Varianten
### Fall A: Projekt nur lokal umstellen

`git remote set-url origin git@github.com:USERNAME/REPO.git`

- Wirkt nur für dieses eine Repository.
### Fall B: Alle Projekte auf SSH umstellen

`git config --global url."git@github.com:".insteadOf "https://github.com/"`

- Wirkt global für alle Repos auf deinem Rechner.


## Verwandte Themen
- [[]]
- [[]]
