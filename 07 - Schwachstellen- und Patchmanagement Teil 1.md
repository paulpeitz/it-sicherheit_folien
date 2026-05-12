---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
title: Schwachstellen- und Patchmanagement
---
<style scoped>
p { text-align: center; }
</style>
![w:600px](img/node.js%20Update.png)

---

<!-- _class: title -->
# Schwachstellen- & Patchmanagement

<br>
<br>
<br>
<br>
<br>
<br>

## Der Lebenszyklus einer Schwachstelle

---

<!-- class: biglist -->
# Agenada

- Der "Hackerparagraph"
- Beispiel Der Equifax Vorfall
- Verständnis von Schwachstellen und deren Klassifikation  
- Finde und melden von Schwachstellen
- Aufbau eines Schwachstellenmanagement-Prozesses  
- Patchmanagement als technischer & organisatorischer Prozess  
- Tools, Best Practices & Fallstudien

---
<!-- _class: chapter -->

# Der „Hackerparagraph“

## Einordnung für Sicherheitsforschung
---
<!-- class: biglist -->
# Der „Hackerparagraph“ (§ 202c StGB)


- § 202c StGB stellt das **Vorbereiten des Ausspähens und Abfangens von Daten** unter Strafe.  
- Ziel war ursprünglich, **Cyberkriminalität** einzudämmen.  
- Betroffen sind u. a. Werkzeuge, die *zur Begehung* solcher Taten bestimmt sind.  
- Für Security-Research führt das zu **Unsicherheiten**, was erlaubt ist und was nicht.

---

# Was der Paragraph kriminalisiert
## Kerninhalte

- Herstellung, Beschaffung, Überlassen oder Verbreiten von:
  - Passwörtern oder Zugangscodes  
  - „Hacker-Tools“, die *zur Tatbegehung bestimmt* sind  
- Strafbarkeit bereits **vor** einer eigentlichen Tat („Vorbereitungshandlung“)  
- Unklare Abgrenzung zwischen legitimen Tools (z. B. Pentest-Frameworks) und verbotenen Werkzeugen

---

# Auswirkungen auf Sicherheitsforscher

- **Rechtsunsicherheit** bei Nutzung gängiger Tools wie Metasploit, Nmap, Hashcat.  
- Risiko, dass Forschung oder Lehre als „Vorbereitungshandlung“ interpretiert wird.  
- Institutionen verlangen oft **formale Freigaben** oder Verträge, um rechtliche Risiken zu minimieren.  
- Hemmnis für **Responsible Disclosure**, da Forscher befürchten, selbst ins Visier zu geraten.

---

# Umgang mit dem Hackerparagraphen
## Strategien für Forschung & Lehre

- Klare **Zweckbindung** dokumentieren (Forschung, Lehre, Pentests mit Auftrag).  
- Nutzung von **Testumgebungen**, CTFs und isolierten Laboren.  
- Schriftliche **Einwilligungen** und Verträge bei realen Systemen.  
- Austausch in der Community über Best Practices und rechtliche Entwicklungen.  
- Politische Diskussion über eine **Präzisierung** des Gesetzes hält an.


---
<!-- _class: chapter -->

# Equifax Data Breach (2017)  
## Datenabfluss über upgepatchte Lücke

<!-- presenter notes
Der Equifax‑Vorfall gilt als einer der bedeutendsten Datenschutzvorfälle der letzten Jahre. Equifax ist eines der drei großen US‑Kreditbüros, die personenbezogene Finanzdaten sammeln, um Kreditwürdigkeit zu bewerten. Diese Daten sind besonders sensibel, da sie nicht einfach ersetzt werden können – eine Sozialversicherungsnummer ist nicht wie ein Passwort. Der Fall zeigt, wie technische Schwachstellen, organisatorische Fehler und fehlende Sicherheitskultur zusammenwirken können.
-->

---
<!-- class: biglist -->

# Überblick

- Angriff: Mai–Juli 2017  
- Bekanntgabe: September 2017  
- Betroffene:  
  - 147,9 Mio. US‑Amerikaner  
  - 15,2 Mio. Briten  
  - 19.000 Kanadier  

<!-- presenter notes
Der Angriff dauerte über zwei Monate und blieb unentdeckt. Erst im September 2017 wurde der Vorfall öffentlich. Die Anzahl der Betroffenen ist enorm – fast die Hälfte der US‑Bevölkerung. Besonders kritisch: Die Daten, die gestohlen wurden, sind dauerhaft nutzbar. Anders als Passwörter können Sozialversicherungsnummern oder Geburtsdaten nicht einfach geändert werden. Der Vorfall zeigt, wie groß die Verantwortung von Unternehmen ist, die solche Daten speichern.
-->

---
# Was ist passiert?

- Ausnutzung einer Schwachstelle in Apache Struts  
- Remote Code Execution (RCE)  
- Lateral Movement im Netzwerk  
- Exfiltration sensibler Daten  
- Angreifer vermutlich staatlich unterstützt

<!-- presenter notes
Die Angreifer nutzten eine Schwachstelle in Apache Struts (CVE‑2017‑5638). Apache Struts ist ein Java‑Framework für Webanwendungen. Die Schwachstelle erlaubte Remote Code Execution – also das Ausführen beliebigen Codes auf dem Server. Nach dem Eindringen bewegten sich die Angreifer lateral im Netzwerk, also seitlich von System zu System. Sie exfiltrierten Daten wie Sozialversicherungsnummern, Geburtsdaten, Adressen und Führerscheinnummern. Die US‑Justiz ordnete den Angriff später einer Gruppe zu, die der chinesischen Volksbefreiungsarmee zugeordnet wird.
-->

---
# Warum konnte es passieren?  
## Technische Ursachen

- Ungepatchte Apache‑Struts‑Schwachstelle  
- Patch war verfügbar, wurde aber nicht eingespielt  
- Abgelaufenes TLS‑Zertifikat  
- Fehlende Netzwerksegmentierung  

<!-- presenter notes
Die zentrale technische Ursache war ein fehlendes Patch‑Management. Der Patch für die Struts‑Schwachstelle war bereits zwei Monate vor dem Angriff verfügbar. Dennoch wurde er nicht eingespielt. Zusätzlich war ein internes TLS‑Zertifikat abgelaufen, wodurch die SSL‑Inspection nicht funktionierte. Das Monitoring konnte den Datenabfluss daher nicht erkennen. Fehlende Netzwerksegmentierung ermöglichte es den Angreifern, sich frei im Netzwerk zu bewegen. Ein kompromittiertes System hätte nicht automatisch Zugriff auf andere kritische Systeme haben dürfen.
-->

---
# Warum konnte es passieren?  
## Organisatorische Ursachen

- Fehlende Sicherheitskultur  
- Unklare Verantwortlichkeiten  
- Mangelhafte interne Kommunikation  
- Schwaches Incident Response  

<!-- presenter notes
Neben technischen Schwächen spielte die Organisation eine zentrale Rolle. Sicherheitskultur bedeutet, dass Sicherheit als strategische Priorität verstanden wird. Bei Equifax war dies nicht der Fall. Patches wurden nicht zeitnah eingespielt, Warnungen nicht ernst genommen. Interne Kommunikation über kritische Updates funktionierte nicht. Incident‑Response‑Prozesse waren unzureichend – der Angriff blieb über zwei Monate unentdeckt. Der Fall zeigt, dass Cybersecurity nicht nur ein technisches, sondern auch ein organisatorisches Problem ist.
-->

---
# Auswirkungen

- Massiver Identitätsdiebstahl  
- Gesamtkosten: 1,38 Mrd. USD  
- Reputationsschaden  
- Rücktritte von Führungskräften  
- Rechtliche Konsequenzen  

<!-- presenter notes
Die Auswirkungen waren enorm. Die gestohlenen Daten sind dauerhaft verwertbar, was ein lebenslanges Risiko für Kreditbetrug bedeutet. Equifax musste über 1,38 Milliarden US‑Dollar für Strafen, Entschädigungen und Sicherheitsverbesserungen aufwenden. Der Reputationsschaden war erheblich, mehrere Führungskräfte traten zurück. Regulatorische Behörden wie die Federal Trade Commission (FTC) verhängten Auflagen und verpflichteten Equifax zu umfangreichen Sicherheitsmaßnahmen und Entschädigungszahlungen.
-->

---
# Lessons Learned

- Patch‑Management ist sicherheitskritisch  
- Zertifikatsmanagement nicht vernachlässigen  
- Monitoring und Segmentierung sind essenziell  
- Sicherheitskultur organisatorisch verankern  
- Transparente Kommunikation im Incident Response  

<!-- presenter notes
Der Equifax‑Fall zeigt, dass grundlegende Sicherheitsmaßnahmen entscheidend sind. Patch‑Management muss priorisiert werden. Zertifikatsmanagement darf nicht vernachlässigt werden, da abgelaufene Zertifikate Monitoring lahmlegen können. Netzwerksegmentierung und Monitoring sind essenziell, um Angreiferbewegungen zu erkennen und einzuschränken. Sicherheitskultur muss im Unternehmen verankert sein. Incident‑Response‑Prozesse müssen klar definiert und regelmäßig geübt werden.
-->

---

# Fazit

Der Equifax‑Breach war kein hochkomplexer Angriff, sondern das Ergebnis grundlegender Sicherheitsversäumnisse.

<!-- presenter notes
Der Angriff zeigt, dass selbst große Unternehmen mit kritischen Daten scheitern können, wenn grundlegende Sicherheitsmaßnahmen nicht eingehalten werden. Der Fall ist ein Lehrstück dafür, wie wichtig Prozesse, Disziplin und Sicherheitskultur sind. Moderne Technologien helfen nur, wenn sie konsequent eingesetzt und gepflegt werden. Der Equifax‑Breach bleibt ein Mahnmal für die gesamte Branche.
-->

---

<!-- _class: chapter -->
# Was sind Schwachstellen?

## Definition und Arten von Schwachstellen

---

# Was ist eine Schwachstelle?
- Fehler oder Eigenschaft eines Systems, die missbraucht werden kann  
- **Exploit:** konkrete Ausnutzung  
- **Threat – Vulnerability – Risk**  
  - Threat: Bedrohung  
  - Vulnerability: Schwachstelle  
  - Risk: Wahrscheinlichkeit × Auswirkung

---

# Arten von Schwachstellen
- Softwarefehler (Buffer Overflow, Injection, Logikfehler)  
- Fehlkonfigurationen (offene Ports, Standardpasswörter)  
- Designfehler (unsichere Protokolle)  
- Organisatorische Schwachstellen (fehlende Prozesse)

---

# Zero-Day vs. bekannte Schwachstellen
- **Zero-Day:** unbekannt, kein Patch verfügbar  
- **Bekannt:** in CVE-Datenbanken dokumentiert  
- Exploits oft schnell verfügbar

---
<!-- _class: chapter -->
# Schwachstellen finden und melden 
## Der erste Schritt

<!-- _notes:
Diese Vorlesung führt in die Grundlagen des Schwachstellen- und Patchmanagements ein. Eine Schwachstelle ist ein Fehler in Software, Hardware oder Konfigurationen, der von Angreifern ausgenutzt werden kann. Patchmanagement umfasst alle Prozesse, um solche Schwachstellen zu beheben. Wir betrachten heute, wie Schwachstellen entdeckt werden, wer sie findet, welche Werkzeuge dabei genutzt werden und wie verantwortungsvolle Meldung funktioniert. Dieses Wissen ist essenziell, um Risiken realistisch einzuschätzen und Sicherheitsmaßnahmen effektiv zu planen.
-->

---

# Agenda

- Wer sucht nach Schwachstellen?
- Warum werden Schwachstellen gesucht?
- Wie werden Schwachstellen gefunden?
- Responsible / Coordinated Disclosure
- Einordnung ins Schwachstellen- & Patchmanagement

<!-- _notes:
Die Agenda zeigt die Struktur der Vorlesung. Wir beginnen mit den Akteuren, die Schwachstellen finden, und analysieren anschließend ihre Motivation. Danach betrachten wir die technischen Methoden zur Schwachstellenfindung. Anschließend besprechen wir Responsible Disclosure, also den verantwortungsvollen Umgang mit Schwachstellenmeldungen. Zum Schluss ordnen wir alles in den Gesamtprozess des Schwachstellen- und Patchmanagements ein.
-->

---

# Wer sucht nach Schwachstellen?  
## Sicherheitsforschende

- Analysieren Software, Protokolle, Hardware  
- Ziel: Sicherheit verbessern, Forschung, Reputation  
- Veröffentlichung über CVE

<!-- _notes:
Sicherheitsforschende – oft „Security Researchers“ genannt – suchen systematisch nach Schwachstellen. Sie analysieren Software, Protokolle oder Hardwarekomponenten. Häufig nutzen sie Methoden wie statische Codeanalyse, Fuzzing oder Reverse Engineering. Ihre Motivation ist meist wissenschaftlich oder reputationsgetrieben. Viele veröffentlichen ihre Ergebnisse als CVE („Common Vulnerabilities and Exposures“), einem globalen Katalog für Schwachstellen. Diese Veröffentlichungen tragen zur Verbesserung der IT-Sicherheit bei.
-->

---

# Wer sucht nach Schwachstellen?  
## Penetrationstester & Red Teams

- Arbeiten im Auftrag eines Unternehmens  
- Simulieren reale Angriffe  
- Finden technische, organisatorische und prozessuale Schwachstellen

<!-- _notes:
Penetrationstester – kurz Pentester – werden beauftragt, Systeme gezielt anzugreifen, um Schwachstellen zu identifizieren. Sie nutzen Tools wie Metasploit oder Burp Suite und kombinieren automatisierte Scans mit manueller Analyse. Red Teams gehen weiter: Sie testen nicht nur Technik, sondern auch Prozesse und Menschen, etwa durch Social Engineering. Ziel ist es, die gesamte Sicherheitslage eines Unternehmens realistisch zu bewerten. Beide Rollen sind essenziell für Compliance-Anforderungen wie ISO 27001 oder NIS2.
-->

---

# Wer sucht nach Schwachstellen?  
## Cyberkriminelle

- Ziel: 
  - finanzieller Gewinn
  - Erpressung
  - Spionage  
- Nutzen Schwachstellen für Malware, Ransomware, Datendiebstahl

<!-- _notes:
Cyberkriminelle suchen Schwachstellen, um sie auszunutzen. Ihre Motivation ist finanzieller Gewinn, Sabotage oder Spionage. Sie entwickeln Exploits, verkaufen Zero-Day-Schwachstellen auf dem Schwarzmarkt oder nutzen bekannte Schwachstellen für Ransomware-Angriffe. Ein „Zero-Day“ ist eine Schwachstelle, die dem Hersteller noch nicht bekannt ist und für die es keinen Patch gibt. Cyberkriminelle arbeiten oft hochprofessionell, teilweise in organisierten Gruppen.
-->

---

# Wer sucht nach Schwachstellen?  
## Hersteller & Open-Source-Community

- Interne Security-Teams (PSIRT)  
- Automatisierte Tests, Code-Reviews  
- Community-Meldungen bei Open-Source-Projekten

<!-- _notes:
Viele Hersteller betreiben interne Sicherheitsteams, sogenannte PSIRTs („Product Security Incident Response Teams“). Diese Teams führen Code-Reviews, automatisierte Tests und interne Penetrationstests durch. In der Open-Source-Welt übernehmen oft freiwillige Entwickler diese Rolle. Sie prüfen Code, melden Schwachstellen und entwickeln Patches. Da Open-Source-Projekte öffentlich sind, können viele Augen Fehler schneller entdecken – ein Prinzip, das als „Linus’ Law“ bekannt ist: „Given enough eyeballs, all bugs are shallow.“
-->

---

# Warum suchen Menschen nach Schwachstellen?

- Verbesserung der IT-Sicherheit  
- Wissenschaftliches Interesse  
- Bug-Bounty-Prämien  
- Reputation (Publikationen, CVE-Einträge)  
- Kriminelle Motivation

<!-- _notes:
Die Motivation variiert stark. Forschende wollen Sicherheit verbessern oder wissenschaftliche Erkenntnisse gewinnen. Bug-Bounty-Programme bieten finanzielle Anreize – Plattformen wie HackerOne zahlen teils hohe Prämien. Reputation spielt ebenfalls eine Rolle: Ein veröffentlichter CVE-Eintrag kann die Karriere fördern. Auf der anderen Seite stehen kriminelle Motive wie Erpressung oder Datendiebstahl. Diese Vielfalt erklärt, warum Schwachstellen sowohl verantwortungsvoll gemeldet als auch missbraucht werden.
-->

---

# Wie werden Schwachstellen gefunden?  
## Automatisierte Scans

- Nessus, OpenVAS/Greenbone  
- Qualys, Rapid7  
- Nmap + NSE  
- Finden bekannte Schwachstellen

<!-- _notes:
Automatisierte Schwachstellenscanner durchsuchen Systeme nach bekannten Schwachstellen. Sie vergleichen Softwareversionen, Konfigurationen und offene Ports mit Datenbanken wie CVE oder NVD („National Vulnerability Database“). Tools wie Nessus, OpenVAS/Greenbone oder Qualys sind weit verbreitet. Nmap mit NSE-Skripten („Nmap Scripting Engine“) kann ebenfalls Schwachstellen erkennen. Automatisierte Scans sind schnell, aber finden keine komplexen Logikfehler.
-->

---

# Wie werden Schwachstellen gefunden?  
## Manuelle Analyse

- Statischer Code-Review  
- Reverse Engineering  
- Fuzzing  
- Logikfehleranalyse

<!-- _notes:
Manuelle Analyse ist oft tiefgehender als automatisierte Scans. Statische Codeanalyse untersucht Quellcode ohne Ausführung. Reverse Engineering analysiert Binärdateien, um Funktionsweise und Fehler zu verstehen. Fuzzing testet Programme mit zufälligen oder manipulierten Eingaben, um Abstürze oder unerwartetes Verhalten zu provozieren. Moderne Fuzzer wie AFL oder libFuzzer nutzen Coverage-Feedback, um Eingaben gezielt zu optimieren. Logikfehler lassen sich meist nur manuell finden.
-->

---

# Wie werden Schwachstellen gefunden?  
## Penetrationstests

- Kombination aus Tools und manueller Analyse  
- Fokus auf reale Angriffsszenarien  
- Ergebnis: Exploits, Risikoanalyse

<!-- _notes:
Penetrationstests kombinieren automatisierte Tools mit manueller Kreativität. Pentester analysieren Netzwerke, Webanwendungen, APIs oder mobile Apps. Sie nutzen Exploit-Frameworks wie Metasploit, Proxy-Tools wie Burp Suite oder Recon-Tools wie Amass. Ein wichtiger Aspekt ist die Risikoanalyse: Nicht jede Schwachstelle ist gleich kritisch. Pentester bewerten Auswirkungen, Ausnutzbarkeit und mögliche Angriffsketten. Das Ergebnis ist ein Bericht mit Priorisierung und Handlungsempfehlungen.
-->

---

# Wie werden Schwachstellen gefunden?  
## Betrieb & Monitoring

- Regelmäßige Sicherheitsüberprüfungen  
- Log-Analyse  
- Konfigurationsprüfungen  
- Compliance-Anforderungen

<!-- _notes:
Im laufenden Betrieb entstehen ständig neue Schwachstellen, etwa durch Fehlkonfigurationen oder neue Softwareversionen. Regelmäßige Sicherheitsüberprüfungen, Log-Analysen und Konfigurationsprüfungen sind daher notwendig. SIEM-Systeme („Security Information and Event Management“) wie Splunk oder Elastic Security sammeln und korrelieren Logdaten, um Anomalien zu erkennen. Compliance-Anforderungen wie DSGVO oder NIS2 verlangen regelmäßige Überprüfungen und dokumentierte Prozesse.
-->

---
<!-- _class: chapter -->
# Melden von Schwachstellen

## zweiter Schritt

---

# Was tun, wenn ich eine Schwachstelle finde?

1. **Dokumentieren**  
   - Reproduktionsschritte  
   - Umgebung, Versionen  
   - Auswirkungen

2. **Nicht ausnutzen!**  
   - Keine Daten exfiltrieren  
   - Keine Systeme verändern
   - 
---

# Was tun, wenn ich eine Schwachstelle finde?
3. **Verantwortungsvoll melden**  
   - Hersteller  
   - CERT/BSI  
   - Bug‑Bounty‑Programme (falls vorhanden)

4. **Rechtliche Risiken beachten**  
   - Unautorisierte Tests können strafbar sein  
   - §202a–c StGB (Deutschland)

---

# Responsible Disclosure vs. Full Disclosure

## Responsible Disclosure
- Forscher meldet Schwachstelle **zuerst vertraulich** an Hersteller  
- Hersteller erhält Zeit zum Patchen  
- Danach koordinierte Veröffentlichung  
- Heute Standard (z. B. Google Project Zero: 90‑Tage‑Frist)
- 
---

# Responsible Disclosure vs. Full Disclosure

## Full Disclosure
- Sofortige öffentliche Veröffentlichung  
- Druck auf Hersteller, aber Risiko für Nutzer  
- Heute eher unüblich und oft kritisiert

---
<!-- _class: normal -->
# Wo kann ich Schwachstellen melden?

## 1. Direkt beim Hersteller
- Security‑Kontaktadresse (security@…)
- Vulnerability Disclosure Policy (VDP)
- Formulare auf der Website

## 2. Nationale Stellen
- **BSI CERT‑Bund** (Deutschland)  
- **CERT/CC** (USA)  
- Vermitteln zwischen Forschern und Herstellern

---

# Wo kann ich Schwachstellen melden?
## 3. Bug‑Bounty‑Plattformen
- HackerOne  
- Bugcrowd  
- Intigriti  
- YesWeHack

---

# Wie läuft eine Meldung typischerweise ab?

1. Einreichung der Schwachstelle  
2. Bestätigung (Acknowledgement)  
3. Validierung durch das Security‑Team  
4. Priorisierung (CVSS‑Score)  
5. Entwicklung eines Patches  
6. Koordinierte Veröffentlichung (Advisory, CVE)

---

# Kann ich eine Schwachstelle veröffentlichen?

**Ja, aber verantwortungsvoll.**

- Nach angemessener Frist (z. B. 90 Tage)  
- Ohne Details, die aktiven Missbrauch ermöglichen, bevor ein Patch existiert  
- In Abstimmung mit dem Hersteller  
- Veröffentlichung üblicherweise über:  
  - Blogposts  
  - Security Advisories  
  - Konferenzen (Black Hat, DEF CON, CCC)

---

# Kann ich eine Schwachstelle verkaufen?
<!-- class: normal -->
## Legale Wege
- Bug‑Bounty‑Programme  
- Sicherheitsfirmen (z. B. Zerodium – sehr umstritten)  
- Wettbewerbe wie Pwn2Own  
- Plattformen mit Responsible‑Disclosure‑Regeln

## Illegale Wege
- Darknet‑Marktplätze  
- Verkauf an Kriminelle oder Staaten ohne Offenlegung  
- → **Strafbar und ethisch nicht vertretbar**

---
<!-- class: biglist -->
# Bug‑Bounty‑Programme

## Was ist ein Bug Bounty?
- Hersteller zahlen **Belohnungen** für gemeldete Schwachstellen  
- Höhe abhängig von:
  - Kritikalität  
  - Ausnutzbarkeit  
  - Produkt (z. B. Browser, Cloud, Mobile)

---

# Bug‑Bounty‑Programme

## Vorteile
- Legaler Rahmen  
- Transparente Regeln  
- Anerkennung (Hall of Fame)  
- Teilweise sehr hohe Prämien

---

# Bekannte Bug‑Bounty‑Plattformen

- **HackerOne** - Größte Plattform, viele große Unternehmen (Meta, Uber, US‑Regierung)

- **Bugcrowd** - Fokus auf Crowd‑Security‑Testing

- **Intigriti** - Europäischer Anbieter, DSGVO‑konform

- **YesWeHack** - Starker Fokus auf EU‑Unternehmen

---

# Pwn2Own – Überblick

- Einer der bekanntesten **Exploit‑Wettbewerbe** weltweit  
- Organisiert von **Trend Micro’s Zero Day Initiative (ZDI)**  

---

# Pwn2Own – Überblick

## Ziel: Live‑Exploits gegen:
  - Browser (Chrome, Edge, Safari)  
  - Virtualisierung (VMware, VirtualBox)  
  - Automotive  
  - Mobile  
  - ICS/SCADA  
  - IoT‑Geräte

---

# Pwn2Own – Ablauf

- Teams melden ihre Exploits **vorab** an ZDI  
- Live‑Demonstration auf der Bühne  
- Erfolgreiche Exploits → Preisgeld + Gerät  
- ZDI meldet Schwachstellen an Hersteller  
- Hersteller patchen → Veröffentlichung der Advisories

---

# Pwn2Own – Bedeutung

- Fördert **Zero‑Day‑Forschung**  
- Hersteller erhalten frühzeitig kritische Schwachstellen  
- Hohe Preisgelder (teilweise >1 Mio. USD pro Wettbewerb)  
- Strenger legaler Rahmen

---

# Weitere Wettbewerbe

- **Google Capture The Flag (CTF)**  
- **DEF CON CTF**  
- **Hack‑a‑Sat** (US‑Air‑Force)  
- **European Cyber Security Challenge (ECSC)**  
- Fokus: Exploitation, Reverse Engineering, Kryptographie, Forensik

---
# Zusammenfassung

- Schwachstellen melden: **Hersteller → CERT → Bug Bounty**  
- Veröffentlichung: **Koordiniert & verantwortungsvoll**  
- Verkauf: **Nur legal über Bounties oder Wettbewerbe**  
- Pwn2Own: Hochkarätiger Exploit‑Wettbewerb mit großem Einfluss  
- Recht: Ohne Erlaubnis testen ist strafbar

---

<!-- _class: chapter -->
# CVE

## Datenbank für Sicherheitslücken

---
<!-- class: normal -->
# CVE – Was steckt dahinter?
## Common Vulnerabilities and Exposures

- **CVE = öffentliches Verzeichnis** für bekannte Schwachstellen  
- Jede Schwachstelle erhält eine **eindeutige ID**, z. B.  
  - `CVE-2021-44228` (Log4Shell)  
- CVE-Einträge enthalten:  
  - Kurzbeschreibung der Schwachstelle  
  - Betroffene Produkte / Versionen  
  - Referenzen zu Advisories, Patches, Exploits  
- Herausgeber: **MITRE Corporation**, unterstützt durch internationale Partner

---

# Wie entsteht ein CVE-Eintrag?

1. **Entdeckung der Schwachstelle**  
   – durch Forscher, Hersteller, Bug-Bounty-Programme  
2. **Meldung an eine CNA**  
   – CNA = CVE Numbering Authority (z. B. Microsoft, Red Hat, Apache)  
3. **Zuweisung einer CVE-ID**  
   – eindeutige Identifikation, bevor Details veröffentlicht werden  
4. **Validierung & Veröffentlichung**  
   – Beschreibung, Referenzen, Schweregrad  
5. **Nutzung durch Security-Tools**  
   – Scanner, Advisories, Patchmanagement-Systeme

---

<!-- _class: chapter -->
# CVSS

## Standardisierte Bewertung von Sicherheitslücken

---

# Was ist CVSS?

> **Common Vulnerability Scoring System** – ein offener Standard zur Bewertung von Sicherheitslücken in Software und Hardware.

- Entwickelt vom **NIST** und gepflegt durch das **FIRST** (Forum of Incident Response and Security Teams)
- Aktuelle Version: **CVSS v3.1** (2019), **CVSS v4.0** (2023)
- Liefert einen numerischen Score von **0.0 – 10.0**
- Ziel: **vergleichbare, reproduzierbare** Schweregradbewertung

---

# Was ist CVSS?


| Score | Schweregrad |
|-------|-------------|
| 0.0 | None |
| 0.1 – 3.9 | Low |
| 4.0 – 6.9 | Medium |
| 7.0 – 8.9 | High |
| 9.0 – 10.0 | Critical |

---

# Wofür wird CVSS verwendet?

## Typische Einsatzbereiche

- **Vulnerability Databases** – NVD, CVE-Einträge enthalten CVSS-Scores
- **Patch-Management** – Priorisierung von Patches nach Kritikalität
- **Risikomanagement** – Grundlage für Risikoberichte & Compliance (BSI, ISO 27001)
- **Einkauf & Lieferanten** – SLA-Definitionen für Reaktionszeiten
- **Penetration Testing** – Einheitliche Kommunikation von Befunden

---

# Die drei Metrikgruppen

```
CVSS v3.1
├── Base Score        (unveränderlich, technische Eigenschaften)
├── Temporal Score    (zeitabhängig, z. B. verfügbarer Exploit)
└── Environmental Score (unternehmensspezifisch, Kontext)
```

<br>
<br>

> Der **Base Score** ist der wichtigste und meistgezitierte Wert.
> Temporal & Environmental können den Score nach unten oder oben korrigieren.

---

# Base Score – Exploitability Metrics

Beschreibt, **wie leicht** eine Schwachstelle ausgenutzt werden kann.

| Metrik | Kürzel | Bedeutung |
|--------|--------|-----------|
| Attack Vector | AV | Wie nähert sich der Angreifer? |
| Attack Complexity | AC | Wie komplex ist der Angriff? |
| Privileges Required | PR | Welche Rechte braucht der Angreifer? |
| User Interaction | UI | Muss ein Nutzer aktiv mitwirken? |

---

# Base Score – Impact Metrics

Beschreibt, **welchen Schaden** eine erfolgreiche Ausnutzung anrichtet (CIA-Triad).

| Metrik | Kürzel | Bedeutung |
|--------|--------|-----------|
| Confidentiality Impact | C | Verlust von Vertraulichkeit |
| Integrity Impact | I | Verlust von Integrität |
| Availability Impact | A | Verlust von Verfügbarkeit |
| Scope | S | Überschreitet die Schwachstelle die Systemgrenze? |

---

# Bedeutung der Parameter im Detail

## Attack Vector (AV)

| Wert | Bedeutung | Gewicht |
|------|-----------|---------|
| Network (N) | Angriff über Netzwerk möglich (z. B. Internet) | 0.85 |
| Adjacent (A) | Angriff nur im lokalen Netz möglich | 0.62 |
| Local (L) | Angriff erfordert lokalen Zugang | 0.55 |
| Physical (P) | Physischer Zugang zum Gerät nötig | 0.20 |

---

# Bedeutung der Parameter im Detail 


## Attack Complexity (AC)

| Wert | Bedeutung | Gewicht |
|------|-----------|---------|
| Low (L) | Keine besonderen Bedingungen nötig | 0.77 |
| High (H) | Bestimmte Bedingungen müssen erfüllt sein | 0.44 |

---

# Bedeutung der Parameter im Detail

## Privileges Required (PR)

| Wert | Bedeutung |
|------|-----------|
| None (N) | Kein Account nötig |
| Low (L) | Normaler Benutzer-Account reicht |
| High (H) | Admin-Rechte erforderlich |

---

# Bedeutung der Parameter im Detail
## Impact-Metriken (C / I / A)

| Wert | Bedeutung |
|------|-----------|
| None (N) | Kein Einfluss auf diesen Aspekt |
| Low (L) | Eingeschränkter Einfluss |
| High (H) | Vollständiger Verlust / komplette Kontrolle |

## Scope (S)
- **Unchanged**: Schaden bleibt im angegriffenen System
- **Changed**: Angriff wirkt sich auf andere Systeme aus (z. B. VM-Escape)

---

# Berechnung des CVSS Base Score

## Schritt 1: ISS & ESS berechnen

```
ISS = 1 − [(1 − ImpactConf) × (1 − ImpactInteg) × (1 − ImpactAvail)]

ESS = AttackVector × AttackComplexity × PrivilegesRequired × UserInteraction
```

---

# Berechnung des CVSS Base Score

## Schritt 2: Impact Sub Score (ISCBase)

```
Scope Unchanged:  ISCBase = 6.42 × ISS
Scope Changed:    ISCBase = 7.52 × [ISS − 0.029] − 3.25 × [ISS − 0.02]^15
```

---

# Berechnung des CVSS Base Score
## Schritt 3: Base Score

```
Scope Unchanged:  BaseScore = Roundup(min(ISCBase + 3.841 × ESS, 10))
Scope Changed:    BaseScore = Roundup(min(1.08 × (ISCBase + 3.841 × ESS), 10))
```

---

# Praxisbeispiel: Log4Shell

## CVE-2021-44228 – CVSS v3.1 Score: **10.0 (Critical)**

```
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H
```

---

# Praxisbeispiel: Log4Shell


| Parameter | Wert | Begründung |
|-----------|------|------------|
| Attack Vector | Network (N) | Über HTTP-Request auslösbar |
| Attack Complexity | Low (L) | Trivial auszunutzen |
| Privileges Required | None (N) | Kein Login nötig |
| User Interaction | None (N) | Vollautomatisch |
| Scope | Changed (C) | JNDI-Lookup → RCE auf anderem System |
| Confidentiality | High (H) | Vollständige Datenoffenlegung |
| Integrity | High (H) | Beliebige Codeausführung |
| Availability | High (H) | System komplett kompromittierbar |

---

# Berechnung: Log4Shell Schritt für Schritt

```
ImpactConf  = 0.56 (High)
ImpactInteg = 0.56 (High)
ImpactAvail = 0.56 (High)

ISS = 1 − [(1−0.56) × (1−0.56) × (1−0.56)]
    = 1 − [0.44^3]
    = 1 − 0.0852 = 0.9148

ESS = 0.85 × 0.77 × 0.85 × 0.85 = 0.4717

ISCBase (Scope Changed):
    = 7.52 × (0.9148 − 0.029) − 3.25 × (0.9148 − 0.02)^15
    ≈ 7.52 × 0.8858 − 3.25 × 0.2576
    ≈ 6.661 − 0.837 ≈ 5.824

BaseScore = Roundup(min(1.08 × (5.824 + 3.841 × 0.4717), 10))
          = Roundup(min(1.08 × 7.635, 10))
          = Roundup(8.246) = 10.0 ✓
```

---

# Temporal Score

Berücksichtigt den **aktuellen Reifegrad** der Bedrohungslage.

| Metrik | Werte | Bedeutung |
|--------|-------|-----------|
| Exploit Code Maturity (E) | Not Defined / Unproven / POC / Functional / High | Verfügbarkeit von Exploit-Code |
| Remediation Level (RL) | Official Fix / Temporary Fix / Workaround / Unavailable | Status des Patches |
| Report Confidence (RC) | Confirmed / Reasonable / Unknown | Verlässlichkeit des Berichts |

> **Temporal Score ≤ Base Score** – er kann den Wert nur senken.

---

# Environmental Score

Passt den Score an **unternehmenseigene Gegebenheiten** an.

- Wie kritisch ist das **betroffene Asset** im eigenen Kontext?
- Gibt es bereits **Mitigationsmaßnahmen** (z. B. WAF, Segmentierung)?

## Modified Base Metrics (Beispiele)

```
CR (Confidentiality Requirement): Low / Medium / High / Not Defined
IR (Integrity Requirement):       Low / Medium / High / Not Defined
AR (Availability Requirement):    Low / Medium / High / Not Defined
```

> Ein öffentlicher Webserver mit kritischen Kundendaten →
> hoher Environmental Score trotz niedrigem Base Score

---

# Kritik an CVSS (1/2)

## Technische Schwächen

- **Keine Kontextualisierung** – AV:N/AC:L klingt schlimm, aber betrifft das System überhaupt mein Netz?
- **Score-Inflation** – Viele Schwachstellen werden als „High" oder „Critical" bewertet; Priorisierung wird schwieriger
- **Pseudopräzision** – Eine Kommastelle suggeriert Genauigkeit, die nicht existiert (7.5 vs. 7.8 praktisch identisch)
- **Nichtlineare Formel** – Kleine Parameteränderungen können große Sprünge verursachen
- **Kein Exploit-Kontext** – CVSS misst Schwere, nicht Wahrscheinlichkeit der Ausnutzung

---

# Kritik an CVSS (2/2)

## Organisatorische & praktische Schwächen

- **Temporal/Environmental selten genutzt** – In der Praxis dominiert der Base Score
- **Subjektivität** – Verschiedene Bewerter kommen zu unterschiedlichen Scores für dieselbe CVE
- **Zeitverzögerung** – Score wird oft Wochen nach Bekanntwerden erst publiziert
- **Kettenangriffe unberücksichtigt** – Mehrere „Low"-Schwachstellen können kombiniert kritisch sein
- **Missbrauch als KPI** – „Alle Critical-CVEs schließen" als Ziel führt zu Fehlanreizen

> *„A CVSS score of 9.8 does not mean you will be hacked. A score of 2.0 does not mean you won't."*

---

# Alternativen & Ergänzungen zu CVSS

| System | Ansatz |
|--------|--------|
| **EPSS** (Exploit Prediction Scoring System) | Wahrscheinlichkeit der Ausnutzung in den nächsten 30 Tagen (0–100%) |
| **SSVC** (Stakeholder-Specific Vulnerability Categorization) | Entscheidungsbaum-basiert, kontextabhängig |
| **CVSS v4.0** | Neue Metrikgruppen, Threat Intelligence integriert |


> **Best Practice**: CVSS Base Score + EPSS kombinieren
> → Hoher Schweregrad **und** hohe Ausnutzungswahrscheinlichkeit = höchste Priorität

---

# Zusammenfassung

| Aspekt | Kernaussage |
|--------|-------------|
| Was ist CVSS? | Standardisierter Score 0–10 für Schwachstellen |
| Berechnung | Exploitability × Impact, nicht-linear |
| Base Score | Technische Eigenschaften, kontextfrei |
| Temporal/Env. | Zeitlage & Unternehmenskontext |
| Stärken | Vergleichbarkeit, weite Verbreitung |
| Schwächen | Kein Kontext, Score-Inflation, Subjektivität |
| Empfehlung | CVSS + EPSS + eigener Kontext = bessere Priorisierung |

---