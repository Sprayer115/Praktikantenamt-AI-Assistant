# Praktikantenamt AI-Assistant - Projektanforderungen

## Projektübersicht

Entwicklung eines KI-gestützten E-Mail-Assistenten zur Automatisierung und Optimierung der E-Mail-Verwaltung für das Praktikantenamt einer Hochschule für angewandte Wissenschaften.

### Projektziel

Aufbau eines automatisierten Workflows mit KI-Unterstützung zur intelligenten Bearbeitung eingehender E-Mails, automatischen Vertragsprüfung und Verwaltung von Praktikantendaten.

### Technologie-Stack

- **Workflow-Orchestrierung**: n8n
- **E-Mail-Trigger**: n8n Email-Integration
- **KI**: Prompt Engineering, Context Engineering, Agent-basierte Systeme
- **Datenverwaltung**: Simpel -> MCP Tools für File Storage / DB
- **Benachrichtigungen**: Mattermost (optional)
- **OCR**: Für Vertragsdatenextraktion

---

## Use Cases

### Use Case 1: E-Mail-Kategorisierung und Weiterleitung

**Priorität**: Hoch (Hauptanwendungsfall)

**Beschreibung**:
Automatische Kategorisierung eingehender E-Mails und Weiterleitung an die zuständigen Stellen.

**Kategorien** (zu definieren):
- Vertragsabgaben → Weiterleitung an BFF (Frau Friedrich)
- Auslandsamtsfragen → Weiterleitung an Mevius
- Praktikumsverschiebungen → Weiterleitung an Prüfungsamt
- Weitere Kategorien nach Bedarf

**Funktionale Anforderungen**:
- KI-gestützte Klassifizierung eingehender E-Mails
- Automatische Weiterleitung basierend auf Kategorie
- Erkennung von E-Mail-Inhalten und Anhängen
- Validierung der Kategorisierung

**Workflow**:
1. E-Mail trifft im Postfach ein
2. n8n triggert Workflow
3. KI analysiert E-Mail-Inhalt
4. Kategorie wird zugewiesen (in das Postfach verschieben)
5. E-Mail wird weitergeleitet

---

### Use Case 2: Automatische Vertragsprüfung und -analyse

**Priorität**: Mittel

**Beschreibung**:
Automatische Validierung und Datenextraktion aus eingereichten Praktikumsverträgen.

**Funktionale Anforderungen**:

**Vertragserkennung**:
- Erkennung von E-Mails mit Vertragsanhängen
- Unterstützte Formate: PDF, gescannte Dokumente

**Datenextraktion** (via OCR):
- Name des Studierenden
- Matrikelnummer
- Firmenname
- Praktikumsdauer / Arbeitstage
- Praktikumszeitraum

**Validierung**:
- Berechnung der Arbeitstage (mindestens 95 Tage erforderlich)
- Abgleich mit Firmenliste (Whitelist/Blacklist)
- Vollständigkeitsprüfung der Vertragsdaten

**Praktikumsprofil**:
- Automatische Erstellung eines Praktikumsprofils
- Speicherung relevanter Daten
- Verwaltung via MCP Tools

**Workflow**:
1. Vertrag wird per E-Mail eingereicht
2. Parallel: Weiterleitung an Frau Friedrich (BFF)
3. Email wird entsprechend kategorisiert
4. Async Processing dieses Postfachs
5. OCR-Verarbeitung des Vertrags
6. Datenextraktion
7. Validierung der Daten
8. Erstellung Praktikumsprofil
9. Optional: Mattermost-Benachrichtigung bei Problemen

---

### Use Case 3: Standardantworten und Antwortvorschläge

**Priorität**: Niedrig

**Beschreibung**:
KI-generierte Antwortvorschläge für häufige Anfragen mit Human-in-the-Loop-Ansatz.

**Funktionale Anforderungen**:
- Analyse der E-Mail-Anfrage
- Generierung kontextbezogener Antwortvorschläge
- Präsentation der Vorschläge an Sachbearbeiter
- Möglichkeit zur Anpassung vor dem Versand
- Lernen aus genehmigten Antworten

**Human-in-the-Loop**:
- Review-Prozess für vorgeschlagene Antworten
- Freigabe durch Sachbearbeiter (Oliver)
- Feedback-Mechanismus zur Verbesserung

---

### Use Case 4: Verwaltung und Administration

**Priorität**: Mittel

**Beschreibung**:
Zentrale Verwaltung aller Praktikantendaten und System-Administration.

**Funktionale Anforderungen**:
- Verwaltung aller Praktikanten via File Storage (MCP Tools)
- Dashboard zur Übersicht
- Suchfunktion für Praktikanten und Verträge
- Statistiken und Reports
- Systemkonfiguration

**Mögliche Schnittstellen**:
- Web-Interface (n8n)
- Mattermost-Integration
- Admin-Dashboard

---

## Technische Anforderungen

### KI/ML-Komponenten

- **Prompt Engineering**: Optimierung der KI-Prompts für präzise Kategorisierung
- **Context Engineering**: Bereitstellung relevanter Kontextinformationen über das Praktikantenamt
- ?**Agent-basierte Architektur**: Modulare Agents für verschiedene Aufgaben?
- **Datengenerierung**: Erstellung von Testdaten für Entwicklung und Validierung

### Infrastruktur

- **E-Mail-Setup**:
  - Dummy-Postfach für Weiterleitungen
  - n8n E-Mail-Trigger-Konfiguration

- **Datenbank/Storage**:
  - File Storage für Praktikantendaten
  - MCP Tools Integration
  - Firmenliste (Whitelist/Blacklist)?

- **Workflow-Engine**:
  - n8n-Installation und -Konfiguration
  - Workflow-Definitionen für alle Use Cases

### MCP (Model Context Protocol) Integration

**Mögliche Anwendungsfälle**:
- Verwaltung von Praktikantendaten im File Storage
- Zugriff auf strukturierte Firmenlisten
- Verwaltung von Templates und Standardantworten
- Historische Datenanalyse

---

## Offene Fragen und TODOs

### Infrastruktur
- [x] Repository anlegen ✅ 2025-12-04
- [ ] Dummy-Postfach einrichten
- [ ] n8n-Umgebung aufsetzen

### Daten
- [ ] Beispielverträge sammeln/erstellen
- [ ] Kategorien für E-Mails definieren

### Entwicklung
- [ ] Use Cases detailliert ausformulieren
- [ ] n8n Email-Trigger konfigurieren
- [ ] OCR-Lösung evaluieren und integrieren
- [ ] MCP Tools für File Storage implementieren

### Konzeption
- [ ] Human-in-the-Loop-Konzept ausarbeiten
- [ ] Praktikantenamt-Kontext dokumentieren

---

## Projektstruktur

```
praktikatenamt-ai-assistant/
├── docs/
│   ├── requirements.md (dieses Dokument)
│   ├── use-cases/
│   └── architecture/
├── n8n-workflows/
│   ├── email-categorization.json
│   ├── contract-validation.json
│   └── response-generation.json
├── ai-agents/
│   ├── categorization/
│   ├── contract-analysis/
│   └── response-generator/
├── mcp-tools/
│   └── file-storage/
└── data/
    ├── company-list/
    ├── templates/
    └── test-data/
```

---

## Nächste Schritte

1. **Phase 1 - Setup**: Repository und Infrastruktur einrichten
2. **Phase 2 - Kategorisierung**: Use Case 1 implementieren (höchste Priorität)
3. **Phase 3 - Vertragsprüfung**: Use Case 2 implementieren
4. **Phase 4 - Antwortgenerierung**: Use Case 3 implementieren
5. **Phase 5 - Administration**: Use Case 4 und Dashboard entwickeln

---
