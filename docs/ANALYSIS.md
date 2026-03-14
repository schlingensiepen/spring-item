# Projektanalyse: spring-item

## 2.1 Projektzweck

Dieses Projekt ist eine **Spring-Boot-Beispielanwendung zur Darstellung von Produktstrukturen** ("Beispielanwendung mit dem Springframework zur Darstellung von Produktstrukturen"). Es basiert auf dem offiziellen Spring Boot Getting Started Guide und erweitert diesen um eine Artikelverwaltung.

Die Anwendung simuliert ein Inventarverwaltungssystem, in dem Produkte (Items) mit Eigenschaften (Characteristics) wie Material und Gewichtsklassen verwaltet werden. Items können hierarchisch zusammengesetzt werden (z. B. ein Auto enthält Achsen, die wiederum Räder enthalten).

**Funktionsumfang:**
- Erstellen, Anzeigen, Auflisten und Löschen von Items über eine HTML-GUI (Thymeleaf)
- REST-API für CRUD-Operationen und Suche auf Items (JSON)
- Darstellung hierarchischer Produktstrukturen als Baumansicht
- Beispieldaten werden beim Start automatisch geladen (SampleStore)

**Abweichungen zwischen Dokumentation und Code:**
- Die README-Dateien beschreiben hauptsächlich den originalen Spring Boot Getting Started Guide. Die tatsächliche Item-Management-Funktionalität ist dort kaum dokumentiert.
- Die Tests erwarten die originale Begrüßung `"Greetings from Spring Boot!"`, der HelloController gibt jedoch `"Greetings from Item Manager in Spring Boot!"` zurück — ein klares Zeichen, dass der Code weiterentwickelt, aber die Tests nicht angepasst wurden.

---

## 2.2 Einstiegspunkte

| Einstiegspunkt | Dateipfad | Beschreibung |
|---|---|---|
| **Main-Klasse** | `complete/src/main/java/com/example/springboot/Application.java` | `@SpringBootApplication` — startet die Anwendung, gibt beim Start alle Spring-Beans auf der Konsole aus |
| **HelloController** | `complete/src/main/java/com/example/springboot/HelloController.java` | `@RestController` — GET `/` liefert Begrüßung mit Anzahl der Items |
| **AboutController** | `complete/src/main/java/com/example/springboot/AboutController.java` | `@Controller` — GET `/about` rendert aboutTemplate.html mit Zeitstempel |
| **ItemController** | `complete/src/main/java/com/example/springboot/ItemController.java` | `@Controller` — GUI-CRUD: GET/POST `/items-gui`, `/items-gui/list`, `/items-gui/{id}/show`, `/items-gui/{id}/delete` |
| **ItemRestController** | `complete/src/main/java/com/example/springboot/ItemRestController.java` | `@RestController` — REST-API: GET/POST `/items`, GET `/items/{id}`, `/items/search?token=X`, `/items/sample` |

**Startbefehle:**
- Gradle: `./gradlew bootRun` (im `complete/`-Verzeichnis)
- Maven: `./mvnw spring-boot:run` (im `complete/`-Verzeichnis)
- Port: **8081** (konfiguriert in `application.properties`)

---

## 2.3 Technologie-Stack

**Sprache & Framework:**
- **Java 17** (Source-Kompatibilität)
- **Spring Boot 3.1.4**
- **Spring MVC** für Web-Controller
- **Thymeleaf** für serverseitige HTML-Templates
- **Jakarta EE 10+** (moderne Servlet-API)

**Build-Tools:**
- Maven (mit mvnw-Wrapper) — primäres Build-Tool
- Gradle (mit gradlew-Wrapper) — alternatives Build-Tool
- Jenkins CI/CD (Jenkinsfile vorhanden)

**Abhängigkeiten (aus pom.xml):**

| Abhängigkeit | Zweck |
|---|---|
| `spring-boot-starter-web` | Spring MVC, eingebetteter Tomcat, REST-Controller, Jackson für JSON |
| `spring-boot-starter-thymeleaf` | Serverseitige Template-Engine für HTML-Views |
| `spring-boot-starter-actuator` | Produktions-Monitoring: `/actuator/health`, Metriken |
| `spring-boot-devtools` (runtime) | Hot-Reload und erweiterte Entwickler-Features |
| `spring-boot-starter-test` (test) | JUnit 5, MockMvc, TestRestTemplate, AssertJ, Hamcrest |

**Implizite Abhängigkeiten (via Spring Boot Starters):**
- Tomcat 10.1.x (eingebetteter Servlet-Container)
- Jackson (JSON-Serialisierung)
- Logback / SLF4J (Logging)

---

## 2.4 Architektur

### Verzeichnisstruktur

```
complete/src/main/java/
├── com/example/springboot/          → Spring-Boot-Controller & Services
│   ├── Application.java             → Main-Klasse
│   ├── HelloController.java         → Basis-REST-Controller
│   ├── AboutController.java         → Template-Controller
│   ├── ItemController.java          → GUI-CRUD-Controller
│   ├── ItemRestController.java      → REST-API-Controller
│   └── AppStore.java                → Application-Scoped Service (Datenhaltung)
│
└── inginf/                          → Domain-Modell
    ├── Item.java                    → Produktentität mit Gewichtslogik
    ├── ItemInstance.java            → Kompositions-Instanz (Item-Verwendung)
    ├── Characteristic.java          → Produkteigenschaft (Name-Wert-Paar)
    └── SampleStore.java             → Initialisierung der Beispieldaten

complete/src/main/resources/
├── templates/                       → Thymeleaf HTML-Templates (6 Dateien)
└── application.properties           → Server-Konfiguration (Port 8081)

complete/src/test/java/com/example/springboot/
├── HelloControllerTest.java         → Unit-Test mit MockMvc
└── HelloControllerIT.java           → Integrationstest mit echtem HTTP
```

### Architekturmuster: **MVC (Model-View-Controller)**

- **Model:** Domain-Klassen im `inginf`-Package (Item, ItemInstance, Characteristic)
- **View:** Thymeleaf-Templates in `src/main/resources/templates/`
- **Controller:** Klassen in `com.example.springboot`
- **Service/Repository:** `AppStore` als zentraler In-Memory-Datenspeicher (Platzhalter für Datenbank)

Es gibt **keine Datenbank** — alle Daten werden in einer `ArrayList` im `AppStore` gehalten und gehen beim Neustart verloren.

---

## 2.5 Stilanalyse

### Schwerwiegende Verstöße

**1. Feldnamen in PascalCase statt camelCase (Java-Konvention verletzt)**

In `complete/src/main/java/inginf/Item.java`:
- Zeile 5: `public int Id;` → sollte `id` sein
- Zeile 16: `String Nomenclature;` → sollte `nomenclature` sein
- Zeile 29: `String Description;` → sollte `description` sein
- Zeile 40: `String Material;` → sollte `material` sein
- Zeile 53: `int WeightedWeight = -1;` → sollte `weightedWeight` sein
- Zeile 64: `int CalculatedWeight = -1;` → sollte `calculatedWeight` sein
- Zeile 75: `int EstimatedWeight = -1;` → sollte `estimatedWeight` sein
- Zeile 105: `ArrayList<ItemInstance> Uses` → sollte `uses` sein

In `complete/src/main/java/inginf/ItemInstance.java`:
- Zeile 5: `String Name;` → sollte `name` sein
- Zeile 16: `double[] Position` → sollte `position` sein
- Zeile 22: `Item Represents` → sollte `represents` sein
- Zeile 30: `ArrayList<Characteristic> UsedBy` → sollte `usedBy` sein

In `complete/src/main/java/inginf/Characteristic.java`:
- Zeile 3: `String Name;` → sollte `name` sein

**2. Nicht-standardmäßiger Package-Name**
- Package `inginf` ist kryptisch und folgt nicht der Java-Konvention für umgekehrte Domain-Namen. Besser wäre z. B. `com.example.springboot.domain` oder `com.example.springboot.model`.

**3. Underscore-Prefix bei Feldern**
- `ItemController.java`, Zeile 22: `AppStore _AppStore;` — Underscore-Präfix ist in Java unüblich.

**4. Inkonsistente Groß-/Kleinschreibung bei Injections**
- `ItemRestController.java`, Zeile 34: `private ApplicationContext Context;` → sollte `context` sein

---

## 2.6 Pattern-Analyse

### Verwendete Patterns

| Pattern | Wo | Beschreibung |
|---|---|---|
| **Singleton (Spring-managed)** | `AppStore.java` (`@ApplicationScope`) | Einzige Instanz verwaltet alle Items |
| **MVC** | Controller + Templates + Domain | Saubere Trennung von Zuständigkeiten |
| **Composite** | `Item.java` (Zeile 105–115) | Items enthalten `ItemInstance`-Listen, die wiederum auf andere Items verweisen → hierarchische Produktstruktur |
| **Strategy (vereinfacht)** | `Item.java`, `getBestWeight()` (Zeile 89–103) | Wählt bestes verfügbares Gewicht: Weighted > Calculated > Estimated |
| **Repository (vereinfacht)** | `AppStore.java` | Zentraler Datenzugriff, allerdings ohne Interface-Abstraktion |

### Pattern-Verletzungen

**1. Manuelle Lazy-Initialization statt Dependency Injection**
- `ItemController.java`, Zeilen 22–27: Manuelle Bean-Abfrage über `context.getBean(AppStore.class)` statt `@Autowired`
- **Vermutliche Ursache:** Möglicherweise entstanden, als die `@ApplicationScope`-Bean Initialisierungsprobleme verursachte; oder fehlendes Wissen über Spring DI.
- **Besser:** `@Autowired private AppStore appStore;`

**2. Mutable Collection Exposure**
- `AppStore.java`, Zeile 27: Gibt die interne `ArrayList` direkt zurück
- **Problem:** Externer Code kann die interne Liste modifizieren
- **Besser:** `Collections.unmodifiableList(itemStore)` zurückgeben
- **Vermutliche Ursache:** Schnelle Implementierung ohne Sicherheitsüberlegungen

**3. Fehlende Test-Aktualisierung**
- `HelloControllerTest.java` und `HelloControllerIT.java` erwarten `"Greetings from Spring Boot!"`, der Controller gibt aber `"Greetings from Item Manager in Spring Boot!"` zurück
- **Vermutliche Ursache:** HelloController wurde erweitert, Tests wurden vergessen

**4. Lineare Suche ohne Optimierung**
- `ItemRestController.java`, Zeilen 87–97: Suche iteriert über alle Items ohne Indexierung
- **Vermutliche Ursache:** Akzeptabel für den aktuellen Umfang, skaliert aber nicht
