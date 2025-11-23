![[!Anhänge/Pasted image 20250612132119.png]]Das Observer-Pattern ermöglicht die automatische Benachrichtigung mehrerer Beobachter (Observers), wenn sich der Zustand eines Subjekts (Observable) ändert. Das Subjekt verwaltet eine Liste von Beobachtern, die sich anmelden (attach) oder abmelden (detach) können. Bei Änderungen ruft das Subjekt die Beobachter über deren `update()`-Methode auf. Es gibt zwei Benachrichtigungsarten:

- **Push-Notification:** Nur Signal, Daten müssen die Beobachter selbst abfragen.
- **Push-Update-Notification:** Subjekt sendet die Änderung direkt mit.

Das Muster sorgt für geringe Kopplung, Flexibilität und klare Trennung von Subjekt und Beobachtern, birgt aber auch Risiken wie Feedbackschleifen oder hohe Kosten bei vielen Beobachtern.

- **`fireUpdate()`**: Vom **Subjekt**, löst die Benachrichtigung aus.
- **`update()`**: Im **Beobachter**, reagiert auf die Änderung.

> `fireUpdate()` → „Hey, alle aufpassen!“  
> `update()` → „Okay, ich habe's gehört – was muss ich tun?“