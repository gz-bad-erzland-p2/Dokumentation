# Reflexion

## Wissens- und Kompetenzzuwachs
Unser Entwicklungsprozess hat mithilfe des Scrum-Prinzips sehr gut funktioniert. Es hat uns eine gute Arbeitsteilung ermöglicht. Dadurch konnten die Stärken jedes einzelnen optimal eingesetzt werden. Weiterhin haben wir uns dadurch gegenseitig gut unterstützen und Wissen teilen können. 
Technisch hatten wir einen besonders guten Erfahrungszuwachs in Git, NextJS und MkDocs.
Die Systemintegratoren konnten besonders ihre Kompetenz bei der Automatisierung von der Servererstellung verbessert. 
Dadurch konnten mehr Server automatisiert werden als im Pflichtenheft mindestens vorgegeben war.

Schwer war es durch die getrennten Räume und Krankheitsausfälle jeden Tag gemeinsam den Fortschritt persönlich zu besprechen.
Gelöst haben wir das Problem in dem wir das Gespräch verschoben oder/und digital ausgeführt haben.
Das müssen wir verbessern.
Den Systemintegratoren ist der Einstieg in die Projektumsetzung aufgrund von fehlendem Wissen schwergefallen. Dies hat sich jedoch im Projektverlauf verbessert.

Eine Schwierigkeit die dem Projektaufbau als Schul- und Wirtschaftsprojekt geschuldet war, ist die schwierige Immersion und die dadurch teilweise unklaren Anforderungen.

## Inhaltliche Reflexion

Zielstellungen an die zu erstellende Website aus dem Pflichtenheft:

> Der Buchungsdienst soll über einen Link auf der Gemeindeseite erreichbar sein. 
Das Logo der Gemeinde soll auf der Mietwebseite erscheinen (Cooperate Design). 
Die Website soll von jedem beliebigen Endgerät (mobil und Desktop) aufrufbar sein.
Die Reservierung durch die Interessenten ist mit einer Online-Hilfe zu begleiten. 
In dieser werden sowohl die vorhandenen Hard- als auch Softwarespezifikationen benannt. 
Alle verfügbaren Schnittstellen am Arbeitsplatz sind anzugeben und sind erläutert. 
Die Website und die Mietkonditionen können anonym besucht und eingesehen werden. 
Für einen Mietvorgang muss eine Registrierung oder Anmeldung (mit vorangegangen Registrierung) 
unter Angabe von persönlichen Daten erfolgen (HGB). 
Diese werden zum Zweck der Buchung gespeichert. Es erfolgt eine stundengenaue Abrechnung der Mietzeit. 
Der Nutzer muss die AGBs und Datenschutzrichtlinien des Gemeindezentrums akzeptieren.
(siehe Pflichtenheft 6.2.6.5. Anforderung Website)

Inhaltlich konnten nahezu alle Anforderungen erfüllt werden. Schwierigkeiten gab es vor allem bei der logischen Implementierung der Zeitauswahl und Abrechnung den Buchungszeitraum. Daraus resultiert eine fehlende dynamische implementierung der bereits gebuchten Zeiträume. Diese werden moment im Buchungsprozess nicht berücksichtigt.  
Zu Testzwecken können künstliche Buchungen eingetragen werden.

Weitere Schwierigkeiten gab es bei der Implementierung der Registrierung (nur außerhalb des Buchungsprozesses) und Anmeldung. Diese wurden im 1. Prototypen nicht umgesetzt.

    
## Zusammenfassung

- [x] Corporate Design
- [x] Begleitende Online Hilfe
- [x] Dokumentation zu Hardwarespezifikationen inkl. Schnittstellen
- [x] Dokumentation zu Softwarespezifikationen
- [x] Registrierungsvorgang innerhalb des Buchungsprozesses
- [ ] Registrierungsvorgang außerhalb des Buchungsprozesses
- [ ] Anmeldevorgang innerhalb und außerhalb des Buchungsprozesses
- [x] Stundengenaue Abrechnung der Mietzeit
- [x] Bestätigung der Datenschutzrichtlinien

Weitere Inhalte:
- Mehr Maschinen konnten automatisiert werden als mindestens vorausgesetzt
