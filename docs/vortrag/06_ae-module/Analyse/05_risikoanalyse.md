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
    `sehr häufig`
    : Ereignis tritt mehrmals im Monat ein.

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
| Irrtum                  | selten                          | begrenzt           | gering     | Onlinehilfe, UI-Lock |
| unsachgemäße Behandlung | selten                          | begrenzt           | mittel     | Personaa vorort, Protokollierung der Geräte nutzung |
| Einbruch, Diebstahl     | selten                          | Vernach-lässigbar  | gering     | Alamanalage, Kontrolle auf Vollständigkeit |
| Hacking                 | mittel                          | beträchtlich       | sehr hoch  | verschlüsselte Passwortspeicherung, Kontrolle gegen SQL Injection  |
| Spionage                | mittel                          | begrenzt           | gering     | Rollenverwaltung     |
| Manipulation            | selten                          | beträchtlich       | hoch       | Kontrolle gegen SQL-Injection|
| Sabotage                | selten                          | Existenz-bedrohend | sehr hoch  | Kontrolle gegen SQL-Injection                     |
| Vandalismus             | selten                          | Vernach-lässigbar  | gering     | Alarmanalge                    |
| Stromausfall            | selten                          | Vernach-lässigbar  | gering     | Notstromaggregat


