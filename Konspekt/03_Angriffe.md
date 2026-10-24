# Kapitel 3 — Angriffstechniken und Netzwerksicherheit
**Skript S. 75–98 (PDF S. 81–104)** · dazu **Praktikum 02 (MitM + Phishing)** und **Praktikum 04 (Pentest)**

> **О чём глава.** Это «каталог атак»: как именно злоупотребляют механизмами из главы 2, какое Schutzziel при этом страдает и чем защищаться. Для каждой атаки держи в голове три вещи: **Mechanismus** (на какой слабости построена), **betroffenes Schutzziel**, **Gegenmaßnahme**.
>
> **Почему важно для оценки 1,0:**
> 1. Контрольные задания скрипта — это **ровно то, что на экзамене**: заполнить таблицу «атака → Schutzziel», распознать атаку в логе (Apache-Log, tcpdump-дамп, результат nmap), назвать тип атаки по сценарию.
> 2. Praktikum 02 и 04 почти наверняка дадут практические вопросы (ARP-Spoofing, nmap, Metasploit, Exploit vs. Payload).
>
> **Этический контекст:** всё это учебный материал курса EITS, атаки выполняются только в изолированной лаборатории из виртуальных машин и **только с разрешения владельца** (Pentest-Auftrag) или в рамках учёбы — см. [01 → 1.6](01_Einfuehrung.md) (§202a/c). Конспект объясняет механизмы, чтобы их **распознавать и защищаться**, а не атаковать чужие системы.
>
> **Обозначения:** ⭐ = очень вероятно на экзамене · 🦈 = анализ логов · ⚠️ = ловушка · ➕ = сверх скрипта · 🐞 = ошибка в скрипте · 🔧 = из Praktikum.

## Содержание
3.2 Netzwerksicherheit · 3.3 Hack the Stack (Verortung im OSI) ⭐ · Sniffing · Spoofing (MAC/IP/DNS) · ARP-Spoofing/MitM ⭐🔧 · DoS/DDoS & Botnetze ⭐ · SYN-Flooding ⭐ · TCP-Hijacking · ARP-Poisoning · DNS-Cache-Poisoning · 3.4 Anwendungsebene: Exploit, Drive-by, SQLi, XSS, Phishing ⭐ · 3.5 Pentest, Port-Scanning, Metasploit ⭐🔧 · Übersichtstabelle Angriff→Schutzziel ⭐ · Praktikum 02 & 04 · **Aufgaben A–H**

---

## 3.2 Netzwerksicherheit (S. 75–76)
- In den Anfangszeiten des Internets spielte Sicherheit **kaum eine Rolle** → die Protokolle aus Kapitel 2 bieten **meist keinerlei Schutz** (keine Authentisierung, keine Verschlüsselung). Sicherheit wurde erst **nachträglich** ergänzt.
- Viele Probleme entstehen zudem durch **fehlerhaft konfigurierte** Rechner, Router, Firewalls.

> **Def. 3.1 Netzwerksicherheit:** *beschreibt, wie ein Netzwerk und die betriebenen Dienste so genutzt werden können, dass die gesetzten Sicherheitsziele erfüllt sind.* Umfasst Entwurf sicherer Protokolle, Verschlüsselung und Absicherung ganzer Infrastrukturen.

- Für **jede** OSI-Schicht gibt es konkrete Bedrohungen. Größte Probleme: **Verlust der Vertraulichkeit** (unautorisiertes Lesen) und daraus folgend **Verlust der Integrität** (Manipulation). Klassiker: **Klartext-Passwörter**, die ein Sniffer mitliest.

### 3.3 Hack the Stack — Angriffe nach OSI-Schicht (Abb. 3.1) ⭐
Очень вероятная задача на соответствие «атака → слой».

| OSI-Schicht | Protokolle | Typische Angriffe |
|---|---|---|
| **7 Anwendung** | FTP, HTTP, DNS, SMTP, NFS | **OWASP Top 10** (SQL-Injection, XSS, CSRF), **GET-DoS, Phishing, DNS-Spoofing (Pharming), Session Hijacking, Malware, Buffer Overflows**, DHCP-Starvation/-Spoofing |
| **4 Transport** | TCP, UDP | **SYN-Flood, DNS-UDP-Flood, Smurf Attack, MitM** |
| **3 Vermittlung** | IP, ICMP | **ICMP-Flood (DoS), Routing-Angriffe**, IP-Spoofing |
| **2 Sicherung** | ARP, Ethernet, (BOOTP, RARP) | **MAC/ARP Sniffing & Spoofing** |
| **1 Bitübertragung** | — | **Hardware-Hacking** |

⚠️ Обрати внимание: **Phishing, SQLi, XSS, DNS-Spoofing** в скрипте отнесены к **слою 7**; **MitM, SYN-Flood** — к слою 4; **ARP-Spoofing/Sniffing** — к слою 2.

---

## 3.3.1 Sniffing (S. 77–80) ⭐🦈

> **Def. 3.2 Sniffing:** *das Mitlesen von Daten. Ein **passiver** Angriff, da der Angreifer in der Regel keine Daten ins Netzwerk sendet.*

- **Verletztes Schutzziel: Vertraulichkeit.** Da weder Sender noch Empfänger direkt angegriffen werden, bleibt Sniffing **meist unbemerkt**.
- Beute: Passwörter, Kreditkartendaten — alles, was **unverschlüsselt** übertragen wird (FTP, HTTP, Telnet).
- **Gegenmaßnahme: Verschlüsselung** — Layer 3 mit **IPsec**, Layer 4 mit **TLS**.
- Werkzeuge (Skript): **tcpdump, Wireshark** (früher Ethereal), **Ettercap**.

**Sniffer-Modi ⭐ (klassische Prüfungsfrage):**
| Modus | Was wird mitgelesen |
|---|---|
| **non-promiscuous** | nur der **eigene** ein-/ausgehende Verkehr |
| **promiscuous** | **alle** Frames auf dem Interface, auch die **nicht an die eigene MAC** adressierten |
| **monitor mode** (nur WLAN) | **jeglicher** empfangbarer WLAN-Verkehr aller Netze, auch ohne Verbindung |

**WLAN (802.11) — Designschwäche:** Funk strahlt über Raumgrenzen hinaus (kein physischer Kabelschutz). **Beacon-/Probe-Requests** werden **immer im Klartext** gesendet (egal welche Verschlüsselung) → Angreifer sieht SSID, verfügbare Netze und welche Geräte sich verbinden wollen.
- Angriffe, die trotz Verschlüsselung wirken (nutzen Protokoll-Designfehler): **Deauthentication/Disassociation Attack** (Geräte vom AP trennen), **Authentication DoS** (AP überlasten).
- **Evil Twin** (Skript-Beispiel mit Airbase-NG ➕): gefälschter AP antwortet auf Probe-Requests → Gerät verbindet sich → bei WPA2 gelangt der Angreifer an den **Hash** des Passworts → **Wörterbuchangriff**.

---

## 3.3.2 Spoofing (S. 80–82) ⭐
**Spoofing** = *Vortäuschen einer falschen Tatsache / Identität.* Auf mehreren Protokollebenen möglich.

### MAC-Spoofing (Schicht 2)
- Die MAC-Adresse wird vom **Betriebssystem** aus dem Adapter gelesen und übertragen → per Software **leicht änderbar** → **keine sichere Identifikation**.
- Zweck: fremde Identität annehmen, **Zugang erschleichen**, Sperren in Proxies/Routern umgehen, Tracking im öffentlichen WLAN verhindern (Privatsphäre).
- **Port-Security** (Switch-Feature): jeder Switch-Port fest mit einer (Liste von) MAC(s) verknüpft; nur diese dürfen kommunizieren. ⚠️ **MAC-Filter ist nur schwacher Schutz**, weil die MAC jederzeit fälschbar ist. Besserer Schutz: **End-to-End-Validierung** auf Transport-/Anwendungsschicht.

### IP-Spoofing (Schicht 3, Abb. 3.2)
- Angreifer erzeugt Pakete mit **gefälschter Absender-IP**, die eine **interne Station vortäuschen** → meistgenutzte Technik zum Überwinden von **Firewalls**.
- Besonders gefährlich bei **Paketfiltern**, die oft nur am Ausgangsport filtern → externe Pakete gelangen ins interne Netz.
- ⚠️ Antworten gehen an die gefälschte IP, der Angreifer sieht sie nicht → für TCP muss er Sequenznummern erraten; bei UDP/ICMP viel einfacher (→ DoS, s. u.).

### DNS-Spoofing (Schicht 7)
- Tarnen gefälschter Webseiten durch **ähnliche Domainnamen** (Typosquatting): `commerzbank.de` vs. `commerzbnak.de`.
- Ziel: **Integrität und Authentizität** untergraben → Phishing.
- **Gegenmaßnahmen:** integritätsgesicherte, authentisierte Protokolle (IMAP, **HTTPS/TLS**); **Zertifikate** verifizieren den Domäneninhaber; auf `https://` in der URL achten.

---

## 3.3.3 ARP-Spoofing: Man-in-the-Middle (S. 82–84) ⭐🦈🔧

**Mechanismus.** Der Angreifer setzt sich **zwischen** zwei Partner (Alice, Bob / Client, Router) und täuscht jedem vor, er sei der jeweils andere. Beide kommunizieren nie direkt, sondern **über ihn** → er arbeitet **unbemerkt als Proxy** und kann **mitlesen und verändern**.

**Warum ARP anfällig ist** (Praktikum 02): ARP ist **nicht authentifiziert**, und viele Systeme akzeptieren **unaufgeforderte ARP-Replies** (Gratuitous ARP). → Angreifer schickt gefälschte ARP-Replies:
- an Host A: „die IP des Routers/B gehört zu **meiner** MAC"
- an Host B/Router: „die IP von A gehört zu **meiner** MAC"
→ beide tragen die Angreifer-MAC in ihren **ARP-Cache** ein, Pakete fließen über den Angreifer. Er muss sie **weiterleiten** (IP-Forwarding), sonst bricht die Verbindung ab.

**Switch vs. Hub:** Ein **Hub** sendet alles an alle NICs → Sniffing trivial. Ein **Switch** isoliert Verkehr anhand seiner MAC-Tabelle → eine NIC sieht nur ihren Verkehr. Umgehung:
- Ältere Switches: **MAC-Flooding** (viele ARP-Anfragen) → Switch schaltet in **fail open** und arbeitet wie ein Hub.
- Moderne Switches: **ARP-Spoofing** (Switch-Tabelle fälschen) → MitM.

**Betroffene Schutzziele:** primär **Vertraulichkeit**; bei Manipulation auch **Integrität**; bei Verwerfen der Pakete **Verfügbarkeit**.
**Nur im lokalen Subnetz** möglich (ARP wird nicht geroutet) — Praktikum 02 betont das ausdrücklich.

**🔧 Praktikum 02 — Ablauf (Laborumgebung aus 3 VMs: Client-Opfer, Webserver, Angreifer):**
1. Angreifer aktiviert **IP-Forwarding** (`echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward`) → leitet Pakete weiter, damit das Opfer nichts merkt.
2. Interface in **promiscuous mode** (`sudo ip link set enp0s3 promisc on`).
3. **`sudo arpspoof -i enp0s3 -r -t ClientIP ServerIP`** — `-r` = bidirektional, `-t` = Target (Opfer). Sendet laufend gefälschte ARP-Replies.
4. Analyse: **urlsnarf** (aufgerufene URLs), **driftnet** (übertragene Bilder), **Wireshark** (`-k`).
5. Opfer ruft im Browser den Webserver auf → der Verkehr erscheint beim Angreifer.

**🦈 Erkennung (Praktikum 02, Aufgabe 2.14):** im ARP-Cache des Opfers (`ip neighbour show` / `ip n`) haben **zwei verschiedene IPs dieselbe MAC** → starkes Indiz. Wireshark: **[ Analyse | Experteninformationen ]** meldet „duplicate use of … detected". ARP-Cache-Zustände: `REACHABLE` (frisch) → `STALE` (älter, nutzbar) → `PROBE` → `FAILED`.
**Grenzen des Labors:** nur **unverschlüsselter** Verkehr ist lesbar; bei **HTTPS** sieht man nur Metadaten.

---

## 3.3.4 Denial of Service (DoS) und Botnetze (S. 84–86) ⭐

**DoS** = *Verweigerung des Dienstes*: **Verfügbarkeit** eines Systems wird beeinträchtigt. Prinzip: in kurzer Zeit **sehr viele (gefälschte) Anfragen** → das Opfer ist nur noch mit deren Beantwortung beschäftigt, legitime Nutzer kommen nicht durch, System wird instabil.

- **UDP-Flood-Beispiel (Abb. 3.4):** Angreifer sendet UDP-Pakete mit **gefälschter Absenderadresse** (die einer „Zombie"-Maschine), verbindet den **chargen**-Dienst (Port 19, erzeugt Zeichen) mit dem **echo**-Dienst (Port 7) → kontinuierlicher Datenstrom lähmt beide. Variante: Paket an **Broadcast-Adresse** (→ Smurf-Prinzip ➕).
- **Gegenmaßnahme:** Adressbereich sperren / Anfrage-Pakete sofort verwerfen, bevor der Puffer überläuft.

**DDoS (Distributed DoS):** Angriff von **vielen** Rechnern gleichzeitig.
- **Bot / Zombie:** ferngesteuerter, meist mit Malware infizierter Rechner; der Besitzer weiß oft nichts davon.
- **Botnetz (Def. 3.3):** *Rechnerverbund, dessen Rechner durch Exploit/Trojaner infiziert und von einem entfernten Server ferngesteuert werden.*
- Steuerung über **Command-&-Control-Server (C&C)**; nach dem Einbruch wird ein **Backdoor** installiert (dauerhafter Zugang). Kommunikation oft über **UDP** oder getarnt in **Instant-Messaging/soziale Netze** (schwer zu filtern).
- Nutzen für Angreifer: Anonymität, kostenlose Bandbreite. Einsatz: **DDoS, Spam, Viren-/Wurmverteilung, Bitcoin-Mining**, illegale Tausch-/Chatnetze.
- ➕ **DoS vs. DDoS:** DoS = eine Quelle (leicht per IP zu sperren); DDoS = tausende Quellen (viel schwerer abzuwehren).

---

## 3.3.5 SYN-Flooding (S. 87, Abb. 3.6) ⭐🦈
**Mechanismus — nutzt den Three-Way-Handshake aus:**
1. Server reserviert beim eingehenden **SYN** Pufferspeicher für die **halboffene** Verbindung (Zustand `SYN_RECEIVED`) und antwortet mit **SYN/ACK**.
2. Angreifer sendet **massenhaft SYNs mit gefälschter Absenderadresse** und **bestätigt nie** (kein abschließendes ACK).
3. Der Puffer füllt sich mit halboffenen Verbindungen → legitime Anfragen werden **nicht mehr angenommen** → **Verfügbarkeit** weg.

**🦈 Erkennung:** in `netstat` sehr viele Verbindungen im Zustand **`SYN_RECV`** von wechselnden IPs (vgl. [02 → Aufgabe G6](02_Rechnernetze.md)).
**Gegenmaßnahmen ➕:** **SYN-Cookies** (Server reserviert erst nach dem ACK Ressourcen), kürzere Timeouts, Rate-Limiting, Firewall/DDoS-Schutz.

---

## 3.3.6 Sequenznummernangriff / TCP-Hijacking (S. 87–88) ⭐
- **Ziel:** eine **bestehende** TCP-Verbindung **übernehmen** oder abbrechen, indem der Angreifer die **korrekte Sequenznummer kennt/errät**.
- Er muss **schneller** sein als der echte Sender; mit der richtigen SN werden seine Pakete akzeptiert, die echten später verworfen. Gute Tools **resynchronisieren** danach die Partner → Opfer merken (außer kurzem Stillstand) nichts.
- **Voraussetzung:** SN muss erratbar sein. Früher **schwache Zufallsgeneratoren** oder einfach **hochzählende** SN → leicht erratbar. Heute **zufällige ISN** → Angriff praktisch ausgeschlossen.
- **Resistent:** **verschlüsselte Verbindungen** (TLS) — eine Übernahme bringt nichts.
- (Skript-Hinweis: `nmap` schätzt die SN-Vorhersagbarkeit; aussichtslos → „Good luck!!!")
- **Betroffen:** Integrität, Authentizität.

## 3.3.7 ARP-Poisoning (S. 88)
= Senden gefälschter ARP-Pakete, um **ARP-Tabellen** zu verändern → Grundlage für **MitM im lokalen Netz** (identisch zu 3.3.3). Der Angreifer kann Verkehr **mitlesen, verändern oder verwerfen** (dann Verfügbarkeit). *(ARP-Spoofing und ARP-Poisoning werden im Skript praktisch synonym verwendet.)*

## 3.3.8 DNS-Cache-Poisoning (S. 88) ⭐
- Angreifer schickt **manipulierte DNS-Einträge an den DNS-Resolver**, die im **DNS-Cache** gespeichert werden.
- Alle folgenden Nutzer bekommen **falsche IP-Adressen** → verbinden sich unbemerkt mit einer **manipulierten (Phishing-)Webseite**. Auch „**Pharming**" genannt.
- **Betroffen:** Integrität, Authentizität. **Schutz:** DNSSEC ➕, HTTPS mit Zertifikatsprüfung.
- ⚠️ Unterschied zu 3.3.2 DNS-Spoofing: dort ähnlicher **Domainname**, hier vergifteter **Cache** eines echten Namens.

---

## 3.4 Angriffe auf Anwendungsebene (S. 89–91)

### 3.4.1 Exploits: Einschleusen von Code ⭐
- Ziel: **fremden Code** auf dem Zielsystem **ausführen**, durch Ausnutzen von Fehlern in Programmen, die **Eingaben/Dateien nicht richtig prüfen**. Besonders lohnend: Programme mit **Administratorrechten**.
- **Remote-Exploit:** Code lässt sich **über das Netzwerk** ausführen, **ohne Zutun des Nutzers** → extrem gefährlich, leicht automatisierbar → schnell viele Rechner übernehmen.
- Das Angriffsprogramm ist meist **klein**, enthält aber Funktionen zum Ausführen beliebiger Kommandos und Nachladen von Software. Läuft das Zielprogramm als Admin → **totale Kontrolle**.
- ➕ Klassische Grundlage: **Buffer Overflow** (Eingabe überschreibt Rücksprungadresse). Schutz: Eingabevalidierung, ASLR, DEP/NX, sichere Programmiersprachen.

### 3.4.2 Drive-by-Download
- Malware als **harmloser Download** getarnt (Webseite, Filesharing, Spam-Mail). Als „nützliches neues Programm" ausgegeben → Nutzer führt sie selbst aus.
- ➕ Im engeren Sinn: schon der **Besuch** einer präparierten Seite infiziert über Browser-/Plugin-Lücken (vgl. [01 → 1.4](01_Einfuehrung.md)).

### 3.4.3 SQL-Injection (SQLi) ⭐
> **Def. 3.4:** *Attacke, bei der ein Angreifer über speziell präparierte Eingaben (z. B. in einer Suchfunktion) Datenbank-Befehle einschleust, sodass die Web-Applikation diese direkt an die Datenbank schickt.*

- Ursache: **ungeprüfte Benutzereingaben** werden in SQL-Abfragen eingebaut.
- Folgen: Daten **auslesen, manipulieren, löschen, einfügen**; ganze Datenbank löschen; **Admin-Rechte** erlangen → Übernahme der Web-App.
- ⚠️ Gehört laut Skript/OWASP zur **Anwendungsschicht (7)**.
- ➕ **Schutz:** **Prepared Statements / Parameterbindung** (wichtigste Maßnahme), Eingabevalidierung, minimale DB-Rechte, WAF.
- ➕ **Prinzip-Beispiel** (zum Verständnis, warum es wirkt): Eine Login-Abfrage wie `... WHERE name='$user'` wird gefährlich, wenn `$user` ungeprüft eingesetzt wird und der Nutzer `' OR '1'='1` eingibt → die Bedingung ist immer wahr. Deshalb: Eingaben nie direkt in den Query-String einsetzen.

### 3.4.4 Cross-Site Scripting (XSS) ⭐
> **Def. 3.5:** *XSS-Attacken fügen fremden Skriptcode in Webseiten ein. Je nach Ausführungsort: **client-seitiges XSS** (Code läuft im Browser) oder **server-seitiges XSS** (Code läuft im Webserver).*

- Ursache: Webanwendung **validiert/escapet** benutzergelieferte Daten nicht, bevor sie zurück an den Browser gehen (Metazeichen nicht „escaped").
- Ablauf: Angreifer codiert Skript in einen **Link**, bringt das Opfer per **Spam-/Kettenmail** zum Klick; Skript wird im Kontext der echten Seite ausgeführt → großer Teil der Seite bleibt „echt" → schwer zu entdecken.
- 2007: ca. **80 %** der ausgenutzten Web-Schwachstellen waren XSS (Symantec).
- Häufigste Ziele: **Zugangsdaten stehlen** (Session-Cookies), **Webseiten verändern**.
- ➕ Schutz: **Output-Encoding/Escaping**, Eingabevalidierung, **Content Security Policy (CSP)**, HttpOnly-Cookies. ➕ Arten: **reflected** (über Link), **stored/persistent** (im Server gespeichert), **DOM-based**.

### 3.4.5 Phishing ⭐🔧
- **Kunstwort** aus *password fishing* — Betrugsmasche, um an **Passwörter und Zugangsdaten** zu kommen.
- **Domain-Phishing:** seriöse Domain stehlen/umleiten auf eine vom Angreifer kontrollierte Seite (oft Banken, Reiseportale) → Abgreifen von Nutzername, Passwort, PIN, Kreditkartendaten.
- ➕ Verwandt: **Spear-Phishing** (gezielt), **Vishing** (Telefon), **Smishing** (SMS), **Quishing** (QR-Code).

**🔧 Praktikum 02, Teil 2 — Phishing mit SET (Social Engineering Toolkit):**
- **Social Engineering** = Sicherheitsarchitektur umgehen, indem man **den Menschen** täuscht (Faktor Mensch, vgl. [01 → 1.2.1](01_Einfuehrung.md)). Formen: Vishing, gefälschte E-Mails, manipulierte PDFs, **Quishing** (QR-Code überkleben).
- SET **klont** eine Login-Seite (im Labor: Google) → **Credential Harvester** zeichnet eingegebene (Test-)Daten auf; Opfer wird danach auf die echte Seite weitergeleitet. Bericht landet in `/root/.set/reports/`.
- **Aufbau einer Domain verstehen (Abb. 6) ⭐ — wichtig gegen Phishing:** Besitzer = **Domain + TLD** (`google.de`). Subdomain, Pfad und Parameter sind nur Zusätze.
  | Täuschung | Beispiel | Trick |
  |---|---|---|
  | ähnlicher Name | `www.goog1e.de` | „l" → „1" |
  | ähnlicher Name | `login.gooogle.de` | extra „o" |
  | falsche Domain | `www.google-login.de` | klingt nur ähnlich |
  | Subdomain-Trick | `google.sicherer-login.de` | echte Domain ist **sicherer-login.de** |
  | Subdomain-Trick | `google.highsecurity.com` | echte Domain ist **highsecurity.com** |
- **Phishing-Checkliste (Praktikum):** Domain/Subdomain prüfen (nur IP? falsch geschrieben? nur naheliegende Subdomain?); Zertifikat prüfen — ⚠️ **HTTPS allein ist kein Vertrauensbeweis** (auch Phishing-Seiten können TLS haben), aber reines HTTP ist heute unüblich.
- **Erweiterung DNS-Spoofing:** kombiniert mit Phishing zeigt die Adresszeile die **echte Domain** → noch glaubwürdiger.

---

## 3.5 Penetration Testing (S. 91–96) ⭐🔧
> **Def. 3.6 Penetrationstest:** *möglichst umfassende Sicherheitsüberprüfung einzelner Rechner oder ganzer Netzwerke, wobei der Tester in die Rolle eines Angreifers schlüpft.*

- Teil des **Sicherheitsmanagements** ([01 → 1.7](01_Einfuehrung.md)): prüft die Funktionstüchtigkeit der Sicherheitssysteme unter **realen Bedingungen**, deckt Schwachstellen auf, **mit Auftrag** (sonst strafbar).
- **🔧 Phasen (Praktikum 04):** **Recon** (Informationsbeschaffung) → **Scanning/Schwachstellenanalyse** → **Exploit** (Eindringen) → **Post-Exploitation** (Loot, Rechte ausweiten) → **Spuren verwischen**. (vgl. die 3 BSI-Phasen [01 → 1.5](01_Einfuehrung.md).)

### 3.5.1 Port-Scanning (S. 92–93) ⭐🦈🔧
- **Erste und wichtigste Phase: Informationsbeschaffung.** Überblick über aktive Rechner, **offene Ports**, laufende Dienste.
- ⚠️ **Port-Scanning ist für sich kein Angriff** — „Anklopfen, ob jemand zu Hause ist". Dient aber der **Angriffsvorbereitung** (unter Port X läuft Dienst mit bekannter Lücke). **Legitim** auch zur Prüfung der eigenen Ressourcen (z. B. unerwünscht offener Webserver auf ungewöhnlichem Port).

**Stufen eines Scans (müssen nacheinander ablaufen):**
| # | Schritt | Inhalt |
|---|---|---|
| 1 | **Target Enumeration** | Ziele bestimmen (Namen, IPs, CIDR) |
| 2 | **Host Discovery (Ping-Scan / Ping-Sweep)** | welche Hosts sind online? (ARP-, TCP-, ICMP-Tests) |
| 3 | **Reverse-DNS-Resolution** (optional) | Hostnamen → Hinweise auf die Funktion |
| 4 | **Port-Scan** | Testanfragen → Zustände **open / closed / filtered** (vgl. [02 → 2.8.2](02_Rechnernetze.md)) |
| 5 | **OS-Erkennung** (optional) | aus Unterschieden im Netzwerkstack das Betriebssystem bestimmen (**Fingerprinting**) |

**🔧 Praktikum 04 — nmap/Zenmap:**
- `ip a` → eigene IP, CIDR, Subnetzmaske, Broadcast ermitteln.
- **Ping-Scan / Host Discovery:** nur welche Hosts leben.
- **Intense Scan:** `nmap -T4 -A -v <netz>/24` → `-T4` aggressives Timing, **`-A`** = OS-Erkennung + Versions-/Dienst-Erkennung + Skript-Scan + Traceroute, `-v` ausführlich, `-n` ohne DNS (schneller). OS-Erkennung und Traceroute brauchen **sudo**.
- Ergebnis: offene Ports, Dienste/Versionen, vermutetes OS. Für WinXP im Labor: **`smb-os-discovery`** verrät Workgroup und Sprache.
- Warum manche Hosts **keine** OS-Klassifizierung bekommen: zu wenige offene/geschlossene Ports oder Firewall filtert → Stack-Verhalten nicht eindeutig.
- ⚠️ `-A`/`-T4` sind **laut, nicht unauffällig** → können **IDS** alarmieren.

**🔧 Schwachstellenscanner — Greenbone/OpenVAS (Praktikum 04, Teil 2):**
- **Schwachstellenscanner** gehen weiter als Portscanner: vergleichen gefundene Dienste mit einer **Datenbank bekannter Schwachstellen** (NVTs, CVEs) und **bewerten den Schweregrad**. Historie: **Nessus** → Fork **OpenVAS** → **Greenbone**.
- Ablauf im Labor: GSA-Weboberfläche (selbstsigniertes Zertifikat zulassen) → Aufgaben-Wizard → Ziel = WinXP-IP → Bericht mit Funden. Auffällig: **SMB auf Port 445** → kritische Lücke (921883 „netapi32.dll").
- ⚠️ Aktive Scans sind **invasiv** → können IDS auslösen.

### 3.5.2 Metasploit-Framework (S. 93–96) ⭐🔧
Pentesting-Umgebung; Kern = **Rex** (Ruby Extension Library). **Module:**
| Modul | Aufgabe |
|---|---|
| **Exploits** | nutzen eine **Schwachstelle** aus, um Zugriff zu erhalten |
| **Payloads** | **Schadcode**, der **nach** erfolgreichem Exploit ausgeführt wird (Zugang etablieren, Account anlegen, Shell) |
| **Encoder** | Payload so **codieren/verschleiern**, dass IDS/Virenscanner ihn schwer erkennen |
| **NOPs** | **NOP-Sleds** (Folge von `0x90`-Befehlen) erzeugen/variieren, um signaturbasierte IDS zu täuschen |
| **Auxiliaries** | Helfer für **Fingerprinting und Vulnerability-Scanning** |
| **Post** ➕🔧 | Post-Exploitation (Infos sammeln nach dem Einbruch) |

> **Exploit vs. Payload ⭐ (Prüfungsfrage aus dem Pentest-Fazit):** *Der **Exploit** nutzt die Schwachstelle aus und verschafft Zugriff, richtet aber allein keinen weiteren Schaden an. Der **Payload** ist der eigentliche Schadcode, der nach erfolgreichem Exploit ausgeführt wird (z. B. eine Shell/Meterpreter-Session).*

**🔧 Praktikum 04 — konzeptioneller Ablauf (gegen eine veraltete WinXP-VM im Labor):**
- `msfconsole -q` → `search type:exploit platform:windows smb` → Exploit auswählen (`use …`), z. B. die **netapi/SMB-Lücke** (Hinweis aus GSM: MS06-040 nur DoS auf XP SP2; der neuere **MS08-067** übernimmt das System).
- **`show options` / `show missing`** → fehlende Felder setzen: **RHOST** = Ziel (WinXP), **LHOST** = Angreifer (ForDeb), **RPORT 445** (SMB), **TARGET** = genaues OS.
- Standard-Payload **`windows/meterpreter/reverse_tcp`**: **Reverse-Verbindung** — das **Opfer verbindet sich zum Angreifer** (umgeht oft Firewalls, die eingehende, aber nicht ausgehende Verbindungen blocken). ➕ Gegenstück: **Bind-Shell** (Angreifer verbindet sich zum Opferport).
- `exploit`/`run` → **Meterpreter-Session**.
- **Post-Exploitation (Loot):** `getuid` (wer bin ich), `getsystem` (**Rechte-Eskalation**), `sysinfo`, `screenshot`, `ps`/`migrate PID` (in `explorer.exe`/`winlogon.exe` wechseln), Keylogger `keyscan_start/dump/stop`, `shell`. **Timestomp** = Zeitstempel fälschen (Spuren verwischen). Zeigt, warum ein kompromittiertes System „komplett dem Angreifer gehört".
- 🦈 Erinnerung: eine **ausgehende** Verbindung eines Servers auf **Port 4444** (Metasploit-Default) ist ein typisches Backdoor-Zeichen (vgl. [02 → Aufgabe G6](02_Rechnernetze.md)).

---

## Übersicht: Angriff → Schutzziel → Gegenmaßnahme ⭐ (Kernbild der Klausur)

| Angriff | Schicht | aktiv/passiv | primär verletztes Schutzziel | Gegenmaßnahme |
|---|---|---|---|---|
| **Sniffing** | 2 | passiv | **Vertraulichkeit** | Verschlüsselung (TLS, IPsec, WPA2) |
| **MAC-Spoofing** | 2 | aktiv | Authentizität | End-to-End-Validierung, 802.1X |
| **ARP-Spoofing / -Poisoning (MitM)** | 2/3 | aktiv | Vertraulichkeit, Integrität (ggf. Verfügbarkeit) | statische ARP-Einträge, Dynamic ARP Inspection, TLS |
| **IP-Spoofing** | 3 | aktiv | Authentizität | Ingress-Filtering, keine reine Paketfilter-Trust |
| **ICMP-/UDP-Flood, Smurf** | 3/4 | aktiv | **Verfügbarkeit** | Rate-Limiting, Filter, Broadcast aus |
| **SYN-Flooding** | 4 | aktiv | **Verfügbarkeit** | SYN-Cookies, Timeout, Firewall |
| **TCP-Hijacking** | 4 | aktiv | Integrität, Authentizität | zufällige ISN, **Verschlüsselung (TLS)** |
| **DNS-Spoofing / Cache-Poisoning** | 7 | aktiv | Integrität, Authentizität | DNSSEC, HTTPS + Zertifikate |
| **DoS / DDoS (Botnetz)** | 3/4/7 | aktiv | **Verfügbarkeit** | DDoS-Schutz, Quellen sperren |
| **Exploit / Buffer Overflow** | 7 | aktiv | alle (Übernahme) | Patches, Eingabeprüfung, DEP/ASLR |
| **SQL-Injection** | 7 | aktiv | Vertraulichkeit, Integrität, (Verfügbarkeit) | **Prepared Statements**, Validierung |
| **XSS** | 7 | aktiv | Vertraulichkeit (Session), Integrität | **Output-Encoding**, CSP, Validierung |
| **Phishing / Social Engineering** | 7/Mensch | aktiv | Authentizität, Vertraulichkeit | Schulung, 2FA, Domain/Zertifikat prüfen |
| **Drive-by / Malware** | 7 | aktiv | alle | Updates, Virenscanner, kein Ausführen |

**Merksatz für die Schutzziel-Zuordnung:** *Nur mitlesen → Vertraulichkeit. Verändern → Integrität. Blockieren/überlasten → Verfügbarkeit. Identität vortäuschen → Authentizität. Übernehmen → alle.*

---

# Aufgaben — типичные и нетипичные
**[T]** типичная · **[N]** нетипичная/перенос · **[1,0]** на высший балл · 🦈 лог. Сначала реши сам, потом сверяйся.

## A. Kontrollaufgaben aus dem Skript

**A1 [T]. (Aufgabe 3.1) Welche Schutzziele werden direkt gefährdet? Ergänzen Sie die Tabelle.**
| Angriffsklasse | Beschreibung | gefährdete Schutzziele |
|---|---|---|
| Sniffing | ??? | ??? |
| SQL-Injection | ??? | ??? |
| ??? | Nachbilden einer vertrauten Webseite (Online-Banking), Benutzer zur Eingabe seiner Daten verleiten | ??? |
| DoS-Angriff | ??? | ??? |
| Spoofing | Vortäuschen einer falschen Identität | ??? |
> **✅ Lösung:**
> | Angriffsklasse | Beschreibung | Schutzziele |
> |---|---|---|
> | **Sniffing** | passives Mitlesen von Daten | **Vertraulichkeit** |
> | **SQL-Injection** | Einschleusen von DB-Befehlen über ungeprüfte Eingaben | **Vertraulichkeit, Integrität, Verfügbarkeit** (Auslesen/Ändern/Löschen) |
> | **Phishing** | gefälschte Webseite, Nutzer gibt Daten ein | **Authentizität, Vertraulichkeit** |
> | **DoS-Angriff** | Überlasten mit Anfragen | **Verfügbarkeit** |
> | **Spoofing** | falsche Identität vortäuschen | **Authentizität** (je nach Variante + Integrität) |

**A2 [T] 🦈. (Aufgabe 3.2.2) Apache-Log-Auszug.** Viele `GET`-Anfragen von einer IP in Sekunden: `/sql/index.php`, `/SQL/index.php`, `/mysql/`, `/pma/`, `/phpmyadmin/`, `/myadmin/`, `/database/`, `/_phpmyadmin/` — alle mit Status **404**; eine erste Zeile mit binärem Müll (`\x16\x03\x01…`) und Status **501**.
a) War der Angriff bereits erfolgreich? b) Kann er erfolgreich werden? c) Ist der Angriff relevant? d) Gegenmaßnahmen?
> **✅ Lösung:**
> Es ist ein **automatisiertes Scanning** nach einer **Datenbank-Administrationsoberfläche** (phpMyAdmin & Co.) — **Angriffsvorbereitung**, kein erfolgreicher Einbruch. Die erste Zeile (`\x16\x03\x01`, Status 501) ist ein **TLS-Handshake an einen HTTP-Port** (Client wollte HTTPS).
> a) **Nein** — alle Treffer sind **404** (nicht gefunden), nichts wurde geöffnet.
> b) **Ja, grundsätzlich** — würde eine dieser Oberflächen existieren (Status 200) und schwach geschützt sein, könnte der Angreifer sie übernehmen.
> c) **Ja, relevant** — solche Scans laufen ständig; der Server ist exponiert und wird systematisch abgeklopft.
> d) **Gegenmaßnahmen:** phpMyAdmin nicht öffentlich erreichbar (nur intern/VPN), falls nötig umbenennen + Auth + IP-Beschränkung; **Fail2ban/Rate-Limiting** gegen Scanner; aktuelle Software/Patches; HTTPS erzwingen; WAF; Monitoring der 404-Muster.

**A3 [T]. (Aufgabe 3.2.3) Passwörter werden unverschlüsselt übertragen; Ali snifft den Traffic mit und will die Zugangsdaten später wiederverwenden. Welche Angriffsart: Brute-Force, Wörterbuch, Social Engineering oder Replay?**
> **✅ Lösung:** **Replay-Angriff.** Ali knackt kein Passwort (kein Brute-Force/Wörterbuch) und täuscht niemanden (kein Social Engineering), sondern **fängt die gültigen Daten per Sniffing ab und spielt sie später erneut ein** (wiederverwenden = replay). Zugrunde liegt ein **passiver** Sniffing-Angriff auf die **Vertraulichkeit**. Schutz: TLS, **Challenge-Response** / Einmal-Token (nonce) gegen Replay.

**A4 [T] 🦈. (Aufgabe 3.2 Forts. 1) tcpdump-Auszug:** von `x.alibaba.org:4086` gehen innerhalb weniger Millisekunden **SYN**-Pakete an `134.169.35.100:23`, `.101:23`, `.119:23`, `.126:23`, `.138:23`, `.142:23`, `.143:23`, `.153:23`. Deuten die Pakete auf einen Angriff hin? Besonderheiten?
> **✅ Lösung:**
> Ja — ein **Port-Scan**, genauer ein **horizontaler Scan** (ein Dienst, viele Hosts): **gleiche Quelle und gleicher Quellport (4086)**, **gleicher Zielport 23 (Telnet)**, **verschiedene Ziel-IPs** im Netz 134.169.35.0/24, alle mit **SYN** in Mikrosekunden-Abstand.
> **Besonderheiten:** (1) Es wird gezielt nach **Telnet** gesucht — ein unverschlüsselter Dienst, der oft schwache/Default-Passwörter hat → lohnendes Ziel. (2) Nur SYNs, kein Handshake-Abschluss → wahrscheinlich **SYN-/Half-Open-Scan** (stealth). (3) Es ist **Angriffsvorbereitung** (Host/Service Discovery), noch kein Einbruch.

**A5 [T] 🦈. (Aufgabe 3.2 Forts. 2) nmap-Ergebnis für 192.168.1.34:** offen sind 80 http, 135 msrpc, 139 netbios-ssn, 443 https, 445 microsoft-ds, 912, 1050, 1054, 1688; „Not shown: 991 closed ports"; MAC 6C:62:6D:…
a) IP des gescannten Rechners? b) Wie viele offene Ports? c) Welchen Dienst würden Sie zuerst untersuchen? Begründung.
> **✅ Lösung:**
> a) **192.168.1.34.**
> b) **9 offene Ports** (80, 135, 139, 443, 445, 912, 1050, 1054, 1688). „991 closed" + 9 open = 1000 Standardports.
> c) **Port 445 (microsoft-ds / SMB)** zuerst — die offenen Ports 135/139/445 deuten auf ein **Windows-System** mit SMB/NetBIOS; SMB ist historisch **sehr anfällig** (netapi-Lücken MS08-067, EternalBlue/WannaCry). Gut vertretbar auch: 80/443 (Webserver → SQLi/XSS). Für 1,0: Begründung über **bekannte Schwachstellen des Dienstes**, nicht nur „weil offen".

## B. Angriff bestimmen (Szenario → Name) [T]
1. Ein Gerät sendet laufend ungefragte ARP-Replies „Gateway-IP = meine MAC".
2. Ein Server bekommt Tausende SYNs von gefälschten IPs und wird unerreichbar.
3. In ein Suchfeld wird `' OR '1'='1` eingegeben und man ist eingeloggt.
4. Eine Mail der „Sparkasse" verlinkt auf `sparkasse.sicher-login.de`.
5. Ein Angreifer liest im offenen WLAN unverschlüsselte Logins mit.
6. Ein manipulierter Link führt dazu, dass im Browser des Opfers fremdes JavaScript läuft.
7. Tausende infizierte Heim-PCs greifen gemeinsam eine Website an.
8. Der DNS-Resolver liefert für `bank.de` dauerhaft eine falsche IP.
> **✅ Lösung:** 1 **ARP-Spoofing/MitM** · 2 **SYN-Flooding** · 3 **SQL-Injection** · 4 **Phishing** (Subdomain-Trick, echte Domain = sicher-login.de) · 5 **Sniffing** (passiv) · 6 **XSS** (client-seitig, reflected) · 7 **DDoS (Botnetz)** · 8 **DNS-Cache-Poisoning**.

## C. Zuordnung [T]
**C1. Angriff → OSI-Schicht:** ARP-Spoofing · SYN-Flood · SQL-Injection · IP-Spoofing · Sniffing · Phishing · ICMP-Flood · XSS
> **✅ Lösung:** Schicht 2: ARP-Spoofing, Sniffing · Schicht 3: IP-Spoofing, ICMP-Flood · Schicht 4: SYN-Flood · Schicht 7: SQL-Injection, Phishing, XSS.

**C2. Begriff → Definition.**
| | Begriff | | Definition |
|---|---|---|---|
|1|Exploit|a|Schadcode, der nach erfolgreichem Eindringen ausgeführt wird|
|2|Payload|b|ferngesteuerter, infizierter Rechner|
|3|Bot/Zombie|c|nutzt eine Schwachstelle aus, um Zugriff zu erhalten|
|4|C&C-Server|d|verschleiert Payloads vor IDS/Virenscannern|
|5|Encoder|e|steuert das Botnetz zentral|
> **✅ Lösung:** 1c · 2a · 3b · 4e · 5d

**C3. Angriff → Gegenmaßnahme.** Sniffing · SYN-Flood · SQL-Injection · ARP-Spoofing · Phishing · XSS
> **✅ Lösung:** Sniffing→Verschlüsselung (TLS) · SYN-Flood→SYN-Cookies · SQL-Injection→Prepared Statements · ARP-Spoofing→statische ARP-Einträge/DAI · Phishing→Schulung+2FA · XSS→Output-Encoding/CSP.

## D. Richtig oder falsch? (mit Begründung) [T/N]
1. Sniffing ist ein aktiver Angriff.
2. Ein Switch verhindert Sniffing vollständig.
3. Port-Scanning ist bereits ein Angriff.
4. HTTPS in der Adresszeile beweist, dass eine Seite seriös ist.
5. MAC-Filter (Port-Security) bieten starken Schutz, weil MAC-Adressen fest sind.
6. Verschlüsselte Verbindungen sind gegen TCP-Hijacking resistent.
7. Beim Exploit richtet der Exploit selbst den eigentlichen Schaden an.
8. Ein Reverse-Shell-Payload baut die Verbindung vom Opfer zum Angreifer auf.
9. ARP-Spoofing funktioniert auch über das Internet.
10. SYN-Flooding greift die Vertraulichkeit an.
> **✅ Lösung:**
> 1. **Falsch** — passiv (sendet i. d. R. keine Daten).
> 2. **Falsch** — erschwert es, aber ARP-Spoofing/MAC-Flooding umgehen das.
> 3. **Falsch** — „Anklopfen", Vorbereitung; für sich kein Angriff (ohne Auftrag aber rechtlich heikel).
> 4. **Falsch** — HTTPS zeigt nur Verschlüsselung; auch Phishing-Seiten haben Zertifikate. Entscheidend ist die **Domain**.
> 5. **Falsch** — MAC ist per Software fälschbar → nur schwacher Schutz.
> 6. **Richtig** — ohne Klartext bringt die Session-Übernahme nichts.
> 7. **Falsch** — der Exploit verschafft nur Zugriff; den Schaden macht der **Payload**.
> 8. **Richtig** — Reverse-TCP; umgeht oft Firewalls, die nur eingehende Verbindungen blocken.
> 9. **Falsch** — nur im lokalen Subnetz (ARP wird nicht geroutet).
> 10. **Falsch** — **Verfügbarkeit**.

## E. Offene Fragen und Transfer [N] [1,0]

**E1. Warum ist ein MitM-Angriff per ARP-Spoofing so wirkungsvoll, und welche Schutzziele sind je nach Verhalten des Angreifers betroffen?**
> **✅ Lösung:** Der Angreifer sitzt unbemerkt als Proxy zwischen beiden Partnern und kann den gesamten Verkehr sehen und beeinflussen. Je nach Verhalten (Praktikum 02): **nur mitlesen → Vertraulichkeit**; **Pakete verändern (z. B. Bilder/Werbung austauschen) → zusätzlich Integrität**; **Pakete verwerfen/blockieren → Verfügbarkeit**. Möglich ist er, weil ARP **nicht authentifiziert** ist und Hosts auch unaufgeforderte Replies übernehmen. Schutz gegen die Folgen: **Ende-zu-Ende-Verschlüsselung (TLS)** — dann sieht der MitM nur Metadaten.

**E2. Erklären Sie den Unterschied zwischen Exploit und Payload an einem Beispiel.**
> **✅ Lösung:** Der **Exploit** nutzt eine konkrete Schwachstelle aus, um Code zur Ausführung zu bringen bzw. Zugriff zu erlangen — allein richtet er keinen weiteren Schaden an. Der **Payload** ist der danach ausgeführte Schadcode mit dem eigentlichen Ziel. Beispiel (Praktikum 04): Der Exploit `ms08_067_netapi` nutzt die SMB-Lücke auf Port 445 aus; als Payload dient `windows/meterpreter/reverse_tcp`, der eine Meterpreter-Session vom Opfer zurück zum Angreifer öffnet (Reverse Shell) und so Fernsteuerung, Rechte-Eskalation, Keylogging usw. ermöglicht.

**E3. Warum nutzt man eine Reverse-Shell statt einer Bind-Shell? [1,0]**
> **✅ Lösung:** Bei einer **Bind-Shell** öffnet das Opfer einen Port, zu dem sich der Angreifer verbindet (**eingehende** Verbindung) — das blocken Firewalls meist. Bei einer **Reverse-Shell** baut das **Opfer** die Verbindung **zum Angreifer** auf (**ausgehende** Verbindung); ausgehender Verkehr ist oft erlaubt, daher umgeht die Reverse-Shell viele Firewalls und NAT. Erkennungszeichen für Verteidiger: ungewöhnliche **ausgehende** Verbindung eines Servers (z. B. Port 4444).

**E4. Ein Betreiber sagt: „Wir nutzen einen Switch, also kann niemand unseren Verkehr mitlesen." Nehmen Sie Stellung.**
> **✅ Lösung:** Falsch. Ein Switch isoliert Verkehr nur anhand seiner MAC-Tabelle, bietet aber **keine kryptografische Sicherheit**. Angreifer im selben Subnetz können per **ARP-Spoofing** die Tabellen/Caches vergiften (MitM) oder ältere Switches per **MAC-Flooding** in den **fail-open**-Modus zwingen (arbeitet dann wie ein Hub). Zuverlässiger Schutz nur durch **Verschlüsselung (TLS/IPsec)** plus Maßnahmen wie statische ARP-Einträge, Dynamic ARP Inspection und Port-Security.

**E5. Warum ist SQL-Injection trotz einfacher Gegenmaßnahme so verbreitet, und wie verhindert man sie? [1,0]**
> **✅ Lösung:** Ursache ist, dass **Benutzereingaben ungeprüft** in SQL-Abfragen eingebaut werden; die Eingabe wird dann als **Befehl** statt als **Daten** interpretiert. Sie ist verbreitet, weil Webanwendungen viele Eingabefelder haben und ein einziges vergessenes Feld genügt. Wirksamste Gegenmaßnahme: **Prepared Statements / parametrisierte Abfragen** (Trennung von Code und Daten), zusätzlich Eingabevalidierung, minimale Datenbankrechte und eine WAF. Analog gilt für **XSS** das **Escapen/Encoden** der Ausgabe.

**E6. [N] Beschreiben Sie den vollständigen Ablauf eines Pentests anhand von Praktikum 04 und ordnen Sie die Werkzeuge zu.**
> **✅ Lösung:**
> 1. **Recon/Scanning:** `ip a` (eigenes Netz), **nmap/Zenmap** Ping-Scan (Host Discovery), Intense Scan `-T4 -A` (offene Ports, Dienste, OS) → findet WinXP mit **SMB auf 445**.
> 2. **Schwachstellenanalyse:** **Greenbone/OpenVAS** scannt gegen CVE/NVT-Datenbank → kritische SMB-Lücke (netapi).
> 3. **Exploit:** **Metasploit** (`msfconsole`): Exploit `ms08_067_netapi` wählen, RHOST/LHOST/TARGET setzen, Payload `meterpreter/reverse_tcp`, `run`.
> 4. **Post-Exploitation (Loot):** Meterpreter: `getuid`, `getsystem` (Rechte-Eskalation), `sysinfo`, `screenshot`, Keylogger, `migrate`.
> 5. **Spuren verwischen** (im Praktikum ausgelassen): Timestomp, Logs.
> Begriffe: **Exploit** nutzt die Lücke, **Payload** ist der Schadcode; der Scanner findet die Lücke, Metasploit nutzt sie.

**E7. [N] Nennen Sie je zwei technische und zwei nicht-technische Maßnahmen gegen Phishing und begründen Sie, warum beides nötig ist.**
> **✅ Lösung:** Technisch: **HTTPS + Zertifikatsprüfung**, **2FA** (gestohlenes Passwort allein nützt nicht), Spam-/Phishing-Filter, DMARC/SPF/DKIM für E-Mail-Authentizität. Nicht-technisch: **Security-Awareness-Schulung**, Domains bewusst prüfen (Praktikum-Checkliste), Meldeprozesse. Beides nötig, weil Phishing den **Faktor Mensch** angreift (Social Engineering): Technik kann vieles abfangen, aber die Entscheidung, Daten einzugeben, trifft der Mensch — deshalb ist Sensibilisierung unverzichtbar ([01 → 1.7](01_Einfuehrung.md)).

## T. Transfer & Verständnis — Skript anwenden ⭐
> Комбинируй атаки и меры, рассуждай о последствиях — как на реальном экзамене.

**T1 [1,0]. Ein Angreifer im selben WLAN kombiniert ARP-Spoofing + DNS-Spoofing + eine gefälschte Login-Seite. Beschreiben Sie die Angriffskette und nennen Sie für jede Stufe die eine Maßnahme, die sie stoppt.**
> **✅ Lösungsweg:** (1) **ARP-Spoofing** → MitM, der Angreifer sieht/lenkt den Verkehr des Opfers → *Maßnahme: statische ARP-Einträge / Dynamic ARP Inspection.* (2) Als MitM fälscht er die **DNS-Antwort** für `bank.de` auf seine IP → *Maßnahme: DNSSEC / DoH.* (3) Das Opfer landet auf der **Phishing-Seite** und gibt Daten ein → *Maßnahme: HTTPS + Zertifikatsprüfung (Browser warnt, da der Angreifer kein gültiges Zertifikat für bank.de hat) + Nutzer-Awareness.* Kernidee: **durchgängiges TLS bricht die ganze Kette**, weil die gefälschte Seite kein gültiges Zertifikat vorweisen kann — zeigt, warum Verschlüsselung + PKI zusammenwirken.

**T2 [1,0]. Warum hilft Verschlüsselung (TLS) gegen Sniffing und MitM, aber NICHT gegen SYN-Flooding, SQL-Injection oder DoS? Leiten Sie es aus dem jeweils angegriffenen Schutzziel ab.**
> **✅ Lösungsweg:** TLS schützt **Vertraulichkeit + Integrität der Übertragung** → gegen **Sniffing** (mitlesen sinnlos) und **MitM** (verändern/einklinken erkennbar) wirksam. Aber: **SYN-Flooding/DoS** greifen die **Verfügbarkeit** an — der Server ertrinkt in Verbindungsversuchen, *bevor* überhaupt Nutzdaten verschlüsselt werden; TLS ändert daran nichts. **SQL-Injection/XSS** passieren **in der Anwendung**, *nachdem* TLS entschlüsselt hat — die bösartige Eingabe ist dann Klartext und wird ungeprüft verarbeitet. Lehre: **jede Maßnahme schützt nur ihr Schutzziel/ihre Schicht** — es gibt kein Allheilmittel.

**T3 [N]. Ein Webserver-Admin sieht in seinen Logs viele 404 auf /admin, /phpmyadmin, /backup. Ist das schon ein Einbruch? Was tun Sie, und was wäre das Alarmsignal für einen erfolgreichen Angriff?**
> **✅ Lösungsweg:** **Kein Einbruch** — 404 = „nicht gefunden", es ist **automatisiertes Scanning (Reconnaissance)**, läuft im Internet ständig. **Tun:** nicht ignorieren — Rate-Limiting/Fail2ban, Admin-Oberflächen nicht öffentlich erreichbar (VPN/IP-Whitelist), patchen, Monitoring. **Alarmsignal für Erfolg:** plötzlich **Status 200** auf so einem Pfad (Oberfläche existiert und wurde erreicht), ungewöhnliche **POST** mit SQL-Fragmenten (`UNION SELECT`, `' OR 1=1`), oder **ausgehende** Verbindungen des Servers (z. B. Port 4444) → Backdoor. Unterscheidung **Vorbereitung vs. erfolgter Angriff** ist hier der Kern.

**T4 [1,0]. Erklären Sie, warum Port-Scanning juristisch heikel, aber technisch „kein Angriff" ist — und wann genau es strafbar wird. (Verknüpft Kap. 1 Recht + Kap. 3.)**
> **✅ Lösungsweg:** **Technisch:** ein Scan verändert/betritt nichts, er „klopft nur an" (fragt Portzustände ab) → im engeren Sinn kein Angriff, nur **Informationsbeschaffung** (Angriffsvorbereitung, [01 → 1.5](01_Einfuehrung.md)). **Juristisch:** ohne **Auftrag des Inhabers** fällt das Ausspähen/Vorbereiten in den Bereich **§ 202c StGB** (Vorbereiten des Ausspähens), und wenn danach eine Lücke genutzt wird, **§ 202a**. **Legal** ist es mit **Pentest-Auftrag** oder im Rahmen der **akademischen Ausbildung** in dafür vorgesehenen Umgebungen. Pointe: dieselbe Handlung ist legal oder strafbar je nach **Vorsatz und Autorisierung** — nicht je nach Technik.

## F. Самопроверка перед Kap. 4 (с ответами)
**F1. Sniffing — Definition, aktiv/passiv, Schutzziel, Schutz?** Mitlesen von Daten; **passiv**; **Vertraulichkeit**; Verschlüsselung (TLS/IPsec).
**F2. Wie wird MitM per ARP-Spoofing erreicht und woran erkennt man ihn?** Gefälschte ARP-Replies → beide Caches zeigen die Angreifer-MAC; Erkennung: **zwei IPs, eine MAC**, Wireshark-Warnung „duplicate use".
**F3. SYN-Flooding — Mechanismus und Schutzziel?** Viele SYNs ohne ACK → Puffer voll mit halboffenen Verbindungen (`SYN_RECV`) → **Verfügbarkeit**; Schutz SYN-Cookies.
**F4. DoS vs. DDoS vs. Botnetz?** DoS = eine Quelle; DDoS = viele; Botnetz = Verbund infizierter Bots, gesteuert über C&C.
**F5. SQLi vs. XSS — wo läuft der Code, was ist die Hauptgegenmaßnahme?** SQLi: Code läuft in der **Datenbank** → Prepared Statements. XSS: Code läuft im **Browser/Server** → Output-Encoding/CSP.
**F6. Exploit vs. Payload?** Exploit nutzt die Lücke/verschafft Zugriff; Payload ist der danach ausgeführte Schadcode.
**F7. Port-Zustände beim Scan?** open (SYN/ACK), closed (RST), filtered (keine Antwort).
**F8. Reverse- vs. Bind-Shell?** Reverse: Opfer → Angreifer (umgeht Firewall). Bind: Angreifer → Opferport.
**F9. Phishing erkennen?** Domain+TLD prüfen (nicht Subdomain/Pfad), Schreibweise, Zertifikat; HTTPS ≠ Vertrauensbeweis.

Если на всё ответил без подсказки — переходим к `04_Anonymitaet_TOR.md`.
