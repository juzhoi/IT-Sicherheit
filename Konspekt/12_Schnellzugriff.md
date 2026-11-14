# ⚡ Schnellzugriff — Kurzantworten für die 60–80 Fragen
**Быстрый слой поверх конспекта.** Для коротких вопросов (Nennen, Definieren, Zuordnen, Richtig/Falsch).

> **Как пользоваться на экзамене:**
> 1. **Cmd+F** по ключевому слову (немецкому или русскому) → ровно одна строка с ответом.
> 2. Если нужно развёрнуто (Erklären/Erläutern) → перейди по 🔗 в подробную главу.
> 3. Готовые немецкие абзацы для открытых вопросов есть в блоках **Musterantwort** внутри глав и в [11_Uebungen.md](11_Uebungen.md) (Probeklausur с решениями).
>
> Каждый термин здесь встречается **один раз** — поэтому Cmd+F сразу попадает в цель (в отличие от поиска по всем главам).
> **Страницы** = страницы скрипта (PDF = +6). ➕ = сверх скрипта.

---

## 🧭 Маршрут: тип вопроса → куда идти
| Что за вопрос | Куда |
|---|---|
| Короткое «что это / назови» | **этот файл**, Cmd+F |
| Открытый вопрос (3–6 предложений) | глава по теме → блок **Musterantwort** / [11_Uebungen.md](11_Uebungen.md) |
| Zuordnung (1a 2b 3c) | этот файл → раздел **🔗 Zuordnungstabellen** (ниже) |
| Расчёт 🧮 | [00_Index.md](00_Index.md) §4 Формулы → подробно в главе |
| Лог/Wireshark 🦈 | [09_Wireshark_Guide.md](09_Wireshark_Guide.md) |
| Про практикум | [08_Praktika.md](08_Praktika.md) |
| Забыл слово | [10_Glossar_DE_RU.md](10_Glossar_DE_RU.md) |

---

## Kapitel 1 — Grundlagen 🔗[01](01_Einfuehrung.md)
- **IT-Sicherheit (Eckert):** schützt Unternehmen/Werte, verhindert wirtschaftliche Schäden durch Verletzung von Vertraulichkeit, Integrität, Verfügbarkeit.
- **Safety** = Funktionssicherheit (Schutz der Umwelt **vor** dem System, unbeabsichtigte Fehler) · **Security** = Informationssicherheit (Schutz des Systems **vor** Angreifern, beabsichtigt).
- **Vertraulichkeit** (confidentiality): wer darf **lesen**? Verletzt durch Sniffing/Datenleck. Schutz: Verschlüsselung, Zugriffskontrolle.
- **Integrität** (integrity): kann man **unbemerkt ändern**? Verletzt durch Manipulation. Schutz: Hash/MAC/Signatur.
- **Verfügbarkeit** (availability): funktioniert es, **wenn nötig**? Verletzt durch DoS/Ransomware. Schutz: Redundanz, Backups.
- **Authentizität** (authenticity): ist es **wirklich** der/das Richtige? Schutz: Passwort, Zertifikat, Challenge-Response.
- **Verbindlichkeit / Nichtabstreitbarkeit** (non-repudiation): kann man es **später abstreiten**? Schutz: digitale Signatur, Protokolle.
- **CIA-Triade** = **C**onfidentiality, **I**ntegrity, **A**vailability (die 3 Kern-Schutzziele).
- **Schwachstelle** (vulnerability): Schwäche/Lücke im System. **Bedrohung** (threat): mögliches Ereignis, das eine Schwachstelle ausnutzt. **Gefährdung**: Bedrohung trifft auf passende Schwachstelle. **Angriff**: aktive Ausnutzung.
- **Risiko** R = **Eintrittswahrscheinlichkeit p · Schadenshöhe S**.
- **Passiver Angriff**: nur **mitlesen** (z. B. Sniffing) → Vertraulichkeit, schwer zu erkennen. **Aktiver Angriff**: **eingreifen/verändern** (z. B. MitM, DoS) → Integrität/Verfügbarkeit.
- **Angreifertypen:** Script-Kiddie, Hacktivist, (organisierte) Kriminelle, Innentäter (Insider), Nachrichtendienste/APT, Konkurrenz.
- **Angriffsphasen:** Reconnaissance → Scanning → Zugriff/Exploit → Zugang sichern → Spuren verwischen.
- **Recht:** **§202a** StGB Ausspähen von Daten, **§202b** Abfangen, **§202c** „Hackerparagraf" (Vorbereiten), **§303a** Datenveränderung, **§303b** Computersabotage. **DSGVO** Art. 32 = Sicherheit der Verarbeitung.
- **Sicherheitsmanagement / ISMS:** PDCA-Zyklus (Plan-Do-Check-Act); Standards **ISO 27001**, **BSI IT-Grundschutz**.
- 🧮 **Verfügbarkeit** = (Gesamtlaufzeit − Ausfallzeit) / Gesamtlaufzeit. „Drei Neunen" 99,9 % ≈ 8,76 h Ausfall/Jahr.

## Kapitel 2 — Rechnernetze 🔗[02](02_Rechnernetze.md)
- **OSI-Modell (7 Schichten):** 1 Bitübertragung · 2 Sicherung (MAC, Frames) · 3 Vermittlung (IP, Routing) · 4 Transport (TCP/UDP) · 5 Sitzung · 6 Darstellung · 7 Anwendung. Merksatz: *Alle Deutschen Studenten Trinken Verschiedene Sorten Bier* (7→1) / *Please Do Not Throw Sausage Pizza Away* (1→7).
- **TCP/IP-Modell (4):** Netzzugang · Internet (IP) · Transport (TCP/UDP) · Anwendung.
- **IP (Schicht 3):** logische Adressierung über Netzgrenzen, **verbindungslos, unzuverlässig** („best effort").
- **IPv4** = 32 Bit (4 Oktette), **IPv6** = 128 Bit.
- **TCP (Schicht 4):** verbindungsorientiert, **zuverlässig**, Reihenfolge, Flusskontrolle. **UDP:** verbindungslos, schnell, **ohne** Garantie (DNS, Streaming, VoIP).
- **TCP-3-Wege-Handshake:** **SYN → SYN/ACK → ACK**. Abbau: FIN/ACK (4-Wege) oder **RST** (Abbruch).
- **TCP-Flags:** **SYN** Verbindungsaufbau · **ACK** Bestätigung · **FIN** sauberer Abbau · **RST** harter Abbruch · **PSH** sofort weiterreichen · **URG** dringend.
- **Ports well-known:** **20/21** FTP · **22** SSH · **23** Telnet · **25** SMTP · **53** DNS · **67/68** DHCP · **80** HTTP · **110** POP3 · **143** IMAP · **443** HTTPS · **445** SMB · **3389** RDP.
- **MAC-Adresse** = 48 Bit, Schicht 2, physisch, pro Netzkarte.
- **DNS:** Name → IP (Port 53). **DHCP:** vergibt automatisch IP-Konfiguration (DORA: Discover-Offer-Request-Ack). **ARP:** IP → MAC im lokalen Netz. **ICMP:** Steuer-/Fehlermeldungen (ping, traceroute).
- 🧮 **Hosts im Subnetz** = 2^(32−n) − 2. **Netz** = IP AND Maske; **Broadcast** = Hostbits alle 1.
- 🧮 **Ack = Seq + Len** (bei SYN/FIN zählt +1).

## Kapitel 3 — Angriffe 🔗[03](03_Angriffe.md)
- **Sniffing:** passives Mitlesen (Vertraulichkeit). Gegen: Verschlüsselung.
- **Spoofing:** Fälschen einer Identität (IP/MAC/DNS/Mail).
- **ARP-Spoofing/-Poisoning:** gefälschte ARP-Replies → **MitM** im lokalen Netz (ARP ist nicht authentifiziert). 🔗[08 P02](08_Praktika.md).
- **MitM (Man-in-the-Middle):** Angreifer zwischen zwei Parteien, liest/verändert Verkehr.
- **DoS / DDoS:** Überlastung → Verfügbarkeit. **DDoS** = verteilt über **Botnetz** (C&C-Server).
- **SYN-Flooding:** viele halboffene TCP-Verbindungen (SYN ohne ACK) → Ressourcen erschöpft. Gegen: SYN-Cookies.
- **TCP-Hijacking / Sequenznummernangriff:** Übernahme einer bestehenden TCP-Sitzung durch Raten/Kennen der Seq-Nummer.
- **DNS-Cache-Poisoning:** gefälschte DNS-Antwort im Cache → Opfer auf falsche IP umgeleitet. Gegen: DNSSEC.
- **SQL-Injection:** ungeprüfte Eingabe als SQL-Code (`' OR 1=1 --`). Gegen: **Prepared Statements**. 🔗[08 P03](08_Praktika.md).
- **XSS (Cross-Site-Scripting):** fremdes JavaScript im Browser anderer Nutzer. **Stored** (gespeichert), **Reflected**, **DOM**. Gegen: Output-Encoding, serverseitige Validierung.
- **Command Injection:** eigene OS-Befehle über ungeprüfte Eingabe (`; ls`, `&& ls`).
- **Drive-by-Download:** Schadcode beim bloßen Besuch einer Seite.
- **Phishing:** Täuschung über gefälschte Mails/Seiten → Zugangsdaten. **Social Engineering** = Angriff über den Menschen.
- **Port-Scanning:** offene Ports/Dienste finden (nmap). **Pentest:** systematische, autorisierte Sicherheitsprüfung.
- **Metasploit:** Exploit-Framework. **Exploit** nutzt Lücke, **Payload** = danach ausgeführter Schadcode.

## Kapitel 4 — Anonymität / TOR 🔗[04](04_Anonymitaet_TOR.md)
- **Proxy:** Vermittler, zeigt eigene IP statt deiner (einstufig, sieht Ziel+Inhalt).
- **TOR:** **Onion Routing** über ≥3 Knoten (**Guard/Entry** → **Middle** → **Exit**); jede Schicht einzeln verschlüsselt; trennt **IP (Identität)** von **Inhalt**.
- **Exit-Knoten** sieht den unverschlüsselten Inhalt (falls kein HTTPS) → Risiko.
- **Hidden Service / `.onion`:** Dienst nur im TOR-Netz, Betreiber-IP verborgen (v3 = 56 Zeichen).
- **TOR-Nachteile:** langsam; Überwachung vieler Knoten; Angreifer mit **erstem + letztem** Knoten kann deanonymisieren.
- **VPN vs. TOR:** VPN = ein Server (kennt Quelle+Ziel+Inhalt), TOR = viele Knoten (keiner kennt beides). **Kaskade** (JonDo) = feste Betreiber.
- **Surface / Deep / Dark Web + Darknet:** → 🔗 Zuordnungstabelle unten / [08 P03](08_Praktika.md).

## Kapitel 5 — Malware 🔗[05](05_Malware.md)
- **Virus:** hängt sich an **Wirtsdatei**, braucht Nutzeraktion zur Ausführung, verbreitet sich **mit** dem Wirt.
- **Wurm:** **selbstständig**, verbreitet sich **ohne** Wirt und ohne Nutzeraktion über Netz/Lücken.
- **Trojaner:** als nützlich **getarnt**, enthält versteckte Schadfunktion; verbreitet sich **nicht** selbst.
- **Backdoor / Hintertür:** heimlicher Zugang unter Umgehung der normalen Authentifizierung.
- **Ransomware:** verschlüsselt Daten, fordert Lösegeld → **Verfügbarkeit**.
- **Rootkit:** versteckt Schadsoftware tief im System. **Spyware:** sammelt heimlich Daten. **Keylogger:** zeichnet Tastatur auf. **Adware:** Werbung. **Bot/Zombie:** ferngesteuert im **Botnetz**.
- **Dropper/Loader:** lädt/installiert eigentliche Malware nach. **Logische Bombe:** Schadfunktion bei bestimmter Bedingung.
- **Erkennung:** **signaturbasiert** (bekannte Muster, nur Bekanntes) · **heuristisch** (verdächtige Merkmale, auch Neues, Fehlalarme) · **verhaltensbasiert** (Beobachtung zur Laufzeit, Sandbox).

## Kapitel 6 — Kryptografie 🔗[06](06_Kryptografie.md)
- **Symmetrisch:** **ein** Schlüssel für Ver- und Entschlüsseln (AES, DES, 3DES). Schnell. Problem: **Schlüsselaustausch**.
- **Asymmetrisch / Public-Key:** **öffentlicher** (verschlüsseln) + **privater** (entschlüsseln) Schlüssel (RSA). Langsam, löst Austausch.
- **Hybrid:** asymmetrisch den **Sitzungsschlüssel** austauschen, dann symmetrisch Daten (TLS).
- **Kerckhoffs-Prinzip:** Sicherheit liegt **nur im Schlüssel**, nicht im geheimen Verfahren.
- **Konfusion** (Zusammenhang Schlüssel↔Chiffre verschleiern) + **Diffusion** (Streuung des Klartexts über die Chiffre).
- **Blockchiffre** (feste Blöcke, AES/DES) vs. **Stromchiffre** (bitweise).
- **AES:** symmetrische Blockchiffre, 128-Bit-Block, Schlüssel 128/192/256 Bit (Rijndael). Sicher.
- **Betriebsmodi:** **ECB** (Blöcke unabhängig → Muster bleiben, unsicher) · **CBC** (XOR mit vorigem Block → IV nötig) · CTR/GCM (➕, GCM = mit Authentisierung).
- **Hashfunktion:** Einwegfunktion, feste Länge; **kollisionsresistent**; prüft **Integrität** (SHA-256). **MD5/SHA-1 unsicher.**
- **MAC:** Hash **mit geheimem Schlüssel** → Integrität **+** Authentizität (symmetrisch, nicht abstreitbar nur zwischen den zwei Parteien).
- **Digitale Signatur:** Hash mit **privatem** Schlüssel verschlüsselt → Integrität, Authentizität, **Verbindlichkeit** (jeder prüft mit öffentlichem Schlüssel).
- **Zertifikat / PKI:** **CA** bestätigt, dass ein öffentlicher Schlüssel zu einer Identität gehört (X.509). Löst Vertrauen beim Public-Key.
- **Diffie-Hellman:** Schlüsselaustausch über unsicheren Kanal (gemeinsames Geheimnis, ohne es zu senden).
- **One-Time-Pad:** Schlüssel so lang wie Text, zufällig, einmalig → **perfekt sicher**, aber unpraktisch.
- 🧮 **RSA:** n=p·q, φ=(p−1)(q−1), e·d≡1 mod φ; c=mᵉ mod n, m=cᵈ mod n.
- 🧮 **Symm. Schlüsselanzahl** = n·(n−1)/2. **Geburtstagsparadoxon:** Kollision ≈ 2^(N/2).
- **Klassisch:** Caesar (monoalphabetisch) · Vigenère (polyalphabetisch) · Playfair (bigraphisch). 🔗[08 P06](08_Praktika.md).

## Kapitel 7 — Zugriffskontrolle 🔗[07](07_Zugriffskontrolle.md)
- **Subjekt** (wer, Nutzer/Prozess) · **Objekt** (worauf, Datei/Ressource) · **Recht/Operation** (lesen/schreiben/ausführen).
- **Zugriffsmatrix:** Subjekte × Objekte → Rechte. **ACL** = spaltenweise (pro Objekt: wer darf was). **Capability** = zeilenweise (pro Subjekt).
- **DAC** (Discretionary): **Eigentümer** vergibt Rechte (Unix). Flexibel, aber Trojaner-anfällig.
- **MAC** (Mandatory): **System/Policy** erzwingt Rechte über Labels/Stufen, Nutzer kann nicht ändern.
- **RBAC** (Role-Based): Rechte an **Rollen**, Nutzer bekommt Rollen.
- **Bell-LaPadula** (Vertraulichkeit): **no read up** (SC(S)≥SC(O)) + **no write down** (SC(S)≤SC(O)). „Read down, write up."
- **Biba** (Integrität): umgekehrt — no read down, no write up.
- **Chinese Wall / Brewer-Nash:** dynamisch, verhindert Interessenkonflikte (nach erstem Zugriff gesperrte Konkurrenzdaten).
- 🧮 **Unix-Rechte:** r=4, w=2, x=1; je Gruppe (Besitzer/Gruppe/Andere) summieren → z. B. `rwxr-x---` = 750.

---

# 🔗 Zuordnungstabellen (für 1a-2b-3c-Aufgaben)

### Angriff → OSI-Schicht
| Angriff | Schicht |
|---|---|
| MAC-Spoofing, ARP-Spoofing | 2 (Sicherung) |
| IP-Spoofing, ICMP-Flood, Smurf | 3 (Vermittlung) |
| SYN-Flooding, TCP-Hijacking, Port-Scan | 4 (Transport) |
| DNS-Spoofing, SQLi, XSS, Phishing, HTTP-Angriffe | 7 (Anwendung) |

### Angriff → verletztes Schutzziel (primär)
| Angriff | Schutzziel |
|---|---|
| Sniffing, Phishing, Datenleck | Vertraulichkeit |
| MitM-Manipulation, SQLi (Daten ändern), XSS | Integrität |
| DoS/DDoS, SYN-Flood, Ransomware | Verfügbarkeit |
| gefälschte Mail/Seite, Spoofing | Authentizität |
| Abstreiten einer Transaktion | Verbindlichkeit |

### Malware-Typ → Merkmal
| Typ | Merkmal |
|---|---|
| Virus | braucht Wirtsdatei + Nutzeraktion |
| Wurm | verbreitet sich selbst über Netz, ohne Wirt |
| Trojaner | als nützlich getarnt, versteckte Schadfunktion |
| Ransomware | verschlüsselt Daten, Lösegeld (Verfügbarkeit) |
| Rootkit | versteckt sich tief im System |
| Backdoor | heimlicher Zugang, umgeht Authentifizierung |
| Bot/Zombie | ferngesteuert im Botnetz (C&C) |

### Krypto-Verfahren → Eigenschaft
| Verfahren | Typ / Zweck |
|---|---|
| AES, DES, 3DES | symmetrisch (Vertraulichkeit) |
| RSA | asymmetrisch (Verschlüsselung, Signatur) |
| Diffie-Hellman | asymmetrischer Schlüsselaustausch |
| SHA-256 | Hash (Integrität) |
| HMAC | MAC (Integrität + Authentizität, symm.) |
| Digitale Signatur | Integrität + Authentizität + Verbindlichkeit |
| Zertifikat / CA | bindet Public Key an Identität |

### Dienst → Port
| Dienst | Port |
|---|---|
| SSH | 22 |
| Telnet | 23 |
| SMTP | 25 |
| DNS | 53 |
| HTTP | 80 |
| HTTPS | 443 |
| SMB | 445 |
| RDP | 3389 |

### Zugriffsmodell → Kernidee
| Modell | Kernidee |
|---|---|
| DAC | Eigentümer vergibt Rechte |
| MAC | System erzwingt über Labels/Stufen |
| RBAC | Rechte über Rollen |
| Bell-LaPadula | Vertraulichkeit: no read up, no write down |
| Biba | Integrität: no read down, no write up |
| Chinese Wall | Interessenkonflikte verhindern |

### Web-/Infrastruktur-Begriff → Bedeutung (Zuordnung aus P03)
| Begriff | Bedeutung |
|---|---|
| Internet | Infrastruktur-Grundlage |
| Darknet | Netz auf dem Internet (TOR, I2P …) — Infrastruktur |
| World Wide Web | alle erreichbaren Inhalte |
| Surface Web | indexiert, frei zugänglich (kleinster Teil) |
| Deep Web | nicht indexiert, hinter Login (größter Teil) |
| Dark Web | Inhalte im Darknet, `.onion`, spezielle Software |

### Werkzeug → Zweck (aus den Praktika)
| Werkzeug | Zweck |
|---|---|
| nmap / Zenmap | Port-/Netzwerkscan, OS-Erkennung |
| arpspoof | ARP-Spoofing / MitM |
| Wireshark | Netzwerkmitschnitt analysieren |
| SET | Phishing (Seiten klonen) |
| sqlmap | automatisierte SQL-Injection |
| Nikto | Webserver-Schwachstellenscan |
| Greenbone/OpenVAS | Schwachstellenscanner |
| Metasploit | Exploits + Payloads |
| msfvenom | Payload erzeugen |
| CrypTool 2 | Chiffren lernen/brechen |
