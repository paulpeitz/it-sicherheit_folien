---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
paginate: true
---

<!-- _class: title -->
# WTF?! 
## Kuriose Vorfälle aus IT-Security & Datenschutz

---

<!-- _class: biglist -->
# Agenda

1. Wenn Software über Leben und Tod entscheidet
2. Die Geburt der Cyber-Kriminalität
3. Kuriose Angriffsvektoren: IoT & Physical
4. Staatliche Cyber-Operationen
5. Wenn Security-Firmen versagen
6. Datenschutz-Kuriositäten & Rechtsgeschichte
7. Social Engineering & der menschliche Faktor
8. Kurioses Finale

---

<!-- _class: chapter -->
# 1. Wenn Software über Leben und Tod entscheidet
## Therac-25 & Ariane 5

---

# Therac-25 (1985–1987)

**Was war das?**
- Medizinisches Bestrahlungsgerät für Krebstherapie
- Hersteller: Atomic Energy of Canada Limited (AECL)

**Was ging schief?**
- **Race Condition** zwischen Bediener-Interface und Bestrahlungssteuerung
- Bei schneller Eingabe: Elektronenstrahl ohne Schutzfilter → **100-fache Überdosis**
- 6 Patienten erhielten massive Überbestrahlung → 3 Todesfälle

**Warum?**
- Software-Interlocks ersetzten Hardware-Interlocks (Kosteneinsparung)
- Kein unabhängiger Code-Review
- Fehlermeldung: nur kryptisches „MALFUNCTION 54" → Bediener drückten einfach weiter

---

# Therac-25 – Das Problem im Detail

<style scoped>
table { font-size: 18pt; }
</style>

**Die Race Condition:**

| Schritt | Operator | Maschine |
|---------|----------|----------|
| 1 | Gibt **X-Ray** Modus ein | Fährt Filter in Position |
| 2 | Korrigiert schnell zu **Electron** | Filter noch nicht in Position |
| 3 | Drückt „Start" | Bestrahlt mit voller Leistung **ohne Filter** |

<br>

**Kernproblem:**
- Software prüfte Maschinenstellung nur einmal pro Zyklus
- Vorgängermodelle (Therac-6/20) hatten **Hardware-Interlocks** als Backup
- Therac-25 verließ sich **ausschließlich** auf Software

---

# Ariane 5 – Explosion (4. Juni 1996)

<style scoped>
p { text-align: center; }
</style>

![w:550](img/ariane5_explosion.jpg)

**370 Millionen Euro – 37 Sekunden – 1 Software-Bug**

---

# Ariane 5 – Was passiert ist

**Der Bug:**
```ada
-- Horizontale Geschwindigkeit (64-Bit Float)
-- wird konvertiert in 16-Bit Integer
L_M_BV_32 := TDB.T_ENTIER_16S(TDB.T_VELO_BH.T_M_BV_GH);
-- Maximalwert 16-Bit: 32.768
-- Tatsächlicher Wert: ~64.000 → OVERFLOW!
```

**Der Kontext:**
- Software wurde **unverändert von Ariane 4** übernommen
- Ariane 4 hatte andere Flugbahn → Werte blieben im 16-Bit-Bereich
- Ariane 5 war **schneller** → Werte sprengten den Bereich
- Die Overflow-Exception wurde **nicht abgefangen** 
- Backup-System: **identischer Code** → gleicher Fehler

---

<!-- _class: biglist -->
# Lesson Learned

- **Defense in Depth:** Nie nur auf einen Schutzmechanismus vertrauen
- **Software-Testing:** Grenzwerte und Randbedingungen müssen getestet werden
- **Wiederverwendung ≠ Sicherheit:** Code ist nur sicher im Kontext, für den er getestet wurde
- **Hardware-Interlocks** als unabhängige Sicherheitsebene

---

<!-- _class: chapter -->
# 2. Die Geburt der Cyber-Kriminalität
## Morris Worm & ILOVEYOU

---

# Der Morris Worm (2. November 1988)

<div class="columns">
<div>

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

</div>
<div>

**Die Technik:**
- `sendmail` Debug-Modus-Bug
- `fingerd` Buffer Overflow
- Wörterbuch-Angriff auf schwache Passwörter
- RSH-Trust-Exploitation

**Die Folgen:**
- Erstes Urteil unter dem **Computer Fraud and Abuse Act** (1986)
- 3 Jahre Bewährung, 400h Sozialarbeit, $10.000 Strafe
- Gründung des **CERT/CC** als direkte Reaktion

</div>
</div>

---

# Robert Morris – Ironie des Schicksals

![w:200](img/robert_morris.jpg)

**Karriere nach dem Wurm:**
- **MIT-Professor** für Informatik
- Mitgründer von **Viaweb** (verkauft an Yahoo → wurde Yahoo Store)
- Mitgründer von **Y Combinator** (bedeutendster Startup-Inkubator der Welt)
  - Finanzierte u.a.: Airbnb, Dropbox, Stripe, Reddit

> Vom ersten verurteilten Hacker der Geschichte zum Silicon-Valley-Investor.

---

# ILOVEYOU (Mai 2000)

<!-- _class: biglist -->

Der Wurm, der **10% aller internetfähigen Rechner weltweit** infizierte und geschätzt **5,5–10 Mrd. USD Schaden** verursachte.

➡️ **Ausführlich behandelt in der separaten Vorlesung „I Love You"**

Kurzversion: Ein frustrierter philippinischer Student, eine VBScript-Datei als Liebesbrief getarnt, und ein Betriebssystem, das Dateiendungen versteckte.

---

<!-- _class: chapter -->
# 3. Kuriose Angriffsvektoren
## IoT, Autos & Kühlschränke

---

# Casino-Hack über ein Aquarium-Thermometer (2017)

<div class="columns">
<div>

**Was passiert ist:**
- Nordamerikanisches Casino wurde gehackt
- Einstiegspunkt: **smartes Aquarium-Thermometer** in der Lobby
- Thermometer war mit dem Casino-Netzwerk verbunden
- Angreifer bewegten sich lateral durch das Netzwerk
- **High-Roller-Kundendatenbank** wurde exfiltriert

</div>
<div>

**Wie die Daten rausgingen:**
- Über das Thermometer zurück ins Internet
- Daten wurden an einen Server in **Finnland** gesendet

**Warum das funktionierte:**
- ❌ Keine Netzwerksegmentierung
- ❌ IoT-Gerät im selben Netz wie kritische Daten
- ❌ Kein Monitoring des ausgehenden Traffics

</div>
</div>

<br>

> *„The attackers used the fish tank to get a foothold in the network."*
> — Darktrace CEO Nicole Eagan

---

# Jeep Cherokee Remote Hack (2015)

![w:500](img/jeep_hack_wired.jpg)

**Charlie Miller & Chris Valasek** übernahmen ein fahrendes Auto – **ferngesteuert**.
Ein Wired-Journalist saß am Steuer. Auf der Autobahn.

---

# Jeep Cherokee – Der Angriffsweg

**Die Kill Chain:**

```
Mobilfunknetz (Sprint)
  → Uconnect Infotainment-System (offener Port 6667)
    → D-Bus Service
      → CAN-Bus Gateway
        → Steuergeräte: Lenkung, Bremsen, Getriebe, Motor
```

**Was die Hacker kontrollierten:**
- 🌡️ Klimaanlage (voll aufgedreht)
- 📻 Radio (Lautstärke Maximum)
- 🖥️ Display (eigenes Bild eingeblendet)
- 🔧 Getriebe (Neutralgang während der Fahrt)
- 🛞 Lenkung (bei niedrigen Geschwindigkeiten)
- 🛑 **Bremsen deaktiviert**

**Folge:** Chrysler rief **1,4 Millionen Fahrzeuge** zurück.

---

# Smart-Kühlschrank verschickt Spam (2014)

<div class="columns">
<div>

**Was passiert ist:**
- Sicherheitsfirma Proofpoint entdeckt **IoT-Botnetz**
- Beteiligte Geräte:
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

# Der „Staudamm-Hack" (2013)

**Die Schlagzeilen:**
> *„Iranische Hacker knacken US-Staudamm!"*

**Die Realität:**
- Ziel war die **Bowman Avenue Dam** in Rye Brook, New York
- Klingt bedrohlich – aber…

<div class="columns">
<div>

**Erwartung:**
- 🏗️ Riesiger Staudamm
- 🌊 Überschwemmungsgefahr
- ☢️ Kritische Infrastruktur

</div>
<div>

**Realität:**
- 🚿 Winzige Hochwasserschleuse
- 📏 Etwa so groß wie ein Garagentor
- 🔧 Steuerung war zum Zeitpunkt des Hacks **manuell abgeklemmt**

</div>
</div>

<br>

**Lesson:** Medienberichterstattung ≠ tatsächliche Bedrohungslage. Aber: Das **Prinzip** war real – kritische Infrastruktur war angreifbar.

---

<!-- _class: biglist -->
# Lesson Learned – IoT & Physical

- **Netzwerksegmentierung** ist überlebenswichtig
- Jedes Gerät mit IP-Adresse ist ein **potenzieller Angriffsvektor**
- **Angriffsfläche wächst** exponentiell mit jedem IoT-Gerät
- **Safety ≠ Security:** Ein Auto kann „sicher" (safe) gebaut sein, aber trotzdem hackbar (insecure)
- Medienberichte kritisch hinterfragen

---

<!-- _class: chapter -->
# 4. Staatliche Cyber-Operationen
## Stuxnet & NotPetya

---

# Stuxnet (entdeckt 2010)

**Der erste bekannte staatliche Cyberangriff auf physische Infrastruktur.**

<div class="columns">
<div>

**Angreifer:** USA & Israel (Codename: „Olympic Games")

**Ziel:** Urananreicherungsanlage Natanz, Iran

**Verbreitung:**
- USB-Sticks (→ Air-Gap überbrückt!)
- Windows-Netzwerk-Shares
- Siemens STEP 7 Projektdateien

**Besonderheiten:**
- **4 Zero-Day-Exploits** gleichzeitig
- Gestohlene digitale Zertifikate (Realtek & JMicron)
- Selbstbegrenzende Verbreitung (max. 3 Infektionen pro USB)

</div>
<div>

**Was Stuxnet tat:**
- Zielte auf **Siemens S7-315/417 SPS**
- Manipulierte **Frequenzumrichter** der Zentrifugen
- Drehzahl: Normal 1.064 Hz → plötzlich 1.410 Hz → dann 2 Hz
- Zentrifugen vibrierten sich **physisch kaputt**
- Gleichzeitig: **Fälschte Sensordaten** → Operateure sahen „alles normal"

**Ergebnis:**
- ~1.000 von 5.000 Zentrifugen zerstört
- Iranisches Atomprogramm um **geschätzt 2 Jahre** verzögert

</div>
</div>

---

# Stuxnet – Verbreitungsweg

<style scoped>
pre { font-size: 14pt; }
</style>

```
                    ┌──────────────────────┐
                    │   Entwickler (NSA/   │
                    │   Unit 8200)         │
                    └──────────┬───────────┘
                               │ USB-Stick
                    ┌──────────▼───────────┐
                    │   Zulieferer-Laptop  │
                    │   (unwissentlich)    │
                    └──────────┬───────────┘
                               │ USB / Netzwerk
                    ┌──────────▼───────────┐
                    │  Natanz IT-Netzwerk  │──── Air Gap ────┐
                    └──────────────────────┘                 │
                                                   ┌────────▼─────────┐
                                                   │ SCADA / OT-Netz  │
                                                   │ Siemens STEP 7   │
                                                   └────────┬─────────┘
                                                            │
                                                   ┌────────▼─────────┐
                                                   │  S7-315/417 SPS  │
                                                   │  → Zentrifugen   │
                                                   └──────────────────┘
```

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
table { font-size: 16pt; }
</style>

**Geschätzter Gesamtschaden: ~10 Milliarden USD**

| Unternehmen | Schaden | Details |
|-------------|---------|---------|
| **Maersk** (Reederei) | ~$300 Mio | 45.000 PCs + 4.000 Server komplett neuinstalliert. 10 Tage lang größte Reederei der Welt **blind**. |
| **Merck** (Pharma) | ~$870 Mio | Produktionsausfälle bei Impfstoffen |
| **FedEx/TNT** | ~$400 Mio | TNT Express monatelang beeinträchtigt |
| **Mondelez** (Lebensmittel) | ~$188 Mio | 1.700 Server, 24.000 Laptops zerstört |
| **Saint-Gobain** (Bau) | ~$384 Mio | IT-Systeme weltweit betroffen |

<br>

**Die Ironie:** Keine dieser Firmen war das eigentliche Ziel. Sie hatten lediglich **Niederlassungen in der Ukraine** und nutzten M.E.Doc.

---

<!-- _class: biglist -->
# Lesson Learned – Staatliche Operationen

- **Cyber ist eine Waffe** – mit realen physischen Konsequenzen
- **Supply-Chain-Angriffe** sind extrem effektiv und schwer zu erkennen
- **Kollateralschäden** sind bei Cyberwaffen kaum kontrollierbar
- Air Gaps sind **überbrückbar** (USB, Zulieferer, Social Engineering)
- Wer in einem Konfliktgebiet Geschäfte macht, wird zum Ziel

---

<!-- _class: chapter -->
# 5. Ironie des Schicksals
## Wenn Security-Firmen versagen

---

# RSA SecurID Breach (2011)

<div class="columns">
<div>

**RSA:** Einer der bekanntesten Namen in IT-Security. Hersteller der **SecurID-Tokens** – Hardware-2FA für Millionen von Nutzern.

**Der Angriff:**
- Phishing-Mail an RSA-Mitarbeiter
- Betreff: *„2011 Recruitment Plan"*
- Anhang: Excel-Datei mit **Flash-Exploit** (CVE-2011-0609)
- Angreifer: mutmaßlich **chinesischer Geheimdienst**

</div>
<div>

**Was gestohlen wurde:**
- **Seed-Daten** für ~40 Millionen SecurID-Tokens
- Damit konnten Angreifer gültige 2FA-Codes **berechnen**

**Folgen:**
- Alle 40 Mio Tokens mussten **ausgetauscht** werden
- Geschätzter Schaden: **$66 Millionen** (nur für RSA direkt)
- Lockheed Martin wurde anschließend mit den gestohlenen Seeds angegriffen

</div>
</div>

<br>

> Die Firma, der man vertraut, um Zugänge abzusichern, wird selbst zum Einfallstor.

---

# LastPass Breach (2022–2023)

**Der weltweit bekannteste Passwort-Manager – gehackt.**

**Phase 1 (August 2022):**
- Angreifer kompromittieren Entwickler-Laptop über **manipuliertes Software-Paket**
- Zugang zu **LastPass Source Code** und internen Systemen

**Phase 2 (November 2022):**
- Mit gestohlenen Zugangsdaten → Zugriff auf **Cloud-Backups**
- **Verschlüsselte Passwort-Vaults aller Nutzer** heruntergeladen

**Das Problem der Kommunikation:**
- LastPass: *„Ihre Passwörter sind sicher verschlüsselt"*
- Realität: **URLs, Seitennamen und Metadaten unverschlüsselt**
- Angreifer wissen genau, **welche Dienste** jeder Nutzer verwendet
- Schwache Master-Passwörter → Offline-Brute-Force möglich

---

# CrowdStrike-Ausfall (19. Juli 2024)

![w:550](img/crowdstrike_bsod.jpg)

**Kein Hack. Kein Angriff. Ein fehlerhaftes Update.**

---

# CrowdStrike – Was passiert ist

**Der Auslöser:**
- Falcon Sensor Update – **Channel File 291**
- Fehlerhafte Konfigurationsdatei für Named-Pipe-Erkennung
- Kernel-Level-Treiber → **Blue Screen of Death (BSOD)**
- Automatisch ausgerollt an **alle Kunden gleichzeitig** (kein Staged Rollout)

**Der Schaden:**

<div class="columns">
<div>

- 🖥️ **8,5 Millionen** Windows-Systeme betroffen
- ✈️ Flughäfen weltweit: Flüge gestrichen
- 🏥 Krankenhäuser: OPs verschoben
- 🏦 Banken: Systeme offline
- 📺 TV-Sender: Kein Sendebetrieb

</div>
<div>

- 💰 Geschätzter Schaden: **$5,4 Milliarden**
- ⏱️ Fix: Manueller Eingriff an **jedem einzelnen Rechner** nötig (Safe Mode → Datei löschen)
- 🔄 Wochen bis zur vollständigen Wiederherstellung

</div>
</div>

---

<!-- _class: biglist -->
# Lesson Learned – Security-Firmen

- **Single Point of Failure:** Ein Security-Produkt auf Millionen Rechnern = Klumpenrisiko
- **Staged Rollouts** sind Pflicht – besonders bei Kernel-Level-Software
- **Incident Communication** entscheidet über Vertrauensverlust
- Auch die **Beschützer brauchen Schutz** – Security-Firmen sind Hochwertziele
- Der größte IT-Ausfall der Geschichte war **kein Angriff**

---

<!-- _class: chapter -->
# 6. Datenschutz-Kuriositäten
## Cambridge Analytica, Schrems & DSGVO-Bußgelder

---

# Cambridge Analytica & Facebook (2018)

<div class="columns">
<div>

**Die Quiz-App:**
- *„This Is Your Digital Life"* (Dr. Aleksandr Kogan)
- 270.000 Nutzer installierten die App
- Facebook Graph API erlaubte Zugriff auf **Daten aller Freunde**
- Ergebnis: Daten von **87 Millionen Nutzern** gesammelt

**Verwendungszweck:**
- **Psychografische Profile** erstellt
- Gezieltes politisches Micro-Targeting
- Einsatz bei: **Brexit-Referendum** und **US-Wahl 2016**

</div>
<div>

![w:450](img/cambridge_analytica_zuckerberg.jpg)

*Mark Zuckerberg vor dem US-Senat (April 2018)*

</div>
</div>

---

# Cambridge Analytica – Folgen

**Strafen & Konsequenzen:**
- Facebook: **$5 Milliarden** Strafe (FTC) – höchste jemals verhängte Datenschutzstrafe in den USA
- Cambridge Analytica: **Insolvenz** (Mai 2018)
- Whistleblower **Christopher Wylie** ging an die Öffentlichkeit

**Technischer Hintergrund:**
```
Facebook Graph API v1.0 (bis 2014):
  App fragt Berechtigungen an
    → Nutzer stimmt zu
      → App erhält Zugriff auf Profil + ALLE Freunde-Profile
        → 270.000 × Ø 340 Freunde = ~87 Mio Profile
```

**Kulturelle Auswirkung:**
- Befeuerte die **DSGVO-Debatte** massiv
- #DeleteFacebook-Bewegung
- Dokumentarfilm: *„The Great Hack"* (Netflix)

---

# Schrems vs. Facebook (2013–heute)

**Ein einzelner Bürger bringt transatlantische Datenabkommen zu Fall.**

![w:300](img/max_schrems.jpg)

**Max Schrems** – österreichischer Jurastudent, Gründer von **noyb.eu** (*None Of Your Business*)

---

# Schrems – Die Timeline

<style scoped>
table { font-size: 16pt; }
</style>

| Jahr | Ereignis |
|------|----------|
| **2011** | Schrems fordert bei Facebook Irland seine Daten an → erhält **1.222 Seiten PDF** |
| **2013** | Beschwerde bei irischer Datenschutzbehörde (NSA-Überwachung/PRISM) |
| **2015** | **Schrems I** – EuGH erklärt **Safe Harbor für ungültig** (C-362/14) |
| **2016** | EU-Kommission beschließt **Privacy Shield** als Nachfolger |
| **2020** | **Schrems II** – EuGH erklärt auch **Privacy Shield für ungültig** (C-311/18) |
| **2023** | EU-Kommission beschließt **Trans-Atlantic Data Privacy Framework** |
| **202?** | Schrems III? noyb hat bereits angekündigt, auch das neue Framework zu prüfen |

<br>

> Ein Jurastudent gegen einen Billionen-Dollar-Konzern – und er gewinnt. **Zweimal.**

---

# DSGVO-Bußgeld-Hitliste

<style scoped>
table { font-size: 15pt; }
</style>

| # | Unternehmen | Bußgeld | Grund | Jahr |
|---|-------------|---------|-------|------|
| 1 | **Meta** (Facebook) | **1,2 Mrd. €** | Datentransfer in die USA ohne Rechtsgrundlage | 2023 |
| 2 | **Amazon** | **746 Mio. €** | Werbe-Targeting ohne gültige Einwilligung | 2021 |
| 3 | **Meta** (Instagram) | **405 Mio. €** | Datenverarbeitung von Minderjährigen | 2022 |
| 4 | **Meta** (WhatsApp) | **225 Mio. €** | Transparenzmangel Datenschutzerklärung | 2021 |
| 5 | **Google** | **50 Mio. €** | Intransparente Einwilligungsmechanismen (CNIL) | 2019 |

---

# DSGVO – Kuriose Fälle

<div class="columns">
<div>

**🏷️ Bußgeld wegen Klingelschildern (2018)**
- Wiener Hausverwaltung entfernte *alle* Namensschilder
- Begründung: „DSGVO"
- Datenschutzbehörde: Das war **nie erforderlich**
- Panik-Überreaktion machte Schlagzeilen

**🏥 H&M: Mitarbeiterüberwachung (2020)**
- Bußgeld: **35 Mio. €**
- Führungskräfte notierten systematisch:
  - Krankheiten von Mitarbeitern
  - Familiäre Probleme
  - Religiöse Überzeugungen
- Daten in **Excel-Tabellen** geführt

</div>
<div>

**📹 Mitarbeiter-Videoüberwachung**
- Notebooksbilliger.de: **10,4 Mio. €** (2021)
- Kameras in Verkaufsräumen, Lagern, Arbeitsplätzen
- Ohne ausreichende Rechtsgrundlage
- **Zwei Jahre** lang durchgängig überwacht

**🍪 Cookie-Banner-Wahnsinn**
- Google: 150 Mio. € (CNIL, 2022)
- „Alle akzeptieren" = 1 Klick
- „Alle ablehnen" = 5 Klicks durch Untermenüs
- **Dark Patterns** als Datenschutzverstoß

</div>
</div>

---

# Streisand-Effekt & Recht auf Vergessenwerden

**Google Spain (2014) – EuGH C-131/12:**
- Mario Costeja González wollte einen alten Zeitungsartikel über Zwangsversteigerung aus Google entfernen
- EuGH: Ja, es gibt ein **Recht auf Vergessenwerden**
- Google erhält seitdem **Millionen von Löschanfragen** pro Jahr

**Der Streisand-Effekt:**

> Wenn der Versuch, Informationen zu unterdrücken, erst recht Aufmerksamkeit erzeugt.

- Benannt nach **Barbra Streisand** (2003): Wollte ein Luftbild ihres Hauses entfernen lassen → Foto wurde vorher **6 Mal** aufgerufen → nach der Klage **420.000 Mal**
- Tritt regelmäßig bei Löschanfragen auf: Die Meldung *„XY fordert Löschung"* erzeugt mehr Aufmerksamkeit als der Originalartikel

---

<!-- _class: biglist -->
# Lesson Learned – Datenschutz

- **DSGVO hat Zähne** – Milliarden-Bußgelder sind Realität
- **Einzelpersonen** können Großes bewirken (Schrems!)
- **Dark Patterns** sind keine clevere UX – sie sind rechtswidrig
- Panik vor der DSGVO führt manchmal zu **absurderen Ergebnissen** als die DSGVO selbst
- Das **Recht auf Vergessenwerden** hat Grenzen – vor allem gegen den Streisand-Effekt

---

<!-- _class: chapter -->
# 7. Social Engineering & der menschliche Faktor
## Twitter, EU-Geheimtreffen & WannaCry

---

# Twitter Bitcoin Scam (15. Juli 2020)

<div class="columns">
<div>

**Der Täter:**
- **Graham Ivan Clark** (17 Jahre alt, Tampa, Florida)

**Die Methode:**
- Social Engineering gegen Twitter-Mitarbeiter
- Gab sich als IT-Support aus (Telefon)
- Erhielt Zugang zum internen **Admin-Tool**

**Übernommene Accounts:**
- 🇺🇸 Barack Obama
- 💰 Elon Musk
- 📦 Jeff Bezos
- 🍎 Apple
- 💎 Bill Gates

</div>
<div>

![w:450](img/twitter_bitcoin_scam.jpg)

*Tweet vom Account von Barack Obama*

**Nachricht auf allen Accounts:**
> *„I'm giving back to the community. All Bitcoin sent to the address below will be sent back doubled!"*

**Ergebnis:**
- ~$120.000 in Bitcoin erbeutet
- Hätte **Millionen** sein können – Clark war auf's schnelle Geld aus, nicht auf maximalen Schaden

</div>
</div>

---

# Twitter Bitcoin Scam – Technisch

**Das interne Admin-Tool:**

```
Twitter Internal Tool ("God Mode"):
  → Account-E-Mail ändern       ✓
  → Account-Passwort zurücksetzen ✓
  → 2FA deaktivieren              ✓
  → Tweets posten als jeder User  ✓
  → Account sperren/entsperren    ✓
```

**Warum ein 17-Jähriger das konnte:**
- Internes Tool hatte **keine ausreichende Zugriffskontrolle**
- **Kein Vier-Augen-Prinzip** für kritische Aktionen
- Anruf bei Twitter-Mitarbeitern im Home-Office (COVID-19) → weniger Verifizierung
- **Kein Monitoring** ungewöhnlicher Admin-Aktionen

**Urteil:** 3 Jahre Jugendstrafe – die **längste Strafe** unter Floridas Cybercrime-Gesetz für einen Minderjährigen.

---

# Journalist hackt EU-Verteidigungsminister-Treffen (2020)

<div class="columns">
<div>

**Die Quelle des Hacks:**
- Niederländische Verteidigungsministerin **Ank Bijleveld** postet ein Foto auf Twitter
- Im Bild sichtbar: **Meeting-Link und PIN** des geheimen EU-Verteidigungsminister-Treffens

</div>
<div>

![w:450](img/eu_meeting_verlaan.jpg)

</div>
</div>

**Was dann passierte:**
- **Daniel Verlaan** (RTL Nieuws) sah das Foto
- Wählte sich **live in der Sendung** in das Meeting ein
- 😮 Verwirrte Blicke der Verteidigungsminister
- EU-Außenbeauftragter Josep Borrell: *„You have been connected to a secret meeting. Please leave immediately."*
- Verlaan winkte fröhlich und legte auf

---

# WannaCry & der zufällige Held (Mai 2017)

**WannaCry-Ransomware:**
- Nutzte **EternalBlue** (NSA-Exploit, geleakt von Shadow Brokers)
- **>200.000 Systeme** in **150 Ländern** infiziert
- NHS (UK): Krankenhäuser geschlossen, OPs abgesagt
- Schaden: geschätzt **$4–8 Milliarden**
- Zugeschrieben: **Nordkorea** (Lazarus Group)

**Der Kill Switch:**
- Im Code: Malware prüft, ob die Domain `iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com` existiert
- Falls ja → **Malware stoppt sofort**
- Die Domain war **nicht registriert**

---

# Marcus Hutchins – Held wider Willen

<div class="columns">
<div>

**Der Moment:**
- **Marcus Hutchins** (22), britischer Sicherheitsforscher (MalwareTechBlog)
- Analysierte WannaCry-Sample
- Fand die unregistrierte Domain
- Registrierte sie für **$10,69**
- → **WannaCry stoppte weltweit**

**Über Nacht zum Helden:**
- Internationale Medienberichterstattung
- „Der Mann, der WannaCry stoppte"

</div>
<div>

**Die dunkle Wendung (August 2017):**
- Hutchins reiste zur DEF CON nach Las Vegas
- Wurde am Flughafen vom **FBI verhaftet**
- Anklage: Entwicklung von **Kronos Banking Trojan** – als Teenager (2014/2015)
- Bekannte sich **schuldig** (2019)
- Urteil: **Keine Gefängnisstrafe** (Time served + Bewährung)

*Von „Held der Cybersecurity" zu „FBI-Verhaftung" in drei Monaten.*

</div>
</div>

---

<!-- _class: biglist -->
# Lesson Learned – Menschlicher Faktor

- **Social Engineering** umgeht jede technische Sicherheitsmaßnahme
- **OPSEC** (Operational Security): Keine sensiblen Infos in Social-Media-Fotos!
- **Überprivilegierte Admin-Tools** ohne Monitoring sind Zeitbomben
- Ein **$10-Domain-Kauf** kann eine globale Pandemie stoppen
- Helden haben manchmal eine **komplizierte Vergangenheit**

---

<!-- _class: chapter -->
# 8. Kurioses Finale
## White Hats & Zusammenfassung

---

# Poly Network Hack (August 2021)

**Der größte DeFi-Hack (zu dem Zeitpunkt): $611 Millionen**

**Was passierte:**
- Angreifer nutzte Schwachstelle in Cross-Chain-Bridge von Poly Network
- Stahl **$611 Mio** in Ethereum, Binance Smart Chain und Polygon

**Was dann passierte:**
- Poly Network veröffentlichte **offenen Brief** an den Hacker auf Twitter
- Ethereum-Community markierte die Adressen → Geld war kaum bewegbar
- Der Hacker begann, Nachrichten **in die Blockchain zu schreiben:**

> *„IT WOULD HAVE BEEN A BILLION HACK IF I HAD MOVED THE REMAINING SHITCOINS!"*
> *„I AM NOT SO INTERESTED IN MONEY ANYMORE."*

- Gab innerhalb weniger Tage **alles zurück**
- Poly Network nannte ihn **„Mr. White Hat"** und bot ihm einen **Job als Chief Security Advisor** an

---

# Zusammenfassung – Wiederkehrende Muster

<style scoped>
table { font-size: 16pt; }
</style>

| Muster | Beispiele |
|--------|-----------|
| **Menschlicher Faktor** | Twitter Scam, RSA-Phishing, EU-Meeting-Foto, WannaCry-Held |
| **Fehlende Segmentierung** | Casino-Thermometer, IoT-Botnetz |
| **Single Points of Failure** | CrowdStrike, M.E.Doc (NotPetya) |
| **Supply Chain als Waffe** | NotPetya (M.E.Doc), Stuxnet (USB) |
| **Software-Qualität** | Therac-25, Ariane 5, CrowdStrike |
| **Recht hinkt Technik hinterher** | Morris Worm, ILOVEYOU, Schrems |
| **Ironie des Schicksals** | RSA gehackt, LastPass gehackt, Held verhaftet |

---

<!-- _class: biglist -->
# Lessons Learned – Die wichtigsten Takeaways

- **Der Mensch** ist und bleibt die größte Schwachstelle – und die beste Verteidigung
- **Defense in Depth** – nie auf eine einzige Schutzmaßnahme vertrauen
- **Testen, Testen, Testen** – besonders an den Grenzen
- **Datenschutz ist kein Papiertiger** – die DSGVO wird durchgesetzt
- Und manchmal entscheidet ein **$10-Domain-Kauf** eines 22-Jährigen über eine globale Krise

---

<!-- _class: title -->
# Fragen?
## Danke für die Aufmerksamkeit!
