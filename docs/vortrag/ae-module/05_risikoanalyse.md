# Risikoanalyse

Um den Schutzbedarf zu beurteilen, wird eine Schwachstellenanalyse durchgeführt. Dazu erfolgt die
Auswahl von Gefährdungsfaktoren zu höherer Gewalt, Fahrlässigkeit, Vorsatz, technischem Versagen
oder organisatorischen Mängeln. Anschließend wird die Eintrittswahrscheinlichkeit zu den
Gefährdungssituationen bestimmt sowie die Höhe des möglichen Schadens.


!!! note "Schwachstellen in einem IT-System"
    ``` mermaid
    graph LR
      A[Benuzter] --> B[Anwendungssystem];
      B <-->|Datenübertragung| C[Backend];
    ```

=== "Eintrittswahrscheinlichkeit"
    `selten`
    : Ereignis könnte nach heutigem Kenntnisstand höchstens alle 5 Jahre eintreten.
    
    `mittel`
    : Ereignis tritt einmal alle fünf Jahre bis einmal im Jahr ein.
    
    
    `häufig`
    : Ereignis tritt einmal im Jahr bis einmal pro Monat ein
    sehr häufig: Ereignis tritt mehrmals im Monat ein.

=== "Schadenshöhe"
    `vernachlässigbar`
    : Die Schadensauswirkungen sind gering und können vernachlässigt werden.
    
    `begrenzt`
    : Die Schadensauswirkungen sind begrenzt und überschaubar
    beträchtlich: Die Schadensauswirkungen können erheblich sein.
    
    `existenzbedrohend`
    : Die Schadensauswirkungen können ein existenziell bedrohliches,
    katastrophales Ausmaß erreichen.

=== "Risikokategorien"

    `gering`
    : Die bereits umgesetzten oder zumindest im Sicherheitskonzept vorgesehenen
    Sicherheitsmaßnahmen bieten einen ausreichenden Schutz. In der Praxis ist es üblich, geringe
    Risiken zu akzeptieren und die Gefährdung dennoch zu beobachten.
    
    `mittel`
    : Die bereits umgesetzten oder zumindest im Sicherheitskonzept vorgesehenen
    Sicherheitsmaßnahmen reichen möglicherweise nicht aus.
    
    `hoch`
    : Die bereits umgesetzten oder zumindest im Sicherheitskonzept vorgesehenen
    Sicherheitsmaßnahmen bieten keinen ausreichenden Schutz vor der jeweiligen Gefährdung.
    
    `sehr hoch`
    : Die bereits umgesetzten oder zumindest im Sicherheitskonzept vorgesehenen
    Sicherheitsmaßnahmen bieten keinen ausreichenden Schutz vor der jeweiligen Gefährdung. In
    der Praxis werden sehr hohe Risiken selten akzeptiert.


| **Gefährdung**          | **Eintrittswahrscheinlichkeit** | **Schadenshöhe**   | **Risiko** | **Schutzmaßnahmen**  |
|-------------------------|---------------------------------|--------------------|------------|----------------------|
| Fehlbedienung           | selten                          | begrenzt           | mittel     | Onlinehilfe, UI-Lock |
| Irrtum                  | selten                          | begrenzt           | gering     |                      |
| unsachgemäße Behandlung | selten                          | begrenzt           | mittel     |                      |
|                         |                                 |                    |            |                      |
| Einbruch, Diebstahl     | selten                          | Vernach-lässigbar  | gering     |                      |
| Hacking                 | mittel                          | beträchtlich       | sehr hoch  |                      |
| Spionage                | mittel                          | begrenzt           | gering     |                      |
| Manipulation            | selten                          | beträchtlich       | hoch       |                      |
| Sabotage                | selten                          | Existenz-bedrohend | sehr hoch  |                      |
| Vandalismus             | selten                          | Vernach-lässigbar  | gering     |                      |
|                         |                                 |                    |            |                      |
| Stromausfall            | selten                          | Vernach-lässigbar  | gering     |


# Schutzbedarfsanalyse

Zur Prävention von Risiken werden Sicherheitsmaßnahmen ergriffen. 

- Durch Schulungen der Mitarbeiter werden die Risiken der Fehlbedingung und des Irrtums minimiert.
- Durch die gezwungene Verwendung von sicheren Passwörtern wird der Diebstahl von Daten verhindert.
- Durch die Bereitstellung einer Online-Hilfe, sowie eines Service-Teams wird die Fehlbedienung
  minimiert.

> Durch Schulungen wird die größte Schwachstelle in einem IT-System minimiert.