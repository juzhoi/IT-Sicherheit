# Glossar DE–RU — alphabetisch
**Für Zuordnungsaufgaben (1a 2b 3c) und schnelles Nachschlagen**

> **О чём файл.** Один алфавитный список всех ключевых терминов курса: **термин (DE) — короткое определение — перевод/пояснение (RU) — ссылка на главу**. Специально под задания «сопоставьте термин с определением»: открываешь, Cmd+F по термину или по русскому слову.
>
> **Как читается строка:** **Begriff** — *Kurzdefinition auf Deutsch* — RU-пояснение — 🔗 глава.

## Навигация по буквам
[A](#a) · [B](#b) · [C](#c) · [D](#d) · [E](#e) · [F](#f) · [G](#g) · [H](#h) · [I](#i) · [K](#k) · [L](#l) · [M](#m) · [N](#n) · [O](#o) · [P](#p) · [R](#r) · [S](#s) · [T](#t) · [U](#u) · [V](#v) · [W](#w) · [X](#x) · [Z](#z)

---
## A
- **Adware** — *Programm, das Nutzerdaten auswertet und Werbung einblendet* — рекламное ПО — 🔗 05
- **Anonymität** — *nur mit unverhältnismäßigem Aufwand ist Rückschluss auf die Identität möglich* — анонимность — 🔗 01, 04
- **Anonymisierung** — *Verändern personenbezogener Daten, sodass sie **nicht mehr** zuordenbar sind (irreversibel)* — анонимизация — 🔗 01
- **ACL (Access Control List)** — *Liste beim **Objekt**: wer darf was (Spalte der Zugriffsmatrix)* — список контроля доступа — 🔗 07
- **ARP (Address Resolution Protocol)** — *löst IP → MAC im lokalen Netz (Request Broadcast, Reply Unicast)* — протокол разрешения адресов — 🔗 02
- **ARP-Spoofing/-Poisoning** — *gefälschte ARP-Replies → MitM im LAN* — подмена ARP — 🔗 03
- **AES** — *symmetrische Blockchiffre, 128-Bit-Block, Schlüssel 128/192/256, 10/12/14 Runden* — стандарт AES — 🔗 06
- **Authentizität** — *Echtheit/Glaubwürdigkeit eines Objekts/Subjekts, überprüfbar* — подлинность — 🔗 01
- **Authentisierung** — *Subjekt **weist** seine Identität nach (Passwort eingeben)* — предъявление личности — 🔗 01
- **Authentifizierung** — *System **prüft** den Identitätsnachweis* — проверка личности — 🔗 01
- **Autorisierung** — *Vergabe/Prüfung von Rechten (was darf das Subjekt)* — авторизация — 🔗 01, 07
- **Avalanche-Effekt** — *1 Bit geändert → ~50 % der Geheimtextbits ändern sich* — лавинный эффект — 🔗 06

## B
- **Backdoor** — *absichtlich eingebauter versteckter Zugang, umgeht Sicherung* — чёрный ход — 🔗 05
- **Bakterie (Rabbit, Fork-Bomb)** — *repliziert sich exponentiell auf **einem** System, frisst Ressourcen* — бактерия — 🔗 05
- **Bedrohung (Threat)** — *konkrete, potenzielle Gefahr mit Bezug zu einem Schutzziel* — угроза — 🔗 01
- **Bell-LaPadula (BLP)** — *MAC-Modell für Vertraulichkeit; no-read-up, no-write-down* — модель Белла-Лападулы — 🔗 07
- **Blockchiffre** — *verschlüsselt Klartext in Blöcken fester Länge (z. B. 128 Bit)* — блочный шифр — 🔗 06
- **Botnetz (Bot/Zombie)** — *Verbund infizierter, ferngesteuerter Rechner (über C&C-Server)* — ботнет — 🔗 03, 05
- **Broadcast** — *Nachricht an alle im Subnetz (letzte Adresse / 255.255.255.255)* — широковещание — 🔗 02
- **Brute-Force** — *alle Schlüssel/Passwörter durchprobieren* — полный перебор — 🔗 03, 06
- **Buffer Overflow** — *Eingabe überschreibt Speicher/Rücksprungadresse → fremder Code* — переполнение буфера — 🔗 03

## C
- **C&C-Server (Command & Control)** — *steuert ein Botnetz zentral* — командный сервер — 🔗 03
- **Caesar-Chiffre** — *monoalphabetische Substitution, Verschiebung um k* — шифр Цезаря — 🔗 08
- **Capability** — *Liste beim **Subjekt**: was darf ich (Zeile der Matrix)* — возможность/мандат — 🔗 07
- **CBC (Cipher Block Chaining)** — *jeder Block XOR mit vorherigem Geheimtextblock + IV* — режим сцепления блоков — 🔗 06
- **Certificate Authority (CA)** — *Zertifizierungsstelle, signiert/beglaubigt öffentliche Schlüssel* — удостоверяющий центр — 🔗 06
- **Chain of Trust** — *Kette Root-CA → Zwischen-CA → Endzertifikat* — цепочка доверия — 🔗 06
- **Challenge-Response** — *Server stellt Aufgabe, nur echter Besitzer löst sie (Secret nicht übertragen)* — запрос-ответ — 🔗 01, 06
- **Chinese Wall** — *Zugriff abhängig von der Historie; nach Zugriff auf U kein Konkurrent U' derselben COI-Klasse* — китайская стена — 🔗 07
- **CIA-Triade** — *Confidentiality, Integrity, Availability* — конфиденциальность/целостность/доступность — 🔗 01
- **CIDR (/n)** — *Suffix = Anzahl Netzbits, z. B. /24 = 255.255.255.0* — бесклассовая маска — 🔗 02
- **Cookie** — *kleine Info im Browser, zur Wiedererkennung* — куки — 🔗 04
- **Covert Channel** — *verdeckter Kanal, versteckt Existenz einer Nachricht* — скрытый канал — 🔗 01
- **Cracker** — *setzt Fähigkeiten kriminell ein (Kopierschutz knacken, Daten verkaufen)* — взломщик-преступник — 🔗 01
- **CRL (Certificate Revocation List)** — *Liste widerrufener Zertifikate* — список отзыва сертификатов — 🔗 06
- **Cross-Site Scripting (XSS)** — *fremder Skriptcode in Webseiten eingeschleust (client-/serverseitig)* — межсайтовый скриптинг — 🔗 03

## D
- **DAC (Discretionary Access Control)** — *Eigentümer vergibt Rechte nach Ermessen, weitergebbar* — дискреционный доступ — 🔗 07
- **Darknet** — *Infrastruktur (Netz) auf dem Internet, spezielle SW nötig (z. B. TOR)* — даркнет (инфраструктура) — 🔗 08
- **Dark Web** — *Inhalte/Dienste, nur über ein Darknet erreichbar (.onion)* — дарквеб (контент) — 🔗 08
- **Datenschutz** — *Schutz der **Person**/Privatsphäre (informationelle Selbstbestimmung)* — защита перс. данных — 🔗 01
- **Datensicherheit** — *Schutz der **Daten** (jeder Art) vor Verlust/Manipulation/Diebstahl* — безопасность данных — 🔗 01
- **Deep Web** — *nicht von Suchmaschinen indexiert (Onlinebanking, E-Mail) — größter Teil* — глубокий веб — 🔗 08
- **DHCP** — *weist dynamisch IP/Maske/Gateway/DNS zu (UDP 67/68, DORA)* — автонастройка IP — 🔗 02
- **Diffie-Hellman (DH)** — *Schlüsselaustausch (kein Verschlüsseln!), gemeinsamer Schlüssel ohne Übertragung; kein MitM-Schutz* — обмен ключами DH — 🔗 06
- **Diffusion** — *Information über Geheimtext verteilen (durch Permutation)* — диффузия — 🔗 06
- **Digitale Signatur** — *mit privatem Schlüssel Hash signieren → Authentizität + Integrität + **Verbindlichkeit*** — цифровая подпись — 🔗 06
- **DNS** — *übersetzt Name ↔ IP (UDP 53), ungesichert* — система доменных имён — 🔗 02
- **DNS-Spoofing / Cache-Poisoning** — *gefälschte DNS-Antworten → Umleitung (Pharming)* — подмена DNS — 🔗 03
- **DoS / DDoS** — *Überlasten des Systems → Verfügbarkeit weg (DDoS = von vielen Bots)* — отказ в обслуживании — 🔗 03
- **Drive-by-Download** — *Malware beim bloßen Besuch/Download einer Seite* — скрытая загрузка — 🔗 03
- **Dropper** — *Programm, das Malware nachlädt/aktiviert* — дроппер — 🔗 05

## E
- **ECB (Electronic Code Book)** — *jeder Block unabhängig → gleiche Blöcke gleich → unsicher (Muster)* — режим ECB — 🔗 06
- **Einwegfunktion (mit Falltür)** — *vorwärts leicht, rückwärts praktisch unmöglich (außer mit privatem Schlüssel)* — односторонняя функция с люком — 🔗 06
- **Exploit** — *nutzt eine Schwachstelle aus, verschafft Zugriff (allein kein Schaden)* — эксплойт — 🔗 03
- **Evil Twin** — *gefälschter WLAN-Access-Point* — злой двойник (Wi-Fi) — 🔗 03

## F
- **Fingerprint (Schlüssel)** — *Hash eines öffentlichen Schlüssels, zum Vergleich über zweiten Kanal* — отпечаток ключа — 🔗 06
- **Browser-Fingerprinting** — *Systemeinstellungen auslesen → individuelles Profil ohne Cookie* — отпечаток браузера — 🔗 04
- **Firewall / Paketfilter** — *filtert Netzverkehr nach Regeln (statische Zugriffsmatrix)* — межсетевой экран — 🔗 02, 07
- **Flusskontrolle** — *Empfänger steuert Senderate über Window (Zero Window = Stopp)* — управление потоком — 🔗 02

## G
- **Gefahr** — *abstrakt: möglicher Schaden ohne konkreten Bezug* — опасность (абстрактно) — 🔗 01
- **Gefährdung** — *Bedrohung **trifft** auf Schwachstelle → ausnutzbar* — угроза+уязвимость — 🔗 01
- **Geburtstagsparadoxon** — *Kollision schon bei ~1,2·√n Werten → Hash doppelt so lang wie Zielsicherheit* — парадокс дней рождения — 🔗 06
- **Grundfunktionen (sicherer Systeme)** — *Identifikation, Authentisierung, Rechteverwaltung, Rechteprüfung, Beweissicherung* — базовые функции — 🔗 01

## H
- **Hacker** — *ursprünglich IT-Experte, folgt oft Hackerethik (positiv besetzt)* — хакер — 🔗 01
- **Hashfunktion** — *beliebige Länge → feste Länge; für Integrität; einweg, kollisionsresistent* — хеш-функция — 🔗 06
- **HMAC** — *MAC auf Hash-Basis (Schlüssel + Hash)* — HMAC — 🔗 06
- **Hidden Service (.onion)** — *nur über TOR erreichbar, verlässt das Netz nicht* — скрытый сервис — 🔗 04
- **HRU-Modell** — *dynamische Zugriffsmatrix, 6 Elementaroperationen* — модель Харрисона-Руццо-Ульмана — 🔗 07
- **Hybrides Verfahren** — *asymmetrisch Schlüssel austauschen + symmetrisch Daten (TLS, PGP, SSH)* — гибридная схема — 🔗 06

## I
- **IBAC (Identity Based)** — *Rechte an die Identität des Benutzers gebunden* — доступ по личности — 🔗 07
- **ICMP** — *Fehler-/Diagnoseinfos (ping=Echo, Time Exceeded, Unreachable)* — протокол ICMP — 🔗 02
- **Integrität** — *Daten nicht unautorisiert & unbemerkt veränderbar* — целостность — 🔗 01
- **IP-Spoofing** — *gefälschte Absender-IP, täuscht interne Station vor* — подмена IP — 🔗 03
- **IPsec** — *Verschlüsselung/Authentisierung auf Schicht 3* — IPsec — 🔗 06
- **IPv4 / IPv6** — *32 Bit (dezimal) / 128 Bit (hex), IPv6 kein NAT, IPsec* — версии IP — 🔗 02
- **I2P** — *dezentrales P2P-Anonymisierungsnetz, Eepsites* — проект невидимого интернета — 🔗 04
- **IV (Initialisierungsvektor)** — *zufälliger Startwert für CBC/OFB/CFB* — вектор инициализации — 🔗 06

## K
- **Kerckhoffs'sches Prinzip** — *Sicherheit nur im **Schlüssel**, nicht im Algorithmus* — принцип Керкгоффса — 🔗 06
- **Keylogger** — *zeichnet Tastatureingaben auf (Spyware)* — кейлоггер — 🔗 05
- **Kollision** — *zwei verschiedene Eingaben, gleicher Hash* — коллизия — 🔗 06
- **Konfusion** — *Zusammenhang Klartext↔Geheimtext verschleiern (durch Substitution)* — конфузия — 🔗 06
- **KRITIS** — *kritische Infrastruktur (Energie, Wasser…), Meldepflicht ans BSI* — критическая инфраструктура — 🔗 01
- **Kryptoanalyse** — *Verschlüsselung brechen ohne Schlüssel* — криптоанализ — 🔗 06
- **Kryptografie** — *Verfahren zum Ver-/Entschlüsseln* — криптография — 🔗 06
- **Kryptologie** — *Oberbegriff für Kryptografie + Kryptoanalyse* — криптология — 🔗 06

## L
- **Least Privilege (restriktive Rechtevergabe)** — *nur unverzichtbare Rechte vergeben* — минимальные привилегии — 🔗 01, 07
- **Loopback (127.0.0.1)** — *der eigene Rechner* — петля/локальный хост — 🔗 02

## M
- **MAC-Adresse** — *physische 48-Bit-Adresse der Netzkarte (Schicht 2), per SW änderbar* — MAC-адрес — 🔗 02
- **MAC (Mandatory Access Control)** — *System erzwingt feste Label-Regeln, Nutzer kann nicht ändern* — мандатный доступ — 🔗 07
- **MAC (Message Authentication Code)** — *Prüfsumme **mit Schlüssel**; Integrität+Authentizität, **keine** Verbindlichkeit* — код аутентификации сообщения — 🔗 06
- **Makrovirus** — *Virus in Dokumenten mit Makros (z. B. Melissa)* — макровирус — 🔗 05
- **Malware** — *Software mit absichtlich schädlichen Funktionen* — вредоносное ПО — 🔗 05
- **Man-in-the-Middle (MitM)** — *Angreifer zwischen zwei Partnern, liest/verändert* — человек посередине — 🔗 03
- **MD5** — *128-Bit-Hash, **unsicher** (Kollisionen)* — хеш MD5 — 🔗 06
- **Metasploit** — *Pentesting-Framework (Exploits, Payloads, Encoder, NOPs, Auxiliaries)* — Metasploit — 🔗 03
- **Meterpreter** — *mächtiger Payload/Session für Post-Exploitation* — Meterpreter — 🔗 03, 08

## N
- **NAT** — *Übersetzung privater ↔ öffentlicher IP-Adressen* — трансляция адресов — 🔗 02
- **netstat** — *zeigt aktive Verbindungen, Ports, Zustände (-o: PID)* — netstat — 🔗 02, 09
- **Nichtabstreitbarkeit (Verbindlichkeit)** — *Aktion kann nicht nachträglich abgestritten werden* — неотказуемость — 🔗 01, 06
- **Nmap** — *Port-/Netzwerkscanner, OS-Erkennung* — nmap — 🔗 03
- **Nonce** — *Number Only Used Once; einmaliger Zufallswert (CTR)* — одноразовое число — 🔗 06
- **No-Read-Up / No-Write-Down** — *BLP-Regeln: nicht höher lesen / nicht tiefer schreiben* — правила BLP — 🔗 07

## O
- **One-Time-Pad (Vernam)** — *beweisbar sicher: Schlüssel zufällig, so lang wie Text, nur 1×* — одноразовый блокнот — 🔗 06
- **Onion-Routing** — *Paket mehrfach verschlüsselt, Route über 3 Knoten (TOR)* — луковая маршрутизация — 🔗 04
- **OWASP (Top 10)** — *häufigste Web-Schwachstellen (SQLi, XSS, …)* — OWASP — 🔗 03, 08

## P
- **Padding** — *letzten Block auf volle Blocklänge auffüllen* — дополнение блока — 🔗 06
- **Payload** — *eigentlicher Schadcode, nach erfolgreichem Exploit ausgeführt* — полезная/вредоносная нагрузка — 🔗 03
- **Penetrationstest** — *Sicherheitsprüfung, Tester in Angreiferrolle (mit Auftrag!)* — пентест — 🔗 03
- **Perfect Forward Secrecy (PFS)** — *pro Sitzung neuer Schlüssel, Mitschnitt nützt nichts* — совершенная прямая секретность — 🔗 06
- **Pharming** — *DNS-Manipulation → Umleitung auf Fake-Seite* — фарминг — 🔗 03
- **Phishing** — *gefälschte Seiten/Mails, um Zugangsdaten zu „fischen"* — фишинг — 🔗 03
- **Phreaker** — *nimmt fremde Identität/Zugänge an* — фрикер — 🔗 01
- **PKI (Public-Key-Infrastruktur)** — *CAs + Zertifikate, die Schlüssel an Identitäten binden* — инфраструктура откр. ключей — 🔗 06
- **Playfair** — *bigraphische Substitution (Buchstabenpaare, 5×5-Matrix)* — шифр Плейфера — 🔗 08
- **Port** — *Nummer (0–65535) identifiziert Anwendung; 0–1023 well-known* — порт — 🔗 02
- **Port-Scanning** — *offene Ports/Dienste finden (open/closed/filtered)* — сканирование портов — 🔗 03
- **Port-Security** — *Switch-Port fest mit MAC verknüpft (schwacher Schutz)* — защита порта по MAC — 🔗 03
- **Promiscuous Mode** — *NIC liest **alle** Frames, auch fremde* — неразборчивый режим — 🔗 03
- **Proxy** — *Vermittler, ersetzt Absender-IP; transparenter Proxy = MitM* — прокси — 🔗 04
- **Pseudonymisierung** — *Name durch Pseudonym ersetzt, **umkehrbar** (Bezüge bleiben)* — псевдонимизация — 🔗 01
- **Public-Key-Verfahren** — *öffentlicher Schlüssel verschlüsselt, privater entschlüsselt* — шифрование с откр. ключом — 🔗 06

## R
- **Ransomware** — *verschlüsselt Daten, fordert Lösegeld → Verfügbarkeit* — шифровальщик-вымогатель — 🔗 05
- **RBAC** — *Rechte an **Rollen** gebunden (Rollenhierarchie, Session)* — ролевой доступ — 🔗 07
- **Referenzmonitor** — *prüft jeden Zugriff gegen die Policy* — эталонный монитор — 🔗 07
- **Replay-Angriff** — *abgefangene (gültige) Daten später erneut einspielen* — атака повтором — 🔗 03
- **Reverse Shell** — *Opfer verbindet sich zum Angreifer (umgeht Firewall)* — обратная оболочка — 🔗 03, 08
- **Risiko (R = p · S)** — *Eintrittswahrscheinlichkeit × Schaden* — риск — 🔗 01
- **Rootkit** — *verschleiert Prozesse/Malware vor dem AV* — руткит — 🔗 05
- **RSA** — *asymmetrisch; Sicherheit = Faktorisierung von n=p·q; c=mᵉ, m=cᵈ* — RSA — 🔗 06

## S
- **Sandboxing** — *isolierte Testumgebung (VM) für gefährliche Software* — песочница — 🔗 08
- **Safety (Funktionssicherheit)** — *System verhält sich konform, keine unzulässigen Zustände (unbeabsichtigt)* — функц. безопасность — 🔗 01
- **Security (Informationssicherheit)** — *Schutz vor beabsichtigten Angriffen* — информ. безопасность — 🔗 01
- **Schwachstelle (Weakness)** — *Schwäche/angreifbarer Punkt eines Systems* — слабое место — 🔗 01
- **Schutzziele** — *Vertraulichkeit, Integrität, Verfügbarkeit, Verbindlichkeit, Authentizität, Anonymität* — цели защиты — 🔗 01
- **Security by Obscurity** — *Sicherheit durch Geheimhaltung des Verfahrens (abgelehnt)* — безопасность через неизвестность — 🔗 06
- **Sequenznummer (Seq)** — *Position der Nutzdaten im TCP-Datenstrom* — порядковый номер — 🔗 02
- **SHA-1/2/3** — *Hash-Algorithmen (SHA-1 veraltet, SHA-2/3 sicher)* — SHA — 🔗 06
- **Signatur (Virensignatur)** — *Byte-/Hash-Fingerabdruck bekannter Malware* — сигнатура вируса — 🔗 05
- **Sniffing** — *passives Mitlesen von Daten → Vertraulichkeit* — снифинг — 🔗 03
- **Social Engineering** — *Mensch täuschen statt Technik (Vishing, Phishing)* — социальная инженерия — 🔗 03
- **Socket** — *Endpunkt (IP+Port); 4-Tupel identifiziert Verbindung* — сокет — 🔗 02
- **Spoofing** — *Vortäuschen falscher Identität (MAC/IP/DNS)* — спуфинг — 🔗 03
- **Spyware** — *übermittelt Nutzerdaten ohne Wissen an Dritte* — шпионское ПО — 🔗 05
- **SQL-Injection** — *SQL-Befehle über ungeprüfte Eingaben einschleusen* — SQL-инъекция — 🔗 03
- **Stromchiffre** — *Klartext bitweise XOR mit Schlüsselstrom (z. B. RC4)* — потоковый шифр — 🔗 06
- **Stuxnet** — *erste Malware gegen Industrieanlagen (Iran)* — Stuxnet — 🔗 05
- **Subnetzmaske** — *trennt Netz- und Hostteil der IP* — маска подсети — 🔗 02
- **SYN-Flooding** — *viele halboffene Verbindungen → DoS* — SYN-флуд — 🔗 03

## T
- **TCP** — *verbindungsorientiert, zuverlässig (Handshake, ACK, Retransmission)* — протокол TCP — 🔗 02
- **TCP-Hijacking (Sequenznummernangriff)** — *bestehende Verbindung übernehmen durch Erraten der Seq* — перехват TCP-сессии — 🔗 03
- **Three-Way-Handshake** — *SYN → SYN/ACK → ACK* — тройное рукопожатие — 🔗 02
- **TLS / SSL** — *sicherer Kanal (Handshake + Record), Zertifikate, hybrid* — TLS/SSL — 🔗 06
- **TOR (The Onion Router)** — *Echtzeit-Anonymisierung über 3 Knoten, verschleiert IP* — TOR — 🔗 04
- **TMG §8** — *Diensteanbieter nicht für fremde durchgeleitete Infos verantwortlich (Exit-Node)* — §8 закона о телемедиа — 🔗 04
- **Trojaner** — *tarnt sich als nützlich, führt heimlich Payload aus; keine Selbstverbreitung* — троян — 🔗 05
- **TTL (Time To Live)** — *Hop-Zähler im IP-Header, jeder Router −1, bei 0 verworfen* — время жизни пакета — 🔗 02

## U
- **UDP** — *verbindungslos, unzuverlässig, 8-Byte-Header (DNS, VoIP)* — протокол UDP — 🔗 02
- **Unix-Rechte (rwx, chmod)** — *r=4, w=2, x=1 je Gruppe (Besitzer/Gruppe/andere)* — права Unix — 🔗 07

## V
- **Verbindlichkeit (Non-Repudiation)** — *Durchführung einer Aktion ist nicht abstreitbar* — обязательность/неотказуемость — 🔗 01
- **Verfügbarkeit** — *autorisierte Nutzer werden nicht unautorisiert beeinträchtigt; = (Gesamt−Ausfall)/Gesamt* — доступность — 🔗 01
- **Vertraulichkeit** — *keine unautorisierte Informationsgewinnung* — конфиденциальность — 🔗 01
- **Verwundbarkeit (Vulnerability)** — *ausnutzbare Schwachstelle (Sicherheitsdienste umgehbar)* — уязвимость — 🔗 01
- **Vigenère** — *polyalphabetische Substitution mit Schlüsselwort* — шифр Виженера — 🔗 08
- **Virus** — *reproduziert sich, indem es **andere Programme infiziert** (braucht Wirt)* — вирус — 🔗 05
- **Virtualisierung** — *mehrere Gast-Systeme auf einem Host (VirtualBox)* — виртуализация — 🔗 08

## W
- **Wiper** — *Malware zur reinen Datenvernichtung* — вайпер — 🔗 05
- **Wireshark / tcpdump** — *Sniffer/Netzwerkanalyse* — анализатор трафика — 🔗 03, 09
- **Wurm** — *eigenständig, verbreitet sich selbst über Netz/Lücken (kein Wirt)* — червь — 🔗 05
- **WWW (World Wide Web)** — *alle erreichbaren Inhalte des Internets* — всемирная паутина — 🔗 08

## X
- **X.509** — *Standard für digitale Zertifikate (Aussteller, Seriennummer, Schlüssel, Gültigkeit)* — стандарт X.509 — 🔗 06
- **XOR (Kontravalenz)** — *umkehrbare bitweise Verknüpfung; Basis von Stromchiffren* — исключающее ИЛИ — 🔗 06
- **XSS** → siehe **Cross-Site Scripting**

## Z
- **Zertifikat** — *bindet öffentlichen Schlüssel an Identität, von CA signiert* — сертификат — 🔗 06
- **Zombie** → siehe **Bot/Botnetz**
- **Zugriffskontrolle** — *wer darf wie auf welches Objekt zugreifen* — контроль доступа — 🔗 07
- **Zugriffsmatrix** — *Subjekte × Objekte → Rechte (Schutzzustand)* — матрица доступа — 🔗 07

---

## Mini-Trainer: Zuordnung über Kreuz (häufige Verwechslungen) ⚠️
Это пары, которые чаще всего путают — проверь, что различаешь:
| Begriff A | Begriff B | Kernunterschied |
|---|---|---|
| Datenschutz | Datensicherheit | Person vs. Daten (jeder Art) |
| Safety | Security | unbeabsichtigt vs. beabsichtigt |
| Authentisierung | Authentifizierung | nachweisen (Subjekt) vs. prüfen (System) |
| Anonymisierung | Pseudonymisierung | irreversibel vs. umkehrbar |
| Gefahr | Bedrohung | abstrakt vs. konkret (Bezug zu Schutzziel) |
| Schwachstelle | Gefährdung | Schwäche vs. Schwäche trifft Bedrohung |
| Virus | Wurm | braucht Wirt / Datei-zu-Datei vs. eigenständig / Host-zu-Host |
| Wurm | Trojaner | selbstverbreitend vs. Täuschung (keine Verbreitung) |
| Bakterie | Wurm | ein System (lokal) vs. übers Netz |
| symmetrisch | asymmetrisch | ein Schlüssel vs. Schlüsselpaar |
| MAC (Message Auth. Code) | digitale Signatur | symmetrisch, keine Verbindlichkeit vs. asymmetrisch, mit Verbindlichkeit |
| Hash | MAC | ohne Schlüssel (nur Integrität) vs. mit Schlüssel (+ Authentizität) |
| MAC-Adresse | MAC (Message Auth. Code) | Netzkartenadresse vs. Krypto-Prüfsumme |
| MAC (Access Control) | DAC | System erzwingt Labels vs. Eigentümer entscheidet |
| Diffie-Hellman | RSA | Schlüsselaustausch (kein Verschlüsseln) vs. Verschlüsselung/Signatur |
| ECB | CBC | Blöcke unabhängig (unsicher) vs. verkettet mit IV |
| Exploit | Payload | nutzt Lücke aus vs. eigentlicher Schadcode |
| Reverse Shell | Bind Shell | Opfer→Angreifer vs. Angreifer→Opfer |
| ACL | Capability | Spalte beim Objekt vs. Zeile beim Subjekt |
| Darknet | Dark Web | Infrastruktur (Netz) vs. Inhalte darauf |
| Deep Web | Dark Web | nicht indexiert (groß) vs. nur über Darknet (klein) |
| TCP | UDP | verbindungsorientiert/zuverlässig vs. verbindungslos/schnell |
| Entry-Node | Exit-Node (TOR) | kennt echte IP vs. unverschlüsselt zum Ziel |
| no-read-up | no-write-down | nicht höher lesen vs. nicht tiefer schreiben |

---
Следующий файл: `11_Uebungen.md` — Probeklausur (пробный экзамен по всем темам).
