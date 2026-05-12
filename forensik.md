---
marp: true
title: Digitale Forensik & Incident Response
description: Grundlagen der digitalen Forensik und Vorgehen bei IT-Sicherheitsvorfällen
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
transition: slide
---

<!-- _class: chapter -->

# Digitale Forensik & Incident Response

## Wie reagieren, wenn ein IT-System angegriffen wurde?

<!--
Einstieg: Thema der Veranstaltung ist die Reaktion auf IT-Sicherheitsvorfälle und die Rolle der digitalen Forensik.
„Digitale Forensik“ = systematische Untersuchung digitaler Systeme, um Spuren von Angriffen zu finden und zu bewerten.
„Incident Response“ (IR) = strukturierter Prozess, wie eine Organisation auf Sicherheitsvorfälle reagiert (erkennen, eindämmen, beseitigen, lernen).
Ziel der Vorlesung: Studierende sollen ein grundlegendes Verständnis der Abläufe, Rollen und technischen Begriffe bekommen, um im Praxisfall nicht planlos zu handeln.
Kurz erklären, dass wir sowohl organisatorische als auch technische Aspekte beleuchten.
-->

---

# Agenda

1. Motivation & Begriffe
2. Grundlagen digitaler Forensik
3. Incident Response – Phasen & Rollen
4. Erste Schritte beim Sicherheitsvorfall
5. Beweissicherung (Live & Post-Mortem)
6. Analyse typischer Artefakte
7. Kommunikation, Dokumentation & Recht
8. Best Practices & Checklisten

<!--
Überblick über die Struktur der Veranstaltung.
„Artefakte“ = alle Spuren, die ein System oder eine Anwendung hinterlässt (Logeinträge, Dateien, Registry-Einträge usw.).
Hinweis: Die Agenda bildet auch einen typischen Ablauf in der Praxis ab – von Motivation über Grundlagen hin zur konkreten Checkliste.
Erwähne, dass wir einige Abkürzungen und Technologien entlang der Agenda erklären werden (z. B. NIST, RAM, NTP, SIEM).
-->

---

# Motivation

- IT-Sicherheitsvorfälle sind **wahrscheinlich**, nicht „ob“, sondern „wann“.

- Digitale Forensik und Incident Response helfen:
  - Schaden zu begrenzen
  - Ursache zu verstehen
  - Wiederanlauf abzusichern
  - Beweise für rechtliche Schritte zu sichern

<!--
„Threat Landscape“: Angriffe werden häufiger und professioneller (z. B. organisierte Cyberkriminalität).
Betriebsunterbrechung: Beispiele nennen (Produktionsanlagen stehen, E‑Commerce-Shop offline).
Rechtliche Konsequenzen: DSGVO = Datenschutz-Grundverordnung, regelt u. a. den Umgang mit personenbezogenen Daten in der EU.
Digitale Forensik liefert technische Fakten, z. B. welche Daten betroffen waren, wann der Angriff begann, welche Accounts genutzt wurden.
Incident Response sorgt dafür, dass diese Informationen in konkrete Maßnahmen umgesetzt werden (z. B. Systeme isolieren, Passwörter ändern, Behörden informieren).
-->

---

# Begriffe

- **Digitale Forensik**
  - Systematisches Sammeln, Sichern, Analysieren und Auswerten digitaler Spuren
- **Incident Response (IR)**
  - Geplantes, strukturiertes Vorgehen zur Behandlung von Sicherheitsvorfällen
- **Security Incident**
  - Ereignis, das Vertraulichkeit, Integrität oder Verfügbarkeit verletzt oder gefährdet
- **IoC – Indicator of Compromise**
  - Hinweise auf eine Kompromittierung (Hash, IP, Domain, Artefakte…)

<!--
„Vertraulichkeit, Integrität, Verfügbarkeit“ = die klassische CIA-Triade (Confidentiality, Integrity, Availability) der Informationssicherheit.
IoC-Beispiele:
- Hash: kryptografischer Fingerabdruck einer Datei (z. B. SHA‑256), kann eine bestimmte Malware eindeutig identifizieren.
- IP-Adresse: numerische Adresse eines Systems im Netzwerk (z. B. „203.0.113.10“).
- Domain: z. B. „evil-example.com“, die mit Malware-Kommandoservern verbunden ist.
Unterschied Forensik vs. IR:
- Forensik: „Was ist genau passiert?“
- IR: „Wie reagieren wir darauf?“
-->

---

# Ziele der digitalen Forensik

- **Was ist passiert?**
  - Angriffsweg, Art der Malware, Täterverhalten
- **Wann ist es passiert?**
  - Zeitlinien, erste Kompromittierung, Lateral Movement
- **Welche Systeme / Daten sind betroffen?**
  - Umfang des Vorfalls
- **Wer ist betroffen?**
  - Kunden, Mitarbeiter, Partner
- **Welche Beweise gibt es?**
  - Für interne Aufklärung und ggf. Strafverfolgung

<!--
„Malware“ = „malicious software“, also Schadsoftware (z. B. Trojaner, Ransomware, Spyware).
„Lateral Movement“ = Seitwärtsbewegung des Angreifers im Netzwerk, z. B. von einem kompromittierten Client auf Server.
Zeitlinien (Timelines) in der Forensik: Kombination verschiedener Zeitstempel aus Logs, Dateisystemen etc., um den Ablauf zu rekonstruieren.
Ziel: nicht nur technisch zu verstehen, sondern auch geschäftlich zu bewerten (z. B. welche Kundendaten kompromittiert wurden).
-->

---

# Prinzipien forensischer Arbeit

- **Reproduzierbarkeit**
  - Andere Sachverständige sollen zum gleichen Ergebnis kommen
- **Integrität der Beweise**
  - Keine Veränderung der Originaldaten
- **Dokumentation**
  - Vollständige Nachvollziehbarkeit aller Schritte
- **Verhältnismäßigkeit & Datenschutz**
  - Nur notwendige Daten, rechtliche Vorgaben einhalten
- **Chain of Custody**
  - Lückenlose Nachverfolgung, wer wann welchen Beweis in Händen hatte

<!--
„Chain of Custody“: oft Formular oder Protokoll, in dem jede Übergabe von Beweismitteln festgehalten wird (Name, Zeitpunkt, Zweck).
Integritätssicherung z. B. durch Hash-Werte (MD5, SHA‑1, SHA‑256; in der Praxis besser moderne Algorithmen wie SHA‑256).
Verhältnismäßigkeit: z. B. nicht den kompletten E‑Mail-Verkehr aller Mitarbeitenden auswerten, wenn nur ein bestimmter Account betroffen ist.
Datenschutz: besonders wichtig bei personenbezogenen Daten; Zusammenarbeit mit Datenschutzbeauftragten.
-->

---

# Incident Response Lebenszyklus (Beispiel NIST)

1. **Vorbereitung (Preparation)**
2. **Erkennung & Analyse (Detection & Analysis)**
3. **Eindämmung (Containment)**
4. **Beseitigung (Eradication)**
5. **Wiederherstellung (Recovery)**
6. **Lessons Learned**

Digitale Forensik begleitet vor allem:
- Erkennung & Analyse
- Eindämmung
- Lessons Learned

<!--
NIST = National Institute of Standards and Technology (USA); veröffentlicht Richtlinien, u. a. zu Incident Response.
„Containment“: Maßnahmen, die den Schaden begrenzen, z. B. Netzwerktrennung eines Systems.
„Eradication“: Entfernen der Ursache, z. B. Malware löschen, Schwachstelle schließen.
„Recovery“: Systeme wieder in einen sicheren Normalzustand bringen (Neuinstallation, Backup-Restore).
Lessons Learned: Wichtiger Schritt, in dem Prozesse und technische Maßnahmen verbessert werden.
-->

---

# Rollen & Verantwortlichkeiten

- **Incident Response Team (IRT) / CSIRT**
  - Technische Analyse, Maßnahmenplanung
- **IT-Betrieb**
  - Umsetzung technischer Maßnahmen
- **Informationssicherheitsbeauftragter (ISB)**
  - Koordination, Richtlinien, Reporting
- **Datenschutzbeauftragter**
  - Bewertung von Datenschutzvorfällen
- **Management / Krisenstab**
  - Entscheidungen, externe Kommunikation
- **Rechtsabteilung**
  - Rechtliche Bewertung, Meldepflichten, Strafanzeige

<!--
CSIRT = Computer Security Incident Response Team; spezialisierte Gruppe, die auf Sicherheitsvorfälle reagiert.
Unterschied IRT / IT-Betrieb:
- IRT analysiert und plant, IT-Betrieb setzt auf Systemebene um.
ISB (Informationssicherheitsbeauftragter): überwacht Einhaltung von Sicherheitsrichtlinien, koordiniert Maßnahmen bei Vorfällen.
Datenschutzbeauftragter: achtet insbesondere auf rechtlich korrekten Umgang mit personenbezogenen Daten (DSGVO).
Wichtig: klare Eskalationswege definieren, damit nicht unklar ist, wer wann entscheidet.
-->

---

# Wann liegt ein Sicherheitsvorfall vor?

Typische Anzeichen:

- Unerwartete Systemabstürze oder Neustarts
- Ungewöhnliche Logins (Ort, Zeit, Account)
- Auffällige Netzwerklast oder Verbindungen zu ungewöhnlichen Zielen
- Unbekannte Prozesse oder Dienste
- Unerklärliche Konfigurationsänderungen
- Antivirus- oder EDR-Alerts
- Meldungen von Nutzenden („Mein PC verhält sich komisch…“)

<!--
„EDR“ = Endpoint Detection and Response; Sicherheitslösung auf Rechnern (Endpoints), die verdächtige Aktivitäten erkennt und reagiert.
„Antivirus“: klassischer Virenscanner, heute oft Bestandteil größerer Endpoint-Security-Suiten.
Ungewöhnliche Logins: z. B. Login aus einem Land, in dem das Unternehmen nicht aktiv ist, oder zu sehr ungewöhnlicher Uhrzeit.
Prozesse: mit Tools wie „Task-Manager“ (Windows) oder „ps/top“ (Linux) sichtbar.
Betonen, dass Nutzerhinweise ernst genommen werden sollten, auch wenn sie unscheinbar wirken.
-->

---

# Erste Reaktion – Do & Don’t

**Do:**

- Ruhe bewahren
- Vorfall **melden** (IRT, IT, ISB)
- Alles **dokumentieren**
- Relevante Daten **nicht löschen**
- Screenshots / Fotos machen (z. B. Fehlermeldungen)

---

# Erste Reaktion – Do & Don’t

**Don’t:**

- System unüberlegt neu starten
- Spuren durch „Aufräumen“ zerstören
- Eigenmächtige Aktionen ohne Abstimmung
- Vorfall inoffiziell „vertuschen“

<!--
Neu starten: kann wichtige flüchtige Spuren im RAM (Random Access Memory) zerstören, z. B. laufende Malware oder Verschlüsselungsschlüssel.
„Aufräumen“: Nutzer klicken gerne auf „Löschen“, um das Problem zu „lösen“, zerstören damit aber Beweismittel.
Dokumentation: Zeitpunkt, was der Nutzer beobachtet hat, was schon unternommen wurde.
Kultur: Fehlerkultur fördern, in der Vorfälle gemeldet werden dürfen, ohne Angst vor Schuldzuweisungen.
-->

---

# Vorgehen beim Vorfall

1. **Erstbewertung**
   - Verdacht begründet? Schweregrad?
2. **Stabilisierung**
   - System weiterhin nutzbar? Produktionsrelevanz?
3. **Eindämmung**
   - Isolieren, aber Spuren schützen

---

# Vorgehen beim Vorfall

4. **Beweissicherung**
   - Logs, Images, Speicher, Konfigurationen
5. **Analyse**
   - Ursache, Umfang, Angriffsweg
6. **Maßnahmen**
   - Patches, Passwortrücksetzungen, Neuaufbau
7. **Abschluss & Lessons Learned**

<!--
„Image“: forensische 1:1-Kopie eines Datenträgers oder Speicherbereiches.
Schweregrad: Kriterien definieren, z. B. Anzahl betroffener Systeme, betroffene Datenarten, Ausfall kritischer Prozesse.
Patches: Softwareupdates, um bekannte Sicherheitslücken zu schließen.
Passwortrücksetzungen: besonders bei kompromittierten Konten und ggf. für Admin-Konten; idealerweise zusammen mit Multi-Faktor-Authentifizierung (MFA).
-->

---

# Vorbereitung ist entscheidend

- **Incident-Response-Plan**
  - Zuständigkeiten, Eskalationswege, Kontaktlisten
- **Forensik-Readiness**
  - Logging-Konzept, Zeit-Synchronisation (NTP), zentrale Log-Sammlung
  - Standardisierte Imaging- und Backup-Prozesse
- **Schulung & Übungen**
  - Tabletop-Exercises, technische Übungen (z. B. CTF)
- **Werkzeuge bereit halten**
  - Forensik-Tools, Boot-Medien, Dokumentationsvorlagen

<!--
„NTP“ = Network Time Protocol; sorgt dafür, dass Systemuhren synchron sind – wichtig für konsistente Zeitstempel in Logs.
Zentrale Log-Sammlung oft über ein SIEM-System (Security Information and Event Management); sammelt Logs und korreliert Ereignisse.
„Tabletop-Exercise“: Planspiel, bei dem ein Vorfall auf dem Papier durchgespielt wird.
„CTF“ = Capture The Flag; Sicherheitswettbewerb oder Übung, bei dem Angriffe bzw. Analysen simuliert werden.
Forensik-Tools: Beispiele nennen (z. B. Autopsy, FTK Imager, Volatility – je nach gewünschter Tiefe).
-->

---

# Beweissicherung – Grundprinzipien

- **„From Volatile to Non-Volatile“**
  - Erst flüchtige Daten (RAM, Netzwerkverbindungen), dann langfristige (Disk, Backups)
- **Original vs. Kopie**
  - Möglichst Arbeit nur an Kopien (Images), Original unangetastet lassen
- **Hash-Werte**
  - Integritätsnachweis (z. B. SHA-256) für Images / Dateien
- **Zeitliche Konsistenz**
  - Zeitzonen berücksichtigen, NTP-Status prüfen, Zeitstempel protokollieren

<!--
„Volatile“ = flüchtig, geht z. B. beim Ausschalten verloren (RAM, aktive Verbindungen).
„Non-Volatile“ = persistent, bleibt auch nach Ausschalten (Festplatte, SSD, Backups).
Hash-Verfahren:
- SHA-256 ist heute üblich; MD5 und SHA-1 gelten als kryptografisch gebrochen, werden aber noch teils zur Identifikation verwendet.
Zeitliche Konsistenz: z. B. beachten, dass Log-Server und betroffene Systeme dieselbe Zeitzone und NTP-Quelle nutzen.
-->

---

# Live-Forensik vs. Post-Mortem

**Live-Forensik:**

- Untersuchung eines laufenden Systems
- Erhebung von:
  - RAM-Inhalt
  - Laufenden Prozessen
  - Offenen Netzwerkverbindungen
- Vorteil: Sicht auf aktive Malware, verschlüsselte Inhalte
- Risiko: Veränderung des Systems, Vorsicht bei Tools

---

# Live-Forensik vs. Post-Mortem

**Post-Mortem-Forensik:**

- Analyse von Datenträger-Images / Backups
- System ausgeschaltet (oder Image im Betrieb erstellt)
- Fokus auf:
  - Dateisystem, Logs, Registry, Artefakte

<!--
Live-Forensik häufig mit speziellen Tools, die RAM und Prozesse auslesen (z. B. Volatility für RAM-Analyse).
Risiko: Jedes gestartete Tool ändert das System (zusätzliche Prozesse, Dateien).
Post-Mortem-Analyse: typischer Ablauf, wenn ein System bereits heruntergefahren oder isoliert ist.
Registry: zentrale Konfigurationsdatenbank unter Windows, enthält viele Spuren (Autostarts, installierte Software usw.).
-->

---

# Typische Artefakte – Betriebssystem

- **Windows**
  - Event Logs
  - Registry (Run-Keys, SERVICES, SAM)
  - Prefetch-Dateien
  - $MFT, $LogFile, $USN Journal
  - Browser-Verläufe, Cookies, Downloads

---

# Typische Artefakte – Betriebssystem

- **Linux / Unix**
  - /var/log/* (auth.log, syslog, secure, …)
  - /etc/passwd, /etc/shadow, sudoers
  - Bash-History
  - crontab / systemd Timer
  - SSH-Keys, known_hosts

<!--
Event Logs (Windows): System-, Sicherheits- und Anwendungs-Logs; Analyse z. B. mit „Event Viewer“ oder PowerShell.
„Run-Keys“: Registry-Schlüssel, über die Programme beim Systemstart oder Login automatisch ausgeführt werden.
$MFT (Master File Table) im NTFS-Dateisystem: enthält Metadaten zu Dateien, auch gelöschte Einträge.
Bash-History: Befehlsverlauf des Nutzers in der Shell; kann Hinweise auf Angreiferaktivitäten geben.
 /etc/passwd, /etc/shadow: Benutzerdaten und Passwort-Hashes; sehr sensibel.
SSH (Secure Shell): Protokoll für verschlüsselte Fernzugriffe; SSH-Keys ermöglichen passwortlose Logins.
-->

---

# Typische Artefakte – Netzwerk & Anwendungen

- **Netzwerk**
  - Firewall-Logs
  - Proxy-Logs
  - NetFlow / sFlow-Daten
  - VPN-Logs

---

# Typische Artefakte – Netzwerk & Anwendungen

- **E-Mail**
  - Mail-Header (Received-Chain)
  - Versand-/Empfangszeitpunkte
  - Spam-/Phishing-Indikatoren
- **Webanwendungen**
  - Webserver-Logs (Access / Error)
  - WAF-Logs
  - Applikationslogs

<!--
Firewall-Logs: zeigen erlaubte/gebockte Verbindungen, z. B. ausgehend zu Command-and-Control-Servern.
Proxy-Logs: protokollieren Webzugriffe, nützlich bei Web-basierten Angriffen oder Datenabflüssen.
NetFlow/sFlow: Metadaten über Netzwerkströme (wer kommuniziert wann mit wem, wie viel Daten).
VPN (Virtual Private Network): verschlüsselte Verbindung ins Firmennetz; Logs können kompromittierte Zugänge enttarnen.
WAF = Web Application Firewall; filtert Angriffe auf Webanwendungen (z. B. SQL-Injection, Cross-Site-Scripting).
Mail-Header-Analyse hilft bei der Rückverfolgung von Phishing-Mails.
-->

---

# Incident-Typen & spezifische Reaktion

- **Malware-Infektion (z. B. Ransomware)**
  - Schnell isolieren, Backups schützen, keine Lösegeldzahlung ohne Strategie
- **Kompromittierte Zugangsdaten**
  - Passwörter zurücksetzen, MFA erzwingen, Log-Analyse
- **Datenabfluss (Data Breach)**
  - Umfang und Art der Daten klären, Meldepflichten prüfen
- **Webanwendungsangriff**
  - Logs sichern, Schwachstelle identifizieren, Patch / WAF-Regel

<!--
Ransomware: verschlüsselt Daten und fordert Lösegeld; wichtig: nicht vorschnell zahlen, sondern rechtliche und strategische Aspekte prüfen.
„MFA“ = Multi-Faktor-Authentifizierung, z. B. Passwort + Token/App; reduziert Risiko bei gestohlenen Passwörtern.
Data Breach: rechtlich besonders brisant, wenn personenbezogene Daten oder Geschäftsgeheimnisse betroffen sind.
Webanwendungsangriffe: OWASP Top 10 als Referenz (häufige Web-Schwachstellen).
-->

---

# Kommunikation & Eskalation

- **Interne Kommunikation**
  - Wer wird informiert? (Management, Fachbereiche, IT, Datenschutz)
  - Kommunikationskanäle (E-Mail, Telefon, Messenger)
- **Externe Kommunikation**
  - Datenschutzaufsicht (z. B. bei personenbezogenen Daten)
  - Betroffene Kunden / Nutzer
  - Strafverfolgungsbehörden
  - Presse / Öffentlichkeit (nur über definierte Stelle)

<!--
Wichtig: keine vertraulichen technischen Details über unsichere Kanäle (z. B. unverschlüsselte E‑Mails an externe).
Rollen definieren: wer spricht mit der Aufsicht, wer mit Kunden, wer mit Medien.
PR-Strategie (Public Relations): Transparenz vs. Reputationsschutz, aber rechtliche Meldepflichten einhalten.
Strafverfolgung: Polizei / Staatsanwaltschaft können bei schwerwiegenden Fällen eingeschaltet werden.
-->

---

# Rechtliche Aspekte

- **Datenschutzrecht (z. B. DSGVO)**
  - Meldepflicht bei Verletzung des Schutzes personenbezogener Daten
  - Dokumentationspflicht
- **Arbeitsrecht**
  - Forensik auf Mitarbeitergeräten, Mitbestimmung, Betriebsrat
- **Strafrecht**
  - Beweissicherung für mögliche Strafanzeigen
- **Verträge / SLAs**
  - Informations- und Meldepflichten gegenüber Kunden / Partnern

<!--
SLA = Service Level Agreement; legt u. a. Reaktionszeiten und Informationspflichten mit Dienstleistern fest.
DSGVO-Meldepflicht: in der Regel 72 Stunden nach Bekanntwerden eines Datenschutzvorfalls – Details sind juristisch komplex.
Arbeitsrecht: z. B. wann Log- und Forensikdaten auf Mitarbeitergeräte angewendet werden dürfen; Betriebsrat ggf. einzubeziehen.
Wichtig klarstellen: Diese Folie ist nur ein Überblick, keine Rechtsberatung – im Ernstfall immer Rechtsabteilung oder externe Juristen einbinden.
-->

---

# Dokumentation des Incidents

- **Einheitliche Vorlage verwenden:**
  - Datum / Uhrzeit (mit Zeitzone)
  - Beteiligte Personen / Rollen
  - Beobachtete Symptome
  - Getroffene Maßnahmen (wer, was, wann, warum)
  - Erhobene Beweise (inkl. Speicherort, Hash-Werte)
- **Dokumentation ist:**
  - Technisch nützlich (Nachvollziehbarkeit)
  - Rechtlich wichtig (Nachweis, Sorgfalt)

<!--
Bsp. Zeitzone: „27. Februar 2026, 10:13 CET“ (Central European Time).
Dokumentation hilft, später falsche Erinnerungen zu vermeiden („Haben wir das System vor oder nach dem Backup heruntergefahren?“).
Hash-Werte in der Doku vermerken, z. B. „Image Server01, SHA‑256: …“.
Vorlagen als Teil des Incident-Response-Plans bereitstellen (z. B. in einem Wiki oder Ticket-System).
-->

---

# Best Practices zur Vorbereitung
<br>

- **Harte Fakten**
  - Regelmäßige, getestete Backups
  - Patch-Management
  - Härtung von Systemen
- **Transparenz**
  - Logging & Monitoring
  - Zentrales SIEM/Log-Management
- **Organisation**
  - Incident-Response-Policy
  - Benannte Verantwortliche
  - Regelmäßige Schulungen

<!--
Patch-Management: strukturierter Prozess, um Software-Updates zeitnah und kontrolliert einzuspielen.
Systemhärtung: unnötige Dienste deaktivieren, Standardpasswörter ändern, minimale Rechte vergeben.
SIEM = Security Information and Event Management; sammelt Logs und korreliert sie, um Angriffe schneller zu erkennen.
Schulungen: nicht nur für IT, auch für alle Mitarbeitenden (z. B. Phishing-Trainings).
-->

---

# Typische Fehler in der Praxis

- Kein oder veralteter Incident-Response-Plan
- Unzureichende Logs (zu kurz, zu wenig)
- Ad-hoc-Maßnahmen ohne Dokumentation
- Systemschnell neu installieren ohne forensische Sicherung
- Fehlende Kommunikation (intern / extern)
- „Security by Obscurity“ statt klarer Prozesse

<!--
„Security by Obscurity“: Sicherheit nur durch Geheimhaltung (z. B. „niemand kennt unsere IP“); gilt als unsichere Strategie.
Neuinstallation ohne Sicherung: man verliert damit alle Hinweise auf den Angriffsweg – später keine Forensik möglich.
Logs: nicht nur 1–2 Tage aufbewahren; für Forensik oft wichtig, Wochen oder Monate zurückgehen zu können.
Fehlende Kommunikation: z. B. wenn Admins Dinge „nebenbei“ fixen, ohne IR‑Team zu informieren.
-->

---

# Checkliste – Erste Schritte beim Vorfall

1. Vorfall erkennen / Verdacht bestätigen
2. Incident melden (interner Prozess)
3. Relevante Personen informieren (IRT, ISB, DSB, Management)
4. Betroffene Systeme identifizieren
5. Eindämmung einleiten (Isolierung, Segmentierung)
6. Beweissicherung starten (Logs, Images, RAM)
7. Dokumentation durchgehend führen

<!--
DSB = Datenschutzbeauftragter; einbeziehen, wenn personenbezogene Daten betroffen sein könnten.
Segmentierung: z. B. betroffene Netzbereiche vom Rest trennen.
Diese Checkliste kann als Poster/Nutzerleitfaden aufbereitet werden (z. B. im Intranet).
Betonung: Dokumentation ist kein „Extra“, sondern integraler Teil der Reaktion.
-->

---

# Zusammenfassung

- **Digitale Forensik ist zentral für:**
  - Aufklärung, Beweisführung, Verbesserung der Sicherheit
- **Incident Response erfordert:**
  - Vorbereitung, klare Rollen, definierte Prozesse
- **Im akuten Fall:**
  - Ruhig bleiben, melden, dokumentieren, Spuren sichern
- **Ziel:**
  - Schaden begrenzen, Ursachen verstehen, künftig besser werden

<!--
Noch einmal den Zusammenhang zwischen Forensik (Fakten ermitteln) und IR (Handeln) hervorheben.
Hinweis auf weiterführende Themen: detaillierte Log-Analyse, spezielle Forensiktools, rechtliche Vertiefung.
Studierende ermutigen, in Projekten oder Praxisphasen reale Incident-Response-Prozesse kennenzulernen.
-->
