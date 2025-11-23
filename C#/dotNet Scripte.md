Console.WriteLine("Test");

Falls du dein Skript ohne Kompilierung direkt ausführen möchtest:

Installiere das dotnet-script-Paket:
dotnet tool install -g dotnet-script

Führe das Skript direkt aus:

dotnet script dotNetScripte.cs

*****
Vorteile von dotnet script:
Du brauchst keine .csproj-Datei.
Es funktioniert ohne explizites Kompilieren.
Ideal für kleine Skripte und Experimente.
