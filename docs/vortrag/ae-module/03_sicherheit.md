# Sicherheit

## Vom BSI definierte Datenverschlüsselung

Das [BSI](https://www.bsi.bund.de) hat eine [Empfehlung](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Publikationen/TechnischeRichtlinien/TR03116/BSI-TR-03116.pdf?__blob=publicationFile&v=1) herausgegeben, die die Datenverschlüsselung in Deutschland regelt. Diese Empfehlung ist für alle Behörden und Unternehmen verbindlich. Sie ist in der [BSI-Grundschutz-Kataloge](https://www.bsi.bund.de/DE/Themen/Grundschutz/GrundschutzKataloge/GrundschutzKataloge_node.html) verankert.

Eine dieser Empfehlung ist die Nutzung des Blowfish-Algorithmus. Die Nutzung des Blowfish-Algorithmus ist in einer Handhabung für Sichere Passwörter in Embedded Devices des BSI empfohlen.

> Weiterhin sollten Passwörter immer unter Verwendung hinreichend sicherer kryptografischer
Mechanismen gespeichert und übertragen werden. Beispielsweise bietet es sich an, Password-
Based Key Derivation Function 2 (PBKDF2), bcrypt (auf Basis des Kryptoalgorithmus Blowfish)
o. ä. zu verwenden. Zumindest sollte aber ein Salted Hash genutzt werden, bei dem als Eingabe
in eine Hashfunktion neben dem Passwort ein Zufallswert eingeht, um Angriffe mit vorbe-
rechneten Daten zu erschweren. Verfahren wie MD5 oder SHA-1 (ohne Salt) sind als unsicher
zu bewerten. Von einer Verwendung ist daher abzuraten.
> [BSI-CS_069](https://www.allianz-fuer-cybersicherheit.de/SharedDocs/Downloads/Webs/ACS/DE/BSI-CS/BSI-CS_069.pdf?__blob=publicationFile&v=1)


## Blowfish

Zur verschlüsselung des Passworts, wird der Blowfish Algorithmus verwendet. Durch die benutzten der Bibliothek [bcrypt](https://en.wikipedia.org/wiki/Bcrypt) wird das Passwort mit einem zufälligen Salt verschlüsselt. Das Verschlüsselte Passwort wird dann in Form eines Hashes in der Datenbank gespeichert.

Somit ist eine Verschlüsselung des Passworts gegeben.

Weitere Informationen zur Funktionsweise und der Sicherheit des Algorithmus finden Sie ihr [Wikipedia](https://en.wikipedia.org/wiki/Blowfish_(cipher)).


# RISIKOANALYSE HIER EINFÜGEN