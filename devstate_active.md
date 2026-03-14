# Projektstatus: Aktiv

**Entscheidungsdatum:** 2026-03-14

## Zusammenfassung der Analyse

Spring-Boot-Beispielanwendung zur Verwaltung hierarchischer Produktstrukturen (Items mit Characteristics und Komposition). Bietet sowohl eine HTML-GUI (Thymeleaf) als auch eine REST-API. Datenhaltung erfolgt In-Memory (ArrayList), keine Datenbank.

**Stärken:**
- Saubere MVC-Architektur mit klarer Trennung von Controllern, Domain-Modell und Views
- Duale API (GUI + REST)
- Spring Boot 3.1.4 mit Java 17

**Identifizierte Probleme:**
- Beide vorhandenen Tests schlagen fehl (Assertion-Mismatch nach Controller-Änderung)
- Feldnamen im Domain-Modell verletzen Java-Konventionen (PascalCase statt camelCase)
- Kryptischer Package-Name `inginf` statt konventionsgemäßem Namen
- Manuelle Lazy-Initialization statt Spring DI in ItemController
- Mutable Collections werden direkt exponiert
- Minimale Testabdeckung (nur HelloController)
- Java 17 JDK auf Entwicklungssystem nicht vorhanden

## Aktuell geplante nächste Schritte

- [ ] Java 17 JDK installieren, damit Tests ausgeführt werden können
- [ ] Fehlgeschlagene Tests reparieren (Assertion an geänderten Controller-Output anpassen)
- [ ] Feldnamen im Domain-Modell auf camelCase umstellen (Item, ItemInstance, Characteristic)
- [ ] Package `inginf` umbenennen (z. B. `com.example.springboot.domain`)
- [ ] ItemController: Manuelle Bean-Abfrage durch `@Autowired` ersetzen
- [ ] AppStore: Mutable Collection nicht direkt zurückgeben
- [ ] Testabdeckung erweitern (ItemRestController, ItemController, Domain-Modell)
- [ ] README.md aktualisieren, damit sie die tatsächliche Funktionalität beschreibt
