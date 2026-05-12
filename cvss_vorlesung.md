---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)

---

<!-- _class: title -->

# CVSS-Score
## Common Vulnerability Scoring System

---

# Agenda

1. **Was ist CVSS?** – Überblick & Motivation
2. **Wofür wird CVSS verwendet?**
3. **Die drei Metrikgruppen** – Aufbau des Scores
4. **Berechnung** – Formel & Gewichtung
5. **Praxisbeispiel** – Log4Shell (CVE-2021-44228)
6. **Bedeutung der einzelnen Parameter**
7. **Kritik an CVSS**
8. **Fazit & Alternativen**

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

# Bedeutung der Parameter im Detail (1/2)

## Attack Vector (AV)

| Wert | Bedeutung | Gewicht |
|------|-----------|---------|
| Network (N) | Angriff über Netzwerk möglich (z. B. Internet) | 0.85 |
| Adjacent (A) | Angriff nur im lokalen Netz möglich | 0.62 |
| Local (L) | Angriff erfordert lokalen Zugang | 0.55 |
| Physical (P) | Physischer Zugang zum Gerät nötig | 0.20 |

## Attack Complexity (AC)

| Wert | Bedeutung | Gewicht |
|------|-----------|---------|
| Low (L) | Keine besonderen Bedingungen nötig | 0.77 |
| High (H) | Bestimmte Bedingungen müssen erfüllt sein | 0.44 |

---

# Bedeutung der Parameter im Detail (2/2)

## Privileges Required (PR)

| Wert | Bedeutung |
|------|-----------|
| None (N) | Kein Account nötig |
| Low (L) | Normaler Benutzer-Account reicht |
| High (H) | Admin-Rechte erforderlich |

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

## Schritt 2: Impact Sub Score (ISCBase)

```
Scope Unchanged:  ISCBase = 6.42 × ISS
Scope Changed:    ISCBase = 7.52 × [ISS − 0.029] − 3.25 × [ISS − 0.02]^15
```

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

- ⚖️ **Keine Kontextualisierung** – AV:N/AC:L klingt schlimm, aber betrifft das System überhaupt mein Netz?
- 📈 **Score-Inflation** – Viele Schwachstellen werden als „High" oder „Critical" bewertet; Priorisierung wird schwieriger
- 🔢 **Pseudopräzision** – Eine Kommastelle suggeriert Genauigkeit, die nicht existiert (7.5 vs. 7.8 praktisch identisch)
- 🔄 **Nichtlineare Formel** – Kleine Parameteränderungen können große Sprünge verursachen
- 🚫 **Kein Exploit-Kontext** – CVSS misst Schwere, nicht Wahrscheinlichkeit der Ausnutzung

---

# Kritik an CVSS (2/2)

## Organisatorische & praktische Schwächen

- 🏢 **Temporal/Environmental selten genutzt** – In der Praxis dominiert der Base Score
- 🤖 **Subjektivität** – Verschiedene Bewerter kommen zu unterschiedlichen Scores für dieselbe CVE
- ⏱️ **Zeitverzögerung** – Score wird oft Wochen nach Bekanntwerden erst publiziert
- 🔗 **Kettenangriffe unberücksichtigt** – Mehrere „Low"-Schwachstellen können kombiniert kritisch sein
- 📊 **Missbrauch als KPI** – „Alle Critical-CVEs schließen" als Ziel führt zu Fehlanreizen

> 💬 *„A CVSS score of 9.8 does not mean you will be hacked. A score of 2.0 does not mean you won't."*
> — CISA

---

# Alternativen & Ergänzungen zu CVSS

| System | Ansatz |
|--------|--------|
| **EPSS** (Exploit Prediction Scoring System) | Wahrscheinlichkeit der Ausnutzung in den nächsten 30 Tagen (0–100%) |
| **SSVC** (Stakeholder-Specific Vulnerability Categorization) | Entscheidungsbaum-basiert, kontextabhängig |
| **CVSS v4.0** | Neue Metrikgruppen, Threat Intelligence integriert |
| **VPR** (Vulnerability Priority Rating) | Tenable-eigener Score mit Threat Intel |

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

# Fragen?

🔗 **Weiterführend:**
- https://www.first.org/cvss/
- https://nvd.nist.gov/vuln-metrics/cvss
- https://www.epss-model.org/
- BSI IT-Grundschutz Kompendium
