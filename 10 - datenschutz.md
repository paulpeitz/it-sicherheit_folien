---
marp: true
theme: custom
title: DSGVO und Datenschutz
footer: ![w:280](img/dhbw-ka.svg)
transition: slide
---
<!-- _class: title -->
# Rechtliche Aspekte des Datenschutzes

---
<!-- _class: chapter -->

# Datenschutzgrundverordnung

## (DSGVO)

---

# Ziele der DSGVO
<!-- class: biglist -->
- Schutz personenbezogener Daten  
- Stärkung der Rechte von Betroffenen  
- Vereinheitlichung des Datenschutzrechts in der EU  
- Verpflichtung zu sicherer Datenverarbeitung  
- Transparenz über Datenflüsse  
- Kontrolle über internationale Datenübermittlungen (z. B. USA)

---

# Personenbezogene Daten
<!-- class: normal -->
Alle Informationen, die eine Person identifizieren oder identifizierbar machen.

Beispiele:
- Name, Adresse, E-Mail  
- IP-Adresse  
- Standortdaten  
- Cookies (identifizierend)  
- Nutzerverhalten, Logdaten  
- Gesundheits- und biometrische Daten (besonders sensibel)

---

# Rechtsgrundlagen (Art. 6 DSGVO)

Wann darf man Daten verarbeiten?

- Einwilligung  
- Vertragserfüllung  
- Rechtliche Verpflichtung  
- Lebenswichtige Interessen  
- Öffentliches Interesse  
- Berechtigtes Interesse

## Jede Verarbeitung braucht eine dokumentierte Rechtsgrundlage.

---

# Grundprinzipien der DSGVO (Art. 5)
<!-- _class: normal -->

- Rechtmäßigkeit & Transparenz  
- Zweckbindung  
- Datenminimierung  
- Richtigkeit  
- Speicherbegrenzung  
- Integrität & Vertraulichkeit  
- Rechenschaftspflicht

Diese Prinzipien müssen sich in Architektur, Logging, Datenmodellen und Prozessen widerspiegeln.

---

# Privacy by Design & Default (Art. 25)

**Privacy by Design:** Datenschutz in die Architektur einbauen.  
**Privacy by Default:** Datensparsame Voreinstellungen.

Beispiele:
- Opt‑in statt Opt‑out  
- Logging ohne personenbezogene Daten  
- Minimale Datenspeicherung  
- Standardmäßig deaktiviertes Tracking

---

# Pflichten für IT‑Systeme

- Verzeichnis von Verarbeitungstätigkeiten  
- Datenschutz-Folgenabschätzung (DSFA)  
- Meldung von Datenpannen (72h)  
- Auftragsverarbeitungsverträge  
- Dokumentation aller Maßnahmen  
- Löschkonzepte  
- Umsetzung der Betroffenenrechte

---

# Rechte der Betroffenen (Art. 12–23 DSGVO)

## Überblick
Betroffene Personen haben umfangreiche Rechte gegenüber Verantwortlichen.  
IT‑Systeme müssen diese Rechte **technisch, sicher und effizient** unterstützen.

---

# Rechte der Betroffenen – Details & technische Anforderungen

## Auskunftsrecht (Art. 15)
- Welche Daten werden verarbeitet?  
- Woher stammen sie?  
- An wen werden sie übermittelt?  
- Wie lange werden sie gespeichert? 

---

# Rechte der Betroffenen – Details & technische Anforderungen

## Technische Anforderungen: 
- Vollständige Dateninventare  
- Durchsuchbare Datenbanken  
- Nachvollziehbare Datenflüsse (Data Lineage)

---

# Recht auf Berichtigung (Art. 16)

## Falsche oder unvollständige Daten müssen korrigiert werden.  

**Technische Anforderungen:**  
- Änderbare Datenmodelle  
- Versionierung oder Audit‑Logs  
- Validierungsmechanismen

---

# Recht auf Löschung („Vergessenwerden“, Art. 17)

## Löschung personenbezogener Daten, wenn kein Zweck mehr besteht.  

**Technische Herausforderungen:**  
- Löschkonzepte für verteilte Systeme  
- Löschung in Microservices, Message Queues, Data Lakes  
- Unveränderbare Logs (z. B. Audit‑Trails) → *Pseudonymisierung statt Löschung*

---

# Recht auf Einschränkung der Verarbeitung (Art. 18)

## Daten dürfen gespeichert, aber nicht weiterverarbeitet werden.  

**Technische Anforderungen:**  
- „Frozen State“ für Datensätze  
- Flags/Statusfelder in Datenbanken  
- Anpassung von ETL‑Pipelines

---

# Recht auf Datenübertragbarkeit (Art. 20)
- Bereitstellung der Daten in **maschinenlesbarem Format** (JSON, CSV, XML).  
- Übertragung an andere Dienste.  
**Technische Anforderungen:**  
- Export‑APIs  
- Standardisierte Datenformate  
- Klare Datenmodelle

---

# Widerspruchsrecht (Art. 21)

## Gegen bestimmte Verarbeitungen (z. B. Tracking, Profiling).  

**Technische Anforderungen:**  
- Opt‑out‑Mechanismen  
- Consent‑Management‑Systeme  
- Anpassung von Tracking‑Tools

---

# Recht auf Widerruf der Einwilligung (Art. 7)

## Jederzeit und ohne Nachteile.  

**Technische Anforderungen:**  
- Consent‑Logs  
- Widerruf muss sofort wirksam sein  
- Systeme müssen „Einwilligungsstatus“ berücksichtigen

---

# Recht auf Nicht‑Automatisierte Entscheidungen (Art. 22)

##Schutz vor ausschließlich algorithmischen Entscheidungen (z. B. Kredit‑Scoring).  

**Technische Anforderungen:**  
- Human‑in‑the‑loop  
- Erklärbare KI (Explainable AI)  
- Dokumentation von Entscheidungslogiken

---

# Recht auf Benachrichtigung bei Datenpannen (Art. 34)

## Betroffene müssen informiert werden, wenn ein hohes Risiko besteht.  

**Technische Anforderungen:**  
- Incident‑Response‑Prozesse  
- Monitoring & Alerting  
- Forensische Logs

---
<!-- _class: chapter -->
# Anonymisierung vs. Pseudonymisierung

---

# Anonymisierung vs. Pseudonymisierung

## Anonymisierung
- Personenbezug dauerhaft entfernt  
- Keine Re‑Identifikation möglich  
- DSGVO **nicht mehr anwendbar**  
- Sehr schwer korrekt umzusetzen (Risiko von Rückschlüssen)

---

# Anonymisierung vs. Pseudonymisierung

## Pseudonymisierung
- Identifikatoren ersetzt (z. B. Hash, Token)  
- Re‑Identifikation technisch möglich (Schlüssel existiert)  
- DSGVO **weiterhin anwendbar**  
- Reduziert Risiko, aber kein Freifahrtschein

---

# Bedeutung für Softwareentwicklung

## Warum relevant?
- Entwickler benötigen Testdaten  
- Logs enthalten oft personenbezogene Daten  
- DevOps‑Pipelines kopieren Daten zwischen Systemen  
- Datenbanken werden für Tests, QA, Staging repliziert

## Konsequenz:
- Produktivdaten dürfen **nicht ungefiltert** in Testsysteme gelangen  
- Testsysteme sind oft schlechter geschützt → höheres Risiko

---

# Pseudonymisierung in der Softwareentwicklung

## Typische Verfahren:
- Hashing (mit Salt)  
- Tokenisierung  
- Ersetzen durch synthetische Werte  
- Mapping‑Tabellen (streng geschützt)

---

# Pseudonymisierung in der Softwareentwicklung

Einsatzbereiche:
- Testdaten in Staging/QA  
- Logging/Monitoring  
- Analytics‑Pipelines  
- Fehleranalyse (Debugging)

Wichtig:
- Schlüssel/Mapping getrennt speichern  
- Zugriff stark beschränken  
- Re‑Identifikation nur für definierte Zwecke

---

# Anonymisierung in der Softwareentwicklung

Einsatz, wenn:
- Daten für Forschung, Statistik, ML‑Training genutzt werden  
- Kein Personenbezug notwendig ist

Methoden:
- Generalisierung (z. B. Altersgruppen statt Geburtsdatum)  
- Rauschen (Differential Privacy)  
- Aggregation  
- K‑Anonymität, L‑Diversität, T‑Closeness
---

# Anonymisierung in der Softwareentwicklung

## Risiken:
- Re‑Identifikation durch Kombination mehrerer Attribute  
- ML‑Modelle können Originaldaten „memorieren“

---

# Testdatenmanagement (TDM) unter der DSGVO

Pflichten:
- Keine echten personenbezogenen Daten in Testsystemen  
- Automatisierte Anonymisierung/Pseudonymisierung in CI/CD  
- Dokumentation der Verfahren  
- Zugriffskontrollen für Entwickler  
- Löschkonzepte auch für Testdaten

---

# Testdatenmanagement (TDM) unter der DSGVO

Best Practices:
- Synthetic Data Generation  
- Data Masking Tools  
- Role‑based Access Control  
- Logging ohne personenbezogene Daten

---
<!-- _class: chapter -->
# Internationale Datenübermittlung (Art. 44 ff.)

---
# Internationale Datenübermittlung (Art. 44 ff.)

Zulässige Übermittlungen:
- Angemessenheitsbeschluss der EU  
- Standardvertragsklauseln  
- Ausnahmen (z. B. Einwilligung)

---

# Safe Harbor (2000–2015)

- Abkommen zwischen EU und USA  
- Selbstzertifizierung von US‑Unternehmen  
- Problem: US‑Überwachungsbefugnisse  
- 2015 vom EuGH („Schrems I“) für ungültig erklärt

---

# EU‑US Privacy Shield (2016–2020)

- Nachfolger von Safe Harbor  
- Strengere Zertifizierung  
- Ombudsperson  
- 2020 vom EuGH („Schrems II“) gekippt  
- Folge: Rechtsunsicherheit für Cloud‑Dienste

---

# EU‑US Data Privacy Framework (seit 2023)

- Dritter Versuch eines transatlantischen Datenschutzabkommens  
- Neue Garantien: Begrenzung von Geheimdienstzugriffen, „Data Protection Review Court“  
- Angemessenheitsbeschluss der EU  
- Kritik: strukturelle Probleme bleiben, weitere Klagen wahrscheinlich

---

# Technische Gesamtanforderungen für IT‑Systeme

- Identifizierbarkeit von Datensätzen (für Auskunft/Löschung)  
- Trennung personenbezogener und pseudonymisierter Daten  
- Rollen- und Berechtigungskonzepte  
- Datenklassifizierung  
- Automatisierte Lösch- und Exportprozesse  
- Vollständige Dokumentation der Datenverarbeitung  
- Logging ohne personenbezogene Daten, wo möglich

---

# Große DSGVO‑Verstöße

- **Meta** (Facebook) – 1,2 Mrd. €  
Unzulässige Datenübermittlung in die USA

- **Amazon** – 746 Mio. €  
Unzulässige personalisierte Werbung

- **WhatsApp** – 225 Mio. €  
Intransparente Datenweitergabe

- **Google** – mehrere hundert Mio. €  
Unklare Einwilligungen

- **British** Airways – 204 Mio. €  
Datenpanne durch unzureichende Sicherheit

---

# Kritik an der DSGVO

## Technische Kritik:
- Unklare Begriffe  
- Hoher Dokumentationsaufwand  
- Komplexe Löschkonzepte  
- Datenminimierung vs. Machine Learning  
- Unsicherheit bei internationalen Datenübermittlungen  
- Logging/IP‑Adressen oft unklar

---

# Kritik an der DSGVO

## Wirtschaftliche Kritik:
- Hohe Compliance‑Kosten  
- Wettbewerbsnachteil gegenüber nicht‑EU‑Unternehmen

## Gesellschaftliche Kritik:
- Uneinheitliche Auslegung  
- Große Unternehmen können Bußgelder einpreisen  
- Kleine Unternehmen überfordert

---

# Fazit

- DSGVO ist technisch anspruchsvoll, aber zentral für moderne IT‑Systeme  
- Internationale Datenübermittlung bleibt ein Problemfeld  
- Anonymisierung & Pseudonymisierung sind Schlüsseltechniken  
- Testdatenmanagement ist ein kritischer Compliance‑Faktor  
- Datenschutz ist ein Architekturthema, kein Add‑on

---




<!-- _class: title -->

# Datenschutz vs. Ermittlungs-befugnisse

---

# Datenschutz vs. Ermittlungsbefugnisse

**Zentrale Frage:**  
Wie balancieren wir *Datenschutz* und *staatliche Ermittlungsinteressen* in einer digitalisierten Gesellschaft?

**Warum relevant?**
- Digitale Spuren sind zentrale Ermittlungsgrundlagen.
- Gleichzeitig wächst die Überwachungskapazität des Staates.
- Datenschutz ist ein Grundrecht – aber kein absolutes.

---

# Beispiel
<!-- class: biglist -->
**Fall:**  
Ein Smartphone eines Terrorverdächtigen wird gefunden.  
Die Polizei fordert Zugriff.  
Der Hersteller verweigert die Entschlüsselung.

**Diskussionsfrage:**  
Soll der Staat Zugriff erzwingen dürfen?

---

# Gesellschaftliche Dimension

- Vertrauen in staatliche Institutionen
- Missbrauchsrisiken (historisch & aktuell)
- Bedeutung von Privatsphäre für Demokratie
- Sicherheitsbedürfnis der Bevölkerung

---

# Technische Dimension

- Verschlüsselung schützt alle – auch Kriminelle
- Hintertüren schwächen Systeme global
- Ermittlungen benötigen digitale Beweise
- Cloud-Infrastrukturen verteilen Daten weltweit

---

# 2. Rechtliche Grundlagen

## Grundrechte

- **Art. 2 GG** – Recht auf informationelle Selbstbestimmung  
- **Art. 10 GG** – Fernmeldegeheimnis  
- **Art. 13 GG** – Unverletzlichkeit der Wohnung (digitale Durchsuchungen?)

**Kernprinzip:**  
Eingriffe nur bei *Gesetzesgrundlage*, *Verhältnismäßigkeit*, *Zweckbindung*.

---

# DSGVO – Relevante Prinzipien

- Datenminimierung  
- Zweckbindung  
- Speicherbegrenzung  
- Integrität & Vertraulichkeit  
- Rechenschaftspflicht

**Konflikt:**  
Ermittlungen benötigen oft *mehr* Daten, DSGVO fordert *weniger*.

---

# Ermittlungsbefugnisse  
## Überblick und Einordnung

Digitale Ermittlungen bewegen sich im Spannungsfeld zwischen  
**Grundrechten** und **staatlicher Sicherheitsvorsorge**.

---

# Strafprozessordnung (StPO)

<!-- class: normal -->
Zentrales Gesetz für strafrechtliche Ermittlungen in Deutschland.

## Was regelt die StPO?
- Voraussetzungen für Eingriffe in Grundrechte  
- Welche Maßnahmen zulässig sind  
- Richterliche Kontrolle und Verhältnismäßigkeit  

## Konflikt
Technische Entwicklungen erzeugen neue Ermittlungsformen, die das Gesetz oft erst nachträglich abbildet.

---

# BKA-Gesetz

## Aufgaben des BKA
- Terrorismusbekämpfung und Gefahrenabwehr  
- Internationale Zusammenarbeit  

## Befugnisse
- Telekommunikationsüberwachung und IT-Systemzugriffe  
- Datenabgleiche mit internationalen Behörden  

## Besonderheit
BKA arbeitet **präventiv** und **repressiv** – dadurch oft tiefere Eingriffe.

---

# Quellen-Telekommunikationsüberwachung

Überwachung verschlüsselter Kommunikation **am Endgerät**, bevor sie verschlüsselt wird.

## Technische Umsetzung
- Installation staatlicher Software („Staatstrojaner“)  
- Zugriff auf Nachrichten vor/ nach der Verschlüsselung  

## Konflikte
- Eingriff in die Integrität des IT-Systems  
- Risiko von Sicherheitslücken  


---

# Online-Durchsuchung (§ 100b StPO)

## Zweck
Fernzugriff auf ein komplettes IT-System ohne Wissen des Betroffenen.

## Zugriff auf
- Dateien  
- Browserhistorie  
- Passwörter  
- Cloud-Zugänge  
- Chatverläufe  

---

# Online-Durchsuchung (§ 100b StPO)

## Konflikte
- Extrem tiefgreifender Eingriff  
- Gefahr staatlicher Malware  
- Kernbereichsschutz besonders relevant

---

# Telekommunikationsüberwachung (TKÜ)

## Zweck
Mithören oder Mitlesen laufender Kommunikation.

## Einsatzbereiche
- Organisierte Kriminalität  
- Terrorismus  
- Schwere Straftaten  
  
---

# Telekommunikationsüberwachung (TKÜ)

## Grenzen
- Nur bei schweren Delikten  
- Richterlicher Beschluss  
- Kernbereich privater Lebensgestaltung geschützt  

## Problem
E2E-Verschlüsselung macht klassische TKÜ weitgehend wirkungslos → Bedarf an Quellen-TKÜ.

---

# Beschlagnahme digitaler Geräte

## Zweck
Sicherstellung digitaler Beweismittel.

## Typische Geräte
- Smartphones  
- Laptops  
- USB-Sticks  
- IoT-Geräte  
- Server  

---

# Beschlagnahme digitaler Geräte
## Maßnahmen
- Physische Sicherstellung  
- Forensisches Imaging  
- Auswertung durch digitale Forensik  

## Konflikte
- Geräte enthalten große Mengen privater Daten  
- Verschlüsselung erschwert Zugriff  
- Cloud-Daten rechtlich komplex

---

# Warum Technik schneller ist als Gesetze

## Strukturelles Problem
- Neue Kommunikationsformen entstehen schneller als Gesetzgebung.  
- Ermittlungsbehörden fordern neue Befugnisse.  
- Datenschutzbehörden warnen vor Grundrechtsverletzungen.  
- Gerichte kassieren regelmäßig überzogene Gesetze.  

## Ergebnis
Ein permanentes Spannungsfeld zwischen  
**Sicherheitsinteressen** und **informationeller Selbstbestimmung**.

---

# Technische Grundlagen & Konflikte

## Ende-zu-Ende-Verschlüsselung

- Nur Sender und Empfänger können Inhalte lesen.
- Anbieter *kann* nicht entschlüsseln.
- Ermittler fordern „Zugänge“ – technisch kaum möglich ohne Schwächung.

---

# Hintertüren

**Definition:**  
Absichtlich eingebaute Möglichkeit, Verschlüsselung zu umgehen.

**Risiken:**
- Missbrauch durch Angreifer
- Missbrauch durch Staaten
- Verlust globaler IT-Sicherheit
- Vertrauensverlust in digitale Dienste

---

# Metadaten

**Inhalte vs. Metadaten**

- Inhalte: Nachrichten, Dateien, Gespräche  
- Metadaten: Wer? Wann? Wo? Wie oft? Mit wem?

**Wichtig:**  
Metadaten sind oft aussagekräftiger als Inhalte.

---

# Cloud-Infrastrukturen

- Daten liegen verteilt über Länder und Jurisdiktionen
- Ermittlungszugriffe oft über internationale Abkommen
- Konflikt zwischen nationalem Recht und globalen Diensten

**Beispiel:**  
US-Behörden fordern Zugriff auf Daten europäischer Nutzer.

---

# Apple vs. FBI (San Bernardino)  
## Hintergrund des Falls

**Ereignis (2015):**  
Terroranschlag in San Bernardino, Kalifornien.  
Ein Täter hinterlässt ein gesperrtes iPhone 5C.

**Forderung des FBI:**  
Apple soll eine spezielle iOS-Version entwickeln, die:
- die PIN‑Sperre entschärft  
- die Auto‑Löschfunktion deaktiviert  
- Brute‑Force‑Versuche ermöglicht  

---

# Apple vs. FBI (San Bernardino)  

## Kernproblem:

Apple müsste eine **generelle Schwächung** der iPhone‑Sicherheitsarchitektur schaffen.

---

# Warum der Konflikt eskalierte  

**FBI-Argumente:**
- Zugriff notwendig zur Terrorismusaufklärung  
- „Einmalige Sonderlösung“  
- Nationale Sicherheit hat Vorrang  

**Apple-Argumente:**
- Hintertüren schwächen *alle* Geräte  
- Präzedenzfall für globale Überwachung  
- Vertrauensverlust in Sicherheitstechnologien  
- Gefahr staatlicher und krimineller Ausnutzung  

---

# Positionen von Apple und FBI  

## Zentrale Frage:
Kann man eine Hintertür bauen, die nur „die Guten“ nutzen?

---

# Apple vs. FBI – Wie der Konflikt endete

- Das FBI zog seine Forderung an Apple zurück.  
- Eine externe Firma knackte das iPhone 5C über eine Sicherheitslücke.  
- Apple musste **keine Hintertür** entwickeln und keinen Präzedenzfall schaffen.  
- Die gefundene Methode funktionierte nur für dieses Modell (ohne Secure Enclave).  

## Bedeutung
- Der Rechtsstreit wurde gegenstandslos – ohne Urteil.  
- Apple verhinderte eine rechtliche Verpflichtung zur Schwächung seiner Sicherheitsarchitektur.  
- Der Fall zeigte, dass Verschlüsselung auch für Staaten schwer zu brechen ist.

---

# Folgen für IT-Sicherheit und Politik

## Technische Konsequenzen
- Apple verstärkte Sicherheitsmechanismen (Secure Enclave, Hardware‑Keys).  
- Hersteller setzen stärker auf „Secure by Default“.  
- Hintertüren gelten seither als globales Sicherheitsrisiko.

## Politische Konsequenzen
- Die Debatte um „Lawful Access“ wurde weltweit intensiviert.  
- Der Fall zeigt bis heute das Grundproblem:  
  **Zugänge für Ermittler schwächen immer auch die Sicherheit aller Nutzer.**  
- Keine technische Lösung existiert, die nur „den Guten“ Zugang gewährt.

---

# Messenger-Dienste & Verschlüsselung

- WhatsApp, Signal, Threema: E2E-Verschlüsselung
- Staaten fordern „Generalschlüssel“
- Technische Community warnt vor globalen Risiken

**Dilemma:**  
Mehr Sicherheit für Bürger = weniger Zugriff für Ermittler.

---

<!-- _class: chapter -->

# Vorratsdatenspeicherung  
## Einordnung, Technik, Rechtsprechung, Kontroversen

---

# Was ist Vorratsdatenspeicherung?

## Grundidee
Pflicht für Telekommunikationsanbieter, **Verkehrs- und Standortdaten** aller Bürger*innen **auf Vorrat** zu speichern – unabhängig von Verdacht oder Anlass.

## Ziel
Ermittlungsbehörden sollen im Nachhinein auf Daten zugreifen können, um:
- Straftaten aufzuklären  
- Netzwerke zu analysieren  
- Bewegungsprofile zu erstellen  

---

# Welche Daten wären betroffen?

## Verkehrsdaten
- Wer kommuniziert mit wem?  
- Wann und wie lange?  
- Von welchem Anschluss?  

## Standortdaten
- Funkzellenprotokolle  
- Bewegungsprofile  
- Aufenthaltsorte über längere Zeiträume  

---

# Warum ist das problematisch?

## Grundrechtseingriffe
- Eingriff in das Recht auf informationelle Selbstbestimmung  
- Gefahr umfassender Bewegungs- und Kontaktprofile  
- „Chilling Effects“: Menschen ändern Verhalten bei Überwachung

## Technische Risiken
- Große Datenmengen → attraktives Ziel für Angriffe  
- Missbrauchspotenzial durch Behörden oder Dritte  

---

# Historie der Vorratsdatenspeicherung in Deutschland

## 2007
Erste Umsetzung der EU-Richtlinie zur Vorratsdatenspeicherung.

## 2010
Bundesverfassungsgericht erklärt das Gesetz für **verfassungswidrig**.

## 2015
Neues Gesetz („Höchstspeicherfristen“) verabschiedet.

---

# Historie der Vorratsdatenspeicherung in Deutschland

## 2017–2022
Mehrere Gerichte setzen das Gesetz aus.

## 2022
EuGH erklärt die deutsche Regelung für **unvereinbar mit EU-Recht**.

---

# EuGH-Entscheidungen zur Vorratsdatenspeicherung

## Kernaussagen
- **Generelle und unterschiedslose Speicherung** ist unzulässig.  
- Speicherung darf nur erfolgen bei:
  - konkreter Bedrohung der nationalen Sicherheit  
  - gezielter Speicherung bestimmter Personengruppen  
  - Speicherung bestimmter geografischer Gebiete bei akuter Gefahr  

## Bedeutung
Die EU setzt enge Grenzen – nationale Gesetze müssen sich daran orientieren.

---

# Argumente der Befürworter

- Ermittlungen benötigen historische Daten  
- Aufklärung schwerer Straftaten (Terrorismus, Kindesmissbrauch)  
- Telekommunikationsdaten oft einzige Spur  
- Internationale Zusammenarbeit erfordert Datenverfügbarkeit  

---

# Argumente der Gegner

- Unverhältnismäßiger Eingriff in Grundrechte  
- Speicherung betrifft **alle**, nicht nur Verdächtige  
- Missbrauchsrisiken und Datenlecks  
- Effektivität umstritten  
- Gefahr der schleichenden Ausweitung („Function Creep“)

---

# Technische Perspektive

## Herausforderungen
- Speicherung großer Datenmengen  
- Sicherung gegen Angriffe  
- Zugriffskontrollen und Protokollierung  
- Trennung von Inhalts- und Verkehrsdaten  

## Risiko
Je größer der Datensatz, desto größer der Schaden bei Kompromittierung.

---

# Alternative Modelle

## Quick Freeze
- Daten werden **nicht** auf Vorrat gespeichert  
- Erst bei konkretem Verdacht werden Daten „eingefroren“  
- Zugriff nur mit richterlicher Anordnung  

## Gezielte Speicherung
- Nur bestimmte Personengruppen oder Orte  
- Beispiel: Funkzellenabfrage bei konkretem Ereignis  

---

# Aktuelle Lage in Deutschland

## Stand
- Vorratsdatenspeicherung in der bisherigen Form ist **ausgesetzt**.  
- Bundesregierung diskutiert Alternativen (z. B. Quick Freeze).  
- EuGH setzt weiterhin enge Grenzen.

## Bedeutung für IT-Sicherheit
- Unternehmen müssen technische Infrastruktur bereitstellen  
- Datenschutzanforderungen bleiben hoch  
- Konflikt zwischen Sicherheit und Privatsphäre bleibt bestehen

---

# Zusammenfassung

- Vorratsdatenspeicherung ist ein tiefgreifender Eingriff in Grundrechte.  
- EuGH und BVerfG haben enge Grenzen gesetzt.  
- Technisch und gesellschaftlich hoch umstritten.  
- Alternative Modelle wie Quick Freeze gewinnen an Bedeutung.  
- Das Spannungsfeld zwischen Sicherheit und Freiheit bleibt bestehen.


