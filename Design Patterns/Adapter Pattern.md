![[!Anhänge/Pasted image 20250612133938.png]]
Target: Erwartete Schnittstelle
Adapter: Implementiert Target, delegiert an ExistingService
ExistingService: Die vorhandene Klasse mit inkompatibler API
### Zweck:
Übersetzt eine Schnittstelle in eine andere, um inkompatible Klassen kompatibel zu machen.
Wird genutzt, wenn alte Systeme mit neuen Bibliotheken/Frameworks zusammenspielen sollen – ohne sie komplett umzubauen.
### Typische Einsatzfälle
Bestandssystem erweitern, ohne dessen Code groß zu ändern.
Drittanbieter-Bibliotheken einbinden, deren Schnittstellen sich nicht anpassen lassen.
### Zwei Umsetzungsarten:
Vererbung (Klasse erweitert die Ziel-Schnittstelle)
Delegation (Adapter enthält eine Referenz auf die Originalklasse und ruft deren Methoden auf)



