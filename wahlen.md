---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
paginate: false
title: IT‑Einsatz bei Wahlen – Chancen, Risiken und Realität

---
<!-- _class: title -->

# IT‑Einsatz bei Wahlen  
## Elektronische Wahlmaschinen – Technik, Risiken und reale Vorfälle

---

# Agenda

1. Motivation & Überblick  
2. Arten elektronischer Wahlsysteme  
3. Funktionsweise  
4. Einsatz weltweit  
5. Sicherheitsrisiken  
6. Reale Vorfälle & Forschung  
7. Rechtliche & gesellschaftliche Aspekte  
8. Fazit & Diskussion

---

# Warum elektronische Wahlsysteme?

- Schnellere Auszählung  
- Barrierefreiheit  
- Weniger menschliche Fehler  
- Logistische Vorteile (z. B. große Länder)

**Aber:**  
Wahlen sind ein *kritischer demokratischer Prozess* → höchste Anforderungen an Transparenz, Nachvollziehbarkeit und Sicherheit.

---

# Arten elektronischer Wahlsysteme

**DRE – Direct Recording Electronic**
- Touchscreen oder Tastenfeld, Stimme wird elektronisch gespeichert  
- Optional: VVPAT (Papierbeleg)

**Optical Scan**
- Papierstimmzettel, Maschine scannt und zählt  
- Papier bleibt als Audit‑Trail

**Internetbasierte Wahlen**
- Web‑ oder App‑basiert  
  
---

# Funktionsweise elektronischer Wahlmaschinen

- Eingabe: Touchscreen, Buttons, Papier  
- Verarbeitung: Firmware, OS, Wahlsoftware  
- Speicherung: Flash‑Speicher, SD‑Karten  
- Übertragung: USB, Netzwerk, teilweise offline  
- Audit: Logs, kryptografische Signaturen, Papierbelege

**Kritisch:** Jede Komponente ist ein potenzieller Angriffspunkt.

---

# Einsatz weltweit

**USA**
- Mischung aus DRE und Optical Scan  
- Trend zurück zu Papierbelegen, viele Maschinen veraltet

**Brasilien**
- Seit 1996 flächendeckender Einsatz  
- Hohe Effizienz, aber politisierte Vertrauensdebatten

**Deutschland**
- Seit 2009 faktisch verboten  
- BVerfG: Wahl muss *für Laien nachvollziehbar* sein

---
<!-- _class: biglist -->
# Warum viele Länder auf elektronische Wahlmaschinen verzichten

- Fehlende Transparenz (proprietäre Systeme)  
- Keine unabhängige Nachprüfbarkeit ohne Papier  
- Komplexität → systemische Fehler möglich  
- Vertrauensverlust durch Sicherheitsdebatten  
- Rechtliche Anforderungen (z. B. Deutschland)

---

# Sicherheitsrisiken
<!-- _class: biglist -->
- Manipulierbare Firmware  
- Unsichere Speicherchips  
- Fehlende Integritätsprüfungen  
- Physischer Zugriff oft ausreichend  
- Unsichere Update‑Mechanismen  
- Veraltete Hardware/Software  
- Supply‑Chain‑Risiken  
- Angriffe auf Wahlinfrastruktur (nicht nur Maschinen!)

---

# Reale Vorfälle & Forschung
<!-- _class: biglist -->
## 1. Sicherheitsforschung (USA)
- J. Alex Halderman zeigte mehrfach, dass DRE‑Systeme manipulierbar sind  
- Firmware konnte ohne Spezialwerkzeug ersetzt werden  
- Wahlsoftware oft ohne moderne Sicherheitsmechanismen

---
<!-- _class: biglist -->
# Reale Vorfälle & Forschung

## 2. DEF CON Voting Village
- Jährliche Sicherheitskonferenz  
- Forscher kompromittieren reale Wahlmaschinen  
- Häufige Probleme:
  - Standardpasswörter  
  - Offene USB‑Ports  
  - Veraltete Betriebssysteme  
  - Unsichere Netzwerkschnittstellen

---
<!-- _class: biglist -->
# Reale Vorfälle & Forschung

## 3. Angriffe auf Wahlinfrastruktur
- Angriffe auf Wählerregister in mehreren US‑Bundesstaaten  
- Kein Einfluss auf Stimmen, aber:
  - Manipulation von Wählerlisten  
  - Störungen am Wahltag  
  - Vertrauensverlust

---
<!-- _class: biglist -->
# Reale Vorfälle & Forschung

## 4. Brasilien – Vertrauenskrise
- Keine belegten Manipulationen  
- Politische Akteure verbreiten Zweifel  
- Zeigt: *Technische Sicherheit ≠ gesellschaftliche Akzeptanz*

---
<!-- _class: biglist -->
# Reale Vorfälle & Forschung

## 5. Deutschland – Nedap‑Wahlcomputer
- 2006–2008 im Einsatz  
- CCC demonstrierte:
  - Manipulation der Software möglich  
  - Keine ausreichende Transparenz  
- 2009: Verfassungsgericht stoppt Einsatz

---

# Rechtliche Anforderungen

## Deutschland (BVerfG 2009)
- Wahl muss **öffentlich nachvollziehbar** sein  
- Keine „Black Box“  
- Elektronische Systeme nur zulässig, wenn:
  - Ergebnis ohne Spezialwissen überprüfbar  
  - Manipulationen ausgeschlossen oder erkennbar

---

# Gesellschaftliche Aspekte

- Vertrauen ist zentral  
- Schon *theoretische* Schwachstellen können Vertrauen zerstören  
- Transparenz wichtiger als Effizienz  
- Papier als „physischer Anker“ für Nachvollziehbarkeit

---

# Vergleich: Papierwahl vs. elektronische Wahl

| Kriterium | Papierwahl | Elektronische Wahl |
|----------|------------|--------------------|
| Geschwindigkeit | langsam | schnell |
| Fehleranfälligkeit | menschliche Fehler | systemische Fehler |
| Nachvollziehbarkeit | hoch | oft gering |
| Manipulationsrisiko | lokal | potenziell großflächig |
| Vertrauen | hoch | abhängig von Transparenz |

---

# Fazit

- Elektronische Wahlsysteme bieten Vorteile, aber auch erhebliche Risiken  
- Viele reale Vorfälle zeigen: Sicherheit ist schwer zu gewährleisten  
- Transparenz und Nachvollziehbarkeit sind entscheidend  
- Papierbasierte Systeme bleiben weltweit Standard  
- Vertrauen ist wichtiger als technische Eleganz

---

# Wahlen über das Internet (i‑Voting)

## Motivation und Idee

- Wahlteilnahme von überall (z. B. Ausland, Mobilitätseinschränkungen)  
- Niedrigere organisatorische Kosten  
- Potenziell höhere Wahlbeteiligung  
- Attraktiv für digitalisierte Staaten

**Aber:**  
Online‑Wahlen sind sicherheitstechnisch deutlich anspruchsvoller als Wahlmaschinen vor Ort.

---

# Wie funktionieren Internetwahlen?

- Authentifizierung über digitale Identitäten (z. B. eID, Smartcard, App)  
- Stimmabgabe über Web‑Interface oder App  
- Verschlüsselung der Stimme (End‑to‑End‑Verfahren)  
- Übermittlung über das Internet  
- Speicherung auf Wahlservern  
- Entschlüsselung und Auszählung nach Wahlschluss  
- Teilweise: Möglichkeit zur Stimmänderung (z. B. Estland)

**Kritisch:**  
Die gesamte Kette ist digital – keine physische Kontrolle.

---

# Einsatz weltweit

## Estland (E‑Voting seit 2005)
- Pionier und weltweit bekanntestes System  
- Nutzung der nationalen digitalen Identität  
- Hohe Akzeptanz, aber wiederkehrende Sicherheitskritik  
- Stimmen können mehrfach abgegeben werden (nur letzte zählt)

---

# Einsatz weltweit

## Schweiz
- Mehrere Pilotprojekte  
- 2019: Abbruch wegen schwerer Sicherheitslücken  
- Teilweise Wiederaufnahme mit strengeren Anforderungen

---

# Einsatz weltweit

## Weitere Länder
- Frankreich: Online‑Wahlen für Auslandsfranzosen (zeitweise)  
- Kanada: Kommunale Pilotprojekte  
- USA: Sehr begrenzte Nutzung (z. B. Militär im Ausland)

---

# Warum Internetwahlen besonders schwierig sind

- Angriffe können **global** erfolgen  
- Wählergeräte (PC/Smartphone) sind unsicher  
- Malware kann Stimmen manipulieren  
- Phishing und Identitätsdiebstahl  
- Server‑Angriffe und DDoS  
- Fehlende Möglichkeit zur öffentlichen Nachvollziehbarkeit  
- Vertrauen in komplexe Kryptografie statt in physische Kontrolle

---

# Sicherheitsanforderungen an Online‑Wahlen

- **End‑to‑End‑Verifizierbarkeit**  
  Wähler*innen können prüfen, dass ihre Stimme korrekt gezählt wurde.

- **Coercion‑Resistance**  
  Wähler dürfen nicht beweisen können, wie sie gewählt haben (Schutz vor Druck).

- **Software‑Unabhängigkeit**  
  Fehler oder Manipulationen müssen erkennbar sein.

- **Transparenz**  
  Offenlegung von Protokollen, Code, Audits.

- **Robuste Infrastruktur**  
  Schutz vor DDoS, Manipulation, Ausfall.

---

# Reale Probleme und Vorfälle bei Internetwahlen

## Estland – Kryptografische Schwachstellen
- 2014: Forscher zeigten, dass Malware auf Wählergeräten Stimmen manipulieren könnte  
- Unsichere Server‑Prozesse und potenzielle Insider‑Risiken  
- Empfehlung der Forscher: System aussetzen (wurde nicht umgesetzt)

---

# Reale Probleme und Vorfälle bei Internetwahlen

## Schweiz – Schwerwiegende Lücken im E‑Voting‑System
- 2019: Externe Sicherheitsanalyse deckte fundamentale Fehler im Kryptosystem auf  
- Angreifer hätten Stimmen unbemerkt verändern können  
- Folge: Stopp aller nationalen E‑Voting‑Pilotprojekte

---

# Reale Probleme und Vorfälle bei Internetwahlen

## USA – Online‑Voting‑Apps
- 2020: Iowa Caucus App mit massiven technischen Problemen  
- Nicht direkt Online‑Voting, aber zeigt Risiken digitaler Wahlprozesse

---

# Grundproblem: Das „Verifizierbarkeits‑Paradoxon“

- Wahlen müssen **geheim** sein  
- Wahlen müssen **nachvollziehbar** sein  
- Wahlen müssen **fälschungssicher** sein

Bei Online‑Wahlen ist es extrem schwierig, alle drei Anforderungen gleichzeitig zu erfüllen.

---

# Vergleich: Wahlmaschinen vs. Internetwahlen

| Kriterium | Wahlmaschinen vor Ort | Internetwahlen |
|----------|------------------------|----------------|
| Angriffsfläche | lokal | global |
| Physische Kontrolle | hoch | gering |
| Nachvollziehbarkeit | möglich (Papier) | schwierig |
| Komplexität | hoch | sehr hoch |
| Vertrauen | abhängig von Transparenz | stark abhängig von Technik |
| Risiko systemischer Manipulation | mittel | sehr hoch |

---

# Fazit zu Internetwahlen

- Technisch faszinierend, aber sicherheitlich extrem anspruchsvoll  
- Reale Vorfälle zeigen: selbst hochentwickelte Systeme sind verwundbar  
- Vertrauen der Bevölkerung ist schwer aufrechtzuerhalten  
- Viele Staaten bleiben bewusst bei papierbasierten Verfahren  
- Forschung geht weiter, aber ein „perfektes“ Online‑Wahlsystem existiert nicht



