# Analyse

## USE-CASE Diagramm

Zu Begin der Analyse haben wir ein basierend auf den Userstorys, ein USE-Case-Diagramm erstellt, es dient dazu die Funktion des Systems genauer zu identifizieren und zu verstehen. Es bildet somit die Grundlage für die Entwicklung. Große Relevanz hatte es in der folgenden Entwurfsphase, in welcher wir uns bei der Planung darauf beziehen konnten und es zur Überprüfung nutzen konnte,  ob alle Funktionen umgesetzt wurden.
![USE-CASE](https://github.com/gz-bad-erzland-p2/Dokumentation/raw/master/docs/assets/svg/UseCase_v3.svg)


## Risikoanalyse

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


| **Gefährdung**          | **Eintrittswahrscheinlichkeit** | **Schadenshöhe**   | **Risiko** | **Schutzmaßnahmen**                                               |
|-------------------------|---------------------------------|--------------------|------------|-------------------------------------------------------------------|
| Fehlbedienung           | häufig                          | begrenzt           | mittel     | Onlinehilfe, UI-Lock                                              |
| Irrtum                  | häufig                          | begrenzt           | gering     | Onlinehilfe, UI-Lock                                              |
| unsachgemäße Behandlung | mittel                          | begrenzt           | mittel     | Personal vor Ort, Protokollierung der Gerätenutzung               |
| Einbruch, Diebstahl     | mittel                          | Vernach-lässigbar  | gering     | Alarmanlage, Kontrolle auf Vollständigkeit                        |
| Hacking                 | mittel                          | beträchtlich       | sehr hoch  | verschlüsselte Passwortspeicherung, Kontrolle gegen SQL Injection |
| Spionage                | mittel                          | begrenzt           | gering     | Rollenverwaltung                                                  |
| Manipulation            | selten                          | beträchtlich       | hoch       | Kontrolle gegen SQL-Injection                                     |
| Sabotage                | selten                          | Existenz-bedrohend | sehr hoch  | Kontrolle gegen SQL-Injection                                     |
| Vandalismus             | mittel                          | Vernach-lässigbar  | gering     | Alarmanlage                                                       |
| Stromausfall            | selten                          | Vernach-lässigbar  | gering     | Notstromaggregat                                                  |

## Schutzbedarfsanalyse

Zur Prävention von Risiken werden Sicherheitsmaßnahmen ergriffen.

Wir minimieren die Risiken von Fehlbedingungen und menschlichen Fehlern durch regelmäßige Schulungen der Mitarbeiter. Durch die verpflichtende Verwendung von sicheren Passwörtern, verhindern wir erfolgreich den Diebstahl von Daten. Um Fehler bei der Bedienung zu vermeiden, stellen wir unseren Nutzern eine Online-Hilfe und ein Service-Team zur Verfügung.

> Durch Schulungen wird die größte Schwachstelle in einem IT-System minimiert.

## Umgesetzte Qualitätsmerkmale

![ISO-25010-Qualitaetskriterien-fuer-Software](https://user-images.githubusercontent.com/57149152/214249290-f8be5a93-f559-4e93-b987-ca380c2d430f.jpg)

Abbildung 2: Qualitätskriterien für Software

Unter Betrachtung der Anforderungen des Gemeindezentrums Bad Erzland wurden folgenden Qualitätskriterien besondere Priorität eingeräumt.

1. Usability:
   Die Office-Sharing Website muss für die Bürger einfach nutzbar sein, damit das Angebot auch für technisch weniger affine Menschen zur Verfügung steht.
   Dafür wurde die Oberfläche so designt das sie besonders intuitiv zu benutzen ist. Weiterhin steht für die Bürger eine Hilfefunktion zur Verfügung, die alle Schritte einer Arbeitsplatzbuchung erklärt. Durch diese Schritte konnten wir eine optimale Usability gewährleisten.

2. Einfache Wartbarkeit:
   Um den Wartungsaufwand für die Software möglichst gering zu halten wurde die Software unter Beachtung des [Clean-Code-Prinzips](https://t2informatik.de/wissen-kompakt/clean-code/) entwickelt. Dadurch konnten wir eine gute Lesbarkeit des Codes gewährleisten. Durch eine Automatisierung der Server wurde die Wartbarkeit weiterhin verbessert. Zusammenfassend konnten wir die einfache Wartbarkeit voll umsetzen.

3. Sicherheit:
   Da Gemeindezentren wie andere öffentliche Einrichtungen für Hackerangriffe besonders gefährdete Ziele sind, wurde auf Sicherheit besonders geachtet.
   Besonders vorgestellt werden:
- [AE Sicherheit](/vortrag/06_ae-module/03_sicherheit/)
- [SI Sicherheit](/vortrag/09_si-module/03_sicherheit/)

[^1]: Schichtenmodell der Informatik: Skript_WissenschaftInformatikIGD19.docx, Seite 4, ©Rie (27.02.2023)
[^2]: Qualitätskriterien für Software: https://www.inztitut.de/blog/glossar/iso-25010/ (24.01.23)
[^3]: https://t2informatik.de/wissen-kompakt/clean-code/ (27.02.2023)
