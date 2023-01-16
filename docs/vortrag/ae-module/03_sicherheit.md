# Sicherheit

## Vom BSI definierte Datenverschlüsselung

Das [BSI](https://www.bsi.bund.de) hat eine [Empfehlung](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Publikationen/TechnischeRichtlinien/TR03116/BSI-TR-03116.pdf?__blob=publicationFile&v=1) herausgegeben, die die Datenverschlüsselung in Deutschland regelt. Diese Empfehlung ist für alle Behörden und Unternehmen verbindlich. Sie ist in der [BSI-Grundschutz-Kataloge](https://www.bsi.bund.de/DE/Themen/Grundschutz/GrundschutzKataloge/GrundschutzKataloge_node.html) verankert.

Eine dieser Empfehlung ist die Nutzung des Advanced Encryption Standard (AES). 

> Bei diesem Verfahren handelt es sich um eine Kernkomponente vieler weitverbreiteter kryptografischer Lösungen. Der AES wurde in einem öffentlichen Prozess entwickelt und vor wie auch nach seiner Standardisierung von einer großen Anzahl von Experten auf mögliche kryptografische Schwächen untersucht. Stand der Forschung ist zur Zeit, dass nur in wesentlich abgeschwächten Versionen des AES signifikante kryptografische Probleme gefunden werden konnten.
> Quelle: [BSI-TR-03116](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Publikationen/TechnischeRichtlinien/TR03116/BSI-TR-03116.pdf?__blob=publicationFile&v=1)

## AES-256

Zur Verschlüsselung des Passworts, wird genau dieser Algorithmus verwendet. Durch die benutzten der Bibliothek [bcrypt](LINK HIEREINFÜGEN) wird das Passwort mit einem zufälligen Salt verschlüsselt. Dieser Salt wird dann in der Datenbank gespeichert.

Somit ist eine Verschlüsselung des Passworts gegeben.
