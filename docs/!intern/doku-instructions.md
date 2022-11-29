# Hinweise zur Doku
!!! quote "Link zum NextJS Projekt"
    https://github.com/gz-bad-erzland-p2/NextJS-Office-Sharing

!!! quote "Link zur Doku"
    https://gz-bad-erzland-p2.github.io/Dokumentation/

## Warum nutzen wir MkDocs?
Wir nutzen [MkDocs](https://www.mkdocs.org/) mit dem [Material Design](https://squidfunk.github.io/mkdocs-material/) zur Dokumentation, da dies einfach und übersichtlich strukturiert ist. Durch den modularisierten Aufbau können mehrere Teammitglieder gleichzeitig an der Doku arbeiten. Ein weiterer Vorteil von MkDocs ist, das es auf [Markdown](https://de.wikipedia.org/wiki/Markdown) basiert. Dadurch ist ein einfacher Anderungsverlauf sichtbar.

## Wo ist die Dokumentation einsehbar?
Die Dokumentation ist unter [dieser URL](https://gz-bad-erzland-p2.github.io/Dokumentation/) einsehbar. Ihr findet den Link ebenfalls oben rechts auf der Repo-Code-Seite in der Ecke.

## Was ist alles zu beachetn?

## Optional: Installation von MkDocs auf Ubuntu

`sudo apt install mkdocs`

Gesamte MkDocs doku hier:  [mkdocs.org](https://www.mkdocs.org).

**Dinge die dein Leben vereinfachen:**

* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

### Namenskonventionen

#### Datei- und Ordnernamen
Alle Datei sowie Ordnernamen sind klein zu benennen.

!!! success "Gutes Beispiel"
    `soll-ist.md`

!!! failure "Schlechtes Beispiel"
    `Soll-Ist.md`


Ein Ordner bündelt immer mehrere Dokumente zum Ordner benannten Thema.

#### Datei- und ÜS-Namen
Die Dateinamen sollten im groben Sinn mit der Hauptüberschrift in der Markdown Datei übereinstimmen.

!!! success "Gutes Beispiel"

    Name: `soll-ist.md`

    ÜS: `Ist Soll Analyse`


!!! failure "Schlechtes Beispiel"
    
    Name: `soll-ist.md`
    
    ÜS: `Analyse zum Wasserfalldiagramm`

??? tip "Hinweis zur Übersichtlichkeit"
    Für eine besser Übersichtlichkeit können Dateinamen auch einen Prefix in Form eines Zahlenwertes haben. Dieser sollte jedoch nicht mehr als 2 Stellen haben.


#### Dokumentaufbau
Jedes Dokument muss zuerst eine Überschrift 1. Grades haben. Diese fast den Inhalt des Themas zusammen.

!!! note "Bitte beachten"
    Bitte beachtet und benutzt die integrierten Funktionen von Markdown zur Einbindung von Tabellen, Bildern, Links, Quellcode, Aufzählungen und Überschriften.
    MkDocs erweitert den Funktionsumfang von Markdown um einige Funktionen. Diese sind [hier](https://squidfunk.github.io/mkdocs-material/reference/admonitions/) zu finden.

### Projekt auschecken
Um das Projekt zu bearbeiten, müsst ihr `Git` installiert haben.
Gebt dann den Befehl `git clone https://github.com/gz-bad-erzland-p2/Dokumentation.git -b master` in ein Terminal ein. Das Projekt wird nun heruntergeladen und bei euch auf dem PC "installiert".
Nachdem ihr einige Dinge geändert habt, könnte ihr über ein entsprechendes grafisches Tool die Änderungen comitten oder per Befehl wiefolgt:

    git add *
    git commit -m "DEINE ÄNDERUNGSNARICHT HIER"
    git push

### Online Änderungen machen
Klickt die entsprechende Datei an und klickt dann auf den Stift oben Rechts im Bild sichtbar.

![Bild Bearbeiten](https://imgur.com/fJn4d7K.png)
Ich würde euch Empfehlen den "Quelltext" in bspw. einen Onlineeditor wie [Stackedit](https://stackedit.io/app#) zu kopieren und dort eure Änderungen zu machen. Danach könnt ihr den "Quelltext" wieder zurück kopieren und die Änderungen mit einer entsprechenden Nachricht (unten im Bild) comitten.

![Bild Commiten](https://imgur.com/WQLovpz.png)
## Änderungen auf die Webseite ausrollen
Nach einem commit werden die Änderungen automatisch durch einen CI-Job auf den GitHub-Web-Server deployt. Nach ein paar Minuten sollten die Änderungen dann auf der Webseite sichtbar werden.
Den Fortschritt der einzelnen Builds in der Pipeline kann man [hier](https://github.com/gz-bad-erzland-p2/Dokumentation/actions) einsehen.

## Nützliche Links
- https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/
- https://www.mkdocs.org/
- https://stackedit.io/app#
- https://github.com/gz-bad-erzland-p2/Dokumentation/actions
