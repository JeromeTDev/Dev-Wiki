| Art der Logik / Komponente             | **Model**                  | **ViewModel**                                 | **View**                             |
| -------------------------------------- | -------------------------- | --------------------------------------------- | ------------------------------------ |
| **Datenstrukturen (z. B. „Exercise“)** | ✅                          | 🔄 (nur wenn aufbereitet z. B. DTO/ViewModel) | ❌                                    |
| **Datenzugriff (API, DB, File)**       | ✅ (z. B. über Repositorys) | 🔄 (ruft Methoden aus Model auf)              | ❌                                    |
| **Business-Logik / Berechnungen**      | ✅                          | 🔄 (wenn nötig, aber dann delegieren)         | ❌                                    |
| **UI-spezifische Logik / Zustände**    | ❌                          | ✅ (z. B. SelectedItem, IsLoading)             | ❌                                    |
| **Property-Bindings (`INotify...`)**   | ❌                          | ✅                                             | ❌                                    |
| **Commands (z. B. ButtonClick)**       | ❌                          | ✅ (`ICommand`-Implementierung)                | 🔄 (bindet an Commands im ViewModel) |
| **XAML / Layout / Styles**             | ❌                          | ❌                                             | ✅                                    |
| **DataBindings (z. B. `{Binding}`)**   | ❌                          | ❌                                             | ✅                                    |
| **Events der UI (z. B. Click)**        | ❌                          | 🔄 (via Command gebunden)                     | ✅ (aber ohne Code-Behind-Logik)      |
### 🧠 Faustregeln:

- **Model** = „Was ist es?“ (reine Daten + Regeln)
- **ViewModel** = „Was soll passieren?“ (Bindungslogik & UI-Logik)
- **View** = „Wie sieht es aus?“ (XAML + Bindings)