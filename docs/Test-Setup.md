# Test-Setup

## Voraussetzungen

- **Java 17 JDK** (oder höher) — Spring Boot 3.1.4 erfordert mindestens Java 17
- Maven-Wrapper (`mvnw`) ist im Projekt enthalten
- Keine externen Dienste (Datenbank etc.) erforderlich — die Anwendung nutzt ausschließlich In-Memory-Datenhaltung

## Tests ausführen

```bash
cd complete
./mvnw test
```

Alternativ mit Gradle:
```bash
cd complete
./gradlew test
```

## Vorhandene Tests

### Unit-Tests

| Test | Datei | Status | Beschreibung |
|---|---|---|---|
| `HelloControllerTest.getHello()` | `src/test/java/com/example/springboot/HelloControllerTest.java` | **FEHLERHAFT** | MockMvc-Test auf GET `/` — erwartet `"Greetings from Spring Boot!"`, Controller gibt aber `"Greetings from Item Manager in Spring Boot!"` zurück |

### Integrationstests

| Test | Datei | Status | Beschreibung |
|---|---|---|---|
| `HelloControllerIT.getHello()` | `src/test/java/com/example/springboot/HelloControllerIT.java` | **FEHLERHAFT** | HTTP-Test mit TestRestTemplate auf GET `/` — gleicher Assertion-Fehler wie Unit-Test |

## Fehlende Testabdeckung (geplant)

### Priorität 1 — Bestehende Tests reparieren
- `HelloControllerTest`: Erwartete Nachricht an geänderten Controller-Output anpassen
- `HelloControllerIT`: Gleiche Anpassung

### Priorität 2 — Controller-Tests
- **ItemRestController**: GET `/items`, GET `/items/{id}`, GET `/items/search`, POST `/items`
- **ItemController**: GET/POST `/items-gui`, Listenansicht, Detailansicht, Löschung
- **AboutController**: GET `/about` Template-Rendering

### Priorität 3 — Domain-Model-Tests
- **Item**: Gewichtslogik (`getBestWeight()`), Kompositionsstruktur, Getter/Setter
- **AppStore**: Item-Erstellung, ID-Generierung, Abruf

### Priorität 4 — Integrationstests
- End-to-End-Tests für den kompletten Item-Lebenszyklus (Erstellen → Anzeigen → Löschen)
- REST-API-Suche mit verschiedenen Suchbegriffen

## Externe Dienste

Aktuell werden **keine externen Dienste** benötigt. Die Anwendung arbeitet vollständig In-Memory. Sollte zukünftig eine Datenbank angebunden werden, wird eine `docker-compose.yml` bereitgestellt.

## Aktuelle Einschränkung

Auf dem Entwicklungssystem ist nur Java 8 JRE installiert. Zum Ausführen der Tests wird ein **Java 17 JDK** benötigt. Installation z. B. über:
- [Eclipse Temurin](https://adoptium.net/) (empfohlen)
- `sudo apt install openjdk-17-jdk` (Linux)
