# spring-item

Beispielanwendung mit dem Spring Framework zur Verwaltung hierarchischer Produktstrukturen.

## Beschreibung

Spring-Boot-Webanwendung, die eine Artikelverwaltung mit hierarchischer Produktkomposition demonstriert. Items können Eigenschaften (Characteristics) wie Material und Gewichtsklassen besitzen und sich aus anderen Items zusammensetzen (z. B. Auto → Achse → Rad).

## Funktionsumfang

- **HTML-GUI** (Thymeleaf): Erstellen, Anzeigen, Auflisten und Löschen von Items unter `/items-gui`
- **REST-API** (JSON): CRUD-Operationen und Suche unter `/items`
- **Beispieldaten** werden beim Start automatisch geladen

## Technologie-Stack

- Java 17, Spring Boot 3.1.4, Spring MVC, Thymeleaf
- Maven und Gradle Build-Support
- In-Memory-Datenhaltung (kein Datenbankserver erforderlich)

## Starten

```bash
cd complete
./mvnw spring-boot:run
```

Die Anwendung ist dann erreichbar unter `http://localhost:8081`.

## Projektstatus

Siehe [devstate_active.md](devstate_active.md) für den aktuellen Entwicklungsstand.

## Ursprung

Basierend auf der Beispielanwendung: https://github.com/spring-guides/gs-spring-boot.git
