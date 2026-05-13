# Jupyter-Style REPL

## Kurzbeschreibung
- Nutze `# %%` als **Zellenmarker** im Python-Code.
- Sende Zeilen, Blöcke oder ganze Dateien an die REPL (IPython).
- Graphen und Outputs erscheinen in GUI-Fenstern oder Inline (je nach IPython-Magic).

## Häufig
```python
# %%
# erster Block
x = 1
y = 2
print(x + y)

# %%
# zweiter Block
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)
plt.plot(x, y)
plt.show()

``` 

## Optionen, die ich nutze

- `<leader>pb` → aktueller kompletter Block an REPL
- `<leader>pl` → aktuelle Zeile an REPL senden
- `<leader>ps` → markierten Block / Visual Mode senden
- `<leader>pr` → ganze Datei senden
- `<leader>pp` → Absatz / Zelle an REPL senden
- `<leader>pt` → REPL toggle
- `<leader>pR` → REPL neu starten
- `<leader>pi` → REPL unterbrechen / Stop





## Typische Kombinationen

- 
- 

## Achtung 

- Inline-Graphen im Terminal nicht sichtbar → GUI-Fenster nötig
- Block senden überschreibt nicht vorherige Variablen – REPL behält Zustand