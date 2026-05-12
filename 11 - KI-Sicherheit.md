---
marp: true
theme: custom
title: KI in der IT-Sicherheit
footer: ![w:280](img/dhbw-ka.svg)
transition: slide
---

<!-- _class: title -->
# KI in der IT‑Sicherheit

<br>
<br>
<br>
<br>

## Sicherheit *von* KI & Sicherheit *durch* KI  

---

<!-- _class: chapter -->
# Warum KI die IT‑Sicherheit verändert

---
<!-- class: biglist -->
# KI als strategischer Verstärker
- KI‑Modelle sind heute tief in Produkte, Prozesse und kritische Infrastrukturen integriert.
- KI ermöglicht:
  - Automatisierung komplexer Aufgaben
  - Skalierung von Angriffen und Verteidigung
  - Personalisierung von Angriffen (z. B. Social Engineering)
- KI ist ein **Multiplikator**:  
  Kleine Teams können große Wirkung erzielen – auf beiden Seiten.

---

# Neue Dynamik
- Angriffe werden:
  - schneller
  - präziser
  - schwerer erkennbar
- Verteidigung wird:
  - datengetriebener
  - proaktiver
  - stärker automatisiert

---

# Beispiele aus der Praxis
- **Jailbreaks von LLMs**: Umgehung von Sicherheitsmechanismen durch geschickte Eingaben.
- **Deepfake‑Phishing**: Stimmen & Videos, die reale Personen imitieren.
- **Automatisierte Exploit‑Generierung**: KI schreibt Proof‑of‑Concepts oder Payloads.
- **KI‑gestützte Malware**: Polymorph, verschleiert, adaptiv, schwer signaturbasiert erkennbar.

---

# Warum das Thema jetzt relevant ist
- KI‑Modelle werden in Unternehmen oft schneller integriert als abgesichert.
- Regulatorische Anforderungen (z. B. EU AI Act) steigen.
- KI‑Angriffe sind bereits in realen Vorfällen dokumentiert.
- Sicherheitsforschung zeigt:  
  KI kann sowohl **Angriffe ermöglichen** als auch **Verteidigung stärken**.

---
<!-- _class: chapter -->
# Wie KI‑Modelle funktionieren (sicherheitsrelevant)

---

# Grundlagen: Was ein Modell ausmacht
- **Trainingsdaten**:  
  Modelle lernen Muster aus großen Datenmengen.  
  → Manipulation dieser Daten = Manipulation des Modells.
- **Modellarchitektur**:  
  Black‑Box‑Charakter erschwert Vorhersagbarkeit.
- **Inferenzphase**:  
  Modelle reagieren auf Eingaben (Prompts) – oft sensibel.
- **Deployment**:  
  API‑Zugriff, Plugins, Integrationen → neue Angriffsflächen.

---

# Warum KI anders ist als klassische Software
- Verhalten ist **nicht deterministisch**.
- Modelle können **halluzinieren**.
- Modelle können **vertrauliche Trainingsdaten ungewollt preisgeben**.
- Kleine Änderungen im Input können **massive Effekte** haben.
- Modelle sind **nicht regelbasiert**, sondern **statistisch**.

---
<!-- _class: chapter -->
# Sicherheit *von* KI‑Systemen

---

#  EU AI Act: Ein Überblick
## Das erste umfassende KI-Gesetz der Welt
### Stand: März 2026

---

# Was ist der EU AI Act?

- **Ziel:** Sicherstellung, dass KI-Systeme in der EU sicher, transparent und ethisch sind.
- **Ansatz:** Risikobasierte Regulierung (nicht die Technologie selbst wird reguliert, sondern deren Anwendung).
- **Gültigkeit:** Gilt für alle Anbieter und Nutzer, deren KI-Systeme in der EU Auswirkungen haben (Extra-Territorialität).

---

# Die Risikopyramide

1. **Inakzeptabel:** Verboten (z.B. biometrische Echtzeit-Überwachung).
2. **Hochrisiko:** Strenge Auflagen (z.B. kritische Infrastruktur, Bildung, HR).
3. **Begrenzt:** Transparenzpflicht (z.B. Deepfakes, Chatbots).
4. **Minimal:** Keine Regulierung (z.B. Videospiel-KI).

---

# Verbotene Praktiken
Seit **Februar 2025** bereits aktiv:

- **Social Scoring:** Bewertung sozialen Verhaltens durch Regierungen.
- **Manipulation:** Techniken zur Beeinflussung des freien Willens.
- **Biometrische Fernidentifizierung:** In öffentlichen Räumen (mit engen Ausnahmen für die Strafverfolgung).
- **Emotionserkennung:** Am Arbeitsplatz oder in Bildungseinrichtungen.

---

# Hochrisiko-KI (High-Risk)
*Frist für die Umsetzung: August 2026*

**Anforderungen:**
- Einführung eines Risikomanagement-Systems.
- Hochwertige Trainingsdaten (Vermeidung von Bias).
- Ausführliche technische Dokumentation.
- **Menschliche Aufsicht** ("Human-in-the-loop").
- Registrierung in einer EU-Datenbank.

---

# General Purpose AI (GPAI)
Regeln für Modelle wie GPT-4 oder Claude:

- **Transparenz:** Offenlegung der Trainingsdaten (Urheberrecht).
- **Systemische Risiken:** Modelle mit sehr hoher Rechenleistung müssen zusätzliche Sicherheitstests durchführen.
- **Code of Practice:** Seit 2025 gelten hier klare Verhaltensregeln für Provider.

---

# Zeitplan & Meilensteine

- **August 2024:** Gesetz tritt offiziell in Kraft.
- **Februar 2025:** Verbote treten in Kraft.
- **August 2025:** Regeln für GPAI-Modelle werden aktiv.
- **August 2026:** (Heute fast erreicht!) Volle Anwendbarkeit für die meisten Hochrisiko-Systeme.
- **August 2027:** Regeln für KI in regulierten Produkten (z.B. Spielzeug, Autos).

---

# Sanktionen & Governance

- **Bußgelder:** Bis zu **35 Mio. €** oder **7 %** des weltweiten Jahresumsatzes.
- **Aufsicht:** - **EU AI Office:** Zentrale Behörde in Brüssel.
  - **Nationale Behörden:** Überwachen die Einhaltung vor Ort.

---
<!-- _class: normal -->
# Sicherheitsprinzipien im KI‑Kontext
- **Confidentiality**:  
  Schutz von Trainingsdaten, Modellparametern, API‑Schlüsseln.
- **Integrity**:  
  Schutz vor Manipulation von Daten, Verhalten, Gewichten.
- **Availability**:  
  Schutz vor API‑Missbrauch, Modell‑DoS, Ressourcenerschöpfung.
- **Accountability**:  
  Nachvollziehbarkeit von Entscheidungen (Explainability).
- **Threat Modeling**:  
  Datenquellen, Modellpipeline, Deployment, Nutzerinteraktion.

---

# Typische KI‑Architektur (vereinfacht)
- Datenbeschaffung → Datenbereinigung → Training → Evaluation → Deployment → Monitoring  
## Jede Phase hat eigene Angriffsflächen:
- Poisoning  
- Backdoors  
- Model Leakage  
- Prompt Injection  
- Supply‑Chain‑Manipulation

---
<!-- _class: chapter -->
# Die OWASP Top 10 für LLM-Anwendungen

---

# LLM01: Prompt Injection 

## Technische Details:
- **Direkte Injektion:** Nutzer zwingen das Modell durch geschicktes Prompting, interne Sicherheitsrichtlinien zu ignorieren.
- **Indirekte Injektion:** Das Modell verarbeitet Daten aus externen Quellen (Webseiten, E-Mails), die versteckte Befehle enthalten.
- **Multimodale Risiken:** Versteckte Anweisungen in Bildern oder anderen Medientypen, die von multimodal fähigen KIs verarbeitet werden.

---

# LLM01: Prompt Injection – Über die Texteingabe hinaus

## Szenarien:
- **Payload Splitting:** Aufteilen bösartiger Prompts in harmlose Fragmente, die erst im Modell kombiniert werden.
- **Adversarial Suffix:** Anhängen scheinbar sinnloser Zeichenfolgen, die Sicherheitsfilter umgehen.

---

# LLM02: Sensitive Information Disclosure

## Gefahrenquellen:
- **Inversion Attacks:** Angreifer extrahieren Trainingsdaten oder rekonstruieren Eingaben aus den Modellantworten.

---

# LLM02: Sensitive Information Disclosure

## Erweiterte Prävention:
- **Differential Privacy:** Hinzufügen von Rauschen zu Daten/Outputs, um Rückschlüsse auf Einzelpersonen zu erschweren.
- **Homomorphe Verschlüsselung:** Ermöglicht die Datenanalyse, während die Informationen verschlüsselt bleiben.
- **Federated Learning:** Training auf dezentralen Daten, um das Risiko einer zentralen Datenspeicherung zu minimieren.

---

# LLM03: Supply Chain Risks

## Spezifische Risiken der KI-Lieferkette:
- **Vulnerable LoRA-Adapter:** Fine-Tuning-Komponenten können Hintertüren in Basismodelle einschleusen.
- **PoisonGPT:** Ein realer Angriff, bei dem ein Modell manipuliert wurde, um gezielt Falschinformationen zu verbreiten.
- **Model Provenance:** Aktuell fehlen oft Garantien über den Ursprung und die Integrität veröffentlichter Modelle.

---

# LLM03: Supply Chain Risks

## Schutzmaßnahmen:
- **CycloneDX & ML-BOM:** Nutzung spezialisierter Bill-of-Materials für KI-Modelle und Datensätze.
- **Model Integrity Checks:** Einsatz von Hash-Werten und Signaturen zur Verifizierung von Modellen.

---

# LLM04: Data and Model Poisoning

## Der "Sleeper Agent" Effekt:
- Manipulationen können so subtil sein, dass das Modell normal funktioniert, bis ein spezieller Trigger (z. B. ein Codewort) aktiviert wird.
- **Malicious Pickling:** Ausführen schädlichen Codes direkt beim Laden eines Modells aus einem Repository.

---

# LLM04: Data and Model Poisoning

## Strategien:
- **Data Version Control (DVC):** Lückenlose Nachverfolgung von Änderungen an Datensätzen.
- **Anomalieerkennung:** Überwachung des "Training Loss" während des Lernprozesses auf verdächtige Muster.

---

# LLM05: Improper Output Handling

## Systemische Auswirkungen:
- **XSS & CSRF:** Wenn LLM-generiertes JavaScript oder Markdown ungefiltert im Browser ausgeführt wird.
- **SSRF & RCE:** Wenn LLM-Outputs direkt in System-Shells (exec/eval) oder Backend-Funktionen fließen.

---

# LLM05: Improper Output Handling

## Wichtige Abgrenzung:
- Im Gegensatz zur "Overreliance" (blindes Vertrauen) geht es hier um die technische Unfähigkeit des Systems, Outputs sicher zu verarbeiten.
- **Lösung:** Anwendung des OWASP ASVS (Application Security Verification Standard) für die Output-Validierung.

---

# LLM06: Excessive Agency

## Die drei Dimensionen des Risikos:
1. **Excessive Functionality:** Tools bieten Funktionen an, die für die Aufgabe nicht nötig sind (z. B. "Delete"-Rechte für ein Lese-Tool).
2. **Excessive Permissions:** Das Tool greift mit zu hohen Rechten (z. B. DB-Admin) auf Folgesysteme zu.
3. **Excessive Autonomy:** Aktionen mit hoher Auswirkung werden ohne menschliche Bestätigung ausgeführt.

---

# LLM07: System Prompt Leakage

## Kernproblem:
- System-Prompts steuern das Verhalten, dürfen aber niemals als Sicherheitskontrolle oder Geheimnis-Speicher betrachtet werden.

---

# LLM07: System Prompt Leakage


## Beispiele für Risiken:
- **Interne Regeln:** Preisgabe von Transaktionslimits oder Kreditentscheidungslogiken in Chatbots.
- **Filter-Kriterien:** Offenlegung, nach welchen Regeln das Modell Anfragen ablehnt, was Umgehungen erleichtert.
- **Lösung:** Logik in deterministische, externe Systeme auslagern statt in den Prompt.

---

# LLM08: Vector and Embedding Weaknesses

## Schwachstellen in RAG-Architekturen:
- **Embedding Inversion:** Angreifer können aus den Vektoren (Zahlenwerten) die ursprünglichen Quellinformationen rekonstruieren.
- **Federation Knowledge Conflict:** Widersprüchliche Daten aus verschiedenen Quellen führen zu Fehlentscheidungen des Modells.
- 
---

# LLM08: Vector and Embedding Weaknesses 

## Schutz der Vektordatenbank:
- **Logical Partitioning:** Strenge Trennung der Datensätze verschiedener Nutzerklassen innerhalb der Datenbank.
- **RAG Triad Assessment:** Bewertung von Kontext-Relevanz, Groundedness und Antwort-Relevanz.

---

# LLM09: Misinformation

## Ursachen für Falschinformationen:
- **Halluzinationen:** Statistische Muster füllen Wissenslücken ohne echtes Verständnis.
- **Overreliance:** Nutzer integrieren ungeprüfte KI-Daten in kritische Prozesse.
  
---

# LLM09: Misinformation

## Reale Konsequenzen:
- **Air Canada Fall:** Das Unternehmen wurde haftbar gemacht, nachdem ein Chatbot falsche Rabatt-Versprechen gab.
- **Gefahr durch Paket-Halluzinationen:** KI schlägt nicht existierende Software-Bibliotheken vor; Angreifer registrieren diese Namen mit bösartigem Code.

---

# LLM10: Unbounded Consumption

## Wirtschaftliche und technische Erschöpfung:
- **Denial of Wallet (DoW):** Angreifer verursachen durch massenhafte Anfragen horrende Kosten beim Cloud-Anbieter des Opfers.
- **Model Extraction via API:** Systematisches Abfragen, um die Logik des Modells zu kopieren (Schattenmodell).

---

# LLM10: Unbounded Consumption

## Spezifische Abwehr:
- **Glitch Token Filtering:** Scannen auf bekannte "Glitch-Tokens", die das Modell instabil machen.
- **Graceful Degradation:** Das System sollte unter Last nicht komplett ausfallen, sondern Teilfunktionen beibehalten.

---

# Zusammenfassende Sicherheitsprinzipien

| Prinzip | Beschreibung |
| :--- | :--- |
| **Zero Trust** | Behandle jede Modellausgabe als potenziell bösartig. |
| **Least Privilege** | Minimiere Rechte für Modelle, Tools und APIs. |
| **Human-in-the-Loop** | Kritische Aktionen erfordern menschliche Freigabe. |
| **Monitoring** | Kontinuierliche Überwachung auf Anomalien und Kosten. |


---
<!-- _class: chapter -->
# Sicherheit *durch* KI‑Systeme

---

# KI als Verteidigungswerkzeug
- KI unterstützt Security‑Teams bei:
  - Angriffserkennung (z. B. Korrelationsregeln im SIEM, EDR-Telemetrie)
  - Log‑Analyse (z. B. automatische Priorisierung von Millionen Events pro Tag)
  - Schwachstellensuche (SAST/DAST, IaC‑Scans, Container‑Scans)
  - Incident Response (SOAR‑Playbooks, automatisierte Workflows)
  - Threat Intelligence (Korrelation von Feeds, TTP‑Erkennung)

---

# KI als Verteidigungswerkzeug

- KI ermöglicht:
  - schnellere Reaktionen (Near‑Realtime‑Alerting, Auto‑Containment)
  - bessere Priorisierung (Risikobewertung nach Asset‑Kritikalität, Exploitability)
  - Erkennung unbekannter Muster (z. B. neue Ransomware‑Familien)

---

# 1. Automatisierte Schwachstellensuche
- KI analysiert Code semantisch statt nur syntaktisch.
- Erkennung typischer Schwachstellen:
  - SQL‑Injection
  - Buffer Overflows
  - Unsichere API‑Nutzung
  - Hardcoded Credentials
  - Unsichere Kryptonutzung (z. B. MD5, fehlende Salts)
  - Insecure Deserialization, Path Traversal, XSS, CSRF

---
<!-- class: normal -->
# 1. Automatisierte Schwachstellensuche

## KI kann:
  - Code erklären (z. B. „Was macht diese Funktion sicherheitsrelevant?“)
  - Risiken priorisieren (CVSS‑Abschätzung, Ausnutzbarkeit, Exposure)
  - Fix‑Vorschläge generieren (z. B. Prepared Statements, Input‑Validierung)
  - Security‑Regeln für CI/CD‑Pipelines vorschlagen
## Beispiel:
  - Pull Request wird automatisch mit KI geprüft, die riskante Änderungen markiert
  - Developer erhält direkt im IDE Hinweise + Beispiel‑Patch

---

# Grenzen der KI‑Codeanalyse
- Halluzinationen möglich:
  - KI „erfindet“ Schwachstellen oder Fixes, die technisch nicht passen.
- Falsch‑Positive und Falsch‑Negative:
  - Rauschen in großen Codebasen
  - Kritische Bugs können unentdeckt bleiben.
- Modelle verstehen Code nicht wirklich – sie erkennen Muster.
  - Kein formaler Beweis von Korrektheit oder Sicherheit.

---

# Grenzen der KI‑Codeanalyse

- KI ersetzt keine manuelle Analyse, sondern ergänzt sie.
- Weitere Grenzen:
  - Kontext fehlt (z. B. Laufzeitumgebung, Konfiguration, Infrastruktur)
  - Proprietärer Code/Abhängigkeiten evtl. nicht bekannt
  - Compliance/Datenschutz: Quelltext darf oft nicht extern verarbeitet werden
- Best Practice:
  - KI‑Analyse kombinieren mit manuellen Code Reviews und Pen‑Tests
  - Ergebnisse stichprobenartig verifizieren


---
# 2. Anomalieerkennung im Netzwerk

## Ziel:
  - „Unnormales“ Verhalten im Netzwerk identifizieren, das auf Angriffe hinweisen kann.
## Was als „Normalverhalten“ gelernt wird:
  - Typische Ports, Protokolle, Kommunikationspartner
  - Übliche Arbeitszeiten, Datenvolumen, Zugriffsmuster
  - Typische Kommunikationspfade (Client ↔ Proxy ↔ Internet, Server‑zu‑Server)

---
# 2. Anomalieerkennung im Netzwerk

## Erkennen u. a.:
  - Ungewöhnliche Traffic‑Muster (z. B. plötzlich hohe Datenmengen ins Ausland)
  - Botnet‑Aktivitäten (regelmäßige, kleine „Beaconing“-Verbindungen)
  - Zero‑Day‑Indikatoren (neue Kombination bekannter TTPs)
  - Lateral Movement 
  ## Vorteile:
  - Erkennt auch bisher unbekannte Angriffe („Unknown Unknowns“)
  - Skaliert mit großen Datenmengen besser als rein regelbasierte Systeme
  - Weniger Pflegeaufwand als rein signaturbasierte Erkennung

---

# Anomalieerkennung: Modelltypen & Praxis

- Modelltypen:
  - **Unsupervised** (häufig): Clustering, Autoencoder, Isolation Forest
    - Lernen nur aus Normaldaten, markieren Ausreißer
  - **Supervised** (wenn Labels vorhanden): Klassifikationsmodelle für „malicious vs. benign“
  - **Semi‑supervised**: Mischung, z. B. Normalmodell + einzelne bekannte Angriffe

---

# Anomalieerkennung: Modelltypen & Praxis

## Typische Features:
- Anzahl Verbindungen pro Host/Port/Zeitfenster
- Statistiken zu Paketgrößen und Verbindungsdauer
- Ziel‑Länder, AS‑Nummern, DNS‑Muster
- Kombinationen von Events (z. B. Login‑Versuche + neue Prozesse + neuer Traffic)

---

# Anomalieerkennung: Herausforderungen & Best Practices

## Herausforderungen:
- Viele False Positives zu Beginn („Cold Start“)
- „Concept Drift“: Normalverhalten ändert sich (z. B. neue Anwendungen, Homeoffice)
- Verschlüsselter Traffic erschwert tiefe Inhaltsanalyse
- Datenschutz: Deep‑Inspection kann problematisch sein

---

# Anomalieerkennung: Best Practices

## Best Practices:
  - Pro Segment eigene Baselines (Büro, Produktion, DMZ, Cloud)
  - „Human in the Loop“: SOC‑Analysten verifizieren Alerts, Feedback fließt zurück ins Modell
  - Kombination mit Regeln/Use Cases

  ## Ergebnis:
  - KI‑gestützte Anomalieerkennung ersetzt klassische IDS/IPS nicht, ergänzt sie aber
    und erhöht die Chance, komplexe, mehrstufige Angriffe früh zu erkennen.

---

# 3. Malware‑Erkennung

## Klassifikation basierend auf:
  - Verhalten (z. B. Registry‑Änderungen, Dateizugriffe, Prozessketten)
  - API‑Calls (WinAPI‑Sequenzen, Netzwerkfunktionen)
  - Byte‑Sequenzen (n‑Gramme, Embeddings von Binärcode)
  - Metadaten (Signaturen, Packertypen, Kompressionsverfahren)

---

# 3. Malware‑Erkennung

- KI erkennt polymorphe Malware besser als Signaturen.
  - Varianten derselben Familie werden trotz geändertem Code erkannt.
- Moderne Systeme kombinieren:
  - statische Analyse (Code ohne Ausführung)
  - dynamische Analyse (Sandboxing, Verhaltensbeobachtung)
  - ML‑Modelle (Verdachts‑Score)

---

# 3. Malware‑Erkennung

## Risiken:
  - Angreifer versuchen, ML‑Modelle gezielt zu umgehen (Adversarial ML)
  - False Positives können legitime Software blockieren

---

# 4. Threat Intelligence

## KI kann:
  - Muster erkennen (Cluster von ähnlichen Angriffen)
  - Zusammenhänge herstellen (z. B. gleiche Infrastruktur, gleiche Tools)
  - Berichte automatisiert erstellen (Executive Summaries, IOC‑Listen)
  - Feeds automatisch normalisieren und deduplizieren

## Nutzen:
  - Bessere Entscheidungsgrundlage für Risk‑Owner
  - Schnellere Aktualisierung von Detection‑Use‑Cases

---
# 5. Incident Response

- Ziel:
  - Sicherheitsvorfälle schnell erkennen, eindämmen, analysieren und beheben.
- KI‑gestützte Playbooks:
  - Standardisierte Abläufe für häufige Incidents (z. B. kompromittierter Account,
    Ransomware‑Verdacht, verdächtiger E‑Mail‑Anhang)
  - KI ordnet eingehende Alerts einem Playbook zu und schlägt nächste Schritte vor

---
# 5. Incident Response

- Typische IR‑Phasen (NIST):
  1. Preparation (Vorbereitung)
  2. Detection & Analysis (Erkennung & Analyse)
  3. Containment, Eradication, Recovery (Eindämmung, Beseitigung, Wiederherstellung)
  4. Post‑Incident Activity (Lessons Learned)
- Rolle von KI:
  - Schnelles Sichten großer Log‑Mengen
  - Vorschläge für Hypothesen („Möglicher Angriffsweg: Phishing → Credential Theft → Lateral Movement“)
  - Generierung von Berichten für Management/Behörden

---

# Incident Response: KI in der Praxis

- Beispiele für automatisierte/teilautomatisierte Aktionen:
  - Verdächtiges Benutzerkonto temporär sperren
  - Host aus dem Netzwerk isolieren (EDR/XDR)
  - Firewall‑Regel zum Blockieren verdächtiger IPs erstellen
  - Snapshots/Backups anstoßen, bevor Daten weiter überschrieben werden

---

# Incident Response: KI in der Praxis

- KI‑gestützte Forensik:
  - Log‑Analyse über verschiedene Quellen (EDR, AD, VPN, Cloud, Mail)
  - Aufbau einer Attack Timeline (Wer? Wann? Von wo? Welche Systeme?)
  - Clustering ähnlicher Incidents (z. B. mehrere gleichartige Phishing‑Fälle)
- Best Practice:
  - **Menschliche Freigabe** bei risikoreichen Aktionen (z. B. Massen‑Account‑Sperrung)
  - KI als „Co‑Pilot“ für Analysten, nicht als Autopilot

---

# Anwendungsfeld 6: Phishing‑Erkennung
## NLP‑Modelle analysieren:
  - Schreibstil (Ton, Grammatik, typische Phrasen)
  - URL‑Struktur (Look‑alike‑Domains, Homoglyph‑Angriffe)
  - HTML‑Merkmale (versteckte Links, obfuskiertes JavaScript)
  - Header‑Informationen (SPF/DKIM/DMARC‑Resultate)
## KI erkennt:
  - KI‑generierte Phishing‑Mails
  - Spear‑Phishing (zielgerichtete, personalisierte Angriffe)
  - Domain‑Spoofing und Brand‑Impersonation

---
<!-- _class: chapter -->
# Bedrohungen *durch* KI‑Systeme

---

# KI als Angriffsverstärker – Überblick

## KI kann Angriffe nicht „magisch“ erfinden, aber:
  - senkt Einstiegshürden für weniger erfahrene Angreifer
  - skaliert bekannte Angriffsmuster (Masse + Personalisierung)
  - beschleunigt technische Analysen

---

# KI als Angriffsverstärker – Überblick

## Typische Einsatzfelder:
  - Reconnaissance (Zielaufklärung)
  - Phishing & Social Engineering
  - Malware‑Unterstützung & Exploit‑Recherche
  - Umgehung von Erkennungssystemen (Evasion)
  - Deepfakes & Desinformation
## Wichtig:
  - Dieselben Technologien können defensiv und offensiv eingesetzt werden


---

# KI‑gestützte Reconnaissance (Zielaufklärung)

## Automatisierte Auswertung öffentlich verfügbarer Informationen:
  - Unternehmenswebseiten, Job‑Portale, Social Media, Git‑Repos, Foren
  - Identifikation von Technologien, Versionen, Lieferkettenbeziehungen
## Mögliche Angreifer‑Vorteile:
  - Schnelles Erstellen von „Profilen“:
    - Welche Abteilungen, welche Tools, welche Dienstleister?
    - Potenzielle „High‑Value Targets“ (z. B. Admins, Management)
  - Clustering von Zielen nach Branche, Größe, Technologie‑Stack

---

# KI‑gestützte Reconnaissance (Zielaufklärung)

## KI‑Unterstützung:
  - Text‑ und Metadatenanalyse (NLP)
  - Klassifikation von Systemen/Hosts anhand von Fingerprints
  - Priorisierung von Zielen nach vermutetem Impact
## Relevanz für Verteidigung:
  - Alles, was öffentlich sichtbar ist, kann massiv skaliert ausgewertet werden
  - „Angreifer‑Perspektive“ auf eigene öffentliche Daten wird wichtiger

---

# Phishing & Social Engineering mit generativer KI

## Texte:
  - Grammatikalisch korrekte Mails in vielen Sprachen
  - Anpassung an Branche, Rolle, Firmensprache (Corporate Wording)
  - Erzeugung großer Mengen leicht variierter Phishing‑Mails
## Personalisierung:
  - Nutzung öffentlich verfügbarer Infos (LinkedIn, Xing, Webseite)
  - Zielgerichtete Spear‑Phishing‑Angriffe auf Einzelpersonen/Teams
  - Glaubwürdige interne Nachrichten („IT‑Support“, „HR“, „CFO“)

---

# Phishing & Social Engineering mit generativer KI


## Auswirkungen:
  - Deutlich bessere Qualität als klassische „Scam‑Mails“
  - Erkennungsmerkmale (Rechtschreibung, Stilbrüche) fallen weg
## Verteidigung:
  - Technische Filter reichen allein nicht mehr
  - Security‑Awareness muss typische Muster und Prozesse adressieren
    (z. B. Rückrufkanäle, 4‑Augen‑Prinzip bei Zahlungen)

---

# KI‑Unterstützung für Malware & Exploit‑Entwicklung

## Recherche und Verständnis:
  - Schnelleres Finden von Informationen zu bekannten Schwachstellen
  - Unterstützung beim Verstehen komplexer Sicherheitskonzepte
## Automatisierung:
  - Generierung von Varianten bekannter Angriffsmuster
  - Unterstützung bei der Anpassung von Code an neue Umgebungen

---

# KI‑Unterstützung für Malware & Exploit‑Entwicklung

## Qualität:
  - Bessere Dokumentation, Kommentare, Erklärungen – auch für Angreifer hilfreich
## Grenzen:
  - KI‑Modelle haben Schutzmechanismen gegen offensichtlichen Missbrauch
  - Für komplexe Angriffe wird weiterhin Fachwissen benötigt
## Relevanz für Verteidigung:
  - Angriffe können schneller iterieren (mehr Varianten in kürzerer Zeit)
  - „Patch‑Geschwindigkeit“ und Härtung werden noch wichtiger

---

# Umgehung von Erkennungssystemen (Evasion & Adversarial ML)

## Ziel:
  - Signatur‑ und ML‑basierte Erkennungssysteme so beeinflussen, dass
    bösartige Aktivitäten als harmlos eingestuft werden.
## Beispiele auf Konzept‑Ebene:
  - Modifikation von Dateien/Traffic‑Mustern, um ML‑Modelle zu täuschen
  - gezieltes Einfügen von „Rauschen“, das Features verändert
  - Anpassung von Angriffen, bis die Detektionsschwelle nicht mehr überschritten wird

---

# Umgehung von Erkennungssystemen


## Folgen:
  - KI‑basierte Verteidigungssysteme werden selbst zum Ziel
  - Transparenz und Robustheit von Modellen werden Sicherheitsanforderung
## Verteidigungsaspekt:
  - Defense‑in‑Depth (mehrere, unabhängige Erkennungsebenen)
  - Monitoring auch der Modelle (Drift, ungewöhnliche Outputs)

---

# Deepfakes & Identitätsmissbrauch

- Bild- und Video‑Deepfakes:
  - Erstellung manipulierte Inhalte von Führungskräften, Politiker:innen etc.
  - Einsatz für Erpressung, Betrug oder Reputationsschädigung
- Voice‑Cloning:
  - Nachahmung von Stimmen in Telefonaten/Sprachnachrichten
  - Kombination mit Social Engineering („Chef ruft an und fordert Überweisung“)

---

# Deepfakes & Identitätsmissbrauch

- Business Email/Communication Compromise:
  - Kombination aus glaubwürdigen Texten, Deepfakes und gefälschten Domains
  - Steigerung der Erfolgswahrscheinlichkeit bei CEO‑Fraud‑Szenarien
- Auswirkungen:
  - Klassische „Vertrauensanker“ (Stimme, Video, Bild) werden unsicher
- Verteidigung:
  - Stärkere Prozess‑ und Identitätsprüfungen (z. B. Rückruf unter bekannter Nummer,
    interne Codes, MFA, 4‑Augen‑Prinzip)
  - Medienkompetenz: „Seeing is no longer believing“

---

# KI in Desinformation & Einflussoperationen

- Content‑Skalierung:
  - Automatisierte Generierung großer Mengen von Posts/Kommentaren/Artikeln
  - Viele leicht unterschiedliche Versionen derselben Botschaft
- Zielgerichtete Kampagnen:
  - Anpassung an bestimmte Zielgruppen (Sprache, Themen, Emotionen)
  - Nutzung von Plattform‑Eigenheiten (Hashtags, Trends)

---

# KI in Desinformation & Einflussoperationen

- Bot‑Netzwerke:
  - KI‑gesteuerte Accounts, die menschliches Verhalten imitieren
  - Verstärkung bestimmter Narrative („Astroturfing“)
- Risiken:
  - Beeinflussung von öffentlicher Meinung, Märkten, Wahlen
  - Vertrauensverlust in Medien und Institutionen
- Relevanz für Organisationen:
  - Reputationsrisiken, gezielte Desinformationskampagnen gegen Unternehmen
  - Notwendigkeit von Monitoring und Krisenkommunikations‑Strategien

---

# Einordnung: Was bedeutet das für die Praxis?

- KI macht:
  - bekannte Angriffe schneller, skalierbarer, glaubwürdiger
  - Einstieg in bestimmte Angriffsformen einfacher
- Aber:
  - KI ersetzt keine grundlegenden Angriffskenntnisse
  - starke Verteidigung kann KI ebenfalls nutzen (Detektion, Analyse, Response)

---

# Einordnung: Was bedeutet das für die Praxis?

- Konsequenzen für Organisationen:
  - Security‑Awareness & AI‑Literacy aufbauen
  - Prozesse robuster machen (Identitätsprüfung, Freigaben, Notfallpläne)
  - Technische Kontrollen weiterentwickeln (KI‑basierte Defense, Monitoring, Logging)
- Kerngedanke:
  - KI ist ein *Multiplikator* – für Angreifer und Verteidiger.
  - Ziel: dafür sorgen, dass die Verteidigungsseite den größeren Hebel hat.