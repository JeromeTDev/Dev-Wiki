## `List<T>` in `C#`


```c#
//1. List erstellen 
// -> System.Collections.Generic einbinden

using System.Collections.Generic;

// Eine Liste von Strings erstellen
List<string> namen = new List<string>();

// Eine Liste mit Anfangswerten
List<int> zahlen = new List<int> { 1, 2, 3, 4 };


//2. Elemente hinzufügen
  
namen.Add("Alice"); namen.Add("Bob"); namen.Add("Charlie"); // Ein Element an einer bestimmten Position einfügen namen.Insert(1, "Anna"); // Fügt "Anna" an Index 1 ein

//3. Elemente lesen

string ersterName = namen[0]; // Index-basierter Zugriff (wie bei Arrays)
Console.WriteLine(ersterName); // "Alice"

//4. Elemente entfernen

namen.Remove("Bob"); // Entfernt "Bob"
namen.RemoveAt(0);   // Entfernt das Element an Index 0 ("Alice")

//5. Liste durchlaufen

foreach (string name in namen)
{
    Console.WriteLine(name);
}

// Oder mit for-Schleife
for (int i = 0; i < namen.Count; i++)
{
    Console.WriteLine(namen[i]);
}

//6. Weitere nützliche Methoden

bool enthaeltAlice = namen.Contains("Alice"); // Überprüft, ob "Alice" vorhanden ist
int anzahl = namen.Count;                     // Gibt die Anzahl der Elemente zurück
namen.Clear();                               // Löscht die gesamte Liste
```

### **Vergleich mit Java**

|Feature|C# (`List<T>`)|Java (`ArrayList`, `List`)|
|---|---|---|
|Typsicherheit|Ja (`List<T>`)|Ja (`ArrayList<String>`)|
|Dynamische Größe|Ja|Ja|
|Index-Zugriff|`list[0]`|`list.get(0)`|
|Element hinzufügen|`list.Add(item)`|`list.add(item)`|
|Element entfernen|`list.Remove(item)`|`list.remove(item)`|
|Größe der Liste|`list.Count`|`list.size()`|
