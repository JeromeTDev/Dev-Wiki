#architektur #mvc #mvvm

### **1. Wer steuert die Logik?**

- **MVC:**
    - Der **Controller** entscheidet, wie die View auf Benutzeraktionen reagiert.
    - Beispiel: Ein Klick auf einen Button löst eine Controller-Methode aus, die das Model updatet und dann die View neu rendert.
    - **Die View kann auch direkt vom Model lesen** (z. B. bei traditionellem Web-MVC).
        
- **MVVM:**
    - Das **ViewModel** hält den UI-Zustand und bietet Datenbindungen an.
    - Beispiel: Ein Button ist direkt an einen Befehl (`Command`) im ViewModel gebunden. Die View weiß nichts vom Model.
    - **Die View aktualisiert sich automatisch** über Datenbindung (z. B. bei Änderungen im ViewModel).

→ **MVVM entfernt die explizite Steuerung durch den Controller zugunsten von automatischen Bindungen.**

### **2. Wer kennt wen?**

|MVC|MVVM|
|---|---|
|**View** kennt **Controller** (ruft ihn auf).|**View** kennt **ViewModel** (nur Bindings).|
|**Controller** kennt **View** und **Model**.|**ViewModel** kennt **Model**, aber **nicht die View**.|
|**Model** kennt niemanden.|**Model** kennt niemanden.|

→ **MVVM hat eine strengere Trennung:** Die View könnte theoretisch ausgetauscht werden, ohne dass das ViewModel es merkt.

---

### **3. Wie wird die UI aktualisiert?**

- **MVC:**
    
    - Der Controller muss die View manuell updaten (z. B. `view.render(model.getData())`).
    - Häufig bei Web-Apps: Nach einem Formular-Submit lädt der Server eine neue View.
        
- **MVVM:**
    
    - Die View beobachtet das ViewModel (z. B. über `Observable`-Properties).
    - Beispiel (WPF/XAML):
```xml
<TextBlock Text="{Binding UserName}" /> <!-- Automatische Aktualisierung -->
```

Kein manuelles "Refresh" nötig!

→ **MVVM nutzt Datenbindung, MVC manuelle Updates.**

### **4. Typische Code-Beispiele**

#### **MVC (Web-Ansatz)**

```javascript
// Controller
app.post("/update", (req, res) => {
    model.setData(req.body); // Model updaten
    res.render("view", model.getData()); // View neu rendern
});
```
- Der Controller ist der "Vermittler".

#### **MVVM (z. B. WPF mit C#)**

```csharp
// ViewModel
public class UserViewModel {
    public string UserName { get; set; } // Automatisch an UI gebunden
    public ICommand SaveCommand { get; } // Befehl für Button
}
```
- Die View bindet sich selbstständig an `UserName` und `SaveCommand`.
---

### **5. Wann welches Muster?**

- **MVC:**
    - Einfache Web-Apps (z. B. Django, Ruby on Rails).
    - Wenn klare HTTP-Request/Response-Zyklen vorliegen.

- **MVVM:**
    - Komplexe UIs mit Echtzeit-Updates (z. B. WPF, Angular, React mit Zustandsmanagement).
    - Wenn Datenbindung genutzt werden soll (z. B. Formulare mit Live-Validierung).
### **Zusammenfassung der Unterschiede**

|Aspekt|MVC|MVVM|
|---|---|---|
|**Zuständigkeit**|Controller steuert die Logik|ViewModel stellt Daten bereit|
|**Kommunikation**|Controller ↔ View ↔ Model|View ↔ ViewModel ↔ Model (Bindings)|
|**Datenbindung**|Meist manuell|Automatisch (z. B. Two-Way-Binding)|
|**Testbarkeit**|Controller schwerer testbar|ViewModel leicht testbar (keine UI-Abhängigkeit)|
|**Typische Anwendungen**|Web-Apps (Rails, Django)|Moderne UI-Frameworks (WPF, Angular, Vue)|

### **Fazit**

- **MVC** ist einfacher, aber kann bei komplexen UIs unübersichtlich werden.
- **MVVM** ist besser für moderne, datenbindungsfähige Anwendungen und erleichtert die Wartbarkeit.


- **Model** = "Was ist gültig?" (Daten + Validierung/Berechnungen).
- **ViewModel** = "Wie zeige ich es an?" (UI-Zustand, Befehle).
- **View** = "Wie sieht es aus?" (XAML/CSS).

✅ **Button-Logik ins ViewModel** (als `ICommand`).  
✅ **Validierung/Kernlogik ins Model**.  
✅ **View bindet sich automatisch via XAML**.