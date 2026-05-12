---
marp: true
theme: custom
footer: ![w:280](img/dhbw-ka.svg)
transition: slide

---
<!-- _class: title -->

# IoT

## Sicherheit im Internet der Dinge

---
<!-- _class: chapter -->


# Stuxnet – Anatomie einer Cyber-Waffe

## Fallstudie zur Sicherheit Cyber-Physischer Systeme

---
<!-- 
Stuxnet markiert den Übergang von der rein digitalen Spionage zur physischen Sabotage, oft als „Kinetische Cyber-Operation“ bezeichnet. Das Besondere war die Überwindung des Air Gaps – also die physische Isolierung des Zielnetzwerks vom öffentlichen Internet. Die Komplexität des Codes (15.000 Zeilen) resultierte daraus, dass Stuxnet nicht nur eine Malware war, sondern ein ganzes Toolkit. Der Einsatz von vier Zero-Day-Exploits gleichzeitig war bis dato beispiellos; normalerweise wird eine solche kostbare Schwachstelle einzeln genutzt, um ihre Entdeckung so lange wie möglich hinauszuzögern. Die Urheberschaft wird im Rahmen der „Operation Olympic Games“ staatlichen Akteuren zugeschrieben, da die Entwicklung Millionen von Dollar und Zugriff auf die exakte Hardware der Zielanlage erforderte.
-->
# Warum Stuxnet alles veränderte

- **Historischer Kontext:**
  - Früher: Vandalismus, Datendiebstahl, Botnetze (virtuelle Ziele).
  - Stuxnet: Cyber-Physischer Angriff (kinetische Ziele).
- **Das Ziel:**
  - Sabotage der Urananreicherung in Natanz (Iran).
  - Vermuteter Akteur: "Operation Olympic Games" (USA/Israel).
- **Technische Komplexität:**
  - 15.000 Zeilen Code.
  - Einsatz von 4 Zero-Day-Exploits gleichzeitig.
  - Überwindung von "Air Gaps" (isolierte Netzwerke).

---
<!--

In der Industrieautomatisierung (OT - Operational Technology) nutzen wir das SCADA-Modell. 

Ein SCADA-Modell (Supervisory Control and Data Acquisition) ist ein strukturelles Rahmenwerk aus Hardware- und Softwarekomponenten, das zur Überwachung, Steuerung und Datenerfassung industrieller Prozesse dient. Es fungiert als zentrales System, das Daten von Feldgeräten (wie Sensoren und Aktuatoren) sammelt, in Echtzeit analysiert und über eine Mensch-Maschine-Schnittstelle (HMI) für Bediener visualisiert.

In Natanz wurden Gaszentrifugen vom Typ IR-1 verwendet, um Uran-Isotope zu trennen. Die Steuerungskette ist hierbei entscheidend:

HMI (Human Machine Interface): Windows-PCs mit Siemens WinCC dienen zur Visualisierung.

Engineering: Die Software STEP 7 wird genutzt, um die Logik der Steuerung zu programmieren.

PLC/SPS: Die Siemens S7-315 ist das Gehirn der Maschine. Sie verarbeitet Echtzeitdaten.

Aktoren: Frequenzumrichter (hier von Vacon und Fararo Paya) wandeln die digitalen Befehle der SPS in die elektrische Frequenz um, die die Motoren der Zentrifugen antreibt.

-->
# Architektur des Ziels: Natanz & SCADA

- **Der Prozess:**
  - Urananreicherung mittels Gaszentrifugen (Typ IR-1).
  - Kritischer Faktor: Drehzahl (1.064 Hz) & Resonanzfrequenz.
- **Die Steuerungshierarchie:**
  - **HMI (Level 1):** Siemens WinCC (Windows PCs).
  - **Engineering (Level 2):** Siemens STEP 7 Software.
  - **PLC / SPS (Level 3):** Siemens S7-315 CPUs.
  - **Aktoren (Level 4):** Frequenzumrichter (Vacon, Fararo Paya).

---

# Infektion: Der LNK-Exploit (CVE-2010-2568)

<!--
Da die Anlage nicht am Internet hing, war der USB-Stick das Trojanische Pferd. Stuxnet nutzte eine Schwachstelle im Windows-Dateisystem-Handler für Verknüpfungsdateien (.LNK). Wenn Windows ein Icon für eine solche Datei anzeigen will, parst die shell32.dll die Datei. Durch eine manipulierte Struktur innerhalb der .LNK-Datei wurde Windows dazu verleitet, eine bösartige .CPL-Datei (Control Panel) via LoadLibrary in den Speicher zu laden. Dies geschah automatisch beim Öffnen des Ordners im Explorer – ohne dass der Nutzer die Datei anklicken musste.
-->

- **Problem:** Zielanlage hat keine Internetverbindung ("Air Gap").
- **Vektor:** Infizierte USB-Wechseldatenträger.
- **Die Schwachstelle:**
  - Windows Shell parst `.LNK` (Verknüpfungs)-Dateien.
  - Fehler beim Laden des Icons aus einer manipulierten `.CPL`-Datei.
  - **Resultat:** Code-Ausführung (`LoadLibrary`) sobald der Ordner angezeigt wird.
- **Kein Klick durch Benutzer notwendig!**


---

# Verbreitung im lokalen Netzwerk

<!--
Einmal im Büro-Netzwerk der Anlage, musste Stuxnet die Rechner finden, auf denen die Siemens STEP 7 Software installiert war. Hierzu nutzte es unter anderem den Print Spooler Exploit (MS10-061). Durch eine RPC-Anfrage (Remote Procedure Call) konnte ein Angreifer eine Datei in das System32-Verzeichnis eines entfernten Rechners schreiben, die dann mit SYSTEM-Privilegien ausgeführt wurde. Zusätzlich wurde ein Peer-to-Peer (P2P)-Update-Mechanismus implementiert: Infizierte Maschinen tauschten über das Netzwerk Versionen aus, um sicherzustellen, dass überall die aktuellste Version der Malware aktiv war.
-->

- **Ziel:** Finden der Engineering Workstations (mit STEP 7).
- **Methode 1: Print Spooler Exploit (MS10-061)**
  - Zero-Day-Lücke im Druckdienst.
  - Senden einer Datei via RPC in `System32`.
  - Ausführung mit `SYSTEM`-Rechten.
- **Methode 2: MS08-067 (Conficker)**
  - Fallback auf bekannte Lücke (falls System ungepatcht).
- **P2P-Update:**
  - Infizierte Rechner vergleichen Versionen via RPC.
  - Neuester Code verteilt sich organisch im LAN.

---

# Persistence & Stealth: Kernel Rootkits

<!--
Um auf modernen Windows-Systemen dauerhaft zu überleben, installierte Stuxnet Treiber im Kernel-Modus. Da Windows nur signierte Treiber akzeptiert, nutzten die Angreifer gestohlene digitale Zertifikate der Hardware-Hersteller Realtek und JMicron. Das Rootkit fungierte als Filesystem Filter Driver (mrxcls.sys). Es klinkte sich in die Systemaufrufe (Hooks auf IRP_MJ_DIRECTORY_CONTROL) ein. Wenn ein Antivirenprogramm oder ein Nutzer die Dateiliste des USB-Sticks oder der Festplatte anforderte, fing das Rootkit diese Anfrage ab und entfernte die Stuxnet-Dateien aus der Liste, bevor die Antwort den Nutzer erreichte.
-->

- **Privilege Escalation:**
  - Exploits in `win32k.sys` & Task Scheduler für Kernel-Rechte.
- **Treiber-Signierung (Der Vertrauensbruch):**
  - Windows verlangt signierte Treiber.
  - **Lösung:** Gestohlene private Schlüssel von **Realtek** und **JMicron**.
- **Funktion des Rootkits (`mrxcls.sys`):**
  - Filesystem Filter Driver.
  - Hooked Systemaufrufe (`IRP_MJ_DIRECTORY_CONTROL`).
  - Filtert eigene Dateien (`.LNK`, `.DLL`) aus der Dateiliste heraus.

---

# Die Brücke zur Hardware: DLL Hijacking

<!--
Der Übergang von der IT (Windows) zur OT (SPS) erfolgte über die Datei s7otbxdx.dll. Dies ist die Standardbibliothek, die STEP 7 nutzt, um Code auf die Siemens-Steuerungen zu schreiben. Stuxnet benannte das Original um und setzte eine eigene, bösartige Version an deren Stelle. Dies ist ein klassischer Man-in-the-Middle-Angriff innerhalb der Software-Architektur:

Wenn der Ingenieur ein SPS-Programm auf die Steuerung laden wollte, fügte Stuxnet heimlich seinen eigenen Schadcode hinzu.

Wenn der Ingenieur den Code von der SPS zur Kontrolle wieder auslesen wollte, entfernte die Stuxnet-DLL den Schadcode in Echtzeit aus dem Datenstrom, sodass im Editor nur der "saubere" Originalcode erschien.
-->

- **Zielsoftware:** Siemens STEP 7 (Engineering Tool).
- **Wrapper-Technik:**
  - Original `s7otbxdx.dll` (Kommunikation zur SPS) wird umbenannt.
  - Stuxnet-DLL ersetzt das Original.
- **Man-in-the-Middle (Engineering Station):**
  - **Write-Befehle:** Injiziert Schadcode in legitime SPS-Programme.
  - **Read-Befehle:** Entfernt Schadcode aus der Rückgabe.
- **Resultat:** Ingenieur sieht sauberen Code ("Green Screen"), während Malware läuft.

---

# In der Steuerung (S7-315)

<!--
Innerhalb der SPS infizierte Stuxnet die Organisationsbausteine (OB). Der OB1 ist der Hauptzyklus, der ständig durchlaufen wird. Der OB35 ist ein Weckalarm-Baustein, der in festen Intervallen (hier 100ms) die normale Logik unterbricht, um zeitkritische Aufgaben zu erledigen. Stuxnet nutzte diese Bausteine, um seine Schadroutinen mit höchster Priorität auszuführen. Bevor der Angriff startete, prüfte die Malware über den Profibus-Feldbus, ob die angeschlossene Hardware (Frequenzumrichter) exakt den Vendor-IDs der iranischen Anlage entsprach und ob die aktuelle Betriebsfrequenz zwischen 807 Hz und 1210 Hz lag. Nur bei einem Treffer wurde der Zerstörungs-Algorithmus aktiviert.
-->

- **Injektionsziel:** Organisationsbausteine (OB).
  - **OB1:** Hauptzyklus (Standard-Logik).
  - **OB35:** Zeitkritischer Interrupt (alle 100ms) – Priorität!
- **Hardware-Fingerprinting (Safety Checks):**
  - Scannt Profibus nach spezifischen Vendor-IDs (Vacon, Fararo Paya).
  - Prüft Betriebsfrequenz (807 Hz – 1210 Hz).
- **Logik:** Angriff erfolgt NUR, wenn exakt diese Konfiguration gefunden wird.
- **Verhindert Kollateralschäden** in falschen Fabriken.

---

# Der Zerstörungs-Algorithmus

<!--
Vertiefte technische Erklärung:Der Angriff nutzte physikalische Gesetzmäßigkeiten aus, insbesondere die Resonanzfrequenz. Jedes rotierende Objekt hat eine Eigenfrequenz, bei der sich Schwingungen extrem verstärken:
$$f_n = \frac{1}{2\pi}\sqrt{\frac{k}{m}}$$

Stuxnet manipulierte die Frequenzumrichter in einem präzisen Zyklus:Overspeed: Die Frequenz wurde kurzzeitig auf 1.410 Hz erhöht. Dies führte dazu, dass sich die Aluminium-Rotoren der Zentrifugen durch die Fliehkraft leicht ausdehnten und am Gehäuse schleiften ("Scraping").Recovery: Danach folgten 27 Tage Normalbetrieb, um Misstrauen zu vermeiden.Underspeed: Zum Schluss wurden die Zentrifugen fast bis zum Stillstand (2 Hz) abgebremst. Dabei mussten sie zwangsläufig ihre kritischen Resonanzbereiche durchlaufen. Die dabei entstehenden Vibrationen führten zur mechanischen Zerstörung der Lager und der Rotoren.
-->

- **Ziel:** Mechanische Resonanzkatastrophe der Zentrifugen.
- **Ablauf (Dauer ca. 1 Monat):**
  - **Monitoring:** Aufzeichnen von Normalwerten.
  - **Overspeed:** Hochfahren auf 1.410 Hz (15 Min).
    - *Effekt:* Dehnung des Aluminium-Rotors ("Scraping").
  - **Recovery:** 27 Tage Normalbetrieb (Tarnung).
  - **Underspeed:** Abbremsen auf 2 Hz (50 Min).
    - Langsames Durchfahren kritischer Resonanzfrequenzen.
    - Resultat: Starke Vibrationen / Zersplittern.

---

# Blindmachen der Überwachung

<!--
Um zu verhindern, dass die Sensoren der Anlage Alarm schlugen, führte Stuxnet eine Replay-Attacke auf der Ebene der SPS aus. Die Malware fing die Kommunikation der Funktion DP_RECV ab, welche die Daten vom Profibus (die Ist-Werte der Zentrifugen) empfängt. Während des Angriffs wurden die echten, alarmierenden Sensordaten (Vibrationen, Drehzahländerungen) verworfen. Stattdessen speiste Stuxnet eine zuvor aufgezeichnete 21-sekündige Schleife von absolut unauffälligen Betriebsdaten ein. Das HMI zeigte somit eine perfekte, konstante Drehzahl von 1.064 Hz an, während die Hardware bereits auseinanderfiel.
-->

- **Problem:** HMI würde Vibrationen/Drehzahlfehler anzeigen -> Notabschaltung.
- **Lösung: Replay Attacke auf der SPS.**
- **Technik:**
  - Abfangen der Funktion `DP_RECV` (Profibus-Eingang).
  - Während des Angriffs: Einspielen aufgezeichneter "Safe Data" (21 Sek. Loop).
- **Effekt:**
  - Kontrollraum sieht "glatte Linie" (1.064 Hz).
  - Sicherheitsroutinen der SPS werden mit falschen Daten gefüttert und greifen nicht ein.

---

# Impact & Lessons Learned

- **Schaden:**
  - Ca. 1.000 Zentrifugen zerstört.
  - Massive Verunsicherung und Verzögerung des iranischen Programms.
- **Lehren für IT-Sicherheit:**
  - **Air Gaps sind nicht dicht:** Physischer Zugriff (USB) ist ein Vektor.
  - **IT/OT Konvergenz:** Angriffe starten im Büro (Windows) und enden in der Fabrik (SPS).
  - **Secure by Design:** SPS führte unsignierten Code aus (heute: Secure Boot).
  - **Defense in Depth:** Perimeter-Schutz reicht nicht; interne Überwachung nötig.

---

<!-- _class: chapter -->

# Sicherheit im Smart Home

## Wenn der Kühlschrank einen Server angreift

---

# Agenda

<style scoped>
li { line-height: 1.7; font-size: 24pt}
</style>

1. Smart Home – Überblick  
2. Warum Smart‑Home‑Sicherheit kritisch ist  
3. Typische Schwachstellen  
4. Netzwerksicherheitsprinzipien  
5. Best Practices  
6. Fallbeispiel: Mirai‑Botnet
7. Fazit

---

<!-- _class: biglist -->

# Was ist ein Smart Home?

<!--
Ein Smart Home ist ein Ökosystem aus heterogenen Geräten, die über verschiedene Protokollstacks kommunizieren. Während WLAN (IEEE 802.11) für bandbreitenintensive Geräte wie Kameras genutzt wird, setzen energieeffiziente Sensoren oft auf Mesh-Netzwerke wie Zigbee (basiert auf IEEE 802.15.4) oder Z-Wave. Diese Protokolle arbeiten häufig im 2,4-GHz- oder 868-MHz-Band und benötigen ein Gateway (Hub), um die Brücke zum IP-basierten Heimnetzwerk und zur Cloud zu schlagen. Die Kommunikation erfolgt dabei oft über das MQTT-Protokoll (Message Queuing Telemetry Transport), einem leichtgewichtigen Publish/Subscribe-Messaging-Protokoll, das ideal für instabile Netzwerkverbindungen ist.
-->

- Vernetzte Geräte im privaten Haushalt  
- Beispiele:
  - Smart Speaker  
  - IP‑Kameras  
  - Smarte Thermostate  
  - Türschlösser  
  - Lichtsysteme  
- Kommunikation über WLAN, Zigbee, Z‑Wave, Bluetooth, IP

---
<!-- _class: biglist -->

# Warum ist Smart‑Home‑Sicherheit kritisch?
<!--
Die Kritikalität ergibt sich aus der IT/OT-Konvergenz im privaten Bereich. Smart-Home-Geräte sind oft "Always-on" und direkt mit Cloud-Backends verbunden, was sie zu idealen Zielen für persistente Bedrohungen macht. Da viele Hersteller aus dem Consumer-Bereich kommen, fehlt oft ein Security Lifecycle Management; Geräte werden auf den Markt geworfen, ohne dass langfristige Sicherheitsupdates eingeplant sind. Ein kompromittiertes Gerät (z. B. ein smarter Kühlschrank) dient Angreifern als Pivot-Punkt, um Firewalls von innen zu umgehen und auf sensible Systeme wie NAS-Speicher oder Arbeitslaptops im selben Subnetz zuzugreifen.
-->
- Geräte oft dauerhaft online  
- Viele Hersteller mit sehr unterschiedlichem Sicherheitsniveau  
- Geräte werden selten aktualisiert  
- Direkter Einfluss auf Privatsphäre & physische Sicherheit  
- Angreifer nutzen Smart‑Home‑Geräte als Einstiegspunkt ins Heimnetz

---
<!-- _class: biglist -->
# Typische Schwachstellen im Smart Home
<!--
Ein technisches Hauptproblem ist das Protokoll UPnP (Universal Plug and Play). Es erlaubt Geräten, eigenständig Port-Weiterleitungen im Router zu erstellen, was oft ungewollte Löcher in die Firewall reißt. Hinzu kommt die fehlende Transportverschlüsselung im lokalen Netzwerk; viele günstige IoT-Geräte übertragen Statusmeldungen oder Passwörter im Klartext via HTTP statt HTTPS. Auch die Authentifizierung bei Begleit-Apps ist oft schwach implementiert, was Brute-Force-Angriffe auf Cloud-Konten ermöglicht, über die dann das gesamte Zuhause gesteuert werden kann.
-->
- Standardpasswörter  
- Unsichere Cloud‑Anbindungen  
- Unverschlüsselte lokale Kommunikation  
- Fehlende Updates  
- Unsichere WLAN‑Konfiguration  
- Offene Ports / UPnP  
- Schwache Authentifizierung bei Apps

---
<!-- _class: biglist -->
# Bedrohungen im Smart Home
<!--
Neben dem Missbrauch für DDoS-Attacken (wie bei Mirai) steht die Privatsphäre im Fokus. Durch Man-in-the-Middle-Angriffe (MitM) können Angreifer unverschlüsselten Datenverkehr abfangen oder manipulieren. Eine besonders kritische Bedrohung ist der Identitätsdiebstahl über Cloud-Konten: Da viele Nutzer dieselben Passwörter für den Staubsauger-Roboter wie für ihr E-Mail-Konto verwenden, führt ein Leak beim Hersteller oft zur Kompromittierung der gesamten digitalen Identität. Zudem ermöglichen Schwachstellen in der lokalen API oft das unbefugte Entriegeln von smarten Türschlössern ohne physische Spuren.
-->
- **Botnet‑Infektionen** (z. B. Mirai)  
- **DDoS‑Missbrauch**  
- **Ausspähen von Kameras**  
- **Manipulation von Türschlössern**  
- **Man‑in‑the‑Middle‑Angriffe**  
- **Identitätsdiebstahl über Cloud‑Konten**

---
<!-- _class: biglist -->
# Netzwerksicherheitsprinzipien für Smart Homes
<!--
Die Abwehr basiert auf drei Säulen:

Zero Trust: Kein Gerät innerhalb des Netzwerks erhält implizites Vertrauen. Jede Kommunikationsanfrage muss authentifiziert werden.

Least Privilege: Ein smarter Thermostat benötigt Zugriff auf den Zeitserver und die Cloud des Herstellers, aber niemals Zugriff auf die SMB-Freigaben des Familien-PCs.

Defense in Depth: Sicherheit darf nicht nur an der Peripherie (Router) stattfinden, sondern muss auf Geräteebene (Verschlüsselung) und Netzwerkebene (Segmentierung) greifen.
-->
- **Zero Trust**: Kein Gerät automatisch vertrauenswürdig  
- **Segmentierung**: IoT‑Geräte in eigenes WLAN/VLAN  
- **Least Privilege**: Minimale Berechtigungen  
- **Verschlüsselung**: WLAN & Protokolle  
- **Monitoring**: Netzwerkverkehr beobachten  
- **Secure‑by‑Design**: Geräteauswahl nach Sicherheitskriterien

---

# Netzwerksegmentierung im Smart Home
<!--
Technisch wird dies meist über VLANs (Virtual Local Area Networks) oder ein separates Gast-WLAN realisiert. In einem VLAN-Szenario wird der Netzwerkverkehr auf Layer 2 logisch getrennt. Eine Firewall (auf Layer 3/4) regelt dann mittels Access Control Lists (ACLs), welche Pakete zwischen dem "IoT-VLAN" und dem "Privat-VLAN" fließen dürfen. So kann man beispielsweise erlauben, dass das Smartphone (Privat-Netz) den Fernseher (IoT-Netz) steuert, aber der Fernseher keine Verbindung zum Smartphone initiieren kann.
-->
### Warum?

- Schadensbegrenzung  
- Schutz sensibler Geräte (PC, NAS)  
- Kontrolle des Datenflusses

### Wie?

- Gast‑WLAN für IoT  
- VLAN‑fähige Router  
- Firewall‑Regeln zwischen Netzen  
- Deaktivieren unnötiger Dienste (UPnP)

---
<!-- _class: biglist -->

# Sichere Kommunikation
<!--
Auf der physikalischen Ebene ist WPA3 der aktuelle Standard, der durch das SAE-Verfahren (Simultaneous Authentication of Equals) deutlich resistenter gegen Offline-Wörterbuchangriffe ist als WPA2. Für die Applikationsebene ist TLS (Transport Layer Security) zwingend erforderlich, insbesondere bei der Nutzung von MQTT über TLS (Port 8883). Für den Fernzugriff sollte auf Port-Weiterleitungen verzichtet werden; stattdessen ist ein VPN (Virtual Private Network) auf Basis von WireGuard oder OpenVPN zu bevorzugen, um einen verschlüsselten Tunnel direkt ins Heimnetz zu legen.
-->
- WPA3 oder mindestens WPA2  
- Deaktivieren von WPS  
- TLS/HTTPS für Cloud‑Zugriffe  
- VPN für Fernzugriff  
- Sichere Protokolle:
  - MQTT über TLS  
  - HTTPS statt HTTP

---
<!-- _class: biglist -->
# Authentifizierung & Identitätsmanagement
<!--
Sichere Systeme nutzen geräte-spezifische Zertifikate (X.509), die während der Fertigung in einem Secure Element oder TPM (Trusted Platform Module) im Gerät gespeichert werden. Dies verhindert das Klonen von Identitäten. Für den Nutzer ist die Multi-Faktor-Authentifizierung (MFA) via TOTP (Time-based One-Time Password) oder FIDO2 der wichtigste Schutz gegen Account-Takeover, falls die Zugangsdaten durch Phishing oder Leaks bekannt werden.
-->
- Starke Passwörter  
- Multi‑Factor Authentication für Cloud‑Konten  
- Geräte‑Zertifikate (wenn unterstützt)  
- Regelmäßige Passwortrotation  
- Keine Passwort‑Wiederverwendung

---
<!-- _class: biglist -->
# Firmware‑ und Patch‑Management
<!--
Sicheres Patch-Management erfordert signierte Firmware. Das Gerät prüft dabei mittels eines im Bootloader hinterlegten öffentlichen Schlüssels die kryptografische Signatur des Updates. Nur wenn die Signatur gültig ist, wird das Update installiert. Dies verhindert, dass Angreifer manipulierte Firmware-Images aufspielen können. Zudem sollte der Update-Prozess über eine verschlüsselte HTTPS-Verbindung erfolgen, um Rollback-Angriffe (das Erzwingen einer alten, verwundbaren Version) zu verhindern.
-->
- Automatische Updates aktivieren  
- Geräte regelmäßig prüfen  
- Nur Hersteller mit gutem Update‑Support wählen  
- Alte oder unsichere Geräte ersetzen  
- Signierte Firmware bevorzugen

---
<!-- _class: biglist -->
# Monitoring & Anomalieerkennung
<!--
Ein IDS (Intrusion Detection System) analysiert den Netzwerkverkehr auf bekannte Angriffsmuster. Im Smart Home ist besonders die Anomalieerkennung effektiv: Wenn ein Gerät, das normalerweise nur kleine Pakete (Status-Updates) sendet, plötzlich beginnt, Port-Scans im lokalen Netz durchzuführen oder massenhaft UDP-Pakete an externe IPs schickt, deutet dies auf eine Botnet-Infektion hin. Moderne Router nutzen hierfür Deep Packet Inspection (DPI), um den Inhalt der Pakete auf Protokollkonformität zu prüfen.
-->
- Router‑Logs prüfen  
- Ungewöhnliche Datenraten erkennen  
- Geräte, die plötzlich „nach Hause telefonieren“  
- IDS/IPS in modernen Routern nutzen  
- Benachrichtigungen bei neuen Geräten im Netzwerk

---

<!-- _class: chapter -->

# Mirai‑Botnet  

## DDoS durch Haushaltsgeräte

---
<!-- _class: biglist -->
# Fallbeispiel: Mirai‑Botnet  

<!--
Mirai (japanisch für „Zukunft“) erlangte 2016 Berühmtheit, als es Teile des Internets durch massive Angriffe auf den DNS-Dienstleister Dyn lahmlegte. Es markiert einen Wendepunkt, da es demonstrierte, wie eine riesige Armee von technologisch schwachen Geräten (IP-Kameras, DVRs, Heimrouter) in der Masse eine gewaltige Schlagkraft entwickelt. Da diese Geräte oft direkt am Internet hängen und selten durch Firewalls geschützt sind, bilden sie eine ideale, globale Infrastruktur für Angreifer.
-->

## Warum ist Mirai relevant für Smart Homes?

- Infizierte vor allem **Heimrouter, IP‑Kameras, DVRs**  
- Nutzt **Standardpasswörter** und **offene Telnet/SSH‑Ports**  
- Wurde für massive DDoS‑Angriffe eingesetzt  
- Zeigt, wie leicht schlecht gesicherte Geräte kompromittiert werden

---
<!-- _class: biglist -->
# Mirai – Technischer Überblick

<!--
echnisch gesehen ist Mirai in C geschrieben und darauf optimiert, auf verschiedenen CPU-Architekturen (ARM, MIPS, x86) zu laufen, die in IoT-Geräten üblich sind. Der Infektionsvektor ist denkbar einfach: Die Malware scannt das Internet nach offenen Telnet-Ports (Port 23) oder SSH-Ports (Port 22). Sobald eine Verbindung steht, führt sie eine Wörterbuch-Attacke mit einer fest kodierten Liste von nur etwa 60 Standard-Anmeldedaten durch (z. B. admin:admin, root:12345). Diese Effizienz beim Scannen ermöglichte es Mirai, innerhalb von Stunden hunderttausende Geräte zu infizieren.
-->

- Programmiert in C  
- Ziel: IoT‑Geräte mit schwacher Authentifizierung  
- Vorgehensweise:
  - Scannen des Internets nach offenen Ports (v. a. 23/Telnet)  
  - Versuch von Standard‑Login‑Kombinationen  
  - Nach erfolgreichem Login: Nachladen der Malware  
  - Gerät wird Teil des Botnets

---

# Mirai – Architektur

<!--
Die Architektur von Mirai ist hochgradig modular und verteilt:

Bots: Die infizierten Geräte selbst. Sie führen die eigentlichen Scans und Angriffe aus.

Command-and-Control (C2): Ein zentraler Server, von dem die Bots Befehle (Ziel-IP, Angriffsart) entgegennehmen.

Loader: Wenn ein Bot ein neues Opfer findet, meldet er die IP und die Zugangsdaten an den Loader-Server. Dieser lädt dann die für die jeweilige Prozessorarchitektur passende Binärdatei auf das neue Opfer nach.

Scan-Module: Diese nutzen asynchrone Sockets, um extrem schnell tausende IP-Adressen gleichzeitig zu prüfen, ohne auf eine Antwort warten zu müssen (stateless scanning).
-->

- **Bots**  
  - Infizierte IoT‑Geräte  
  - Führen DDoS‑Angriffe aus  
- **Command‑and‑Control (C2)‑Server**  
  - Steuert Bots  
  - Gibt Angriffsbefehle  
- **Loader‑Server**  
  - Installiert Mirai auf neuen Geräten  
- **Scan‑Module**  
  - Finden neue Opfergeräte

---

# Mirai – Angriffstechniken

<!--
Mirai beherrscht verschiedene DDoS-Methoden auf unterschiedlichen Ebenen des OSI-Modells:

TCP SYN Flood (Layer 4): Hierbei werden massenhaft SYN-Pakete gesendet, um den Three-Way-Handshake des Zielservers zu initiieren, aber nie abzuschließen. Dies verbraucht alle verfügbaren Verbindungsressourcen (Backlog).

UDP Flood (Layer 4): Es werden massenhaft UDP-Pakete an zufällige Ports des Opfers gesendet. Der Server muss für jedes Paket prüfen, ob eine Anwendung lauscht, und mit einem "ICMP Destination Unreachable" antworten, was die Bandbreite und CPU sättigt.

HTTP GET Flood (Layer 7): Diese Attacke simuliert echte Nutzeranfragen an eine Webseite. Da die Verarbeitung einer Datenbankabfrage auf dem Server deutlich mehr Ressourcen verbraucht als das Senden des Requests auf Angreiferseite, bricht der Webdienst unter der Last zusammen.
-->

- **Brute‑Force über Telnet**  
- **DDoS‑Methoden**:
  - TCP SYN Flood  
  - UDP Flood  
  - HTTP GET Flood  
- **Persistenz**:
  - Mirai lebt nur im RAM  
  - Neustart entfernt die Malware  
  - Aber Geräte werden schnell erneut infiziert, wenn Schwachstelle bleibt

---
<!-- _class: biglist -->
# Mirai – Warum war es so erfolgreich?

<!--
Der Erfolg beruhte auf der ökonomischen Asymmetrie: Ein Angreifer kann mit minimalem Aufwand Millionen Geräte scannen, während der Schutz jedes einzelnen Geräts in der Verantwortung von Millionen technisch unbedarfter Nutzer liegt. Viele dieser Geräte besitzen zudem keine Update-Funktion für die Firmware. Ein weiterer Faktor ist die Persistenz: Obwohl Mirai nur im flüchtigen Arbeitsspeicher (RAM) lebt und ein Neustart die Malware löscht, ist die Re-Infektionsrate so hoch, dass das Gerät oft in weniger als zwei Minuten nach dem Reboot erneut übernommen wird, solange das Standardpasswort aktiv ist.
-->

- Millionen IoT‑Geräte mit Standardpasswörtern  
- Hersteller ohne Update‑Mechanismen  
- Nutzer ohne Sicherheitsbewusstsein  
- Geräte dauerhaft online  
- Geringe Rechenleistung → schwer zu schützen

---
<!-- _class: biglist -->
# Schutz vor Mirai & ähnlichen Botnets

<!--
Die wichtigste Maßnahme ist das Härten der Konfiguration: Standardpasswörter müssen zwingend durch komplexe, individuelle Passwörter ersetzt werden. Technisch gesehen sollten Dienste wie Telnet komplett deaktiviert und durch verschlüsseltes SSH mit Key-basierter Authentifizierung ersetzt werden. Auf Netzwerkebene hilft das Deaktivieren von UPnP am Router, damit Geräte sich nicht selbstständig nach außen öffnen können. Zudem sollten IoT-Geräte in einem isolierten VLAN ohne Zugriff auf das restliche Heimnetzwerk betrieben werden (Segmentierung).
-->

- Standardpasswörter ändern  
- Telnet/SSH deaktivieren  
- Firmware aktualisieren  
- IoT‑Geräte in isoliertes Netz  
- UPnP deaktivieren  
- Router‑Firewall aktivieren  
- Geräte nur von vertrauenswürdigen Herstellern kaufen

---

# Fazit

<!--
Stuxnet und Mirai zeigen zwei Seiten derselben Medaille: Stuxnet war ein hochspezifisches Präzisionsinstrument für physische Sabotage, während Mirai eine "Massenwaffe" aus unsicheren Alltagsgegenständen schuf. Beide unterstreichen, dass Sicherheit im IoT keine Option, sondern eine Notwendigkeit ist. Die Zukunft liegt in "Secure by Design"-Ansätzen, bei denen Geräte bereits ab Werk mit deaktivierten unsicheren Diensten, individuellen Passwörtern und funktionierenden Update-Mechanismen ausgeliefert werden.
-->

- Smart‑Home‑Geräte erweitern die Angriffsfläche  
- Netzwerksicherheit ist entscheidend  
- Mirai zeigt, wie gefährlich unsichere IoT‑Geräte sind  
- Kombination aus:
  - Sicheren Geräten  
  - Netzwerkarchitektur  
  - Updates  
  - Nutzerbewusstsein  
- Sicherheit ist ein kontinuierlicher Prozess

