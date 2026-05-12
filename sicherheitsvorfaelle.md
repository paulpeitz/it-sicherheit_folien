---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
paginate: true
---

<!-- _class: title -->
# Zeitreise durch die IT-Security

---

<!-- _class: chapter -->
# Die Anfänge: 1960er–1980er

---
<!-- _class: huge -->
# US-Atomwaffen-Codes: „00000000" (1962–1977)

**Der Hintergrund:**
- US-Minuteman-ICBMs erhielten **PAL-Codes** (Permissive Action Links)
- Sicherheitsmechanismus: Nur mit korrektem 8-stelligem Code startbar
- Vom Verteidigungsministerium verordnet unter Präsident Kennedy

---

# US-Atomwaffen-Codes: „00000000" (1962–1977)

**Das Problem:**
- Die **Strategic Air Command** (SAC) befürchtete, dass komplexe Codes im Ernstfall den Start **verzögern** könnten
- Lösung: Alle PAL-Codes wurden auf **00000000** gesetzt
- **15 Jahre lang** – bis 1977
- Dokumentiert von Bruce Blair (ehemaliger Minuteman-Offizier)

> **Lesson:** Security vs. Usability – wenn Sicherheit als Hindernis empfunden wird, wird sie umgangen.

---

# Kevin Mitnick – Der berühmteste Hacker der Welt

**Die Person:**
- **Kevin David Mitnick** (1963–2023)
- Begann mit **Phone Phreaking** als Teenager
- Meister des **Social Engineering**

**Die Taten (1980er–1995):**
- Hack von Pacific Bell, Nokia, Motorola, Sun Microsystems, Fujitsu
- Stahl Source Code, interne Memos, Passwörter
- Hauptwaffe: **Nicht Technik, sondern Überredungskunst**
- Rief einfach an und überzeugte Mitarbeiter, ihm Zugangsdaten zu geben

---

# Kevin Mitnick – Der berühmteste Hacker der Welt

**Die Jagd:**
- 2,5 Jahre auf der Flucht vor dem FBI
- Verhaftet am 15. Februar 1995

**Die Strafe:**
- **5 Jahre Haft** (damals längste Haftstrafe für Cybercrime in den USA)
- 8 Monate Einzelhaft – das FBI glaubte, er könne *„einen Atomkrieg auslösen, indem er in ein Telefon pfeift"*

**Danach:**
- Wurde **Security-Berater** und Buchautor
- *„The Art of Deception"* – Standardwerk für Social Engineering

---

# Morris Worm (2. November 1988)

**Der Täter:**
- **Robert Tappan Morris** (23)
- Doktorand an der Cornell University
- Sohn eines NSA-Kryptographie-Wissenschaftlers

**Die Motivation:**
- Wollte „nur die Größe des Internets messen"

**Das Problem:**
- Bug im Re-Infektionsmechanismus
- Sollte sich selbst limitieren → tat es nicht
- ~6.000 Rechner infiziert (**~10% des damaligen Internets**)

---

# Morris Worm (2. November 1988)

**Die Technik:**
- `sendmail` Debug-Modus-Bug
- `fingerd` Buffer Overflow
- Wörterbuch-Angriff auf schwache Passwörter
- RSH-Trust-Exploitation

**Die Folgen:**
- Erstes Urteil unter dem **Computer Fraud and Abuse Act** (1986)
- 3 Jahre Bewährung, 400h Sozialarbeit, $10.000 Strafe
- Gründung des **CERT/CC** als direkte Reaktion
- Morris wurde später **MIT-Professor** und Mitgründer von **Y Combinator**

---

# AIDS Trojan – Die erste Ransomware (1989)

**Der Täter:**
- **Dr. Joseph Popp** – Evolutionsbiologe, Harvard-Absolvent

**Die Methode:**
- Verschickte **20.000 Disketten** per Post an Teilnehmer einer WHO-AIDS-Konferenz
- Beschriftung: *„AIDS Information – Introductory Diskette"*
- Nach **90 Neustarts**: Dateien verschlüsselt, Erpressungsnachricht

---

# AIDS Trojan – Die erste Ransomware

**Die Forderung:**
- $189 für eine „Jahreslizenz", $378 für eine „Lebenslizenz"
- Zu zahlen an ein **Postfach in Panama**

**Technisch:**
- Symmetrische Verschlüsselung (schwach)
- Konnte relativ leicht rückgängig gemacht werden

**Die Folgen:**
- Popp wurde verhaftet, aber für **verhandlungsunfähig** erklärt
- Die Idee „Daten verschlüsseln + Lösegeld fordern" war geboren

---

<!-- _class: chapter -->
# Die 1990er: Das Internet wird gefährlich

---

# Kevin Poulsen – „Dark Dante" (1990)

**Die Tat:**
- KIIS-FM (Los Angeles) verlost einen **Porsche 944 S2** an den 102. Anrufer
- Poulsen übernimmt **alle 25 Telefonleitungen** des Radiosenders
- Blockiert alle anderen Anrufer → gewinnt den Porsche

**Weitere Hacks:**
- Kompromittierte FBI-Datenbanken
- Hackte sich in die Telefonüberwachung des FBI → konnte sehen, **wer abgehört wird**
- 17 Monate auf der FBI-Fahndungsliste

---

# Kevin Poulsen – „Dark Dante" (1990)

**Die Folgen:**
- **5 Jahre Haft** + Verbot, Computer 3 Jahre lang zu benutzen
- Heute: **Chefredakteur bei WIRED** und angesehener Technik-Journalist

> Vom FBI-Meistgesuchten zum WIRED-Chefredakteur.


---

<!-- _class: chapter -->
# Die 2000er: Hacker werden professionell

---
<!-- _class: small -->
# UFO-Jäger in US-Militärsystemen (2001–2002)

**Die Person:**
- Britischer Systemadministrator aus London
- **Asperger-Diagnose** (später im Verfahren relevant)

**Die Motivation:**
- Überzeugt, dass die US-Regierung **UFO-Technologie & freie Energie** versteckt
- *„Ich suchte nach Beweisen für die Unterdrückung von Anti-Schwerkraft-Technologie."*

**Die Taten:**
- **97 US-Militär- und NASA-Computer** kompromittiert
- 14 Monate lang (Feb 2001 – März 2002)
- Methode: Suchte nach Windows-Rechnern **ohne Passwort** (Blank Admin Passwords)

---
<!-- _class: small -->
# UFO-Jäger in US-Militärsystemen (2001–2002)

**Was er fand:**
- Angeblich ein Foto eines UFOs auf einem NASA-JPL-Rechner
- *„Es war ein zigarrenförmiges Objekt mit geodätischen Kuppeln"*
- Konnte das Bild nicht downloaden – zu langsame Verbindung
- Hat **keinen Screenshot** gemacht

**Die Folgen:**
- USA forderten **Auslieferung** → drohten 60+ Jahre Haft
- 10 Jahre dauernder Rechtsstreit
- 2012: Britische Regierung **blockierte Auslieferung** (gesundheitliche Gründe)
- Der „größte militärische Computer-Hack aller Zeiten" (US-Ankläger) blieb unbestraft

---

# Estnische Cyberangriffe (April–Mai 2007)
<!-- _class: small -->
**Der Auslöser:**
- Estland verlegt ein sowjetisches Kriegsdenkmal aus dem Stadtzentrum von Tallinn
- Russland ist empört

**Der Angriff:**
- **Massive DDoS-Wellen** über 3 Wochen
- Ziele: Parlament, Ministerien, Banken, Medien, ISPs
- Estland (als stark digitalisiertes Land) war besonders verwundbar
- **Online-Banking** offline, **Regierungswebsites** nicht erreichbar

**Die Folgen:**
- Erster **Cyberangriff gegen einen ganzen Staat**
- NATO eröffnet **Cooperative Cyber Defence Centre of Excellence (CCDCOE)** in Tallinn
- Estland wird zum **Vorreiter für Cyber Defence** in Europa


---

# HMRC Datenverlust (Oktober 2007)

**Das Desaster:**
- **Her Majesty's Revenue and Customs** (britische Steuerbehörde)
- Ein Junior-Beamter brennt **2 CDs** mit Kindergeld-Daten
- Daten von **25 Millionen Personen** (fast die Hälfte der britischen Bevölkerung)
- Inhalt: Namen, Adressen, Geburtsdaten, Sozialversicherungsnummern, Bankdaten

**Der Versand:**
- Verschickt per **interner Post** (TNT) – nicht per Kurier, nicht verschlüsselt
- Die CDs **kamen nie an**
- Sind bis heute verschollen

**Die Folgen:**
- HMRC-Chef tritt zurück
- Regierungskrise in Großbritannien
- Millionen Briten mussten ihre Bankkonten überwachen
- Anstoß für strengere **Data-Handling-Vorschriften** im öffentlichen Sektor

> 25 Millionen Datensätze. 2 CDs. Unverschlüsselt. Per Post. Verloren.

---

# Sensible Daten auf gebrauchten Festplatten

**Studie: University of Hertfordshire / Blancco (2019)**
- **159 gebrauchte Festplatten** auf eBay und anderen Plattformen gekauft
- Ergebnis: nur **26%** waren korrekt gelöscht
- Auf **42%** waren Daten leicht wiederherstellbar
- Gefunden u.a.:
  - 🏥 Medizinische Patientendaten
  - 📄 Bewerbungsunterlagen mit Pässen
  - 💰 Steuererklärungen und Gehaltsabrechnungen
  - 🏢 Interne Unternehmensdaten
  - 📸 Privatfotos und persönliche Dokumente

---

# Sensible Daten auf gebrauchten Festplatten

**Studie: BLR Group / Ontrack (2022)**
- **100 gebrauchte Datenträger** auf eBay gekauft
- **42%** enthielten noch sensible Daten
- Darunter: Daten eines **Frachttransport-Unternehmens**, eines **Software-Entwicklers** und einer **Musikschule**

**Bundeswehr-Laptop auf eBay (2024)**
- Gebrauchter **Bundeswehr-Laptop** auf eBay für wenige Euro verkauft
- Käufer fand darauf **vertrauliche Dokumente** zum Raketenabwehrsystem **LEFLASYS**
- Festplatte war **nicht gelöscht** worden
- Bundeswehr bestätigte den Vorfall



---

# Warum passiert das immer wieder?

**Das Grundproblem:**
- „Datei löschen" = nur Verweis wird entfernt
- „Festplatte formatieren" (Schnellformatierung) = nur Dateisystem-Tabelle gelöscht
- **Daten bleiben physisch auf der Platte** → mit Tools wie **Autopsy**, **Recuva** oder **PhotoRec** trivial wiederherstellbar

**Wer ist betroffen?**
- Privatpersonen (Unwissenheit)
- Unternehmen (fehlende Prozesse)
- Behörden und Militär (erschreckend häufig)

---
<!-- _class: small -->
# Warum passiert das immer wieder?

**Korrekte Entsorgung:**
- **Überschreiben:** Gesamte Platte mit Zufallsdaten überschreiben (1× reicht bei modernen HDDs)
- **Kryptographisches Löschen:** Selbstverschlüsselnde SSDs → Schlüssel löschen
- **Degaussing:** Magnetisches Löschen bei HDDs
- **Physische Vernichtung:** Schreddern, Bohren, Einschmelzen
- **Zertifizierte Dienstleister:** BSI-konforme Datenträgervernichtung

**Relevante Normen:**
- **DIN 66399** – Vernichtung von Datenträgern (Schutzklassen 1–3, Sicherheitsstufen 1–7)
- **NIST SP 800-88** – Guidelines for Media Sanitization

---

# TJX – 94 Millionen Kreditkarten über offenes WLAN

**Das Ziel:**
- 2007 - TJX Companies (T.J. Maxx, Marshalls, HomeGoods)
- Einer der größten Einzelhändler der USA

**Der Angriffsweg:**
- Hacker **Albert Gonzalez** und Team
- Einstiegspunkt: **unverschlüsseltes WLAN** in einer Filiale in Miami
- WEP-Verschlüsselung (damals bereits geknackt)
- Vom WLAN → ins Firmennetzwerk → zu den Zahlungssystemen

---

# TJX – 94 Millionen Kreditkarten über offenes WLAN

**Der Schaden:**
- **94 Millionen** Kredit- und Debitkartendaten gestohlen
- Über einen Zeitraum von **18 Monaten**
- Gesamtschaden: **$256 Millionen**

**Die Folgen:**
- Gonzalez: **20 Jahre Haft** (damals längste Strafe für Cybercrime)
- Beschleunigte die Einführung von **PCI DSS** (Payment Card Industry Data Security Standard)
- WEP wurde endgültig als unsicher gebrandmarkt → WPA2 wurde Pflicht

---

<!-- _class: chapter -->
# Staatliche Cyberwaffen

---

# Shamoon – Saudi Aramco (15. August 2012)

**Das Ziel:**
- **Saudi Aramco** – wertvollstes Unternehmen der Welt
- Angriff am Vorabend eines Feiertags (Ramadan)

**Was passierte:**
- Malware „Shamoon" verbreitete sich im Netzwerk
- Um 11:08 Uhr Ortszeit: **Gleichzeitige Aktivierung**
- **35.000 Computer** wurden gelöscht
- Master Boot Record überschrieben mit dem Bild einer **brennenden US-Flagge**

---

# Shamoon – Saudi Aramco (15. August 2012)

**Die Folgen:**
- Aramco war **2 Wochen offline**
- Mitarbeiter arbeiteten mit **Fax und Papier**
- Aramco kaufte weltweit **50.000 Festplatten** auf → globale Preise stiegen
- Ölproduktion selbst nicht betroffen (IT/OT-Trennung!)

**Zuschreibung:**
- Iran (mutmaßlich Vergeltung für Stuxnet)
- Gruppe: „Cutting Sword of Justice"

**Lesson:** IT/OT-Trennung hat funktioniert – die Ölproduktion lief weiter. Aber die IT-Infrastruktur war nicht wiederherstellbar.

---

# Snowden NSA Leaks (Juni 2013)

- **Edward Snowden** (29), NSA-Contractor (Booz Allen Hamilton)
- Systemadministrator mit Top-Secret-Clearance
- Arbeitete an der NSA-Außenstelle in Hawaii

**Was er enthüllte:**
- **PRISM:** Direktzugriff auf Server von Google, Apple, Facebook, Microsoft, Yahoo
- **XKeyscore:** Echtzeit-Durchsuchung aller Internet-Aktivitäten
- **Tempora (GCHQ):** Anzapfen von Unterseekabeln
- **Bullrun:** NSA untergräbt Verschlüsselungsstandards
- Überwachung von Staatschefs (u.a. Angela Merkels Handy)

---

# Snowden NSA Leaks (Juni 2013)

**Wie er es tat:**
- Sammelte Dokumente über Monate
- Nutzte seinen **Admin-Zugang** (Principle of Least Privilege versagt)
- Flog nach **Hongkong** → übergab Dokumente an Journalisten
- Floh nach **Moskau** (lebt dort bis heute)

**Die Folgen:**
- **Größte Enthüllung** in der Geschichte der Geheimdienste
- Beschleunigte **Verschlüsselung** im Internet (HTTPS Everywhere)
- Reform der NSA-Überwachung (USA FREEDOM Act 2015)
- EU-US Safe Harbor für ungültig erklärt (→ Schrems I)

---

# Sony Pictures Hack (November 2014)
<!-- _class: small -->
**Der Auslöser:**
- Sony produzierte den Film **„The Interview"**
- Komödie über ein Attentat auf Kim Jong-un
- Nordkorea: *„Ein Akt des Krieges"*

**Der Angriff (Lazarus Group):**
- **100 TB Daten** gestohlen
- Unveröffentlichte Filme geleakt (u.a. „Fury", „Annie")
- Interne E-Mails veröffentlicht (peinliche Kommentare über Schauspieler)
- Gehaltstabellen aller Mitarbeiter öffentlich
- Systeme mit Wiper-Malware zerstört

---

# Sony Pictures Hack (November 2014)
<!-- _class: small -->
**Die Forderung:**
- Film nicht veröffentlichen – oder es wird schlimmer

**Die Folgen:**
- Kinos sagten Premiere ab → Sony zog Film zurück
- **Obama:** *„Das war ein Fehler"* → Sony veröffentlichte doch (digital)
- FBI bestätigte: **Nordkorea** verantwortlich
- US-Sanktionen gegen Nordkorea verschärft
- Sony-CEO Amy Pascal trat zurück

**Die Ironie:**
- Ein **mittelmäßiger Kinofilm** wurde durch den Hack zum kulturellen Ereignis
- Ohne den Hack hätte den Film kaum jemand gesehen

---

# Ukraine Power Grid – Erster Cyberangriff auf ein Stromnet


**23. Dezember 2015:**
- Angreifer übernehmen SCADA-Systeme von 3 ukrainischen Energieversorgern
- **Schalten ferngesteuert 30 Umspannwerke ab**
- **230.000 Menschen** ohne Strom – mitten im Winter
- Gleichzeitig: **DDoS auf Telefon-Hotlines** → Kunden können Störung nicht melden

**Dauer:** 1–6 Stunden (manuelles Wiedereinschalten)

---
<!-- _class: small -->
# Ukraine Power Grid – Erster Cyberangriff auf ein Stromnet

**17. Dezember 2016 (CrashOverride/Industroyer):**
- Erneuter Angriff, diesmal auf Übertragungsnetz in Kiew
- **Automatisierte Malware** statt manueller Steuerung
- Speziell für **ICS/SCADA-Protokolle** entwickelt (IEC 104, IEC 61850)

**Zuschreibung:** Russland (Sandworm / GRU Unit 74455)

**Bedeutung:**
- Erster **bestätigter Cyberangriff auf ein Stromnetz**
- Bewies: **IT-Angriffe können physische Infrastruktur lahmlegen**
- 2016 war technisch ausgereifter → die Bedrohung wächst

---

# NotPetya (27. Juni 2017)

**Getarnt als Ransomware. Tatsächlich: Russischer Cyberangriff auf die Ukraine.**

**Einfallstor – Supply-Chain-Angriff:**
- **M.E.Doc** – ukrainische Steuersoftware (Pflicht für Unternehmen in der Ukraine)
- Angreifer kompromittierten den **Update-Server** von M.E.Doc
- Automatisches Software-Update → **Malware auf jedem Rechner mit M.E.Doc**

**Technisch:**
- Nutzte **EternalBlue** (gleicher NSA-Exploit wie WannaCry) + **Mimikatz** für Credential Harvesting
- Verschlüsselung war **irreversibel** – kein funktionierender Entschlüsselungsschlüssel
- Nicht Ransomware, sondern **Wiper** (Zerstörungstool)

---

# NotPetya – Globaler Kollateralschaden

<style scoped>
table { font-size: 14pt; }
</style>

**Geschätzter Gesamtschaden: ~10 Milliarden USD**

| Unternehmen | Schaden | Details |
|-------------|---------|---------|
| **Maersk** (Reederei) | ~$300 Mio | 45.000 PCs + 4.000 Server komplett neuinstalliert. 10 Tage lang größte Reederei der Welt **blind**. |
| **Merck** (Pharma) | ~$870 Mio | Produktionsausfälle bei Impfstoffen |
| **FedEx/TNT** | ~$400 Mio | TNT Express monatelang beeinträchtigt |
| **Mondelez** (Lebensmittel) | ~$188 Mio | 1.700 Server, 24.000 Laptops zerstört |
| **Saint-Gobain** (Bau) | ~$384 Mio | IT-Systeme weltweit betroffen |

**Die Ironie:** Keine dieser Firmen war das eigentliche Ziel. Sie hatten lediglich **Niederlassungen in der Ukraine** und nutzten M.E.Doc.


---

<!-- _class: chapter -->
# IoT & kuriose Angriffsvektoren

---
<!-- _class: small -->
# Smart-Kühlschrank verschickt Spam (2014)

<div class="columns">
<div>

**Was passiert ist:**
- Sicherheitsfirma Proofpoint entdeckt **IoT-Botnetz**
  - 🧊 Kühlschränke
  - 📺 Smart-TVs
  - 📱 Media-Center
  - 🏠 Heimrouter

**Ergebnis:**
- **750.000 Spam- und Phishing-Mails** verschickt
- In Wellen von je 100.000 Mails
- Kein einzelnes Gerät schickte mehr als 10 Mails

</div>
<div>

**Warum das möglich war:**
- Standard-Passwörter nie geändert
- Keine Firmware-Updates
- Geräte 24/7 online
- Niemand überwacht den Traffic eines Kühlschranks

**Die Ironie:**
- Der Kühlschrank hat eine E-Mail-Funktion, damit er den Besitzer benachrichtigen kann, wenn die Milch leer ist
- Stattdessen verschickt er jetzt **Viagra-Werbung**

</div>
</div>

---

# Jeep Cherokee Remote Hack (2015)

**Charlie Miller & Chris Valasek** übernahmen ein fahrendes Auto – **ferngesteuert**.
Ein Wired-Journalist saß am Steuer. Auf der Autobahn.

**Die Kill Chain:**

```
Mobilfunknetz (Sprint)
  → Uconnect Infotainment-System (offener Port 6667)
    → D-Bus Service
      → CAN-Bus Gateway
        → Steuergeräte: Lenkung, Bremsen, Getriebe, Motor
```

---

# Jeep Cherokee Remote Hack (2015)


**Was die Hacker kontrollierten:**
- 🌡️ Klimaanlage, 📻 Radio (Lautstärke Maximum), 🖥️ Display
- 🔧 Getriebe (Neutralgang während der Fahrt)
- 🛞 Lenkung (bei niedrigen Geschwindigkeiten)
- 🛑 **Bremsen deaktiviert**

**Folge:** Chrysler rief **1,4 Millionen Fahrzeuge** zurück.


---

# Casino-Hack über ein Aquarium-Thermometer (2017)


**Was passiert ist:**
- Nordamerikanisches Casino wurde gehackt
- Einstiegspunkt: **smartes Aquarium-Thermometer** in der Lobby
- Thermometer war mit dem Casino-Netzwerk verbunden
- Angreifer bewegten sich lateral durch das Netzwerk
- **High-Roller-Kundendatenbank** wurde exfiltriert

---

# Casino-Hack über ein Aquarium-Thermometer (2017)

**Wie die Daten rausgingen:**
- Über das Thermometer zurück ins Internet
- Daten wurden an einen Server in **Finnland** gesendet

**Warum das funktionierte:**
- ❌ Keine Netzwerksegmentierung
- ❌ IoT-Gerät im selben Netz wie kritische Daten
- ❌ Kein Monitoring des ausgehenden Traffics

---

# Strava Heatmap enthüllt Militärbasen (Januar 2018)



**Was passiert ist:**
- Fitness-App **Strava** veröffentlicht globale **Heatmap** aller Nutzer-Aktivitäten
- 1 Milliarde Aktivitäten, 10 TB GPS-Daten
- Gedacht als Marketing für die Fitness-Community

**Das Problem:**
- In Kriegsgebieten (Syrien, Irak, Afghanistan) leuchteten **isolierte Strukturen** auf
- Soldaten joggten mit Fitness-Trackern um **geheime Militärbasen**
- Lauf- und Patrouillenrouten klar erkennbar
  
---

# Strava Heatmap enthüllt Militärbasen (Januar 2018)

**Enthüllt wurden u.a.:**
- Geheime US-Basen in Syrien
- Patrouillenrouten in Afghanistan
- CIA-Einrichtungen („Black Sites")
- Französische Militärbasen in Afrika
- Türkische Stützpunkte

**Folge:**
- Pentagon verbot Fitness-Tracker in Einsatzgebieten
- Strava fügte Opt-Out-Funktionen hinzu
- Wurde zum Lehrbuchbeispiel für **OPSEC-Versagen**


--- 

<!-- _class: chapter -->
# Die Ransomware-Ära

---

# WannaCry & der zufällige Held (Mai 2017)

<div class="columns">
<div>

**WannaCry-Ransomware:**
- Nutzte **EternalBlue** (NSA-Exploit, geleakt von Shadow Brokers)
- **>200.000 Systeme** in **150 Ländern** infiziert
- NHS (UK): Krankenhäuser geschlossen, OPs abgesagt
- Schaden: geschätzt **$4–8 Milliarden**
- Zugeschrieben: **Nordkorea** (Lazarus Group)
  
</div>
<div>

**Der Kill Switch:**
- Im Code: Malware prüft Domain `iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com`
- Falls Domain existiert → **Malware stoppt sofort**
- Die Domain war **nicht registriert**

</div>
</div>

---

# WannaCry & der zufällige Held (Mai 2017)

<div class="columns">
<div>

**Marcus Hutchins (22):**
- Britischer Sicherheitsforscher (MalwareTechBlog)
- Fand die Domain → registrierte sie für **$10,69**
- → **WannaCry stoppte weltweit**

</div>
<div>

**Die dunkle Wendung (August 2017):**
- Hutchins reiste zur DEF CON
- Wurde am Flughafen vom **FBI verhaftet**
- Anklage: Entwicklung von **Kronos Banking Trojan** als Teenager (2014/2015)
- Urteil: **Keine Gefängnisstrafe** (Time served + Bewährung)

</div>
</div>

---

# Uniklinik Düsseldorf – Tod durch Ransomware? (September 2020)

<div class="columns">
<div>

**Was passierte:**
- **DoppelPaymer-Ransomware** trifft Uniklinik Düsseldorf
- 30 Server verschlüsselt
- Krankenhaus muss **Notaufnahme schließen**
- Rettungswagen werden umgeleitet
 
</div>
<div>

**Der tragische Fall:**
- Eine Patientin mit **Aortenaneurysma** muss nach Wuppertal umgeleitet werden
- Zusätzliche Fahrzeit: ~30 Minuten
- Die Patientin **stirbt**

</div>
</div>

---

# Uniklinik Düsseldorf – Tod durch Ransomware?


**Die Debatte:**
- War dies der **erste Tod durch Ransomware**?
- Staatsanwaltschaft stellte Ermittlungen ein → Patientin wäre möglicherweise auch so gestorben
- Debatte: **Kausalität vs. Verantwortung**

**Kontext:**
- Einfallstor: **Citrix-Schwachstelle** (CVE-2019-19781)
- Patch war seit Januar 2020 verfügbar, Wurde **8 Monate lang nicht eingespielt**

---

<!-- _class: chapter -->
# Mega-Breaches & Datenlecks
## 3 Milliarden Konten, Tippfehler & Supply Chains

---

# Ashley Madison – Der Seitensprung-Leak (Juli 2015)

<div class="columns">
<div>

**Die Plattform:**
- **Ashley Madison** – Dating-Portal für Seitensprünge
- Slogan: *„Life is short. Have an affair."*
- **37 Millionen** Nutzerprofile
  
</div>
<div>

**Der Hack (Impact Team):**
- Stahlen die **gesamte Nutzerdatenbank**
- Forderung: Plattform abschalten
- Ashley Madison weigerte sich
- → **Alle Daten veröffentlicht** (9,7 GB)

</div>
</div>

---

# Ashley Madison – Der Seitensprung-Leak (Juli 2015)

<div class="columns">
<div>

**Inhalt des Leaks:**
- Namen, E-Mails, Kreditkartendaten
- Sexuelle Vorlieben und Chat-Verläufe
- GPS-Koordinaten der Nutzer

</div>
<div>


**Die realen Folgen:**
- **Erpressungswellen** weltweit
- Prominente, Politiker, Militärangehörige geoutet
- Mindestens **2 Suizide** in direktem Zusammenhang
- Millionen-Klagen gegen Betreiber Avid Life Media
- CEO Noel Biderman trat zurück

</div>
</div>

---

# Bangladesh Bank SWIFT Hack (Februar 2016)

<div class="columns">
<div>

**Der Plan:**
- **Lazarus Group** (Nordkorea) hackt Zentralbank von Bangladesh
- Ziel: **$951 Millionen** über SWIFT-Netzwerk stehlen
- 35 betrügerische Überweisungen an die Federal Reserve Bank of New York

</div>
<div>

**Was funktionierte:**
- **$81 Millionen** erfolgreich auf Konten auf den Philippinen transferiert
- Geld wurde dort durch **Casinos gewaschen** (damals von Anti-Geldwäsche-Gesetzen ausgenommen)

</div>
</div>

---

# Bangladesh Bank SWIFT Hack (Februar 2016)


**Was den Rest rettete – ein Tippfehler:**
- Eine Überweisung ging an die **„Shalika Foundation"**
- Sollte heißen: „Shalika **Fandation**"
- Die Routing-Bank (Deutsche Bank) wurde misstrauisch
- Stoppte die Transaktion → benachrichtigte Bangladesh Bank
- **$870 Millionen** wurden rechtzeitig blockiert

**Die Ironie:**
- Das SWIFT-Terminal der Bangladesh Bank: **$10-Netzwerk-Switches**, keine Firewall
- Der größte Bankraub der Geschichte scheiterte an einem **Rechtschreibfehler**


---
<!-- _class: small -->
# xz-utils Backdoor – Social Engineering auf Open Source (März 2024)

<div class="columns">
<div>

**Die Methode:**
- Entwickler **„Jia Tan"** beginnt 2021, zum Open-Source-Projekt **xz-utils** beizutragen
- xz ist eine **Kompressionsbibliothek**, genutzt auf praktisch jedem Linux-System
- 2 Jahre lang: fleißige Beiträge, Code-Reviews, Community-Arbeit
- Gewinnt **Vertrauen** → wird **Co-Maintainer**


</div>
<div>


**Die Backdoor:**
- Versteckte eine **Remote Code Execution** Backdoor in den Build-Dateien
- Zielte auf **OpenSSH** (über systemd) → unautorisierter Root-Zugang
- Wäre in **fast jede Linux-Distribution** gelangt

</div>
</div>

---
<!-- _class: small -->
# xz-utils Backdoor – Social Engineering auf Open Source (März 2024)

<div class="columns">
<div>


**Die Entdeckung – purer Zufall:**
- **Andres Freund** (Microsoft/PostgreSQL-Entwickler)
- Bemerkte, dass SSH-Logins **500ms langsamer** waren
- Untersuchte die Ursache → fand die Backdoor

</div>
<div>


**Die Folgen:**
- CVE-2024-3094 (CVSS **10.0**)
- Riesige Debatte über **Open-Source-Sicherheit**
- Einzelne Maintainer verantwortlich für Software auf Milliarden Geräten
- Mutmaßlich **staatlich gesponsert** (China? Russland?)

</div>
</div>

---


<!-- _class: chapter -->
# Social Engineering & der menschliche Faktor


---
<!--_class: small -->
# Twitter Bitcoin Scam (15. Juli 2020)

**Graham Ivan Clark** (17 Jahre alt, Tampa, Florida)

**Die Methode:**
- Social Engineering gegen Twitter-Mitarbeiter
- Gab sich als IT-Support aus (Telefon)
- Erhielt Zugang zum internen **Admin-Tool**

**Übernommene Accounts:**
- Barack Obama
- Elon Musk
- Jeff Bezos
- Apple
- Bill Gates


---

# Twitter Bitcoin Scam (15. Juli 2020)
<!--_class: small -->
**Nachricht auf allen Accounts:**
> *„I'm giving back to the community. All Bitcoin sent to the address below will be sent back doubled!"*

**Ergebnis:**
- ~$120.000 in Bitcoin erbeutet

**Technisches Problem:**

Twitter Internal Tool ("God Mode"):
- **Kein Vier-Augen-Prinzip** für kritische Aktionen
- **Kein Monitoring** ungewöhnlicher Admin-Aktionen

**Urteil:** 3 Jahre Jugendstrafe

---

# Mat Honan – Kaskaden-Hack (August 2012)

**Der Betroffene:** Tech-Journalist bei **WIRED**

**Die Kaskade – alles per Telefon, ohne technischen Hack:**

<style scoped>
table { font-size: 14pt; }
</style>

| Schritt | Was passierte | Ergebnis |
|---------|---------------|----------|
| 1 | Anruf bei **Amazon**: „Ich möchte eine Kreditkarte hinzufügen" | Letzte 4 Ziffern einer Kreditkarte hinterlegt |
| 2 | Erneuter Anruf bei **Amazon**: „Ich brauche Zugang, meine KK endet auf XXXX" | Zugang zu Honan's Amazon-Konto |
| 3 | Amazon-Konto zeigt **letzte 4 Ziffern der echten Kreditkarte** | 4 Ziffern gewonnen |
| 4 | Anruf bei **Apple**: „Ich brauche Zugang, meine KK endet auf XXXX" | Apple-ID-Passwort zurückgesetzt |
| 5 | Apple-ID → Zugang zu **iCloud** → **Find My iPhone** → **Remote Wipe** | iPhone, iPad, MacBook gelöscht |
| 6 | Über Apple-ID → Zugang zu **Gmail** (Backup-E-Mail) | Google-Konto übernommen |
| 7 | Über Gmail → **Twitter-Passwort** zurückgesetzt | Twitter-Account übernommen |

**Verlust:** Alle Fotos seines Kindes (kein Backup), Karrieredokumente, alle Accounts.

---

<!-- _class: chapter -->
# Datenschutz, Privacy & Recht

---

# Target – Der Laden weiß mehr als der Vater (2012)

<div class="columns">
<div>

**Was passierte:**
- US-Einzelhändler **Target** analysiert Kaufverhalten
- Entwickelt einen **„Schwangerschafts-Score"**
- Basiert auf Kaufmustern: Lotionen, Vitamine, Watte, Zink, Magnesium

</div>
<div>

**Der Vorfall:**
- Vater beschwert sich bei Target-Filialleiter:
- *„Meine Tochter ist noch ein Teenager – warum schicken Sie ihr Coupons für Babykleidung und Kinderbetten?!"*
- Filialleiter entschuldigt sich

</div>
</div>

---

# Target – Der Laden weiß mehr als der Vater (2012)

**Einige Tage später:**
- Filialleiter ruft zurück, um sich erneut zu entschuldigen
- Vater: *„Ich habe mit meiner Tochter gesprochen. Sie ist tatsächlich schwanger. Ich schulde Ihnen eine Entschuldigung."*

**Target wusste es vor dem Vater.**

**Die Lehre:**
- Predictive Analytics auf Kaufverhalten kann **intimste Details** offenlegen
- Target lernte daraus: Babyprodukt-Coupons **zwischen** zufällige Angebote (Rasenmäher, Weingläser) mischen – damit es weniger auffällt



---

# Clearview AI – 2020

<div class="columns">
<div>

**Was Clearview AI tat:**
- Scrapte **über 3 Milliarden Fotos** aus Social Media
- Facebook, Instagram, YouTube, Twitter, Venmo
- Baute eine **Gesichtserkennungs-Datenbank**
- Verkaufte den Zugang an **Strafverfolgungsbehörden**

</div>
<div>

**Wie es funktionierte:**
- Polizist lädt Foto eines Verdächtigen hoch
- Clearview findet **alle öffentlichen Fotos** dieser Person
- Inkl. Name, Links zu Social-Media-Profilen
- Genauigkeit: angeblich **98,6%**

</div>
</div>

---
<!-- _class: small -->
# Clearview AI – Gesichtserkennung ohne Einwilligung


**Die Enthüllung (NYT, Januar 2020):**
- **1.000+ Polizeibehörden** nutzten Clearview AI – heimlich
- FBI, ICE, US Marshals
- Firmengründer **Hoan Ton-That**: keine Sicherheitserfahrung

**Rechtliche Folgen:**
- 🇬🇧 UK: **£7,5 Mio.** Strafe (ICO)
- 🇫🇷 Frankreich: **€20 Mio.** Strafe (CNIL)
- 🇮🇹 Italien: **€20 Mio.** Strafe
- 🇦🇺 Australien: Clearview für **illegal** erklärt
- 🇺🇸 Illinois: Vergleich über **$52 Mio.** (BIPA)

> Du hast nie zugestimmt. Dein Gesicht ist trotzdem in der Datenbank.



---

# Grindr Standortdaten – Priester geoutet (2022)

<!-- _class: small -->

**Der Fall:**
- **The Pillar** (katholisches Nachrichtenmedium) veröffentlichte einen Bericht
- Monsignor **Jeffrey Burrill** (US-Bischofskonferenz) nutzte **Grindr** (LGBTQ+ Dating-App)
- Bewiesen durch **kommerziell gekaufte Standortdaten** der App

**Wie das funktionierte:**
- Grindr verkaufte **anonymisierte** Standortdaten an Datenhändler
- „Anonymisiert" – aber das Gerät wurde **an seiner Wohnung, seinem Büro und im Vatikan** geortet
- → **Re-Identifizierung** trivial
- Burrill trat zurück

---

<!-- _class: title -->

# ENDE