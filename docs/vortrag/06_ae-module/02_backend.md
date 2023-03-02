# Backend

## Datenbank

Für die Datenbank haben wir uns für MySQL entschieden. Für die Kommunikation mit der Datenbank verwenden wir [Prisma](https://www.prisma.io/).

!!! info "Information"
    <span class="biggerFont">Prisma ist ein Datenbank-ORM-Tool (Object-Relational Mapping), das es erleichtert, eine Datenbank mit einer Anwendung zu verbinden und Daten zwischen beiden auszutauschen. Es bietet eine typsichere und intuitive Schnittstelle, um auf Datenbanken zuzugreifen und diese zu manipulieren.</span>

### Warum Prisma?
#### 1. Performance
Prisma verwendet einen Query-Builder, der effiziente SQL-Abfragen generiert und so die Leistung der Anwendung verbessert.
#### 2. Verwaltung von Datenbeziehungen
Prisma kann komplexe Datenbeziehungen verwalten und bietet eine einfache Möglichkeit, Daten aus mehreren Tabellen abzurufen oder zu manipulieren.
#### 3. Datenmigration
Prisma bietet eine einfache Möglichkeit, Datenbankmigrationen durchzuführen, um Änderungen an der Datenbankstruktur zu verwalten.
#### 4. Sicherheit
Prisma ist gegen SQL-Injections geschützt, weil es eine Query-Syntax verwendet, die sich von herkömmlichen SQL-Abfragen unterscheidet und die Eingabevalidierung automatisch durchführt.
Weitere Informationen werden im Punkt [Sicherheit](03_sicherheit.md#sichere-datenbankkommunikation) genannt.

### ERD Diagramme

![Image title](../../assets/svg/ERDDatenbankPage_v2.svg)

![](../../assets/svg/prisma-erd.svg)

## Konfigurationsdatei für Prisma

In der Konfigurationsdatei für Prisma wird die Datenbankverbindung konfiguriert.
Des Weiteren muss, basierend auf dem ERD Diagramm ein Schema zur Migration der Datenbank erstellt.


```mysql
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model Customer {
  customerID Int       @id @default(autoincrement())
  gender     Gender
  name       String    @db.VarChar(255)
  surname    String    @db.VarChar(255)
  email      String    @db.VarChar(255)
  password   String    @db.VarChar(255)
  address     Address?
  booking    Booking[]
}

model Address {
  addressID     Int      @id @default(autoincrement())
  street       String   @db.VarChar(255)
  streetNumber Int
  zipCode      Int
  city         String   @db.VarChar(255)
  customer     Customer @relation(fields: [customerID], references: [customerID])
  customerID   Int      @unique
}

model Booking {
  bookingID     Int        @id @default(autoincrement())
  uuid          String     @db.VarChar(255)
  startDate     DateTime
  endDate       DateTime
  hardware      Hardware[]
  briefing      Boolean    @default(false)
  specification String     @db.VarChar(255)
  bookingDate   DateTime   @default(now())
  totalHours    Int
  totalCosts    Float
  customer      Customer   @relation(fields: [customerID], references: [customerID])
  customerID    Int
}

model Hardware {
  hardwareID      Int             @id @default(autoincrement())
  typ             HardwareTyp     @default(PC)
  operatingSystem OperatingSystem @default(WINDOWS)
  bokking         Booking         @relation(fields: [bookingID], references: [bookingID])
  bookingID       Int
}

enum OperatingSystem {
  WINDOWS
  DEBIAN
  UBUNTU
  LINUX_MINT
}

enum HardwareTyp {
  Laptop
  PC
  Barebone
  BYOD
}

enum Gender {
  Herr
  Frau
  Divers
}

```

## REST-API

Für die Kommunikation zwischen dem Frontend und dem Backend verwenden wir eine REST-API. Die REST-API wird ebenfalls durch das Framework [NestJS](https://nestjs.com/) erstellt.

### Payload / Request Body

Die Daten werden in einem JSON-Format übertragen. Das JSON-Format ist ein textbasiertes Format, das leicht von Menschen gelesen und geschrieben werden kann.

#### Beispiel JSON

```json
{
  "name": "Max",
  "surname": "Mustermann",
  "email": "test@test.de",
  ...
}
```

### HTTP-Methoden

Die HTTP-Methoden werden verwendet, um die Aktionen zu beschreiben, die auf den Ressourcen ausgeführt werden sollen. 

HTTP-Methoden:

- GET
- POST
- PUT
- PATCH
- DELETE

#### POST

POST wird verwendet, um eine neue Ressource zu erstellen. Die Ressource wird in der Datenbank gespeichert.

Endpunkt zum Speichern der Daten: `/api/reservation`


```ts
const response = await fetch('/api/reserveration', {
    body: JSON.stringify({
        name,
        surname,
        email,
        password,
        street,
        streetNumber,
        zipCode,
        city,
        startDate,
        endDate,
        operatingSystem,
        operatingSystem2,
        uuid,
        hardware,
        hardware2,
        briefing,
        gender,
        specification,
        totalHours: calculateTotalHours(startDate, endDate),
        totalCosts: calculateTotalHours(startDate, endDate) * PRICE_PER_HOUR,
    }),
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    }
});
```

### HTTP-Statuscodes

Durch HTTP-Statuscodes kann der Client erkennen, ob die Anfrage erfolgreich war oder nicht.

HTTP-Statuscodes:

- 200: OK
- 201: Created
- 400: Bad Request
- 500: Internal Server Error

Wenn das Speichern der Daten in der Datenbank erfolgreich war, wird ein HTTP-Statuscode 201 zurückgegeben und das Frontend kann fortfahren.
