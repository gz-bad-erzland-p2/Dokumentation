# Entwicklungskonventionen

## Namensgebung
### Dateinamen

Dateinamen sind im `CamelCase` zu benennen. Dabei ist der erste Buchstabe groß zu schreiben und alle weiteren Wörter im Namen sind mit einem Großbuchstaben zu beginnen.

!!! success "Gutes Beispiel"
    `SollIstAnalyse.md`

!!! failure "Schlechtes Beispiel"
    `soll-ist-analyse.md`

!!! danger "ACHTUNG"
Die Dateinamen müssen bei Komponenten immer das Wort `Component` enthalten.

### Ordner

Ordner sind klein zu schreiben. Es werden keine Leerzeichen oder andere separatoren verwendet. Ein Ordner bündelt immer mehrere Dokumente zum benannten Thema.

!!! success "Gutes Beispiel"
    
`api`
    \- `wizard`
        \- `db`

!!! failure "Schlechtes Beispiel"
    
`api-wizard-db`

### Andere Namen

#### HTML / TS / TSX
* Datentypen: `UpperCamelCase`
* Konstruktoren: `UpperCamelCase`
* Variablen: `lowerCamelCase`
* Methoden: `lowerCamelCase`
* Konstanten: `UPPER_CASE`
* Enums: `UPPER_CASE`
* Klassen: `UpperCamelCase`
* Interfaces: `UpperCamelCase`
* Exceptions: `UpperCamelCase`

#### CSS
* CSS-Klassen: `lower-case`
* CSS-IDs: `lower-case`

## Git

### Commit Messages

Commit Messages sind bevorzugt in Englisch zu schreiben. Sie beinhalten eine kurze Zusammenfassung der Änderungen und sind im Imperativ zu schreiben. Sie beginnen mit einem Großbuchstaben und enden **nicht** mit einem Punkt.

!!! success "Gutes Beispiel"
    `Add new component`

!!! failure "Schlechtes Beispiel"
    `added new component.`

### Branches

#### Aufbau
* `main` - Hauptbranch (Read-Only)
* `nav-bar` - Branch in dem nur Änderungen an der Navigationsleiste gemacht und committed werden

!!! danger "ACHTUNG"
Branches sind im `kebab-case` zu benennen. Dabei sind alle Wörter klein zu schreiben und mit einem Bindestrich getrennt.

!!! question "Wann wird ein neuer Branch erstellt?"
Bitte erstellt für jede Entwicklung einer Komponente einen eigenen Branch. Der Branchname ist in `kevav-case` zu schreiben und enthält den Namen der Komponente. Der Branchname enthält den Namen der Komponente.


#### Merge/ Pull Request

!!! danger "ACHTUNG"
Es wird **nie** direkt in den `main`-Branch committed. Es wird immer ein Pull-Request erstellt und dieser muss von mindestens einem anderen Entwickler genehmigt werden. Danach kann die Änderung in den `main`-Branch gemerged werden.

!!! info "Hinweis"
Eine Pull-Request ist eine Anfrage an einen anderen Entwickler, die Änderungen in den `main`-Branch zu mergen.

### Befehle

* `git add .` - Fügt alle Änderungen zum Commit hinzu
* `git commit -m "<commit-message>"` - Commited die Änderungen mit einer Commit-Message

* `git branch` - Zeigt alle Branches an
* `git checkout -b <branch-name>` - Erstellt einen neuen Branch und wechselt in diesen
* `git branch -d <branch-name>` - Löscht einen Branch

* `git checkout <branch-name>` - Wechselt in einen anderen Branch
* `git merge <branch-name>` - Merged einen Branch in den aktuellen Branch

* `git push origin <branch-name>` - Pushed einen Branch auf den Remote-Server
* `git pull origin <branch-name>` - Pullt einen Branch vom Remote-Server


## UI-Design

Das Design ist nach den in Figma erstellten Konzepten zu erstellen.

Siehe https://www.figma.com/file/A5B3B7TBqCzpDUkpVlvEI5/Untitled?node-id=2%3A15&t=kyYyLXwLs7iXFrln-1

### Tailwind

Tailwind ist ein CSS-Framework, welches die Erstellung von CSS vereinfacht. Es wird in diesem Projekt verwendet, um die CSS-Dateien möglichst klein zu halten und die Komponenten möglichst einfach zu gestalten.

Die Dokumentation von Tailwind ist unter https://tailwindcss.com/docs zu finden.

### Farben

Es werden nur die vorher gemeinsam bestimmten Farben verwendet. Diese sind in der Datei `docs/!intern/colors.md` und in der Tailwind-Config-Datei `tailwind.config.js` zu finden.