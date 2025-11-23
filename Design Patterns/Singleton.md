#designpattern

Das Singleton Pattern sorgt dafür, dass eine Klasse genau eine einzige Instanz hat und stellt einen globalen Zugriffspunkt auf diese Instanz bereit.

### Wie funktioniert es?

Die Klasse besitzt eine private statische Variable (instance), die die einzige Instanz der Klasse hält.
Der Konstruktor der Klasse ist privat oder nicht öffentlich, sodass keine weiteren Instanzen von außen erzeugt werden können.
Über eine öffentliche statische Methode (getInstance()) wird die Instanz abgefragt:
Wenn die Instanz noch nicht existiert (null), wird sie erzeugt.
Wenn die Instanz bereits existiert, wird diese zurückgegeben.
Dadurch wird sichergestellt, dass immer dieselbe Instanz verwendet wird.

### Beispiel:

GlobalCounter implementiert das Singleton.
getInstance() gibt immer dieselbe GlobalCounter-Instanz zurück.
Alle Aufrufe auf getInstance() arbeiten mit der gleichen Zähler-Instanz.
Änderungen an value sind somit global sichtbar.

```java

class GlobalCounter {

    private static GlobalCounter instance;

    public static GlobalCounter getInstance() {
        if (instance == null) {
            instance = new GlobalCounter();
        }
        return instance;

    }

    private int value;

    public int getValue() {
        return value;
    }

    public void count(int amount) {
        value += amount;
    }

}

public class SingletonSample extends GlobalCounter {

    public static void main(String[] args) {
        // Abfragen der "ersten Instanz" und hochzaehlen
        GlobalCounter counter1 = GlobalCounter.getInstance();
        counter1.count(10);
        System.out.println(counter1.getValue()); // Ergebnis 10

        // Abfragen der "zweiten Instanz" und hochzaehlen
        GlobalCounter counter2 = GlobalCounter.getInstance();
        counter2.count(7);
        System.out.println(counter2.getValue()); // Ergebnis 17

        // Both variables reference the same instance
        System.out.println(counter1.getValue()); // Ergebnis 17
        System.out.println(counter2.getValue()); // Ergebnis 17
    }
}
```
