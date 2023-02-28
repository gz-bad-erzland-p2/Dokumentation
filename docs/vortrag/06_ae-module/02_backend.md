# Backend

## Datenbank

Für die Datenbank haben wir uns für MySQL entschieden. Für die kommunikation mit der Datenbank verwenden wir [Prisma](https://www.prisma.io/).

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

TODO: REST-API-Framework
