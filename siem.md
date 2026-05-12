---
marp: true
theme: custom
paginate: true
title: Security Information & Event Management (SIEM)
footer: ![w:280](img/dhbw-ka.svg)
transition: slide

---
<!-- _class: chapter -->

# Security Information & Event Management (SIEM)


## Einführung in moderne Sicherheitsüberwachung

---

# Was ist ein SIEM?

- Zentrale Plattform zur **Sammlung, Korrelation und Analyse** sicherheitsrelevanter Ereignisse.
- Kombiniert zwei Konzepte:
  - **SIM (Security Information Management)** – Langzeit-Analyse, Reporting, Compliance.
  - **SEM (Security Event Management)** – Echtzeit-Überwachung, Alarmierung.
- Ziel: **Erkennen, Bewerten und Reagieren** auf sicherheitsrelevante Vorfälle.

---

# Warum braucht man ein SIEM?

- Moderne IT-Landschaften erzeugen enorme Mengen an Logdaten.
- Angriffe sind oft **versteckt**, verteilt und schwer manuell zu erkennen.
- SIEM schafft:
  - Transparenz über Systeme, Netzwerke, Anwendungen.
  - Zentrale Sicht auf sicherheitsrelevante Ereignisse.
  - Grundlage für Incident Response und Forensik.

---

# Einsatzgebiete

- Unternehmensnetzwerke (KMU bis Großkonzerne)
- Kritische Infrastrukturen (KRITIS)
- Cloud-Umgebungen & hybride Architekturen
- SOC (Security Operations Center)
- Behörden & regulierte Branchen (Finanz, Gesundheit, Energie)

---

# Kernfunktionen eines SIEM

- **Log-Management**  
  Sammeln, Normalisieren, Speichern von Logs aus vielen Quellen.

- **Echtzeit-Korrelation**  
  Zusammenführen von Ereignissen zu sicherheitsrelevanten Mustern.

- **Alarmierung & Dashboards**  
  Visualisierung, Priorisierung, Eskalation.

---

# Kernfunktionen eines SIEM

- **Threat Intelligence Integration**  
  Abgleich mit IOC-Feeds (IPs, Domains, Hashes).

- **Compliance & Reporting**  
  DSGVO, ISO 27001, BSI, PCI-DSS, etc.

- **Forensik & Incident Response Unterstützung**  
  Rekonstruktion von Angriffspfaden.

---

# Typische Datenquellen

- Firewalls, IDS/IPS
- Betriebssysteme (Windows Event Logs, Syslog)
- Anwendungen & Datenbanken
- Cloud-Dienste (AWS CloudTrail, Azure Logs)
- Endpoint Security (EDR)
- Netzwerkgeräte (Router, Switches)
- Identity-Systeme (AD, IAM)

---

# Wie funktioniert ein SIEM?

1. **Datenaufnahme**  
   Logs werden gesammelt, normalisiert und gespeichert.

2. **Analyse & Korrelation**  
   Regeln, Machine Learning, statistische Modelle.

3. **Erkennung**  
   Alerts bei verdächtigen Mustern oder Anomalien.

4. **Reaktion**  
   Manuell oder automatisiert (SOAR).

5. **Reporting & Audit**  
   Compliance, Trendanalysen, KPI-Messung.

---

# Beispiel: Angriffserkennung durch Korrelation

- Mehrere fehlgeschlagene Logins → Bruteforce-Verdacht  
- Erfolgreicher Login von ungewöhnlicher IP → Anomalie  
- Privilegienerhöhung → Kritisch  
- Datenexfiltration → Incident

SIEM erkennt die **Kette**, nicht nur Einzelereignisse.

---

# Erweiterung: SIEM + SOAR

- **SOAR (Security Orchestration, Automation & Response)**  
  Ergänzt SIEM um:
  - Automatisierte Reaktionen (z. B. IP blockieren)
  - Playbooks für Incident Response
  - Ticketing-Integration

→ Entlastet SOC-Teams und beschleunigt Reaktionen.

---

# Vorteile eines SIEM

- Zentrale Sicht auf Sicherheitslage
- Schnellere Erkennung von Angriffen
- Unterstützung bei Forensik & Incident Response
- Erfüllung regulatorischer Anforderungen
- Skalierbarkeit für große Datenmengen

---

# Herausforderungen & Schwächen

- **Komplexität**  
  Einrichtung, Pflege, Regelwerk erfordern Expertise.

- **False Positives**  
  Hohe Alarmflut ohne gutes Tuning.

- **Kosten**  
  Lizenzen, Infrastruktur, Personal.

---

# Herausforderungen & Schwächen

- **Datenqualität**  
  „Garbage in, garbage out“ – schlechte Logs → schlechte Erkennung.

- **Skalierungsprobleme**  
  Große Datenmengen → Performance & Storage.

- **Angriffsfläche**  
  SIEM selbst wird zum attraktiven Ziel.

---

# Typische Probleme in der Praxis

- Unvollständige Logquellen
- Fehlende Use Cases / Korrelationen
- Keine kontinuierliche Pflege
- SOC-Überlastung („Alert Fatigue“)
- Mangelnde Integration mit anderen Security-Tools

---

# Moderne Trends im SIEM-Bereich

- Cloud-native SIEM (z. B. Azure Sentinel)
- ML-basierte Anomalieerkennung
- User & Entity Behavior Analytics (UEBA)
- Zero Trust Monitoring
- Integration mit XDR-Plattformen
- Automatisierung durch SOAR

---

# Zusammenfassung

- SIEM ist ein zentrales Werkzeug für moderne IT-Sicherheit.
- Es sammelt, korreliert und analysiert sicherheitsrelevante Daten.
- Einsatz in Unternehmen, SOCs und kritischen Infrastrukturen.
- Herausforderungen: Kosten, Komplexität, False Positives.
- Zukunft: Automatisierung, ML, Cloud-native Architekturen.

---

