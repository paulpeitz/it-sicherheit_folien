---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
title: Schwachstellen- und Patchmanagement
---

# Schwachstellenmanagement
## Der Lebenszyklus einer Schwachstelle

---

# Schwachstellenmanagement – Überblick

**Prozessschritte:**
1. Asset-Identifikation  
2. Schwachstellenidentifikation  
3. Bewertung & Priorisierung  
4. Risikobewertung  
5. Behandlung  
6. Verifikation  
7. Dokumentation & Reporting

---
<!-- class: biglist -->
# 1. Asset-Identifikation

## Fragen:
- Welche Systeme existieren?
- Wo befinden sie sich (On-Prem, Cloud, Mobile, OT/IoT)?
- Wer ist technisch und fachlich verantwortlich?

---

# 1. Asset-Identifikation

## Wichtige Aspekte:
- Automatisierte, kontinuierliche Erfassung  
- Klassifizierung (kritisch, sensibel, öffentlich)  
- Lifecycle-Status (EOL-Systeme = extreme Risikoquelle)

> Ohne vollständiges Inventar gibt es kein wirksames Schwachstellenmanagement.

---

# 2. Schwachstellen identifizieren

**Externe Informationsquellen:**
- CERT-Meldungen (BSI, CERT-Bund, US-CERT)
- Hersteller-Advisories (Microsoft, Cisco, VMware)
- Security-Feeds (CISA KEV-Katalog, Exploit-DB)

**Wichtig:**
- Erkennen auch von Fehlkonfigurationen (Misconfigurations)
- API- und Cloud-spezifische Schwachstellen berücksichtigen

---

# 3. Bewertung & Priorisierung

**Technische Bewertung:**
- CVSS Score (Base, Temporal, Environmental)
- Exploit-Verfügbarkeit (PoC, Metasploit-Modul?)
- Angriffskomplexität & Angreifermodell
  
---

# 3. Bewertung & Priorisierung

**Kontextfaktoren:**
- Ist das System internet-facing?
- Enthält es sensible Daten?
- Ist es kritisch für das Business (z. B. Produktionssysteme)?

---

# 3. Bewertung & Priorisierung

**Priorisierung nach Risiko-Kategorien:**
- **Kritisch** (sofort patchen, oft 24–72h)
- **Hoch**
- **Mittel**
- **Niedrig**

---

# 4. Risikobewertung

**Auswirkungen:**
- Betriebsunterbrechung
- Datenverlust
- Reputationsschäden
- Vertrags- oder Compliance-Verstöße

**Einbettung in Unternehmens-Risikomanagement:**
- Mapping auf ISO 27005 / BSI 200-3
- Abgleich mit Business Impact Analysen

---

# 5. Behandlung von Schwachstellen

- **Patchen** (bevorzugte Option)
- **Konfigurationsänderungen** (z. B. Abschalten unsicherer Protokolle)
- **Workarounds** (temporäre Maßnahmen)
- **Kompensierende Maßnahmen**, z. B.:  
  - Firewall-Regeln  
  - WAF-Schutz  
  - Netzsegmentierung  
  - Härtung bzw. Hardening

---

# 5. Behandlung von Schwachstellen

**Patch-Strategie:**
- Regelmäßige Patch-Zyklen (monatlich/vierteljährlich)
- Out-of-band Patching bei kritischen CVEs
- Staging: Test ➝ Pilot ➝ Rollout

---

# 6. Verifikation

**Nach erfolgreichem Patchen:**
- Erneuter Schwachstellenscan
- Funktionstests des Systems
- Log-Analyse zur Überprüfung, ob Patch aktiv ist
- Überprüfung von Abhängigkeiten (Libraries, Services)

**Ziel:**
Sicherstellen, dass die Schwachstelle tatsächlich geschlossen ist und keine neuen Probleme entstanden sind.

---

# 7. Dokumentation & Reporting

**Inhalte:**
- Alle gefundenen Schwachstellen
- Datum der Entdeckung & Schweregrad
- Zuständigkeiten & Deadlines
- Durchgeführte Maßnahmen
- Nachweise (Scans, Logs)

---

# 7. Dokumentation & Reporting

**Verwendung der Dokumentation:**
- Audits (ISO 27001, SOC 2)
- Management-Berichte (KPIs, Trends)
- Lessons Learned zur Prozessverbesserung

**Typische KPIs:**
- Time to Detect (TTD)
- Time to Remediate (TTR)
- Anzahl offener CVEs nach kritischem Level

---



# Patchmanagement – Grundlagen
- Patch = Softwareänderung zur Fehlerbehebung  
- Arten:  
  - Sicherheitsupdate  
  - Bugfix  
  - Hotfix  
  - Service Pack / Rollup

---

# Patchquellen
- Betriebssystemhersteller  
- Softwareanbieter  
- Open-Source-Communities  
- Spezialsysteme (OT, Medizingeräte)

---

# Patchmanagement-Prozess
1. Patch-Informationen sammeln  
2. Relevanz prüfen  
3. Testen  
4. Rollout planen  
5. Rollout durchführen  
6. Überwachen & ggf. Rollback

---

# Testen von Patches
- Vermeidung von Ausfällen  
- Staging-/Testumgebungen  
- Funktionstests  
- Regressionstests

---

# Rollout-Strategien
- Manuell vs. automatisiert  
- Phasenweiser Rollout  
- Wartungsfenster  
- Rollback-Plan

---

# Notfallpatching
- Out-of-band Updates  
- Kritische Schwachstellen  
- Aktive Ausnutzung  
- Wenig Testzeit → hohes Risiko

---

# Herausforderungen im Patchmanagement
- Legacy-Systeme  
- Abhängigkeiten  
- 24/7-Systeme  
- Unklare Zuständigkeiten  
- „Never touch a running system“

---

# Tools – Patchmanagement
- WSUS, SCCM/MECM  
- Landscape, Spacewalk  
- Ansible, Puppet, Chef  
- MDM: Intune, Jamf

---

# Threat Intelligence
- CERT-Meldungen  
- Hersteller-Advisories  
- RSS-Feeds  
- Kommerzielle Dienste

---

# Warum patchen Unternehmen nicht sofort?
- Angst vor Ausfällen  
- Komplexe Abhängigkeiten  
- Ressourcenmangel  
- Fehlende Testumgebungen

---

# Schwer patchbare Systeme
- Medizingeräte  
- Industrieanlagen (OT/ICS)  
- Alte Spezialsoftware  
- Kompensierende Maßnahmen notwendig

---

# Zukunft: Automatisierung & Self-Healing
- Automatisierte Remediation  
- Policy-basierte Updates  
- Self-Healing-Systeme  
- Chancen & Risiken

---

<!-- _class: chapter -->

# Digitale Forensik & Incident Response
## Wie reagieren, wenn ein IT‑System angegriffen wurde?

<!-- _speaker:
Kurze Einführung: Ziel der Vorlesung ist es, den Studierenden einen praxisnahen Überblick über digitale Forensik und Incident Response zu geben. Ich erkläre, warum diese Themen heute so wichtig sind und wie sie in realen Unternehmen angewendet werden.
-->

---

# Agenda

- Grundlagen digitaler Forensik verstehen  
- Sicherheitsvorfälle erkennen und einordnen  
- Incident‑Response‑Prozesse kennen  
- Forensische Methoden & Tools anwenden  
- Typische Fehler vermeiden

<!-- _speaker:
Ich gehe die Lernziele durch und erkläre, dass die Studierenden am Ende wissen sollen, wie man strukturiert auf Angriffe reagiert. Wichtig ist, dass sie sowohl technische als auch organisatorische Aspekte verstehen.
-->

---

# Was ist digitale Forensik?

Digitale Forensik ist die **systematische Untersuchung digitaler Systeme**, um Beweise zu sichern und Vorfälle aufzuklären.

- Beweise sichern  
- Ursachen identifizieren  
- Verantwortliche ermitteln  
- Wiederanlauf unterstützen

<!-- _speaker:
Ich betone, dass Forensik nicht nur „Hacker finden“ bedeutet, sondern auch Compliance, Wiederherstellung und Prävention umfasst. Wichtig ist die wissenschaftliche, nachvollziehbare Vorgehensweise.
-->

---

# Typische Einsatzszenarien

- Malware‑Infektionen  
- Ransomware  
- Datenexfiltration  
- Insider‑Threats  
- Manipulation von Logdaten  
- Industriespionage

<!-- _speaker:
Ich gebe zu jedem Punkt ein kurzes Beispiel aus der Praxis. Besonders Ransomware und Datenexfiltration sind heute häufige Fälle.
-->

---

# Grundprinzipien der Forensik

- **Integrität**  
- **Reproduzierbarkeit**  
- **Dokumentation**  
- **Chain of Custody**  
- **Minimale Eingriffe**

<!-- _speaker:
Ich erkläre die Bedeutung der Chain of Custody und warum jede Veränderung am System problematisch ist. Beispiele: Hashwerte, Write‑Blocker, forensische Images.
-->

---

# Incident Response: Überblick

Phasen nach NIST SP 800‑61:

1. Preparation  
2. Detection & Analysis  
3. Containment  
4. Eradication  
5. Recovery  
6. Post‑Incident Activity

<!-- _speaker:
Ich stelle das NIST‑Modell vor und betone, dass es weltweit als Standard gilt. Wichtig: IR ist ein Prozess, kein Ad‑hoc‑Feuerlöschen.
-->

---

# Phase 1: Preparation

- IR‑Team definieren  
- Rollen & Verantwortlichkeiten  
- Kommunikationswege  
- Forensik‑Werkzeuge bereitstellen  
- Logging & Monitoring  
- Playbooks  
- Übungen

<!-- _speaker:
Ich erkläre, dass gute Vorbereitung entscheidend ist. Ohne Logging und klare Rollen ist Incident Response kaum möglich.
-->

---

# Phase 2: Detection & Analysis

- Vorfall erkennen  
- Schweregrad bestimmen  
- Umfang einschätzen

Quellen:

- SIEM  
- IDS/IPS  
- Endpoint‑Monitoring  
- Logs  
- Nutzerhinweise  
- Threat‑Intelligence

<!-- _speaker:
Ich gehe auf typische Erkennungswege ein und erkläre, warum Nutzerhinweise oft unterschätzt werden. Beispiel: „Mein Rechner ist plötzlich sehr langsam.“
-->

---

# Indikatoren eines Angriffs (IoCs)

- Unbekannte Prozesse  
- Auffällige Netzwerkverbindungen  
- Neue Benutzerkonten  
- Geänderte Systemdateien  
- Log‑Lücken  
- Hohe CPU‑Last  
- Verdächtige PowerShell‑Befehle

<!-- _speaker:
Ich zeige Beispiele aus realen Fällen, z. B. ungewöhnliche PowerShell‑Ketten oder C2‑Verbindungen. Wichtig: IoCs sind Hinweise, keine Beweise.
-->

---

# Phase 3: Containment

- Schaden begrenzen  
- Beweise erhalten

Strategien:

- Netzwerksegmentierung  
- Account‑Sperrungen  
- Blockieren von IPs  
- Imaging vor Eingriffen  
- Systeme isolieren

<!-- _speaker:
Ich erkläre, warum man Systeme nicht sofort ausschalten sollte. RAM‑Daten wären sonst verloren. Isolation ist meist besser als Abschalten.
-->

---

# Phase 4: Eradication

- Malware entfernen  
- Schwachstellen schließen  
- Persistenz entfernen  
- Root‑Cause‑Analyse

<!-- _speaker:
Ich betone, dass Eradication erst nach Containment kommt. Beispiel: Entfernen von Scheduled Tasks, Registry‑Run‑Keys, Backdoors.
-->

---

# Phase 5: Recovery

- Systeme wiederherstellen  
- Monitoring intensivieren  
- Validierung  
- Schrittweises Wiederverbinden

<!-- _speaker:
Ich erkläre, dass Recovery oft unterschätzt wird. Wichtig: Systeme erst wieder freigeben, wenn klar ist, dass keine Persistenz mehr existiert.
-->

---

# Phase 6: Post‑Incident Activity

- Lessons Learned  
- Playbooks aktualisieren  
- Schwachstellenanalyse  
- Bericht an Management  
- Meldung an Behörden

<!-- _speaker:
Ich betone die Bedeutung der Nachbereitung. Viele Organisationen überspringen diesen Schritt – und machen später dieselben Fehler erneut.
-->

---

# Forensische Datensicherung

- Bit‑genaue Images  
- RAM‑Dump  
- Log‑Export  
- Netzwerkverkehr  
- Mobile Forensics

<!-- _speaker:
Ich erkläre die Unterschiede zwischen RAM‑Dump und Disk‑Image und warum RAM oft die wichtigsten Hinweise enthält (z. B. Keys, Prozesse, C2‑Adressen).
-->

---

# Forensische Artefakte

- Logfiles  
- Browser‑Historien  
- Prefetch  
- Registry  
- $MFT, $UsnJrnl  
- Shell‑History  
- Netzwerk‑Artefakte

<!-- _speaker:
Ich gebe Beispiele, wie Prefetch‑Dateien oder $MFT‑Einträge Hinweise auf ausgeführte Programme liefern. Wichtig: Artefakte kombinieren.
-->

---

# Live vs. Dead Forensics

**Live Forensics**  
+ RAM‑Daten  
– Veränderungen am System

**Dead Forensics**  
+ Hohe Integrität  
– Keine flüchtigen Daten

<!-- _speaker:
Ich erkläre, wann Live Forensics sinnvoll ist (z. B. bei aktiver Malware) und wann Dead Forensics bevorzugt wird (z. B. juristische Fälle).
-->

---

# Typische Tools

- Volatility  
- Autopsy / Sleuth Kit  
- FTK / EnCase  
- Wireshark / Zeek  
- Sysinternals  
- ELK / Splunk

<!-- _speaker:
Ich gebe zu jedem Tool ein kurzes Beispiel. Volatility für RAM, Autopsy für Dateisysteme, Sysinternals für Windows‑Analyse.
-->

---

# Beispiel: Vorgehen bei Ransomware

1. System isolieren  
2. RAM‑Dump  
3. Disk‑Image  
4. IoCs identifizieren  
5. Verschlüsselungsprozess analysieren  
6. Persistenz entfernen  
7. Backups prüfen  
8. Wiederherstellung  
9. Lessons Learned

<!-- _speaker:
Ich erkläre, warum RAM‑Dumps bei Ransomware besonders wertvoll sind (z. B. Keys im Speicher). Außerdem: Backups nicht blind vertrauen.
-->

---

# Häufige Fehler

- System sofort ausschalten  
- Beweise überschreiben  
- Keine Dokumentation  
- Unkoordinierte Kommunikation  
- Zu frühe Wiederinbetriebnahme  
- Unklare Rollen

<!-- _speaker:
Ich erzähle typische Fehler aus der Praxis. Besonders häufig: „Wir haben den Server neu gestartet, weil er langsam war.“
-->

---

# Kommunikation im Incident

- Interne Kanäle  
- Externe Kommunikation  
- Keine sensiblen Infos über E‑Mail/Chat  
- Krisenkommunikation  
- Juristische Aspekte

<!-- _speaker:
Ich betone, dass Kommunikation oft kritischer ist als Technik. Beispiel: Abstimmung mit PR, Legal, Management.
-->

---

# Rechtliche Aspekte

- DSGVO‑Meldepflicht  
- Beweisverwertbarkeit  
- Arbeitsrecht  
- Zusammenarbeit mit Behörden  
- Dokumentationspflichten

<!-- _speaker:
Ich erkläre, wann eine Meldung nach DSGVO Art. 33 notwendig ist und warum Dokumentation auch juristisch relevant ist.
-->

---

# Zusammenfassung

- Forensik klärt Vorfälle auf  
- Incident Response folgt klaren Phasen  
- Beweissicherung ist zentral  
- Vorbereitung entscheidet  
- Dokumentation & Kommunikation sind kritisch

---


