# Kapitel 2 — Grundlagen Rechnernetze
**Skript S. 29–74 (PDF S. 35–80)**

> **О чём глава.** Как данные физически и логически добираются от одного компьютера до другого: модели слоёв (OSI, TCP/IP), адресация (MAC, IP, порты), главные протоколы (IP, TCP, UDP), служебные протоколы (DNS, DHCP, ARP, ICMP) и консольные команды для диагностики.
>
> **Почему важно для оценки 1,0:**
> 1. **Wireshark-задания на экзамене** — это чистая глава 2. Чтобы понять лог, нужно видеть в нём слои, флаги TCP, Seq/Ack, ARP, DNS, ICMP.
> 2. **Все атаки главы 3** (ARP-Spoofing, SYN-Flood, DNS-Poisoning, TCP-Hijacking, Port-Scan) — это злоупотребление механизмами из этой главы. Кто понял механизм, тот понимает атаку.
> 3. Классические расчётные задачи: **Subnetting** и **Seq/Ack-номера**.
>
> **Обозначения:** ⭐ = очень вероятно на экзамене · 🧮 = расчёт · 🦈 = важно для Wireshark · ⚠️ = ловушка · ➕ = сверх скрипта · 🐞 = ошибка/неточность в скрипте.

## Содержание
2.2 Grundbegriffe, Verbindungsmodelle · 2.3 OSI-Modell ⭐ · 2.4 TCP/IP-Modell ⭐ · 2.5 Kapselung 🦈 · 2.6 IP, IP-Header 🦈, Routing, IPv4-Adressen, Subnetting 🧮⭐, IPv6 · 2.7 TCP ⭐🦈 (Header, Flags, Handshake, Seq/Ack 🧮, Delayed ACK, Retransmission, Flusskontrolle) · 2.8 Stauvermeidung, Verbindungsabbau, RST, Zustände · 2.9 UDP · 2.10 MAC, Ports, Sockets · 2.11 Netzwerkkommandos 🦈 · 2.12 DNS, DHCP, ARP, ICMP ⭐🦈 · Sicherheitsbezug (Brücke zu Kap. 3) · 🐞 Fehler im Skript · **Aufgaben A–J** (в конце файла)

---

## 2.2 Grundbegriffe und Verbindungsmodelle (S. 29–32)

### Internet und Netz
- **Internet:** *offenes, dezentral organisiertes, globales Rechnernetz aus vielen gekoppelten Subnetzen.*
- Subnetze werden über **Router** (Relaystationen, Zwischenstationen) gekoppelt.
- **Route** = Weg vom Sender zum Empfänger; **Hop** = eine Etappe (ein Router-Übergang).
- **Computernetz (Tanenbaum):** *„Ansammlung zusammenhängender, selbständiger Computer"* — *zusammenhängend* = können Informationen austauschen; *selbständig* = kein Rechner steuert den anderen direkt.
- **Protokoll (Def. 2.1):** *Ein (Kommunikations-)Protokoll definiert die Reihenfolge von Verarbeitungsschritten zur Durchführung der Kommunikation zwischen zwei Systemen.* (Проще: набор правил «кто, что, когда и в каком формате отправляет».)

### Verbindungsmodelle ⭐
| | **verbindungsorientiert** (connection oriented) | **verbindungslos** (connectionless) |
|---|---|---|
| Prinzip | **Telefonprinzip**: erst Verbindung aufbauen (von beiden bestätigt), dann Daten, dann abbauen | **Post-Prinzip**: jede Sendung (Datagramm) einzeln adressiert, unabhängig |
| Garantien | richtiger Partner, **vollständig, richtige Reihenfolge, fehlerfrei** | **keine Garantie** für Ankunft und Vollständigkeit |
| Sitzung | ja (**Session**) | nein |
| Adresse nötig | mindestens einmal beim Aufbau | in **jedem** Paket |
| Analogie | Telefongespräch | Brief / **Postkarte** (ohne Quittung) bzw. Einschreiben (mit Quittung) |
| Vertreter | **TCP** (RFC 793), X.25, ATM | **UDP** (RFC 768), **IP** |

### Client-Server-Prinzip (S. 31–32, Abb. 2.2)
Browser (Client) schickt **HTTP-Request** (`GET /info.html HTTP/1.1`, `Host: www.hs-mittweida.de`) → Webserver antwortet mit **HTTP-Response** (`HTTP/1.1 200 OK`, `Server: Apache/2.2`, `Content-Length: 112` …).
HTTP selbst baut **keine Verbindung** auf — das erledigen **TCP** (zuverlässige Verbindung) und **IP** (Paketzustellung); physisch überträgt **Ethernet** (Kupfer, Glasfaser, Funk). → An fast jedem Kommunikationsvorgang sind **mehrere Protokolle** beteiligt → daher Schichtenmodelle.

---

## 2.3 ISO/OSI-Referenzmodell (S. 32–36) ⭐

**Объяснение.** До конца 70-х протоколы были **проприетарными** — компьютеры разных фирм не могли общаться. ISO создала **OSI (Open Systems Interconnection)** — эталонную модель из **7 слоёв**. Каждый слой решает **одну задачу** и пользуется услугами слоя **под собой**, а свои услуги отдаёт слою **над собой**. Сама модель **не задаёт протоколов**, только **обязанности** слоёв. Поэтому протокол одного слоя можно заменить, не трогая остальные (например, Wi-Fi вместо кабеля — браузер этого не замечает).

### Die 7 Schichten
Запоминалка снизу вверх: **„Please Do Not Throw Salami Pizza Away"** (Physical, Data Link, Network, Transport, Session, Presentation, Application). По-немецки: **„Alle deutschen Studenten trinken verschiedene Sorten Bier"** (сверху вниз: Anwendung, Darstellung, Sitzung, Transport, Vermittlung, Sicherung, Bitübertragung).

| # | Schicht (DE / engl.) | Aufgabe (Skript) | PDU ➕ | Adresse | Geräte | Protokolle (Beispiele) |
|---|---|---|---|---|---|---|
| 7 | **Anwendungsschicht** / application | Dienste für Benutzer bereitstellen (Web, E-Mail, Dateitransfer) | Daten / Nachricht | — | (Gateway, Proxy) | HTTP, SMTP, FTP, DNS, DHCP |
| 6 | **Darstellungsschicht** / presentation | systemabhängige → **netzeinheitliche Darstellung** (Codierung), **Kompression, Verschlüsselung** | Daten | — | — | ASCII/UTF-8, JPEG, (TLS) |
| 5 | **Sitzungsschicht** / session | **Synchronisation** der Kommunikation zw. Anwendungsprozessen; laut Skript auch Reihenfolge und erneute Übertragung (**checkpointing, recovering**) | Daten | — | — | NetBIOS, RPC |
| 4 | **Transportschicht** / transport | **Ende-zu-Ende-Kommunikation zwischen Diensten/Prozessen**, **Flusssteuerung** | **Segment** (TCP) / Datagramm (UDP) | **Port** | (Firewall) | **TCP, UDP** |
| 3 | **Vermittlungs-/Netzwerkschicht** / network | **Routing**, **logische Adressierung**, Stauauflösung (congestion control), **Fragmentierung**, Dienstgüte | **Paket** | **IP-Adresse** | **Router** | **IP, ICMP** |
| 2 | **Sicherungsschicht** / data link | Bits → **Frames**, **Prüfsummen** (Fehlererkennung), **physikalische Adressierung** | **Frame** | **MAC-Adresse** | **Switch, Bridge** | Ethernet, WLAN (802.11), **ARP** ➕ |
| 1 | **Bitübertragungsschicht** / physical | physische Verbindung, **Bits übertragen** | **Bit** | — | **Repeater, Hub**, Kabel, Stecker | Ethernet (phys.), DSL |

🐞 Skript zählt **Router** zur Schicht 1 — falsch. Router arbeiten auf **Schicht 3** (entscheiden anhand der IP-Adresse). Schicht 1: Repeater, Hub, Kabel. Schicht 2: Switch, Bridge.
⚠️ **ARP** sitzt zwischen Schicht 2 und 3 (verbindet sie, s. 2.12.3) — in Prüfungen meist „Schicht 2" oder „2/3" akzeptiert. **ICMP** ist Teil von IP → Schicht 3.
⚠️ **MAC-Adresse:** 48 Bit = 12 Hex-Ziffern, z. B. `05:66:6b:67:98:2f`.

### Begriffe des Modells (S. 33–35)
| Begriff | Bedeutung |
|---|---|
| **vertikale Kommunikation** | jede Schicht spricht **nur** mit der Schicht direkt darüber/darunter |
| **virtuelle (horizontale) Verbindung** | gleiche Schichten auf beiden Systemen „sprechen" logisch miteinander (über ihr Protokoll), physisch aber nur über die unteren Schichten. **Ausnahme: Schicht 1** — kommuniziert physikalisch direkt |
| **Instanz (Entity)** | aktives Element in einer Schicht; eine Schicht kann **mehrere Instanzen** haben |
| **Partner-Instanzen** | zwei korrespondierende Instanzen derselben Schicht auf verschiedenen Systemen |
| **Dienst (Service)** | Schicht N (**Service Provider**) bietet Dienst für Schicht N+1 (**Service User**) und nutzt dafür Schicht N−1 |
| **Dienstzugangspunkt (SAP, Service Access Point)** | Schnittstelle zu einer konkreten Instanz |

Der physikalische Übertragungskanal gehört **nicht** zum Modell. Eine Schicht kann in der Praxis „leer" sein, wenn ein Protokoll ihre Aufgabe mit übernimmt (**Ethernet = Schicht 1 + 2**).

### Vorteile des Schichtenmodells ⭐ (Kontrollfrage 1)
- **Abstraktion und Komplexitätsreduzierung** — jede Schicht löst nur ihr Teilproblem.
- **Austauschbarkeit**: Protokolle einer Schicht sind **transparent ersetzbar** (WLAN statt Kabel, IPv6 statt IPv4), solange die Schnittstelle gleich bleibt.
- **Herstellerunabhängigkeit / Interoperabilität** (offene Standards).
- Modell beschreibt **Verhalten**, nicht Implementierung.

---

## 2.4 TCP/IP-Modell (S. 36–38) ⭐

**Объяснение.** OSI — теоретический эталон. Реальный интернет работает на **семействе протоколов TCP/IP**, у которого своя модель из **4 слоёв**. В отличие от OSI, это описание **конкретной реализации**.

| TCP/IP-Schicht | entspricht OSI | Aufgabe | Protokolle |
|---|---|---|---|
| **4 Anwendungsschicht** | 5 + 6 + 7 | erzeugt Nutzdaten, Schnittstelle zum Benutzer, höchste Abstraktion | HTTP, SMTP, FTP, DNS, DHCP, SSH |
| **3 Transportschicht** | 4 | Ende-zu-Ende zwischen Prozessen; **TCP** (verbindungsorientiert, zuverlässig) oder **UDP** (Datagramme, keine Prüfung, ob/wie sie ankommen) | TCP, UDP |
| **2 Internetschicht** | 3 | Versand/Empfang von **IP-Paketen**, **Routing**, (De-)**Fragmentierung**; **ICMP** für Statusinformationen (Teil von IP) | IP, ICMP |
| **1 Netzzugriffsschicht** (Netzzugang) | 1 + 2 | Übertragung über physikalisches Medium + Fehlerkorrektur; konkrete Technik egal, solange Schnittstelle stimmt | Ethernet, WLAN, ARP |

> **Musterantwort „OSI vs. TCP/IP":** *Das OSI-Modell ist ein abstraktes Referenzmodell mit 7 Schichten, das Zuständigkeiten beschreibt, aber keine Protokolle festlegt. Das TCP/IP-Modell besteht aus 4 Schichten und beschreibt die konkrete Implementierung der Internetprotokollfamilie. Die OSI-Schichten 5–7 sind im TCP/IP-Modell in der Anwendungsschicht zusammengefasst, die Schichten 1–2 in der Netzzugriffsschicht.*

---

## 2.5 Schachtelung von Protokollen / Kapselung (S. 38–39) ⭐🦈

**Объяснение.** Каждый слой при отправке добавляет к данным **свой заголовок (Header)** и **не меняет** полезные данные слоя выше. Как матрёшка или как письмо в конверте, вложенном в посылку. На приёме каждый слой **читает свой заголовок и снимает его** (**Dekapselung**).

```
Anwendung:    [                 HTTP-Daten                 ]
Transport:    [TCP-Header][          HTTP-Daten            ]   = TCP-Segment
Internet:     [IP-Header][TCP-Header][     HTTP-Daten      ]   = IP-Paket / IP-Datagramm
Netzzugang:   [Eth-Header][IP-H.][TCP-H.][ HTTP-Daten ][FCS]   = Ethernet-Frame
```
- Größen 🦈: Ethernet-Header **14 Byte** (+ 4 Byte FCS, in Wireshark meist nicht sichtbar), IPv4-Header **20 Byte** (ohne Optionen), TCP-Header **20 Byte** (ohne Optionen; mit Optionen bis 60), UDP-Header **8 Byte**.
- Ein Frame mit nacktem TCP-ACK ist in Wireshark daher typischerweise **54 Byte** (14 + 20 + 20) bzw. **66 Byte** mit TCP-Timestamps (14 + 20 + 32).
- Schichten können Pakete **aufteilen** (Fragmentierung/Segmentierung) — für die höhere Schicht **transparent**.
- **Gateway**: packt Nutzdaten aus und sendet sie mit anderer Technologie weiter (z. B. DSL → Satellit) → Protokollmenge ändert sich unterwegs, Prinzip bleibt.

🦈 **Так это видно в Wireshark:** в окне деталей пакета строки идут сверху вниз ровно по слоям:
```
Frame 4: 145 bytes on wire
Ethernet II, Src: 00:21:cc:5e:8a:a5, Dst: 00:1a:4f:12:34:56        ← Schicht 2
Internet Protocol Version 4, Src: 192.168.1.20, Dst: 10.0.0.5       ← Schicht 3
Transmission Control Protocol, Src Port: 49152, Dst Port: 80, ...   ← Schicht 4
Hypertext Transfer Protocol                                          ← Schicht 7
```

---

## 2.6 Das Internet-Protokoll (IP) (S. 39–47)

**Eigenschaften von IP ⭐:**
- Schicht 3 (Internetschicht), **verbindungslos**, **zustandslos (stateless)**: jedes Paket wird **unabhängig** behandelt, kein Bezug zu anderen Paketen.
- **Nur** für **Routing und Übertragung** zuständig. Prüft **nicht**: Reihenfolge, Ankunft, Zustand der Daten (das macht TCP).
- Durch Einfachheit und **Fragmentierung** auf fast jedem Medium einsetzbar.
- IP weiß **nichts** über die Anwendung (keine Ports) und nichts über die physikalische Adresse (keine MAC) → arbeitet „allein auf seiner Schicht".

### 2.6.1 IP-Header (S. 39–41, Abb. 2.6) ⭐🦈
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-------+-------+---------------+-------------------------------+
|Version|  IHL  | Type of Serv. |          Total Length         |
+-------+-------+---------------+-----+-------------------------+
|         Identification        |Flags|     Fragment Offset     |
+---------------+---------------+-----+-------------------------+
|  Time to Live |    Protocol   |        Header Checksum        |
+---------------+---------------+-------------------------------+
|                       Source Address (32 Bit)                 |
+---------------------------------------------------------------+
|                    Destination Address (32 Bit)               |
+---------------------------------------------------------------+
|                    Options (optional, bis 40 Byte)            |
+---------------------------------------------------------------+
```

| Feld | Größe | Bedeutung |
|---|---|---|
| **Version** | 4 Bit | 4 = IPv4 |
| **IHL** (Internet Header Length) | 4 Bit | Headerlänge in **32-Bit-Wörtern** → `5` = 5·4 = **20 Byte** |
| **Type of Service (ToS)** | 8 Bit | Priorisierung (von vielen Routern schlecht umgesetzt) |
| **Total Length** | 16 Bit | Gesamtlänge des IP-Pakets **inkl. Header** in Byte (max. 65 535) |
| **Identification (Kennung)** | 16 Bit | alle **Fragmente** eines Datagramms haben **denselben** Wert → Zusammensetzen |
| **Flags** | 3 Bit | Bit 0 = immer 0; **DF** (Don't Fragment) = nicht fragmentieren; **MF** (More Fragments) = es folgen weitere Fragmente (bei allen außer dem letzten = 1) |
| **Fragment Offset** ➕ | 13 Bit | Position des Fragments im Original, in **8-Byte-Einheiten** |
| **TTL (Time to Live)** | 8 Bit | laut Spezifikation Lebenszeit in Sekunden, praktisch **Hop-Zähler**: jeder Router **−1**; bei **0 → Paket verworfen**, Sender bekommt ICMP-Meldung. Verhindert endlos kreisende Pakete |
| **Protocol** | 8 Bit | welches Protokoll im Datenteil steckt: **1 = ICMP, 6 = TCP, 17 = UDP** |
| **Header Checksum** | 16 Bit | prüft **nur den Header** (schneller, weil jeder Router sie nach TTL-Änderung neu berechnen muss); Daten prüft das höhere Protokoll |
| **Source / Destination Address** | je 32 Bit | IP von Sender / Empfänger; bei mehreren Empfängern Multicast-Adresse |
| **Options** | 0–40 Byte | in 32-Bit-Wörter aufgefüllt; z. B. Security, **Strict/Loose Source Routing**, Record Route, Time Stamp |

🐞 Skript: „Im Protokoll-Feld wird der **Port** der Transportschicht angegeben" — gemeint ist die **Protokollnummer** (6/17), **kein Port**.

#### 🦈🧮 Beispiel aus dem Skript: Header aus Hex lesen (S. 39–40)
```
45 00 00 28 c0 9e 40 00 2f 06 31 96 41 d0 e4 df 91 fe a0 ed
```
| Bytes | Feld | Wert |
|---|---|---|
| `4` `5` | Version / IHL | **IPv4**, 5 · 4 = **20 Byte** Header |
| `00` | ToS | 0 |
| `00 28` | Total Length | 0x28 = **40 Byte** → 40 − 20 = **20 Byte** Nutzlast (= genau ein TCP-Header ohne Daten, z. B. ein ACK) |
| `c0 9e` | Identification | 0xc09e |
| `40 00` | Flags + Offset | 0x4000 = 0**1**00 0000… → **DF = 1**, MF = 0, Offset 0 |
| `2f` | TTL | 0x2f = **47** |
| `06` | Protocol | **6 = TCP** |
| `31 96` | Header Checksum | 0x3196 |
| `41 d0 e4 df` | Source | 0x41=65, 0xd0=208, 0xe4=228, 0xdf=223 → **65.208.228.223** |
| `91 fe a0 ed` | Destination | 0x91=145, 0xfe=254, 0xa0=160, 0xed=237 → **145.254.160.237** |

**Hex → Dezimal schnell:** erste Ziffer · 16 + zweite Ziffer. `e4` = 14·16 + 4 = 228. Tabelle: a=10, b=11, c=12, d=13, e=14, f=15.

**Flags-Byte entschlüsseln:** die ersten 16 Bit von „Flags + Offset" binär schreiben:
| Hex | Binär (erste 3 Bit = Flags) | Bedeutung |
|---|---|---|
| `40 00` | **010**0 0000 0000 0000 | DF=1, MF=0, Offset 0 → nicht fragmentiert, darf nicht fragmentiert werden |
| `20 00` | **001**0 0000 0000 0000 | DF=0, MF=1, Offset 0 → **erstes Fragment**, weitere folgen |
| `20 b9` ➕ | **001**0 0000 1011 1001 | MF=1, Offset 0xb9 = 185 · 8 = **1480 Byte** → mittleres Fragment |
| `00 b9` ➕ | **000**0 0000 1011 1001 | MF=0, Offset 1480 → **letztes Fragment** |
| `00 00` | 000… | nicht fragmentiert, DF nicht gesetzt |

### 2.6.2 IP-Routing (S. 41–42)
- **Routing** = Finden eines (möglichst kurzen) **Weges** zur Zieladresse.
- Ziel im **eigenen Subnetz** → **kein Routing**, direkte Zustellung (per ARP → MAC).
- Ziel in **anderem Subnetz** → Paket an den **Router** (Default Gateway), der mit mehreren Netzen verbunden ist; ggf. über viele Router (Hops), max. Anzahl begrenzt durch **TTL**.
- **Nicht-Determinismus:** Pakete derselben Verbindung können **verschiedene Routen** nehmen (Router entscheidet, z. B. nach Auslastung) → können **in anderer Reihenfolge** ankommen oder bei Streckenausfall **verloren gehen**. → Deshalb braucht man TCP für Zuverlässigkeit.

**Entscheidungsregel (Hinweis S. 45) ⭐:**
```
IP_Ziel AND Maske  ==  IP_eigen AND Maske   →  gleiches Subnetz, direkt zustellen
                   !=                        →  an Default Gateway (Router) senden
```

### 2.6.3 IPv4-Adressen (S. 42–45) ⭐
- **32 Bit**, geschrieben als 4 **Oktette** (je 8 Bit) dezimal: `192.168.153.23`. → 2³² ≈ **4,3 Milliarden** Adressen.
- Vergabe weltweit durch INTERNIC (➕ heute IANA → Regional Internet Registries, z. B. RIPE NCC für Europa).
- Adresse = **Netzanteil** + **Hostanteil (Geräteteil)**. Welcher Teil was ist, bestimmt die **Subnetzmaske**.

#### Adressklassen (klassenbasiert, historisch — Abb. 2.7)
| Klasse | erste Bits | 1. Oktett | Standardmaske | Netzanteil | Adressen je Netz | Zweck |
|---|---|---|---|---|---|---|
| **A** | `0` | **0–127** | 255.0.0.0 (/8) | 8 Bit | 2²⁴ = 16 777 216 | sehr große Netze; **0.0.0.0/8** = Standardroute, **127.0.0.0/8 = Loopback** |
| **B** | `10` | **128–191** | 255.255.0.0 (/16) | 16 Bit | 2¹⁶ = 65 536 | 16 384 Netze |
| **C** | `110` | **192–223** | 255.255.255.0 (/24) | 24 Bit | 2⁸ = 256 (254 Hosts) | 2²¹ = 2 097 152 Netze |
| **D** | `1110` | **224–239** | — | — | — | **Multicast** |
| **E** | `1111` | **240–255** | — | — | — | experimentell / reserviert |

🐞 Skript: Klasse C „255 Adressen" → richtig: **256 Adressen, 254 nutzbare Hosts**.
🐞 Skript: APIPA „196.254.0.0/16" → richtig: **169.254.0.0/16**.

#### Besondere Adressen ⭐ (➕ ergänzt)
| Bereich | Bedeutung |
|---|---|
| **10.0.0.0/8**, **172.16.0.0/12** (172.16–172.31), **192.168.0.0/16** | **private Adressen** (RFC 1918) — im Internet nicht geroutet, brauchen **NAT** |
| **127.0.0.0/8** (meist 127.0.0.1) | **Loopback** — der eigene Rechner |
| **169.254.0.0/16** | **APIPA / Link-Local**: Rechner hat **keinen DHCP-Server gefunden** und sich selbst eine Adresse gegeben → 🦈 Diagnose-Hinweis! |
| **0.0.0.0** | „unbekannt / diese Adresse" (z. B. DHCP-Discover-Absender) bzw. Standardroute |
| **255.255.255.255** | **Broadcast** ins lokale Netz (z. B. DHCP-Discover-Ziel) |
| **224.0.0.0–239.255.255.255** | Multicast |
| Netzadresse (alle Hostbits 0) | bezeichnet das **Netz selbst** — nicht an Hosts vergeben |
| Broadcastadresse (alle Hostbits 1) | an **alle Hosts** im Subnetz — nicht an Hosts vergeben |

#### CIDR und Subnetzmaske ⭐
- **CIDR (Classless Inter-Domain Routing):** Suffix `/n` = Anzahl der **Netzbits** (Einsen in der Maske). `/24` = `11111111.11111111.11111111.00000000` = `255.255.255.0`.
- **Netzanteil** = IP **AND** Maske. **Hostanteil** = IP **AND** (invertierte Maske).
- **Anzahl Hosts = 2^(32 − n) − 2** (minus Netzadresse und Broadcast).

**Tabelle der Masken (unbedingt griffbereit) 🧮:**
| CIDR | Maske | Hostbits | Adressen | **Hosts** | Blockgröße im betroffenen Oktett |
|---|---|---|---|---|---|
| /8 | 255.0.0.0 | 24 | 16 777 216 | 16 777 214 | — |
| /16 | 255.255.0.0 | 16 | 65 536 | 65 534 | — |
| /19 | 255.255.**224**.0 | 13 | 8 192 | 8 190 | 32 (3. Oktett) |
| /20 | 255.255.**240**.0 | 12 | 4 096 | 4 094 | 16 (3. Oktett) |
| /22 | 255.255.**252**.0 | 10 | 1 024 | 1 022 | 4 (3. Oktett) |
| /23 | 255.255.**254**.0 | 9 | 512 | 510 | 2 (3. Oktett) |
| /24 | 255.255.255.0 | 8 | 256 | **254** | 256 |
| /25 | 255.255.255.**128** | 7 | 128 | **126** | 128 |
| /26 | 255.255.255.**192** | 6 | 64 | **62** | 64 |
| /27 | 255.255.255.**224** | 5 | 32 | **30** | 32 |
| /28 | 255.255.255.**240** | 4 | 16 | **14** | 16 |
| /29 | 255.255.255.**248** | 3 | 8 | **6** | 8 |
| /30 | 255.255.255.**252** | 2 | 4 | **2** | 4 (Punkt-zu-Punkt-Links) |

Werte eines Masken-Oktetts: 128, 192, 224, 240, 248, 252, 254, 255. **Blockgröße = 256 − Maskenwert** (z. B. 256 − 224 = 32).

#### 🧮 Rezept: Netzadresse, Broadcast, Hostbereich (schnelle Methode)
1. **Interessantes Oktett** finden = das Oktett, in dem die Maske nicht 255 und nicht 0 ist.
2. **Blockgröße** = 256 − Maskenwert in diesem Oktett.
3. **Netzadresse**: Wert des Oktetts **auf das nächstkleinere Vielfache der Blockgröße abrunden**; Oktette rechts davon = 0.
4. **Broadcast** = nächste Netzadresse − 1 (rechts davon alles 255).
5. **Hosts**: Netzadresse + 1 … Broadcast − 1. Anzahl = 2^Hostbits − 2.

**Beispiel 2.1 aus dem Skript (S. 44):** `192.168.153.23 / 255.255.255.240` (/28)
- Blockgröße 256 − 240 = 16; 23 abgerundet auf Vielfaches von 16 → **16**
- **Netz 192.168.153.16**, Hostanteil 23 − 16 = **0.0.0.7**, Broadcast **192.168.153.31**, Hosts .17–.30 (**14**)
- Binär-Kontrolle: `…00010111 AND …11110000 = …00010000` = 16 ✔
- 28 Bit Netzkennung, 4 Bit Gerätekennung.

**Beispiel aus dem Skript: 192.168.0.0/24** → Maske 255.255.255.0, Netz 192.168.0.0, Broadcast 192.168.0.255, **254 Hosts**.

#### 🧮 Rezept: Subnetze bilden (Netz aufteilen)
Gegeben ein Netz und eine Anforderung („N Subnetze" und/oder „H Hosts je Subnetz"):
1. **Hostbits h**: kleinstes h mit 2^h − 2 ≥ H.
2. **Subnetzbits s**: kleinstes s mit 2^s ≥ N.
3. Prüfen: ursprüngliche Netzbits + s + h ≤ 32.
4. **Neue Maske** = /(32 − h).
5. Subnetze aufzählen: Netzadressen im Abstand der Blockgröße 2^h.

**Skript-Beispiel (S. 45): 4 Subnetze, je bis zu 14 Hosts:**
- Hosts: 2⁴ = 16 → **4 Hostbits** (16 − 2 = 14 ✔); Subnetze: 2² = 4 → **2 Subnetzbits**.
- Skript-Lösung: 4 + 2 = 6 Bit → „/26 = 255.255.255.192".
- 🐞⚠️ **Wichtig zu verstehen:** /26 ist die Maske des **Gesamtblocks**, der alle 4 Subnetze enthält (4 · 16 = 64 Adressen). Die Maske **jedes einzelnen Subnetzes** ist **/28 = 255.255.255.240** (4 Hostbits). Auf der Prüfung: Skript-Lösung nennen **und** kurz ergänzen: *„Der Gesamtbereich umfasst /26; jedes der 4 Subnetze hat die Maske /28 (255.255.255.240) mit je 14 Hosts."*

**Skript-Beispiel (S. 45): das 10. Subnetz in 184.72.0.0 mit Hilfsmaske `184.72.000000NN.NNHHHHHH`** (4 Subnetzbits N, 6 Hostbits H → Subnetze /26):
- Subnetz-Index 10 = `1010` → `184.72.000000`**10**`.`**10**`000000` = **184.72.2.128** (Netzadresse)
- Broadcast: Hostbits auf 1 → `184.72.00000010.10111111` = **184.72.2.191**
- Hostbereich **184.72.2.129 – 184.72.2.190** (62 Hosts)
- (Index beginnt bei **0**: Subnetz 0 = 184.72.0.0.)

### 2.6.4 IPv6 (S. 45–47)
| Merkmal | IPv4 | IPv6 |
|---|---|---|
| Adresslänge | **32 Bit** | **128 Bit** (2¹²⁸ ≈ 340 Sextillionen) |
| Schreibweise | dezimal, Punkte | **hexadezimal**, 8 Blöcke à 16 Bit, Doppelpunkte |
| NAT | nötig (Adressmangel) | **nicht mehr nötig** → echte Ende-zu-Ende-Verbindungen |
| IPsec | optional | im Skript: **obligatorisch** (Authentication Header + Encryption Header getrennt) ➕ seit RFC 6434 (2011) nur noch „empfohlen" |
| Header-Prüfsumme | ja | **keine** → schlanker, schnelleres Routing |
| Broadcast | ja | **nein, ersetzt durch Multicast** |
| Konfiguration | manuell / DHCP | zusätzlich **automatische Adresskonfiguration** (SLAAC ➕) |
| ARP | ARP | **NDP** (Neighbor Discovery Protocol) |
| Priorisierung | ToS (schlecht umgesetzt) | **Quality of Service** (z. B. für VoIP) |
| Routing | — | effizienter durch **hierarchische Adressierung**, **Address Scoping** |

**Aufbau einer IPv6-Adresse:** z. B. `2001:0db8:85a3:08d3:1319:8a2e:0370:7347` (🐞 im Skript fehlt die „2" vorne)
- **Bits 0–63 = Präfix**: erste 32 Bit vergibt die **RIR** (Kennung des **ISP**), Rest für interne Netzkennung des ISP.
- **Bits 64–127 = Interface Identifier** (`1319:8a2e:0370:7347`). Standardmäßig fließt die **MAC-Adresse** ein (EUI-64 ➕) → **Datenschutzproblem**: Gerät und damit Person eindeutig verfolgbar. ➕ Lösung: **Privacy Extensions** (zufällige, wechselnde Interface-IDs).
- ➕ Kürzungsregeln: führende Nullen pro Block weglassen (`0db8` → `db8`), **eine** Folge von Null-Blöcken durch `::` ersetzen (`fe80:0000:0000:0000:…` → `fe80::…`). `::1` = Loopback, `fe80::/10` = Link-Local.
- Anteil am Verkehr laut Skript (2016) ≈ 13 % → Vorlesung konzentriert sich auf IPv4.

---

## 2.7 Transmission Control Protocol (TCP) (S. 47–56) ⭐

**Eigenschaften:** Transportschicht, **verbindungsorientiert**, **zuverlässig**, speichert den **Zustand** der Verbindung. Ablauf: Verbindung **aufbauen → Daten übertragen → abbauen**.

TCP kümmert sich darum, dass (Skript, wörtlich gut für „Nennen Sie"):
1. Pakete auf **Gültigkeit** geprüft und einer **Verbindung zugeordnet** werden,
2. die **richtige Reihenfolge** wiederhergestellt wird,
3. **beschädigte oder fehlende** Pakete **erneut gesendet** werden,
4. der erfolgreiche **Empfang bestätigt** wird (ACK),
5. **Duplikate verworfen** werden.

→ **Virtuelle Ende-zu-Ende-Verbindung**: als wären die Partner direkt verbunden. Mehrere Dienste pro Netzwerkinterface, unterschieden durch **Ports** (0–1023 reserviert, vergeben von der **IANA**; bis 65 535 frei).

### 2.7.1 TCP-Header (S. 48–50, Abb. 2.8) ⭐🦈
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-------------------------------+-------------------------------+
|          Source Port          |       Destination Port        |
+-------------------------------+-------------------------------+
|                        Sequence Number                        |
+---------------------------------------------------------------+
|                    Acknowledgment Number                      |
+-------+-----------+-+-+-+-+-+-+-------------------------------+
| Offset| Reserved  |U|A|P|R|S|F|            Window             |
|       |           |R|C|S|S|Y|I|                               |
|       |           |G|K|H|T|N|N|                               |
+-------+-----------+-+-+-+-+-+-+-------------------------------+
|           Checksum            |         Urgent Pointer        |
+-------------------------------+-------------------------------+
|                    Options (optional) + Padding               |
+---------------------------------------------------------------+
```
| Feld | Größe | Bedeutung |
|---|---|---|
| **Quellport / Zielport** | je 16 Bit | ordnen das Segment einer **Anwendung** auf Sender-/Zielsystem zu |
| **Sequenznummer (SN, Seq)** | 32 Bit | Position der Nutzdaten (erstes Byte) im **Gesamtdatenstrom** → richtige Reihenfolge; nach 4 294 967 295 wieder 0 |
| **Quittierungsnummer (AN, Ack)** | 32 Bit | **nächste erwartete SN** = bestätigt alles davor |
| **Offset (Data Offset)** | 4 Bit | Headerlänge in **32-Bit-Wörtern** (meist **5** = 20 Byte) → wo Daten beginnen |
| **Reserviert** | 6 Bit | 0 |
| **Flags** | 6 Bit | URG, ACK, PSH, RST, SYN, FIN (s. u.) |
| **Fenstergröße (Window)** | 16 Bit | wie viele Bytes (ab der Ack-Nummer) der Absender dieses Segments **empfangen kann** → Flusskontrolle |
| **Prüfsumme** | 16 Bit | über TCP-Header, Nutzdaten **und Pseudo-Header** (Quell-IP, Ziel-IP, 8 Null-Bits, Protokoll-Nr. 6, TCP-Länge) |
| **Urgent Pointer** | 16 Bit | bei URG: SN + Zeiger = Ende der dringenden Daten |
| **Optionen** | variabel | z. B. **MSS** (Maximum Segment Size) → verhindert IP-Fragmentierung; auf 32 Bit aufgefüllt |

⚠️ **Prüfsumme ≠ Schutz vor Manipulation** (Skript S. 50): Sie erkennt **zufällige** Übertragungsfehler, aber ein Angreifer (MitM) kann Daten ändern und die Prüfsumme einfach **neu berechnen**. → Für Integrität gegen Angreifer: MAC/Signatur (Kap. 6), z. B. TLS.

### TCP-Flags ⭐🦈
| Flag | Name | Bedeutung | Wann man es im Log sieht |
|---|---|---|---|
| **SYN** | Synchronize | Verbindungsaufbau, **initiale Sequenznummern synchronisieren** | Paket 1 und 2 des Handshakes |
| **ACK** | Acknowledgement | Ack-Feld ist gültig, wird ausgewertet | fast in **jedem** Segment nach dem ersten SYN |
| **FIN** | Finish | **ordnungsgemäßer** Abbau (Gegenstück zu SYN): „ich sende nichts mehr" | Ende der Verbindung |
| **RST** | Reset | Verbindung **sofort abbrechen / zurücksetzen**; Antwort auf Anfrage an **geschlossenen Port** | Fehler, abgelehnte Verbindung, Portscan-Antwort |
| **PSH** | Push | Daten **sofort** an die Anwendung weiterreichen | Segmente mit Nutzdaten (`[PSH, ACK]`) |
| **URG** | Urgent | dringende Daten, Urgent Pointer gültig | selten |

➕ Merksatz für die Reihenfolge der Bits: **U**nskilled **A**ttackers **P**ester **R**eal **S**ecurity **F**olks.

### 2.7.2 Verbindungsaufbau: Three-Way-Handshake (S. 50–51, Abb. 2.9) ⭐🦈🧮
```
Client                                               Server (LISTEN)
  |  (1) SYN, Seq = A                                   |
  |---------------------------------------------------->|
  |                    (2) SYN+ACK, Seq = B, Ack = A+1   |
  |<----------------------------------------------------|
  |  (3) ACK, Seq = A+1, Ack = B+1                      |
  |---------------------------------------------------->|
  |============ Verbindung hergestellt (ESTABLISHED) ===|
```
1. Client → Server: **SYN**, zufällige **ISN A** (Initial Sequence Number), Quell- und Zielport.
2. Server → Client: **SYN + ACK**, eigene zufällige **ISN B**, **Ack = A + 1**.
3. Client → Server: **ACK**, **Ack = B + 1**.

- **Jedes SYN (und jedes FIN) verbraucht eine Sequenznummer**, auch ohne Nutzdaten → deshalb „+1".
- Beide Seiten haben eigene, unabhängige ISNs → **Voll-Duplex** (Daten gleichzeitig in beide Richtungen).
- ➕ ISNs sind **zufällig**, damit ein Angreifer sie nicht erraten kann (→ Sequenznummernangriff / TCP-Hijacking, Kap. 3).
- Variante: **simultaner Verbindungsaufbau** (beide kennen die Ports vorher) — nicht weiter behandelt.

🐞 Skript S. 51, Schritt 2: „trägt als Quittierungsnummer die um eins inkrementierte ISN **des Servers** ein" — falsch. Richtig: **Ack = ISN des Clients + 1 (A + 1)**. Server-ISN B + 1 bestätigt erst der Client in Schritt 3.

🦈 **Wireshark zeigt standardmäßig relative Sequenznummern**: die ISN wird als **0** angezeigt. Ein Handshake sieht daher so aus:
```
1  192.168.1.20 → 10.0.0.5  TCP 74  49152 → 80 [SYN] Seq=0 Win=64240 Len=0 MSS=1460
2  10.0.0.5 → 192.168.1.20  TCP 74  80 → 49152 [SYN, ACK] Seq=0 Ack=1 Win=65160 Len=0 MSS=1460
3  192.168.1.20 → 10.0.0.5  TCP 66  49152 → 80 [ACK] Seq=1 Ack=1 Win=64256 Len=0
```
Erkennen: der **Client** ist, wer das erste `[SYN]` schickt; er nutzt einen **hohen, zufälligen Port** (ephemeral, z. B. 49152), der Server einen **bekannten Port** (80).

### 2.7.3 Datenübertragung — Seq/Ack rechnen (S. 51–52, Abb. 2.10) ⭐🧮
**Regeln:**
```
nächste Seq des Senders  = aktuelle Seq + Länge der Nutzdaten (Len)
Ack des Empfängers       = Seq des empfangenen Segments + Len   (= nächstes erwartetes Byte)
SYN und FIN zählen als 1 Byte, reine ACKs (Len=0) erhöhen nichts.
```
**Skript-Beispiel:** Client Seq = 100, sendet **10 Byte** → Server Ack = 100 + 10 = **110**. Nächstes Segment Seq = 110, **23 Byte** → Ack = **133**.

**Nutzdatenlänge aus den Headern berechnen** (so macht es der Empfänger):
```
TCP-Nutzdaten = IP Total Length − IP-Headerlänge (IHL·4) − TCP-Headerlänge (Offset·4)
```
Beispiel: Total Length 1500, IHL 5, Offset 8 → 1500 − 20 − 32 = **1448 Byte**.

### 2.7.4 Verzögerte Acknowledgements (Delayed ACK) und Piggybacking (S. 53–54)
- Problem: Für **jedes** Segment ein eigenes ACK → bei vielen kleinen Paketen hohe Netzlast.
- **Delayed ACK:** Empfänger wartet eine Zeitspanne (z. B. **200 ms**, muss **< 500 ms** sein, RFC 1122) und bestätigt **mehrere Segmente mit einem ACK** (kumulative Bestätigung).
- Beispiel Abb. 2.11: Pakete 10 B und 20 B → ein gemeinsames ACK; drittes Paket (42 B) → nach Ablauf der 200 ms eigenes ACK.
- **Piggybacking:** ACK + Ack-Nummer werden an ein Segment **mit Nutzdaten in Gegenrichtung angehängt** → kein eigenes ACK-Paket nötig.
- Ziel beider: **weniger Pakete, bessere Ausnutzung** der Leitung.

### 2.7.5 Retransmissions (S. 54–55, Abb. 2.12) ⭐🦈
- IP garantiert nichts → TCP muss Verluste selbst erkennen.
- **Fall 1: Segment in der Mitte geht verloren.** Empfänger bekommt ein Segment mit **zu großer SN** → sendet **sofort ein ACK mit der SN des fehlenden Segments** (= Duplicate ACK). Sender wartet einen **Timeout** (vielleicht kam es nur verspätet) und überträgt dann **erneut**.
  - Skript-Beispiel: Segmente Seq 100 (10 B), **110 (20 B, verloren)**, 130 (42 B). Server antwortet auf 130 mit **Ack = 110**. Client sendet erneut ab 110 — und packt die 42 Byte von Seq 130 gleich mit dazu → Retransmission-Paket **62 Byte** (20 + 42) → Server: **Ack = 172**.
  - ➕ In der Praxis: nach **3 Duplicate ACKs** sofortige Wiederholung ohne Timeout (**Fast Retransmit**).
- **Fall 2: letztes Segment oder ACK geht verloren.** Kein Folgesegment zeigt die Lücke → **Retransmission-Timer** (für jedes gesendete Segment beim Sender gestartet): kein ACK bis Ablauf → erneut senden.
- **Beschädigte** Segmente (falsche Prüfsumme) werden verworfen → wie verloren behandelt.

🦈 Wireshark markiert das automatisch: `[TCP Dup ACK 5#1]`, `[TCP Retransmission]`, `[TCP Fast Retransmission]`, `[TCP Previous segment not captured]`.

### 2.7.6 Flusskontrolle (Flow Control) (S. 55–56) ⭐
**Объяснение.** Пакеты теряются не только в кабеле. Часто виноват сам получатель: его **приёмный буфер** заполняется, потому что приложение медленно забирает данные. Тогда **получатель** притормаживает отправителя.
- **Flusskontrolle = Steuerung durch den Empfänger** über die **Fenstergröße (Window)** im TCP-Header: „so viele Bytes habe ich im Empfangspuffer frei".
- Puffer voll → ACK mit **Window = 0** → Sender stoppt, bis das Fenster wieder > 0 ist.
- 🦈 In Wireshark: `[TCP ZeroWindow]`, `[TCP Window Full]`, `[TCP Window Update]`.

---

## 2.8 Stauvermeidung, Verbindungsabbau, Abbruch, Zustände (S. 56–60)

### Stauvermeidung (Congestion Control) ⭐
- Ursache hier: **Router auf dem Weg** ist überlastet (nicht der Empfänger!) → verwirft Pakete. Netzauslastung vorher unbekannt und schwankend.
- Verfahren (RFC 5681): **Slow Start** — Übertragungsrate beginnt klein und wächst **exponentiell**; ab einem **Schwellenwert** nur noch **linear** (**Congestion Avoidance**). Bei Fehlern/Verlust: zurück zu Slow Start, Schwelle **herabgesetzt**.
- Ziel: höchste Rate finden, bei der **kein Stau** (und damit keine Retransmissions) entsteht.
- Problem: Bei **Funk** geht ein Paket oft wegen schlechten Empfangs verloren, nicht wegen Überlast → Rate senken ist dann **nicht sinnvoll**.

| | **Flusskontrolle** | **Staukontrolle** |
|---|---|---|
| Schützt | den **Empfänger** (Puffer) | das **Netz** (Router) |
| Gesteuert durch | **Empfänger** über Window-Feld | **Sender** (schätzt Netzlast aus Verlusten) |
| Mechanismus | Fenstergröße, Zero Window | Slow Start, Congestion Avoidance |

⚠️ Klassische Prüfungsfrage: „Unterscheiden Sie Fluss- und Staukontrolle."

### 2.8.1 Verbindungsabbau (Four-Way) (S. 57, Abb. 2.13) ⭐🦈
```
Client                                       Server
  |  FIN, Seq = A                              |
  |------------------------------------------->|   Client: FIN_WAIT_1
  |                         ACK, Ack = A+1     |
  |<-------------------------------------------|   → halb geschlossen (Server darf weiter senden)
  |          ... Server sendet evtl. Restdaten |
  |                         FIN, Seq = B       |
  |<-------------------------------------------|
  |  ACK, Ack = B+1                            |
  |------------------------------------------->|   Verbindung entfernt, Ressourcen frei
```
- FIN verbraucht wie SYN **1 Sequenznummer**.
- Nach dem ersten FIN+ACK ist die Verbindung **halb geschlossen (half-close)**: eine Richtung zu, die andere noch offen.
- ➕ Oft wird das ACK mit dem eigenen FIN kombiniert (`[FIN, ACK]`) → im Log sieht man dann nur 3 Pakete.

### 2.8.2 Verbindungsabbruch mit RST (S. 57–58) ⭐🦈
RST-Segment → Verbindung beim Sender **sofort komplett beendet**: kein Warten, Puffer geleert, Ressourcen frei. Fälle:
1. **Fehler** in bestehender Verbindung → Abbruch.
2. Anfrage an einen **geschlossenen Port** (kein Socket gebunden, keine Anwendung lauscht) → Antwort **RST**.
3. Segment an **offenen Port**, aber **ohne vorher aufgebaute Verbindung** (z. B. ein ACK „aus dem Nichts") → RST.

🦈 **Darauf beruht Port-Scanning (Kap. 3.5):**
| Antwort auf SYN | Port-Zustand |
|---|---|
| **SYN/ACK** | **offen** (open) |
| **RST** (bzw. RST/ACK) | **geschlossen** (closed) |
| **keine Antwort** / ICMP unreachable | **gefiltert** (Firewall) |

### 2.8.3 Zustände einer TCP-Verbindung (S. 58–60, Abb. 2.14)
TCP-Stack speichert für **jede** Verbindung ihren Zustand → **endlicher Automat** (Zustandsautomat, Transitionen = Pfeile, beschriftet mit auslösendem Ereignis). Im Skript nicht im Detail — ➕ für netstat-Ausgaben trotzdem nützlich:

| Zustand | Bedeutung |
|---|---|
| **CLOSED** | keine Verbindung |
| **LISTEN** | Server wartet auf Verbindungen (offener Port) |
| **SYN_SENT** | Client hat SYN gesendet, wartet auf SYN/ACK |
| **SYN_RECEIVED** (SYN_RECV) | Server hat SYN erhalten und SYN/ACK gesendet, wartet auf ACK → **„halboffene Verbindung"** (⚠️ viele davon = **SYN-Flooding**, Kap. 3) |
| **ESTABLISHED** | Verbindung steht, Datenaustausch |
| **FIN_WAIT_1 / FIN_WAIT_2** | aktiver Schließer hat FIN gesendet / dessen ACK erhalten |
| **CLOSE_WAIT** | passive Seite hat FIN erhalten, Anwendung hat noch nicht geschlossen |
| **LAST_ACK** | passive Seite hat eigenes FIN gesendet, wartet auf letztes ACK |
| **TIME_WAIT** | aktiver Schließer wartet nach dem letzten ACK noch (2 · MSL), damit verspätete Pakete nicht in neue Verbindung geraten |

---

## 2.9 User Datagram Protocol (UDP) (S. 60–61) ⭐
**Eigenschaften:** Transportschicht, **verbindungslos, nicht zuverlässig, ungesichert**, RFC 768 (nur 3 Seiten!).
- **Keine** Quittungen, keine Reihenfolge, keine Retransmission — Kontrolle bleibt der **Anwendung** überlassen.
- Header: nur **4 Felder à 16 Bit = 8 Byte**: **Quellport, Zielport, Länge, Prüfsumme**.
- **Weniger Aufwand** als TCP, aber höhere Wahrscheinlichkeit für Paketverluste.
- **Keine Verschlüsselung, keine Authentisierung, keine Integritätssicherung.**
- Firewall: Filterung nach IP und Port möglich; **zustandsbehaftete Filterung** nur über **dynamische Zeitfenster** (weil es keine Verbindung gibt).
- **Vorteil: geringe Schwankungen der Übertragungsverzögerung (Jitter)** — bei TCP würde ein verlorenes Paket neu angefordert → Stocken. Bei UDP sinkt nur kurz die Qualität → ideal für **VoIP, Video-Streaming, Online-Spiele, DNS-Anfragen**.

| Kriterium | **TCP** | **UDP** |
|---|---|---|
| Verbindung | verbindungsorientiert (Handshake) | verbindungslos |
| Zuverlässigkeit | ja: ACK, Retransmission, Reihenfolge, Duplikate | nein |
| Fluss-/Staukontrolle | ja | nein |
| Header | 20–60 Byte | **8 Byte** |
| Geschwindigkeit / Overhead | langsamer, mehr Overhead | schnell, wenig Overhead |
| Zustand | zustandsbehaftet | zustandslos |
| Anwendungen | HTTP(S), SMTP, FTP, SSH | DNS-Anfrage, DHCP, VoIP, Streaming, Gaming |
| Socket-Art | **Stream Socket** | **Datagram Socket** |
| Sicherheit ➕ | Handshake erschwert IP-Spoofing (Angreifer muss Seq erraten) | **IP-Spoofing leicht** → Basis für **Amplification-DDoS** (DNS, NTP) |

**Kontrollfrage 4 — warum zwei Transportprotokolle?** Weil Anwendungen **unterschiedliche Anforderungen** haben: Dateiübertragung braucht **Vollständigkeit** (lieber spät als falsch) → TCP. Echtzeit (VoIP, Video) braucht **geringe, gleichmäßige Verzögerung** (lieber ein Paket verlieren als warten) → UDP. Ein einziges Protokoll könnte nicht beides optimal.

---

## 2.10 Datenaustausch auf Ebene des Betriebssystems (S. 61–63)

### 2.10.1 MAC-Adressen ⭐
- **Media Access Control Address** = **physische Adresse** jedes Netzwerkadapters (Kabel oder Funk), **48 Bit**.
- **Erste 24 Bit = OUI** (Organizationally Unique Identifier), von der **IEEE** an Hersteller vergeben → man erkennt den **Hersteller**. Rest = Geräte-ID.
- Gilt nur im **lokalen Netz** (Schicht 2): Im Internet sieht man nur die MAC des **letzten Routers**, weil jeder Router den Ethernet-Header neu schreibt.
- ⚠️ ➕ MAC-Adressen lassen sich per Software **ändern (MAC-Spoofing)**, z. B. `ip link set eth0 address …` / `ifconfig eth0 hw ether …` / Treibereinstellung unter Windows → MAC ist **kein sicheres Authentisierungsmerkmal** (MAC-Filter im WLAN leicht umgehbar).

### 2.10.2 Ports ⭐
- Port = **Verbindungspunkt / Endpunkt** einer Kommunikationsbeziehung auf einem Rechner; Nummer **0–65 535** (16 Bit).
- Für eine Verbindung immer **zwei** Ports: Sender- und Empfängerseite.
- TCP- und UDP-Ports sind **unabhängig** voneinander (TCP 53 ≠ UDP 53).
- **Well-known Ports 0–1023**: standardisiert (RFC 1700, IANA). ➕ 1024–49151 registered, 49152–65535 dynamic/ephemeral (Client-Seite).

**Wichtige Ports ⭐ (Skript Abb. 2.16 + ➕ Ergänzungen, die man kennen muss):**
| Port | Prot. | Dienst | Sicherheitsnotiz ➕ |
|---|---|---|---|
| 7 | TCP | Echo | Testdienst |
| 19 | TCP | Chargen | Testdienst, für DoS missbrauchbar |
| **20 / 21** | TCP | **FTP** (20 Daten, 21 Steuerung) | **Klartext** inkl. Passwort |
| **22** ➕ | TCP | **SSH** (auch SCP/SFTP) | verschlüsselt |
| **23** | TCP | **Telnet** | **Klartext** → durch SSH ersetzen |
| **25** | TCP | **SMTP** | Mailversand |
| **53** | UDP/TCP | **DNS** | UDP für Anfragen, TCP für Zonentransfer |
| **67 / 68** | UDP | **DHCP** (67 Server, 68 Client) | |
| **80** | TCP | **HTTP** | **Klartext** |
| 110 ➕ | TCP | POP3 | Klartext |
| 143 ➕ | TCP | IMAP | Klartext |
| **443** ➕ | TCP | **HTTPS** (HTTP über TLS) | verschlüsselt |
| 445 ➕ | TCP | SMB (Windows-Freigaben) | häufiges Angriffsziel (WannaCry) |
| 513 | TCP | rlogin | Klartext (🐞 Skript: UDP) |
| 2049 | UDP/TCP | NFS | |
| 3306 ➕ | TCP | MySQL | |
| 3389 ➕ | TCP | RDP (Remotedesktop) | häufiges Angriffsziel |

### 2.10.3 Sockets ⭐
- **Socket** = *standardisierte, plattformunabhängige Schnittstelle, die das Betriebssystem einem Programm zur Verfügung stellt, um Daten zu senden und zu empfangen.* Auch für **Interprozesskommunikation** auf demselben Rechner.
- **Stream Sockets** → TCP; **Datagram Sockets** → UDP.
- Eine Verbindung wird eindeutig durch ein **4-Tupel (Socketpaar)** identifiziert: **(Server-IP, Server-Port, Client-IP, Client-Port)** ➕ (+ Protokoll = 5-Tupel).

**Ablauf (Abb. 2.17):**
1. Server erstellt **Socket A**.
2. Server **bindet** A an eigene Adresse/Port und **wartet** (listen).
3. Client erstellt **Socket B**.
4. Client verbindet sich mit Socket B zum Server (Tupel Serveradresse + Port). (🐞 Skript vertauscht hier A und B.)
5. Socket A **akzeptiert** und erstellt einen **neuen Socket C** für diese Verbindung; A wartet weiter auf neue Clients.
6. Verbindung C = 4-Tupel. Zweite parallele Verbindung desselben Clients → **neuer Client-Port** → unterscheidbar (z. B. zwei parallele Downloads).
7. Datenaustausch über C.
8. Einer beendet die Verbindung → C wird auf beiden Seiten entfernt.

➕ Socket-API-Funktionen: `socket()` → `bind()` → `listen()` → `accept()` (Server) / `connect()` (Client) → `send()/recv()` → `close()`. Genau so funktioniert auch der **MiniWebServer.java** aus dem Praktikum (ServerSocket auf Port 4444, `accept()`, HTTP-Antwort schreiben).

---

## 2.11 Grundlegende Netzwerkkommandos (S. 63–66) ⭐🦈
Einsatz u. a. in der **Forensik** (Datensammlung): aktuelle IP, DNS-Server, Gateway, Proxy, **ARP-Cache**, **aktive Verbindungen**. Kommandozeilentools bevorzugt, weil Ausgabe leicht in Textdatei **umgeleitet** werden kann (`> datei.txt`) → Beweissicherung.

| Befehl (Win / Linux) | Funktion | Protokoll | ➕ modern |
|---|---|---|---|
| **ping** | **Erreichbarkeit** eines Hosts prüfen, Laufzeit (RTT) messen | **ICMP Echo Request (Typ 8) / Echo Reply (Typ 0)** | — |
| **tracert / traceroute** | **Pfad** (alle Router/Hops) zum Ziel anzeigen | Pakete mit **schrittweise steigender TTL** (1, 2, 3 …); jeder Router, bei dem TTL 0 wird, antwortet mit **ICMP Time Exceeded** | `mtr` |
| **netstat** | aktive **Verbindungen**, Ports, Zustände, Protokollstatistik; `-a -n -o` = Process-to-Port-Mapping (PID) | — | `ss -tulpn` (Linux) |
| **ipconfig / ifconfig** | IP, Maske, Gateway; `ipconfig /all`: MAC, DHCP-Lease, DHCP- und DNS-Server | — | `ip a` (Linux) |
| **arp** | ARP-Cache (IP ↔ MAC) anzeigen / ändern | ARP | `ip neigh` |
| nslookup / dig ➕ | DNS-Namen auflösen | DNS | — |

➕ `tracert` (Windows) nutzt **ICMP Echo**, `traceroute` (Linux) standardmäßig **UDP** an hohe Ports; beide werten **ICMP Time Exceeded** aus.

### Ausgaben lesen 🦈
**ping (Skript):**
```
$ ping hs-mittweida.de
PING hs-mittweida.de (141.55.192.190) 56(84) bytes of data.
64 bytes from www.htwm.de (141.55.192.190): icmp_seq=1 ttl=53 time=36.9 ms
...
3 packets transmitted, 3 received, 0% packet loss, time 2001ms
rtt min/avg/max/mdev = 36.933/37.618/38.799/0.838 ms
```
- `hs-mittweida.de (141.55.192.190)` → **DNS-Auflösung** fand statt (ping verrät die IP).
- `56(84) bytes`: 56 Byte Daten + 8 Byte ICMP-Header + 20 Byte IP-Header = **84**. `64 bytes from` = 56 + 8 ICMP.
- `ttl=53` ➕: typische Start-TTL **64 (Linux/macOS)**, **128 (Windows)**, 255 (Netzwerkgeräte) → 64 − 53 = **≈ 11 Hops** entfernt, vermutlich **Linux/Unix**-Server (einfaches **OS-Fingerprinting**).
- `0% packet loss` → Verbindung stabil; `rtt` = Round Trip Time.
- Keine Antwort heißt **nicht** sicher „Host aus": ICMP kann von einer **Firewall gefiltert** sein.

**netstat (Skript):**
```
Proto Local Address        Foreign Address      State
tcp   127.0.1.1:53         0.0.0.0:*            LISTEN
tcp   192.168.178.158:5674 103.41.177.62:443    VERBUNDEN (ESTABLISHED)
```
- Zeile 1: lokaler DNS-Dienst **lauscht** auf Port 53 (nur auf Loopback-Adresse → von außen nicht erreichbar).
- Zeile 2: eigener Rechner (Port 5674, ephemeral → **Client**) ist mit einem **HTTPS-Server** (Port 443) verbunden.
- Forensik: unbekannte ESTABLISHED-Verbindungen zu fremden IPs → Hinweis auf **Trojaner/Spyware**; mit `-o` die **PID** des Prozesses finden.

**traceroute (Skript):** 10 Hops bis zur Hochschule; Hop 1 = `fritz.box (192.168.178.1)` = **eigener Router/Default Gateway**; `* * *` = Router antwortet nicht (filtert ICMP). Nutzen: Standort eines Angreifers eingrenzen, weitere Systeme mit Logdaten finden.

**ifconfig (Skript):**
```
Link encap:Ethernet  Hardware Adresse 00:21:cc:5e:8a:a5        ← MAC
inet Adresse:192.168.178.158  Bcast:192.168.178.255  Maske:255.255.255.0
inet6-Adresse: fe80::224:d7ff:fec8:fc0c/64  Gueltigkeitsbereich:Verbindung   ← IPv6 Link-Local
UP BROADCAST MULTICAST  MTU:1500  Metrik:1
```
→ Netz 192.168.178.0/24, Broadcast .255, **MTU 1500** Byte (max. IP-Paketgröße im Ethernet). ➕ `ff:fe` in der Mitte der IPv6-ID → aus MAC abgeleitet (EUI-64) → Datenschutzproblem (s. 2.6.4).

---

## 2.12 Basisdienste und Netzmanagement-Protokolle (S. 67–72) ⭐
Wichtig, weil sie **häufig Angriffspunkte** sind — und alle **ohne eingebaute Sicherheit** entworfen wurden.

### 2.12.1 Domain Name System (DNS) ⭐🦈
- **Zweck:** übersetzt **Namen ↔ IP-Adressen** (`www.hs-mittweida.de` → `141.55.192.190`). Analogie: **Telefonbuch**.
- **Verteilter, hierarchischer Dienst der Anwendungsschicht**; DNS-Server bilden eine **verteilte Datenbank**.
- Namensraum in **Zonen** unterteilt; jede Zone = unabhängiger Verwaltungsbereich mit verantwortlichem Server (**DNS Primary**, ➕ „autoritativ").
- **Ports:** Client-Anfragen über **53/UDP** (verbindungslos); **Zonentransfer** zwischen DNS-Servern über **53/TCP**.
- **Caching mit TTL:** Primary gibt Daten mit begrenzter Gültigkeitsdauer weiter; nachgeordnete Server puffern sie (nicht alle beachten die TTL korrekt). Kann ein Server nicht lokal auflösen → fragt **übergeordnete** Server.

**Ablauf einer Auflösung (Abb. 2.18, Beispiel 2.2) ⭐:**
```
Client ──(1) "IP von www.hs-mittweida.de?"──► lokaler DNS-Server (Resolver)
   Resolver ──(2)──► Root-Nameserver  ──(3) "frag den .de-Server"
   Resolver ──(4)──► .de-Nameserver   ──(5) "frag den hs-mittweida.de-Server"
   Resolver ──(6)──► hs-mittweida.de-Nameserver ──(7) "141.55.192.190" (autoritativ)
Client ◄──(8) Antwort── Resolver (speichert im Cache für TTL)
```
➕ Begriffe: Client → Resolver = **rekursive** Anfrage („gib mir die fertige Antwort"); Resolver → Root/TLD/autoritativ = **iterative** Anfragen („sag mir, wen ich fragen soll").
- War der Name kürzlich angefragt → Antwort sofort **aus dem Cache**.

**Sicherheit ⭐:**
- DNS ist **ungesichert** (keine Authentisierung/Integrität), nur Filterung über TCP/UDP-Ports möglich.
- **Manipulation von DNS-Daten** → Anfragen **blockieren** oder auf **fremde Rechner umleiten** → Angreifer schlüpft in die Rolle fremder Server (**DNS-Spoofing / Cache Poisoning**, Kap. 3.3.8) → Phishing.
- Kryptografische Absicherung existiert (➕ **DNSSEC**: Signaturen = Integrität/Authentizität, keine Vertraulichkeit; **DoH/DoT** = DNS über HTTPS/TLS → Vertraulichkeit), aber wenig verbreitet.

🦈 Im Log:
```
192.168.1.20 → 192.168.1.1  DNS 78  Standard query 0x1a2b A www.hs-mittweida.de
192.168.1.1 → 192.168.1.20  DNS 94  Standard query response 0x1a2b A www.hs-mittweida.de A 141.55.192.190
```
Die **Transaction ID** (0x1a2b) verbindet Anfrage und Antwort. `A` = IPv4-Record (➕ AAAA = IPv6, MX = Mailserver, NS = Nameserver, CNAME = Alias, PTR = Rückwärtsauflösung).

### 2.12.2 DHCP (Dynamic Host Configuration Protocol) ⭐🦈
- **Verbindungsloses Protokoll der Anwendungsschicht** (über UDP), weist Rechnern **dynamisch** IP-Adresse + Konfiguration zu: **Subnetzmaske, Standard-Gateway, DNS-Server**. → neue Rechner ohne manuelle Konfiguration ins Netz.
- **Ports:** Client fragt per **Broadcast an 67/UDP**; Server antworten an **68/UDP**.
- Ablauf laut Skript: Client fragt per Broadcast → **jeder** erreichbare DHCP-Server schickt ein Angebot → Client **wählt eines** und quittiert → übrige Angebote verfallen.
- ➕ Die vier Nachrichten = **DORA**:
  | # | Nachricht | Von → An | Inhalt |
  |---|---|---|---|
  | 1 | **Discover** | Client `0.0.0.0` → `255.255.255.255` | „Gibt es einen DHCP-Server?" |
  | 2 | **Offer** | Server → Client | Angebot: IP, Maske, Gateway, DNS, Lease-Dauer |
  | 3 | **Request** | Client → Broadcast | „Ich nehme das Angebot von Server X" |
  | 4 | **Ack** | Server → Client | Bestätigung, Adresse ist vergeben (Lease) |
- Adressen werden **zeitlich befristet** vergeben (**Lease**).
- Findet ein Client **keinen** DHCP-Server → **APIPA 169.254.x.x**.

**Sicherheit ⭐:**
- **DHCP für IPv4: keine Authentisierung, keine Integritätssicherung, keine Verschlüsselung.**
- **DHCPv6** (Ports **546/547 UDP**): Authentisierung, Integritätssicherung, Replay-Erkennung möglich — aber **unverschlüsselt**.
- **Angriffe:**
  - **Falsche DHCP-Nachrichten** (➕ **Rogue DHCP Server**) → verbreiten **falsche Routing- bzw. DNS-Informationen** → Kommunikation **umgelenkt, verhindert, verfälscht** (z. B. Angreifer als Gateway → MitM).
  - **Horten von Adressen** (➕ **DHCP Starvation**): böswilliger Client stellt massenhaft Anfragen (mit gefälschten MACs) → Adresspool leer → **Verfügbarkeit** für andere beeinträchtigt.
- **Schutz (Skript):** **manueller Modus** = statische Zuordnung IP ↔ MAC (erschwert fremde Clients); im dynamischen Modus schützt die **zeitliche Befristung** gegen Horten. ➕ Switch-Funktion **DHCP Snooping** (nur vertrauenswürdige Ports dürfen Offers senden).

### 2.12.3 Address Resolution Protocol (ARP) ⭐🦈
**Объяснение.** Чтобы отправить IP-пакет соседу в **локальной** сети, нужно знать его **MAC-адрес**, потому что Ethernet-кадр адресуется по MAC. ARP отвечает на вопрос «**какой MAC у этого IP?**» и связывает слой 3 со слоем 2. Для IPv6 эту задачу выполняет **NDP**.

```
Sender                                         alle im LAN
  | ARP-Request (Broadcast, ff:ff:ff:ff:ff:ff):   |
  | "Wer hat IP 1.2.3.4? Sag es 192.168.1.20"     |
  |---------------------------------------------->|
  |        ARP-Reply (Unicast) vom Besitzer:      |
  | "1.2.3.4 ist bei a:b:c:d:e:f"                 |
  |<----------------------------------------------|
  → beide speichern die Zuordnung im ARP-Cache
```
- MAC-Adresse = Schicht 2 (genauer: Unterschicht **MAC, 2a** im IEEE-Modell); ARP verbindet Schicht 2 und 3.
- **ARP-Cache:** Tabelle IP ↔ MAC der zuletzt kontaktierten Rechner.
- Befehle: `arp -a` (Tabelle anzeigen), `arp -s <IP> <MAC>` (**statischer** Eintrag), `arp -d` (Eintrag löschen).
- **Welche Geräte brauchen eine MAC?** Nur solche, die auf Schicht 2 **selbst adressiert** werden. **Repeater/Hub**: leiten nur weiter → keine MAC nötig. **Bridge/Switch**: untersuchen Frames, teilen in **Kollisionsdomänen**, nehmen aber nicht aktiv teil → für Basisfunktion keine MAC; **brauchen** aber eine, wenn sie **selbst administriert** werden (Telnet, SNMP, HTTP) oder **Spanning Tree** nutzen.

**Sicherheit ⭐ (Kontrollfrage 7, Brücke zu Kap. 3):**
- ARP hat **keine Authentisierung**: Jeder darf antworten, und Rechner übernehmen Antworten (➕ sogar unaufgeforderte, **Gratuitous ARP**) in ihren Cache.
- → **ARP-Spoofing / ARP-Poisoning:** Angreifer sendet gefälschte ARP-Replies „Gateway-IP ist bei **meiner** MAC" → Opfer schickt Verkehr an den Angreifer → **Man-in-the-Middle**.
- **Nur im lokalen Netz (Broadcast-Domäne)** möglich, weil ARP-Broadcasts **nicht geroutet** werden und Router den Ethernet-Header neu schreiben.
- Schutz: **statische ARP-Einträge** (`arp -s`), ➕ Dynamic ARP Inspection am Switch, Tools wie arpwatch, Verschlüsselung (TLS) gegen die Folgen.

🦈 Im Log:
```
00:21:cc:5e:8a:a5 → ff:ff:ff:ff:ff:ff  ARP 42  Who has 192.168.1.1? Tell 192.168.1.20
00:1a:4f:12:34:56 → 00:21:cc:5e:8a:a5  ARP 60  192.168.1.1 is at 00:1a:4f:12:34:56
```
**Alarmzeichen für Spoofing:** zwei verschiedene MACs für **dieselbe IP**; viele unaufgeforderte „is at"-Replies; Wireshark-Warnung **„duplicate use of 192.168.1.1 detected!"**.

### 2.12.4 ICMP (Internet Control Message Protocol) ⭐🦈
- Transportiert **Fehler- und Diagnoseinformationen** für IPv4 (und als ICMPv6 für IPv6); auch zur Steuerung von TCP/UDP.
- **Verbindungslos**, wird in **IP-Datagrammen** transportiert (Protokollnummer **1**), gehört zur **Internetschicht (3)**.
- **Keine Authentisierung, Verschlüsselung, Integritätssicherung** → leicht angreifbar: Einschleusen gefälschter ICMP-Nachrichten kann die **Verfügbarkeit** gefährden oder die **Netzstruktur ausspähen** (Ping-Sweep, Traceroute).
- Nachrichten haben **Type** + **Code**; können nach Typ **selektiv gesperrt** werden. Paketfilter behandeln ICMP oft **zustandslos** → genaue Filterung nur eingeschränkt.

**Wichtige ICMP-Typen ⭐ (➕ Nummern ergänzt):**
| Type | Name | Wann |
|---|---|---|
| **0** | **Echo Reply** | Antwort auf ping |
| **3** | **Destination Unreachable** | Ziel nicht erreichbar (Codes s. u.) |
| 4 | Source Quench | „langsamer senden" (veraltet) |
| **5** | **Redirect** | Router: „nimm einen besseren Weg" (➕ für MitM missbrauchbar) |
| **8** | **Echo Request** | ping |
| **11** | **Time Exceeded** | **TTL = 0** unterwegs (→ traceroute) |
| 12 | Parameter Problem | fehlerhafter Header |
| 13/14 | Timestamp / Timestamp Reply | |
| 15/16 | Information Request / Reply | |

**Codes bei Destination Unreachable (Type 3, Skript):** **0** = net unreachable · **1** = host unreachable · **2** = protocol unreachable · **3** = port unreachable · **4** = fragmentation needed and DF set · **5** = source route failed.
Aufbau: Type (8 Bit) | Code (8 Bit) | Checksum (16 Bit) | unused (32 Bit) | **IP-Header + erste 64 Bit des Original-Datagramms** (damit der Sender weiß, welches Paket betroffen war).

🐞 Skript S. 71: Destination Unreachable werde gesendet, „wenn das TTL-Feld den Wert Null hat" — falsch. Bei **TTL = 0** kommt **Time Exceeded (Type 11)**. Destination Unreachable (Type 3) kommt, wenn Netz/Host/Port nicht erreichbar ist.

🦈 Im Log:
```
192.168.1.20 → 8.8.8.8       ICMP 98  Echo (ping) request  id=0x0001, seq=1/256, ttl=64
8.8.8.8 → 192.168.1.20       ICMP 98  Echo (ping) reply    id=0x0001, seq=1/256, ttl=117 (request in 1)
10.0.0.1 → 192.168.1.20      ICMP 70  Time-to-live exceeded (Time to live exceeded in transit)
10.0.0.5 → 192.168.1.20      ICMP 70  Destination unreachable (Port unreachable)
```
(`Port unreachable` = Antwort auf **UDP** an einen geschlossenen Port; bei TCP käme stattdessen ein RST.)

---

## Sicherheitsbezug: welche Schwäche → welcher Angriff (Brücke zu Kap. 3) ⭐
Для ответов вида «Warum ist Protokoll X unsicher?» и для понимания главы 3:

| Protokoll | Schwäche (Design) | Angriff (Kap. 3) | Schicht | Schutzziel |
|---|---|---|---|---|
| Ethernet / Hub | alle sehen alle Frames | **Sniffing** | 1/2 | Vertraulichkeit |
| MAC | per Software änderbar | **MAC-Spoofing** | 2 | Authentizität |
| ARP | keine Authentisierung, Caches übernehmen Antworten | **ARP-Spoofing → MitM** | 2/3 | Vertraulichkeit, Integrität |
| IP | Absenderadresse nicht geprüft | **IP-Spoofing**, DDoS mit gefälschten Absendern | 3 | Authentizität |
| ICMP | ungeschützt | Ping-Flood, Smurf, Redirect-Angriffe, Netzaufklärung | 3 | Verfügbarkeit, Vertraulichkeit |
| TCP | Server reserviert Ressourcen schon nach SYN | **SYN-Flooding** (viele halboffene Verbindungen) | 4 | Verfügbarkeit |
| TCP | vorhersagbare Sequenznummern | **TCP-Hijacking / Sequenznummernangriff** | 4 | Integrität, Authentizität |
| TCP | RST/SYN-ACK verraten Portzustand | **Port-Scanning** | 4 | (Aufklärung) |
| UDP | verbindungslos, Absender fälschbar | **Amplification-DDoS** | 4 | Verfügbarkeit |
| DNS | keine Authentisierung, Caching | **DNS-Spoofing / Cache Poisoning** | 7 | Integrität, Authentizität |
| DHCP | keine Authentisierung | **Rogue DHCP, Starvation** | 7 | Verfügbarkeit, Integrität |
| HTTP, FTP, Telnet | **Klartext** | Mitlesen von Passwörtern | 7 | Vertraulichkeit |

---

## 🐞 Fehler und Ungenauigkeiten im Skript (Übersicht)
Auf der Prüfung: Wenn eine Frage genau darauf zielt, **richtige Variante** nennen, ggf. mit „laut Skript … , korrekt ist …".
| Stelle | Skript | Korrekt |
|---|---|---|
| S. 35, Schicht 1 | Router gehört zu Schicht 1 | Router = **Schicht 3** |
| S. 41 | Protokollfeld enthält „Port" | enthält **Protokollnummer** (6 TCP, 17 UDP, 1 ICMP) |
| S. 43, Klasse C | 255 Adressen je Netz | **256** Adressen, **254** Hosts |
| S. 43, APIPA | 196.254.0.0/16 | **169.254.0.0/16** |
| S. 45, Subnetzbeispiel | Maske /26 für 4 Subnetze à 14 Hosts | /26 = Gesamtblock; jedes Subnetz **/28** |
| S. 46, IPv6-Beispiel | `001:0db8:…` | `2001:0db8:…` |
| S. 46, IPsec | obligatorisch | ursprünglich Pflicht, seit RFC 6434 nur empfohlen |
| S. 51, Handshake Schritt 2 | Ack = Server-ISN + 1 | Ack = **Client-ISN + 1** |
| S. 62, Ports | rlogin 513/UDP | rlogin **513/TCP** |
| S. 63, Sockets Schritt 4 | Client nutzt Socket A | Client nutzt **Socket B** und verbindet sich zu A |
| S. 71, ICMP | Destination Unreachable bei TTL = 0 | TTL = 0 → **Time Exceeded (Type 11)** |

---

# Aufgaben — типичные и нетипичные

**[T]** = типичная · **[N]** = нетипичная / на перенос · **[1,0]** = на высший балл · 🦈 = анализ логов.
Сначала реши сам (прикрой решение), потом сверяйся.

## A. Kontrollfragen aus dem Skript (S. 73) [T]

**A1. Das OSI-Modell unterscheidet streng zwischen Diensten, Protokollen und Schnittstellen. Warum ist das von Vorteil?**
> **✅ Lösung:** Ein **Dienst** beschreibt, *was* eine Schicht für die darüberliegende Schicht leistet; die **Schnittstelle** (SAP), *wie* die höhere Schicht darauf zugreift; das **Protokoll**, *wie* die Partner-Instanzen derselben Schicht die Leistung untereinander erbringen. Durch die Trennung kann man das Protokoll einer Schicht **austauschen**, ohne die anderen Schichten zu ändern, solange Dienst und Schnittstelle gleich bleiben (z. B. WLAN statt Ethernet — der Browser merkt nichts). Vorteile: **Abstraktion, Komplexitätsreduzierung, Austauschbarkeit, Herstellerunabhängigkeit/Interoperabilität**, unabhängige Weiterentwicklung der Schichten.

**A2. 🧮 Subnetzmaske 255.255.224.0. Welche Rechner liegen im gleichen Subnetz? a) 10.47.63.69 b) 10.47.64.89 c) 10.47.97.2 d) 10.47.95.2 e) 10.47.32.99 f) 192.168.1.29**
> **✅ Lösung:** 255.255.**224**.0 = /19; interessantes Oktett = 3., **Blockgröße 256 − 224 = 32** → Netze 10.47.0.0, .32.0, .64.0, .96.0 …
> | Adresse | 3. Oktett → abgerundet auf Vielfaches von 32 | Netz |
> |---|---|---|
> | a) 10.47.63.69 | 63 → 32 | 10.47.32.0 |
> | b) 10.47.64.89 | 64 → 64 | 10.47.64.0 |
> | c) 10.47.97.2 | 97 → 96 | 10.47.96.0 |
> | d) 10.47.95.2 | 95 → 64 | 10.47.64.0 |
> | e) 10.47.32.99 | 32 → 32 | 10.47.32.0 |
> | f) 192.168.1.29 | — | 192.168.0.0 |
> **Gleiche Subnetze: a + e** (10.47.32.0/19) und **b + d** (10.47.64.0/19). c und f liegen jeweils allein.
> Binär-Kontrolle für a): 63 = 0011 1111, AND 1110 0000 = 0010 0000 = 32 ✔.
> ⚠️ Falle: d) 10.47.95.2 „sieht aus" wie c) (95 ≈ 97), liegt aber im Netz von b).

**A3. Welcher Nachrichtentyp bzw. welches Protokoll wird beim Aufruf von ping verwendet?**
> **✅ Lösung:** **ICMP**: der Sender schickt **Echo Request (Type 8)**, der Zielhost antwortet mit **Echo Reply (Type 0)**. Ist das Ziel nicht erreichbar, kommt ggf. **Destination Unreachable (Type 3)** (Net/Host unreachable) vom letzten Router. ICMP liegt auf Schicht 3 und wird in IP-Paketen mit Protokollnummer 1 transportiert.

**A4. Warum wurden auf der Transportschicht mehrere Protokollklassen eingeführt? Warum reicht ein einziges Protokoll nicht aus?**
> **✅ Lösung:** Anwendungen haben **gegensätzliche Anforderungen**. Dateiübertragung, Web, E-Mail brauchen **vollständige, fehlerfreie, geordnete** Daten → TCP mit Bestätigungen und Retransmissions (Verzögerung egal). Echtzeitanwendungen wie VoIP, Video-Streaming, Online-Spiele brauchen **geringe und gleichmäßige Verzögerung**; ein verlorenes Paket neu anzufordern wäre schlimmer als es zu verlieren → UDP. Kurze Anfrage-Antwort-Dienste (DNS) sparen mit UDP den Handshake-Overhead. Ein Protokoll allein müsste immer einen Kompromiss eingehen.

**A5. Worin unterscheiden sich prinzipiell TCP und UDP?**
> **✅ Lösung:** Tabelle TCP vs. UDP aus 2.9: verbindungsorientiert vs. verbindungslos; zuverlässig (ACK, Retransmission, Reihenfolge, Duplikate) vs. keine Garantien; Fluss- und Staukontrolle vs. keine; Header 20–60 vs. 8 Byte; mehr Overhead vs. schnell; Stream- vs. Datagram-Socket; Beispiele HTTP/SSH vs. DNS/VoIP.

**A6. Wie erfolgt der Verbindungsaufbau bei TCP?**
> **✅ Lösung:** **Three-Way-Handshake:** (1) Client → Server: **SYN**, zufällige ISN A, Ports. (2) Server → Client: **SYN+ACK**, eigene ISN B, **Ack = A+1**. (3) Client → Server: **ACK**, **Ack = B+1**. Danach ESTABLISHED, Voll-Duplex. SYN verbraucht eine Sequenznummer, daher +1. Skizze mit Pfeilen zeichnen (gibt Punkte!).

**A7. Mit welcher Technik kann ich die MAC-Adresse innerhalb des lokalen Netzwerkes fälschen? Funktioniert das auch über das lokale Netz hinaus?**
> **✅ Lösung:** (1) **MAC-Spoofing**: eigene MAC per Software ändern (`ip link set dev eth0 address …`, `ifconfig eth0 hw ether …`, Treibereinstellung unter Windows) → z. B. MAC-Filter umgehen. (2) **ARP-Spoofing**: gefälschte ARP-Replies senden, die einer fremden **IP** die **eigene MAC** zuordnen → Opfer trägt sie in seinen ARP-Cache ein → Datenverkehr läuft über den Angreifer (MitM). **Nur im lokalen Netz** (Broadcast-Domäne) wirksam: MAC-Adressen und ARP gehören zu Schicht 2; ARP-Broadcasts werden **nicht geroutet**, und jeder **Router schreibt den Ethernet-Header neu** (eigene MAC als Absender). Im Internet sieht ein Empfänger nur die MAC des letzten Routers.

---

## B. Subnetting 🧮

**B1 [T]. 192.168.10.77/27: Maske, Netzadresse, Broadcast, Hostbereich, Anzahl Hosts?**
> **✅ Lösung:** /27 = **255.255.255.224**; Blockgröße 32; 77 → **64**.
> Netz **192.168.10.64**, Broadcast **192.168.10.95**, Hosts **.65 – .94**, **30 Hosts** (2⁵ − 2).

**B2 [T]. 172.16.45.200/20: Maske, Netz, Broadcast, Hostbereich, Hosts?**
> **✅ Lösung:** /20 = **255.255.240.0**; interessantes Oktett = 3., Blockgröße 16; 45 → **32**.
> Netz **172.16.32.0**, Broadcast **172.16.47.255** (nächstes Netz 172.16.48.0 − 1), Hosts **172.16.32.1 – 172.16.47.254**, 2¹² − 2 = **4 094 Hosts**.

**B3 [N]. 10.20.30.40/12: Netzanteil und Hostanteil (Skript-Methode mit invertierter Maske), Broadcast.**
> **✅ Lösung:** /12 = **255.240.0.0**; interessantes Oktett = 2., Blockgröße 16; 20 → 16.
> Netzanteil = IP AND Maske = **10.16.0.0**. Invertierte Maske = 0.15.255.255 → Hostanteil = IP AND 0.15.255.255 = **0.4.30.40** (20 − 16 = 4). Broadcast **10.31.255.255**. Hosts: 2²⁰ − 2 = 1 048 574.

**B4 [T]. Liegen 192.168.1.130 und 192.168.1.125 im selben Netz bei /25? Und bei /24?**
> **✅ Lösung:** /25 = 255.255.255.128 → 130 AND 128 = 128, 125 AND 128 = 0 → Netze .128 und .0 → **nein**, Kommunikation nur über einen Router. Bei /24 → beide 192.168.1.0 → **ja**.

**B5 [T]. Das Netz 192.168.5.0/24 soll in 6 Abteilungsnetze mit je bis zu 25 Hosts aufgeteilt werden. Maske? Liste der Subnetze? Netz- und Broadcastadresse des 3. Subnetzes?**
> **✅ Lösung:** Hosts: 2⁵ − 2 = 30 ≥ 25 → **5 Hostbits**; Subnetze: 2³ = 8 ≥ 6 → **3 Subnetzbits** (24 + 3 + 5 = 32 ✔).
> Maske **/27 = 255.255.255.224**. Subnetze (Blockgröße 32): .0, .32, .64, .96, .128, .160, .192, .224 (8 Stück, 2 Reserve).
> 3. Subnetz (Index 2, Zählung ab 0): **Netz 192.168.5.64**, **Broadcast 192.168.5.95**, Hosts .65–.94.
> ⚠️ Klären, ob „3. Subnetz" ab 0 oder ab 1 gezählt wird — Skript zählt ab **0** (Index 0 = erstes Subnetz). Im Zweifel die Annahme hinschreiben.

**B6 [N]. Ein Unternehmen hat 10.0.0.0/8 und braucht Subnetze mit jeweils mindestens 500 Hosts. Welche Maske? Wie viele solcher Subnetze sind möglich?**
> **✅ Lösung:** 2⁹ − 2 = 510 ≥ 500 → **9 Hostbits** → /23 = **255.255.254.0**. Subnetzbits = 23 − 8 = 15 → **2¹⁵ = 32 768** Subnetze.

**B7 [N] 🦈. Fehlersuche: Ein PC hat IP 192.168.0.70, Maske 255.255.255.192, Gateway 192.168.0.129. Er erreicht Rechner .80 und .100, aber nicht das Internet. Warum?**
> **✅ Lösung:** /26, Blockgröße 64 → PC liegt im Netz **192.168.0.64/26** (.65–.126). Gateway .129 liegt im Netz **192.168.0.128/26** → **nicht im eigenen Subnetz** → PC kann das Gateway nicht direkt (per ARP) erreichen → kein Internet. .80 und .100 liegen im eigenen Netz → funktioniert. Lösung: Gateway-Adresse aus .65–.126 verwenden oder die Maske passend ändern (z. B. /24).

**B8 [T]. Welche Maske verwendet man sinnvollerweise für eine Punkt-zu-Punkt-Verbindung zwischen zwei Routern?**
> **✅ Lösung:** **/30 = 255.255.255.252** → 4 Adressen: Netz, 2 Hosts, Broadcast → genau 2 nutzbare Adressen, keine Verschwendung. (➕ /31 nach RFC 3021 ebenfalls möglich, nicht klassisch.)

**B9 [N]. Welche Maske passt am besten für ein Netz mit 1 000 Hosts?**
> **✅ Lösung:** 2¹⁰ − 2 = 1 022 ≥ 1 000 (2⁹ − 2 = 510 zu klein) → **10 Hostbits → /22 = 255.255.252.0**.

**B10 [T]. Welche der Adressen kann im Netz 192.168.1.0/26 einem Host zugewiesen werden? a) 192.168.1.63 b) 192.168.1.64 c) 192.168.1.0 d) 192.168.1.62**
> **✅ Lösung:** Netz .0, Broadcast .63, Hosts .1–.62. a) **nein**, Broadcast · b) **nein**, gehört zum nächsten Netz (.64 = dessen Netzadresse) · c) **nein**, Netzadresse · d) **ja**.

**B11 [T]. Wandeln Sie um: /21 in Dezimalmaske; 255.255.255.248 in CIDR; Anzahl Hosts jeweils.**
> **✅ Lösung:** /21 = 11111111.11111111.11111**000**.0 = **255.255.248.0**, 2¹¹ − 2 = **2 046** Hosts. 255.255.255.248 = 24 + 5 = **/29**, 2³ − 2 = **6** Hosts.

---

## C. Header aus Hex lesen 🦈🧮
(Typisch für Wireshark-Aufgaben: Hex-Dump unten im Wireshark-Fenster.)

**C1 [T]. Analysieren Sie den IPv4-Header:**
`45 00 00 3c 1c 46 40 00 40 06 b1 e6 ac 10 0a 63 ac 10 0a 0c`
> **✅ Lösung:**
> | Bytes | Feld | Wert |
> |---|---|---|
> | 45 | Version / IHL | IPv4, 5 · 4 = **20 Byte** |
> | 00 3c | Total Length | **60 Byte** → Nutzlast 40 Byte |
> | 1c 46 | Identification | 0x1c46 |
> | 40 00 | Flags | **DF = 1**, nicht fragmentiert |
> | 40 | TTL | **64** (typisch Linux; Paket hat noch keinen Router passiert oder wurde direkt beim Sender mitgeschnitten) |
> | 06 | Protocol | **TCP** |
> | b1 e6 | Checksum | 0xb1e6 |
> | ac 10 0a 63 | Source | 172.16.10.**99** (0x63 = 99) |
> | ac 10 0a 0c | Destination | 172.16.10.**12** (0x0c = 12) |
> Beide Adressen sind **privat** (172.16.0.0/12) → Verkehr im internen Netz.

**C2 [N]. Was für ein Paket ist das? Welches Betriebssystem hat es vermutlich gesendet?**
`45 00 00 54 a3 1f 00 00 80 01 xx xx c0 a8 00 05 08 08 08 08` *(Prüfsumme ausgeblendet)*
> **✅ Lösung:** IPv4, 20 Byte Header; Total Length 0x54 = **84 Byte** → 64 Byte Nutzlast = 8 Byte ICMP-Header + 56 Byte Daten → genau das Format von **ping** („56(84) bytes"). Protocol **01 = ICMP** → sehr wahrscheinlich **Echo Request**. Flags 00 00 → nicht fragmentiert, DF nicht gesetzt. TTL 0x80 = **128** → typische Start-TTL von **Windows**. Quelle 192.168.0.5 (privat) → Ziel **8.8.8.8** (Google Public DNS). Fazit: ein Windows-PC pingt 8.8.8.8 (Konnektivitätstest).

**C3 [N] [1,0]. Drei Pakete wurden mitgeschnitten (Auszug aus dem IP-Header):**
| Paket | Total Length | Identification | Flags+Offset | Protocol |
|---|---|---|---|---|
| P1 | 05 dc | 2a 10 | 20 00 | 11 |
| P2 | 02 18 | 2a 10 | 01 72 | 11 |
| P3 | 05 dc | 2a 10 | 20 b9 | 11 |
Gehören sie zusammen? In welcher Reihenfolge? Wie groß war die ursprüngliche Nutzlast? Welches Protokoll?
> **✅ Lösung:**
> - **Gleiche Identification 0x2a10** → Fragmente **desselben** Datagramms. Protocol 0x11 = **17 = UDP**.
> - Flags/Offset: P1 `2000` → MF=1, Offset 0 → **erstes** Fragment; P3 `20b9` → MF=1, Offset 0xb9 = 185 · 8 = **1480** → **zweites**; P2 `0172` → MF=0, Offset 0x172 = 370 · 8 = **2960** → **letztes**. Reihenfolge: **P1, P3, P2** (Ankunft in anderer Reihenfolge ist bei IP normal — Nicht-Determinismus!).
> - Längen: 0x05dc = 1500 → Nutzlast 1480 je Fragment; 0x0218 = 536 → Nutzlast 516.
> - Original-Nutzlast = Offset des letzten + dessen Nutzlast = 2960 + 516 = **3 476 Byte** (= 1480 + 1480 + 516 ✔).
> - Sicherheits-Bonus ➕: manipulierte/überlappende Fragment-Offsets wurden historisch für Angriffe genutzt (Teardrop) und um Firewalls/IDS zu umgehen.

**C4 [N]. Analysieren Sie den TCP-Header:**
`c0 00 00 50 00 00 03 e8 00 00 00 00 50 02 fa f0 xx xx 00 00`
> **✅ Lösung:**
> | Bytes | Feld | Wert |
> |---|---|---|
> | c0 00 | Quellport | 0xc000 = **49152** (ephemeral → Client) |
> | 00 50 | Zielport | 0x50 = **80** (HTTP → Server) |
> | 00 00 03 e8 | Seq | **1000** |
> | 00 00 00 00 | Ack | 0 (ACK-Flag nicht gesetzt) |
> | 5 | Data Offset | 5 · 4 = **20 Byte** Header |
> | 02 | Flags | 0000 00**1**0 → **SYN** |
> | fa f0 | Window | **64 240** |
> | 00 00 | Urgent Pointer | 0 |
> → **Erstes Paket eines Three-Way-Handshakes** (Client 49152 → Webserver Port 80, ISN = 1000).
>
> **Flags-Byte schnell lesen** (Bits von links: CWR ECE URG ACK PSH RST SYN FIN):
> | Hex | Flags |
> |---|---|
> | 02 | SYN |
> | 12 | SYN, ACK |
> | 10 | ACK |
> | 18 | PSH, ACK |
> | 11 | FIN, ACK |
> | 04 | RST |
> | 14 | RST, ACK |
> | 29 ➕ | URG, PSH, FIN („Xmas-Scan" von nmap) |
> | 00 ➕ | keine Flags („Null-Scan") |

---

## D. Sequenz- und Quittierungsnummern 🧮

**D1 [T]. Client-ISN = 1000, Server-ISN = 5000. Der Client sendet nach dem Handshake 200 Byte, der Server antwortet mit 500 Byte. Danach schließt der Client die Verbindung, dann der Server. Ergänzen Sie Seq und Ack aller Segmente.**
> **✅ Lösung:**
> | # | Richtung | Flags | Seq | Ack | Len |
> |---|---|---|---|---|---|
> | 1 | C → S | SYN | 1000 | — | 0 |
> | 2 | S → C | SYN, ACK | 5000 | **1001** | 0 |
> | 3 | C → S | ACK | 1001 | **5001** | 0 |
> | 4 | C → S | PSH, ACK | 1001 | 5001 | 200 |
> | 5 | S → C | ACK | 5001 | **1201** | 0 |
> | 6 | S → C | PSH, ACK | 5001 | 1201 | 500 |
> | 7 | C → S | ACK | 1201 | **5501** | 0 |
> | 8 | C → S | FIN, ACK | 1201 | 5501 | 0 |
> | 9 | S → C | ACK | 5501 | **1202** | 0 |
> | 10 | S → C | FIN, ACK | 5501 | 1202 | 0 |
> | 11 | C → S | ACK | 1202 | **5502** | 0 |
> Regeln: SYN und FIN zählen +1, Daten zählen +Len, reine ACKs zählen 0. (5 und 6 könnten per **Piggybacking** zu einem Segment zusammengefasst sein.)

**D2 [T]. Ein Client mit aktueller Seq 2000 sendet nacheinander 100, 250 und 50 Byte. Welche Ack-Nummern sendet der Server bei sofortiger Bestätigung? Welche bei Delayed ACK, wenn die ersten beiden Segmente innerhalb von 200 ms ankommen?**
> **✅ Lösung:** Sofort: **2100, 2350, 2400**. Delayed ACK: ein gemeinsames ACK **2350** (bestätigt kumulativ beide), danach **2400**.

**D3 [N]. Segmente: Seq 5000 (500 B), Seq 5500 (500 B) **geht verloren**, Seq 6000 (500 B), Seq 6500 (500 B). Welche Acks schickt der Empfänger? Was passiert danach?**
> **✅ Lösung:** Auf 5000 → **Ack 5500**. Auf 6000 → wieder **Ack 5500** (Duplicate ACK, Lücke erkannt). Auf 6500 → nochmals **Ack 5500** (2. Duplicate ACK). Sender wiederholt Seq 5500 (nach Timeout bzw. ➕ Fast Retransmit nach 3 Dup-ACKs). Hat der Empfänger 6000 und 6500 gepuffert, bestätigt er danach **kumulativ Ack 7000**. (Skript-Variante: Sender schickt fehlende und folgende Daten zusammen in einem größeren Retransmission-Segment — Ergebnis ebenfalls Ack 7000.) In Wireshark: `[TCP Dup ACK]`, `[TCP Retransmission]`.

**D4 [T] 🦈. Wireshark (relative Nummern):**
```
5  192.168.1.20 → 93.184.216.34  TCP  [PSH, ACK] Seq=1 Ack=1 Len=517
6  93.184.216.34 → 192.168.1.20  TCP  [ACK] Seq=1 Ack=?? Len=0
7  93.184.216.34 → 192.168.1.20  TCP  [PSH, ACK] Seq=1 Ack=518 Len=1400
8  192.168.1.20 → 93.184.216.34  TCP  [ACK] Seq=?? Ack=?? Len=0
```
> **✅ Lösung:** Paket 6: Ack = 1 + 517 = **518**. Paket 8: Seq = **518** (Client hat bisher 517 Byte gesendet), Ack = 1 + 1400 = **1401**.
> ⚠️ Relativ heißt: ISN wird als 0 angezeigt; mit echter ISN wäre alles um die ISN verschoben.

**D5 [N]. Ein IP-Paket hat Total Length 1052, IHL 5; der TCP-Header hat Data Offset 8 und Seq 74 000. Welche Ack-Nummer sendet der Empfänger?**
> **✅ Lösung:** Nutzdaten = 1052 − 5·4 − 8·4 = 1052 − 20 − 32 = **1000 Byte** → Ack = 74 000 + 1000 = **75 000**.

---

## E. Zuordnungsaufgaben [T]

**E1. Ordnen Sie den OSI-Schichten zu:** Hub · Switch · Router · HTTP · TCP · IP · ICMP · MAC-Adresse · Portnummer · Verschlüsselung/Kompression (laut Skript) · Frame · Bits auf dem Kabel · Ethernet · DNS
> **✅ Lösung:** Schicht 1: Hub, Bits auf dem Kabel, (Ethernet phys.) · Schicht 2: Switch, MAC-Adresse, Frame, Ethernet · Schicht 3: Router, IP, ICMP · Schicht 4: TCP, Portnummer · Schicht 6: Verschlüsselung/Kompression · Schicht 7: HTTP, DNS.

**E2. Ordnen Sie die Ports zu.**
| | Port | | Dienst |
|---|---|---|---|
| 1 | 22 | a | HTTPS |
| 2 | 23 | b | DNS |
| 3 | 25 | c | SSH |
| 4 | 53 | d | DHCP-Server |
| 5 | 67 | e | Telnet |
| 6 | 80 | f | SMTP |
| 7 | 443 | g | HTTP |
| 8 | 21 | h | FTP-Steuerung |
> **✅ Lösung:** 1c · 2e · 3f · 4b · 5d · 6g · 7a · 8h

**E3. Ordnen Sie die Befehle zu.**
| | Befehl | | Zweck |
|---|---|---|---|
| 1 | ping | a | Pfad über alle Router zum Ziel anzeigen |
| 2 | traceroute | b | IP↔MAC-Zuordnungen des lokalen Netzes anzeigen |
| 3 | netstat -ano | c | Erreichbarkeit eines Hosts prüfen |
| 4 | ipconfig /all | d | aktive Verbindungen mit Prozess-ID anzeigen |
| 5 | arp -a | e | eigene IP, MAC, DHCP- und DNS-Server anzeigen |
> **✅ Lösung:** 1c · 2a · 3d · 4e · 5b

**E4. Ordnen Sie die ICMP-Nachrichten zu.**
| | Situation | | ICMP |
|---|---|---|---|
| 1 | Antwort auf ping | a | Type 11 Time Exceeded |
| 2 | ping senden | b | Type 3 Code 3 Port Unreachable |
| 3 | Router verwirft Paket mit TTL 0 | c | Type 0 Echo Reply |
| 4 | UDP-Paket an geschlossenen Port | d | Type 8 Echo Request |
| 5 | Paket zu groß, DF gesetzt | e | Type 3 Code 4 Fragmentation needed and DF set |
> **✅ Lösung:** 1c · 2d · 3a · 4b · 5e

**E5. Ordnen Sie die TCP-Flags zu.**
| | Situation | | Flag(s) |
|---|---|---|---|
| 1 | erstes Paket eines Verbindungsaufbaus | a | RST |
| 2 | Server bestätigt den Verbindungswunsch | b | FIN |
| 3 | geordnetes Beenden | c | SYN |
| 4 | Antwort auf SYN an geschlossenen Port | d | SYN, ACK |
| 5 | Daten sofort an Anwendung übergeben | e | PSH |
> **✅ Lösung:** 1c · 2d · 3b · 4a · 5e

**E6. Ordnen Sie zu: Protokoll → Transportprotokoll + Port.** DNS-Anfrage · DNS-Zonentransfer · DHCP · HTTP · VoIP (RTP) ➕ · SSH
> **✅ Lösung:** DNS-Anfrage: UDP 53 · Zonentransfer: TCP 53 · DHCP: UDP 67/68 · HTTP: TCP 80 · VoIP (RTP): UDP (dynamische Ports) · SSH: TCP 22.

---

## F. Richtig oder falsch? (mit Begründung) [T/N]
1. IP garantiert, dass Pakete in der richtigen Reihenfolge ankommen.
2. Die TCP-Prüfsumme schützt vor Manipulation durch einen Man-in-the-Middle.
3. Ein Switch arbeitet auf Schicht 3.
4. Eine Adresse aus 169.254.0.0/16 deutet darauf hin, dass kein DHCP-Server erreichbar war.
5. ARP-Spoofing funktioniert auch über das Internet hinweg.
6. UDP besitzt einen 8 Byte großen Header.
7. Bei der Flusskontrolle bremst der Router den Sender.
8. TTL wird von jedem Router um 1 verringert.
9. Ein FIN-Segment ohne Nutzdaten erhöht die Sequenznummer nicht.
10. Antwortet ein Host nicht auf ping, ist er ausgeschaltet.
11. DNS-Anfragen von Clients laufen normalerweise über UDP Port 53.
12. Das OSI-Modell legt konkrete Protokolle für jede Schicht fest.
13. In IPv6 gibt es keinen Broadcast mehr.
14. Eine Netzadresse kann einem Host zugewiesen werden.
> **✅ Lösung:**
> 1. **Falsch.** IP ist verbindungslos und nicht-deterministisch; Reihenfolge stellt **TCP** her.
> 2. **Falsch.** Angreifer kann die Prüfsumme neu berechnen; sie erkennt nur zufällige Fehler. Schutz: MAC/Signatur, TLS.
> 3. **Falsch.** Schicht **2** (MAC-Adressen). Router = Schicht 3.
> 4. **Richtig.** APIPA/Link-Local — Selbstzuweisung ohne DHCP.
> 5. **Falsch.** ARP wirkt nur im lokalen Netz (Broadcast-Domäne, wird nicht geroutet).
> 6. **Richtig.** 4 Felder à 16 Bit: Quellport, Zielport, Länge, Prüfsumme.
> 7. **Falsch.** Flusskontrolle = **Empfänger** über Window-Feld. Netzüberlast → Staukontrolle.
> 8. **Richtig.** Bei 0 wird das Paket verworfen, ICMP Time Exceeded an den Sender.
> 9. **Falsch.** FIN verbraucht wie SYN **eine** Sequenznummer.
> 10. **Falsch.** ICMP kann von einer Firewall gefiltert werden.
> 11. **Richtig.** TCP 53 v. a. für Zonentransfers (und große Antworten).
> 12. **Falsch.** Es beschreibt nur **Zuständigkeiten/Verhalten**, keine Protokolle oder Implementierungen.
> 13. **Richtig.** Broadcast wird durch **Multicast** ersetzt.
> 14. **Falsch.** Netz- und Broadcastadresse sind reserviert (deshalb 2^h − 2).

---

## G. Log-Analyse 🦈 (wie auf der Prüfung: Ausgabe im Terminal deuten)

### G1 [T] [1,0]. tshark-Mitschnitt
```
 1 0.000000 192.168.1.20 → 10.0.0.5     TCP  74 49152 → 80 [SYN] Seq=0 Win=64240 Len=0 MSS=1460
 2 0.000412 10.0.0.5     → 192.168.1.20 TCP  74 80 → 49152 [SYN, ACK] Seq=0 Ack=1 Win=65160 Len=0 MSS=1460
 3 0.000450 192.168.1.20 → 10.0.0.5     TCP  66 49152 → 80 [ACK] Seq=1 Ack=1 Win=64256 Len=0
 4 0.000900 192.168.1.20 → 10.0.0.5     HTTP 145 GET /login.php?user=anna&pw=Sommer2026 HTTP/1.1
 5 0.001300 10.0.0.5     → 192.168.1.20 TCP  66 80 → 49152 [ACK] Seq=1 Ack=80 Win=65152 Len=0
 6 0.004100 10.0.0.5     → 192.168.1.20 HTTP 512 HTTP/1.1 200 OK  (text/html)
 7 0.004200 192.168.1.20 → 10.0.0.5     TCP  66 49152 → 80 [ACK] Seq=80 Ack=447 Win=64128 Len=0
 8 0.010000 192.168.1.20 → 10.0.0.5     TCP  74 49153 → 443 [SYN] Seq=0 Win=64240 Len=0 MSS=1460
 9 0.010300 10.0.0.5     → 192.168.1.20 TCP  54 443 → 49153 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0
```
a) Wer ist Client, wer Server? Welche Ports?
b) In welchen Paketen findet der Verbindungsaufbau statt? Wurde er erfolgreich abgeschlossen?
c) Warum steht in Paket 5 `Ack=80`? Wie viele Byte HTTP-Daten hat der Client gesendet?
d) Wie viele Byte hat der Server in Paket 6 gesendet? Begründen Sie mit Paket 7.
e) Was bedeuten Pakete 8–9?
f) Bewerten Sie den Mitschnitt aus Sicht der IT-Sicherheit.
> **✅ Lösung:**
> a) **Client 192.168.1.20** (sendet das erste SYN, Quellport 49152 = ephemeral), **Server 10.0.0.5** (Port **80 = HTTP**).
> b) Pakete **1–3** = Three-Way-Handshake (SYN → SYN/ACK → ACK) → **erfolgreich**, Verbindung ESTABLISHED.
> c) Paket 4 hat Seq=1 und trägt 145 − 66 = **79 Byte** HTTP-Daten (66 Byte = Frame ohne Nutzlast, s. Paket 3: 14 Ethernet + 20 IP + 32 TCP). Ack = 1 + 79 = **80** = nächstes erwartetes Byte.
> d) 512 − 66 = **446 Byte**; Kontrolle: Client bestätigt mit Ack = 1 + 446 = **447** ✔.
> e) Neue Verbindung zu **Port 443 (HTTPS)**: Server antwortet mit **RST, ACK** → Port 443 ist **geschlossen** (kein Dienst lauscht). Der Server bietet also **kein HTTPS** an.
> f) **Zugangsdaten im Klartext** in der URL (`user=anna&pw=Sommer2026`) über unverschlüsseltes HTTP → jeder, der mitschneiden kann (Sniffing, MitM per ARP-Spoofing), liest das Passwort → **Vertraulichkeit verletzt**; zusätzlich landen GET-Parameter in Server-Logs und im Browserverlauf. Maßnahmen: **HTTPS (TLS)** aktivieren (Port 443 öffnen), Login per **POST** statt GET, HTTP auf HTTPS umleiten (➕ HSTS).

### G2 [T]. Was passiert hier?
```
 1 0.000000 10.0.0.66 → 10.0.0.5  TCP 58 41000 → 21 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 2 0.000050 10.0.0.66 → 10.0.0.5  TCP 58 41000 → 22 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 3 0.000090 10.0.0.66 → 10.0.0.5  TCP 58 41000 → 23 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 4 0.000130 10.0.0.66 → 10.0.0.5  TCP 58 41000 → 80 [SYN] Seq=0 Win=1024 Len=0 MSS=1460
 5 0.000400 10.0.0.5  → 10.0.0.66 TCP 54 21 → 41000 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0
 6 0.000410 10.0.0.5  → 10.0.0.66 TCP 58 22 → 41000 [SYN, ACK] Seq=0 Ack=1 Win=64240 Len=0 MSS=1460
 7 0.000450 10.0.0.66 → 10.0.0.5  TCP 54 41000 → 22 [RST] Seq=1 Win=0 Len=0
 8 0.000460 10.0.0.5  → 10.0.0.66 TCP 54 23 → 41000 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0
 9 0.000470 10.0.0.5  → 10.0.0.66 TCP 58 80 → 41000 [SYN, ACK] Seq=0 Ack=1 Win=64240 Len=0 MSS=1460
10 0.000500 10.0.0.66 → 10.0.0.5  TCP 54 41000 → 80 [RST] Seq=1 Win=0 Len=0
```
> **✅ Lösung:**
> - **Port-Scan** (genauer: **SYN-Scan / Half-Open-Scan**, typisch `nmap -sS`) von 10.0.0.66 auf 10.0.0.5. Indizien: viele SYNs auf **verschiedene Zielports** in Mikrosekunden, **gleicher Quellport**, ungewöhnlich kleines Window 1024 (nmap-typisch).
> - **Offen:** 22 (SSH), 80 (HTTP) — Antwort SYN/ACK. **Geschlossen:** 21 (FTP), 23 (Telnet) — Antwort RST/ACK.
> - Der Scanner beantwortet SYN/ACK **mit RST statt ACK** → Handshake wird **nie abgeschlossen** → keine vollständige Verbindung, wird von Anwendungen oft nicht geloggt („stealth").
> - Einordnung: **Angriffsvorbereitung** (Informationen sammeln, Kap. 1.5). Rechtlich ohne Auftrag problematisch (Vorbereitung, je nach Fall §202a/c). Gegenmaßnahmen: Firewall (nur nötige Ports), IDS erkennt Scan-Muster, unnötige Dienste abschalten.

### G3 [T] [1,0]. ARP-Mitschnitt — was ist auffällig?
```
1 0.000 00:1a:4f:12:34:56 → ff:ff:ff:ff:ff:ff ARP 60 Who has 192.168.1.20? Tell 192.168.1.1
2 0.001 00:21:cc:5e:8a:a5 → 00:1a:4f:12:34:56 ARP 42 192.168.1.20 is at 00:21:cc:5e:8a:a5
3 5.120 de:ad:be:ef:00:01 → 00:21:cc:5e:8a:a5 ARP 42 192.168.1.1 is at de:ad:be:ef:00:01
4 7.120 de:ad:be:ef:00:01 → 00:21:cc:5e:8a:a5 ARP 42 192.168.1.1 is at de:ad:be:ef:00:01
5 7.121 de:ad:be:ef:00:01 → 00:1a:4f:12:34:56 ARP 42 192.168.1.20 is at de:ad:be:ef:00:01
```
> **✅ Lösung:**
> - Paket 1–2: normaler Ablauf — Gateway 192.168.1.1 (MAC **00:1a:4f:12:34:56**) fragt per Broadcast nach .20, .20 antwortet per Unicast.
> - Paket 3–5: **unaufgeforderte ARP-Replies** (kein Request vorher) von MAC **de:ad:be:ef:00:01**:
>   - an .20: „**192.168.1.1** is at de:ad:be:ef:00:01" → aber .1 gehört laut Paket 1 zu 00:1a:4f:12:34:56 → **gefälscht**;
>   - an das Gateway: „**192.168.1.20** is at de:ad:be:ef:00:01" → ebenfalls gefälscht.
>   - Regelmäßige Wiederholung (alle ~2 s), damit die Caches „vergiftet" bleiben.
> - Diagnose: **ARP-Spoofing / ARP-Cache-Poisoning in beide Richtungen** → Angreifer sitzt als **Man-in-the-Middle** zwischen PC .20 und Gateway; der gesamte Internetverkehr des PCs läuft über ihn. Wireshark würde warnen: *„duplicate use of 192.168.1.1 detected!"*
> - Betroffene Schutzziele: Vertraulichkeit (Mitlesen), Integrität (Manipulation), ggf. Verfügbarkeit (wenn er nicht weiterleitet).
> - Maßnahmen: statischer ARP-Eintrag für das Gateway (`arp -s`), ➕ Dynamic ARP Inspection am Switch, arpwatch/IDS, und gegen die Folgen: Ende-zu-Ende-Verschlüsselung (TLS, VPN).

### G4 [N]. Zwei Kollegen melden Netzwerkprobleme. Deuten Sie die Ausgaben.
**PC 1:**
```
IPv4-Adresse  . . . . . . . . . : 169.254.88.12
Subnetzmaske  . . . . . . . . . : 255.255.0.0
Standardgateway . . . . . . . . :
DHCP aktiviert. . . . . . . . . : Ja
```
**PC 2:**
```
IPv4-Adresse  . . . . . . . . . : 192.168.1.57
Subnetzmaske  . . . . . . . . . : 255.255.255.0
Standardgateway . . . . . . . . : 192.168.1.66
DHCP-Server . . . . . . . . . . : 192.168.1.66
DNS-Server  . . . . . . . . . . : 192.168.1.66
```
(Der echte Router und DHCP-Server der Firma ist 192.168.1.1.)
> **✅ Lösung:**
> - **PC 1:** **APIPA-Adresse** 169.254.x.x, kein Gateway → der PC hat **keinen DHCP-Server erreicht** und sich selbst eine Adresse gegeben → nur Kommunikation mit anderen APIPA-Rechnern, kein Internet. Ursachen: DHCP-Server ausgefallen, Kabel/VLAN-Problem, oder **Adresspool erschöpft** — möglicherweise durch **DHCP-Starvation** (Horten von Adressen, Angriff auf die Verfügbarkeit).
> - **PC 2:** hat eine Konfiguration von **192.168.1.66** statt vom echten Server .1 bekommen, und genau diese Adresse ist auch **Gateway und DNS-Server** → Verdacht auf einen **Rogue DHCP Server**: der Angreifer leitet allen Verkehr (Gateway) und alle Namensauflösungen (DNS) über sich → **Man-in-the-Middle**, DNS-Umleitung auf Phishing-Seiten möglich. Grund: DHCPv4 hat **keine Authentisierung**, der Client nimmt das erste/irgendein Angebot.
> - Maßnahmen: Rechner .66 finden (MAC über `arp -a`, Switchport) und vom Netz nehmen; ➕ **DHCP Snooping** am Switch; manueller DHCP-Modus (IP↔MAC statisch) laut Skript.

### G5 [T]. traceroute deuten
```
$ traceroute www.example.org
 1  fritz.box (192.168.178.1)  2.9 ms
 2  100.64.0.1 (100.64.0.1)  9.8 ms
 3  * * *
 4  ae1.fra.de.example.net (62.53.2.57)  14.2 ms
 5  93.184.216.34 (93.184.216.34)  21.7 ms
```
a) Was ist Hop 1? b) Was bedeutet `* * *`? c) Mit welcher TTL wurde das Paket gesendet, auf das Hop 4 geantwortet hat? d) Welche ICMP-Nachricht haben die Router 1–4 geschickt?
> **✅ Lösung:**
> a) Der **eigene Router / Default Gateway** (FritzBox, privates Netz 192.168.178.0/24).
> b) Dieser Router **antwortet nicht** (ICMP gefiltert bzw. rate-limited); das Paket wird trotzdem weitergeleitet, wie Hop 4 zeigt.
> c) **TTL = 4**: traceroute sendet mit TTL 1, 2, 3, …; der n-te Router setzt TTL auf 0 und antwortet.
> d) **ICMP Time Exceeded (Type 11)**. Das Ziel (Hop 5) antwortet stattdessen mit Echo Reply (Windows tracert) bzw. **Port Unreachable** (Linux traceroute mit UDP).
> ➕ 100.64.0.0/10 ist **Carrier-Grade-NAT**-Bereich des Providers (Teilnehmer hat keine eigene öffentliche IPv4).

### G6 [N] [1,0]. netstat auf einem Webserver (10.0.0.5)
```
Proto  Lokale Adresse     Remoteadresse          Status         PID
TCP    0.0.0.0:80         0.0.0.0:0              LISTENING      1120
TCP    10.0.0.5:80        203.0.113.17:51234     SYN_RECEIVED   1120
TCP    10.0.0.5:80        198.51.100.4:40211     SYN_RECEIVED   1120
TCP    10.0.0.5:80        192.0.2.200:61001      SYN_RECEIVED   1120
...    (ca. 2 000 weitere Zeilen SYN_RECEIVED von wechselnden IPs)
TCP    10.0.0.5:80        10.0.0.20:49830        ESTABLISHED    1120
TCP    10.0.0.5:51515     185.220.101.7:4444     ESTABLISHED    3368
```
Welche zwei Probleme erkennen Sie?
> **✅ Lösung:**
> 1. **Tausende Verbindungen im Zustand SYN_RECEIVED** von wechselnden (vermutlich **gefälschten**) IP-Adressen → **halboffene Verbindungen**: Server hat SYN/ACK geschickt, das abschließende ACK kommt nie → **SYN-Flooding** (DoS, Kap. 3.3.5). Der Server reserviert für jede halboffene Verbindung Ressourcen, bis die Tabelle voll ist → **Verfügbarkeit** gefährdet. Maßnahmen ➕: SYN-Cookies, Timeout verkürzen, Rate-Limiting, DDoS-Schutz beim Provider.
> 2. **Ausgehende** Verbindung des Servers (lokaler ephemeral Port 51515) zu einer externen IP auf **Port 4444** — ein Webserver baut normalerweise keine Verbindungen nach außen auf; 4444 ist der **Standardport von Metasploit** (Reverse Shell, Kap. 3.5.2). → Verdacht auf **Kompromittierung/Backdoor**. Nächste Schritte: Prozess mit **PID 3368** identifizieren (Task-Manager / `tasklist`), Ausgabe für die Forensik sichern (`netstat -ano > beweis.txt`), System isolieren.

### G7 [N]. DNS-Mitschnitt
```
1 0.000 192.168.1.20 → 192.168.1.1  DNS 76 Standard query 0x7c11 A www.meinebank.de
2 0.004 192.168.1.1  → 192.168.1.20 DNS 92 Standard query response 0x7c11 A www.meinebank.de A 6.6.6.6
3 0.031 192.168.1.1  → 192.168.1.20 DNS 92 Standard query response 0x7c11 A www.meinebank.de A 193.17.42.10
```
> **✅ Lösung:** Auf **eine** Anfrage (Transaction-ID 0x7c11) kommen **zwei Antworten mit unterschiedlichen IPs**. Die erste kommt verdächtig schnell (4 ms) — der Client akzeptiert die **erste** passende Antwort und verwirft die echte (31 ms). → **DNS-Spoofing**: ein Angreifer im lokalen Netz (z. B. nach ARP-Spoofing) fälscht die Antwort mit der Absenderadresse des Resolvers. Folge: Nutzer landet beim Aufruf der Bank auf **6.6.6.6** → Phishing (Authentizität, Vertraulichkeit). DNS hat keine Authentisierung; Schutz: ➕ DNSSEC, DoH/DoT, und HTTPS mit Zertifikatsprüfung (Browser warnt, weil der Angreifer kein gültiges Zertifikat für www.meinebank.de hat).

---

## H. Offene Fragen und Transfer [N] [1,0]

**H1. Warum braucht man sowohl IP- als auch MAC-Adressen?**
> **✅ Lösung:** Die **MAC-Adresse** (Schicht 2) adressiert ein Gerät **im lokalen Netzsegment**; Ethernet-Frames werden damit von Netzwerkkarte zu Netzwerkkarte zugestellt. Sie ist flach (keine Hierarchie) und eignet sich nicht für weltweites Routing. Die **IP-Adresse** (Schicht 3) ist **logisch und hierarchisch** (Netzanteil + Hostanteil) und erlaubt **Routing über viele Netze**. Auf dem Weg bleibt die IP-Adresse gleich, während die MAC-Adressen **an jedem Router neu gesetzt** werden. **ARP** verbindet beide: es ermittelt zur IP des nächsten Ziels (Host oder Gateway) im LAN die MAC.

**H2. [1,0] Beschreiben Sie, was im Netzwerk passiert, wenn ein frisch gestarteter Laptop im Browser `http://www.hs-mittweida.de` aufruft.**
> **✅ Lösung:**
> 1. **DHCP** (UDP 67/68, DORA): Laptop erhält IP, Maske, Gateway, DNS-Server.
> 2. **ARP**: Laptop ermittelt die MAC des **DNS-Servers** bzw. — wenn dieser in einem anderen Netz liegt — des **Gateways** (Broadcast „Who has …", Unicast-Antwort).
> 3. **DNS** (UDP 53): Anfrage „A www.hs-mittweida.de" → Resolver löst rekursiv über Root → .de → hs-mittweida.de auf → Antwort 141.55.192.190.
> 4. **Routing-Entscheidung**: Ziel AND Maske ≠ eigenes Netz → Pakete gehen an die **MAC des Gateways**, IP-Ziel bleibt 141.55.192.190; jeder Router verringert die **TTL**.
> 5. **TCP Three-Way-Handshake** zu Port 80 (SYN, SYN/ACK, ACK).
> 6. **HTTP-Request** (`GET / HTTP/1.1`) und **Response** (`200 OK` + HTML) in TCP-Segmenten, mit ACKs, Flusskontrolle, ggf. Retransmissions; **Kapselung** HTTP → TCP → IP → Ethernet.
> 7. **Verbindungsabbau** mit FIN/ACK.
> Sicherheitsbezug: jede Stufe ist angreifbar — Rogue DHCP, ARP-Spoofing, DNS-Spoofing, Mitlesen von HTTP. → HTTPS verwenden.

**H3. Warum ist ARP-Spoofing nur im lokalen Netz möglich, IP-Spoofing aber auch über das Internet?**
> **✅ Lösung:** ARP arbeitet mit **Broadcasts auf Schicht 2**, die Router **nicht weiterleiten**; ARP-Caches gelten nur für das eigene Segment. Ein Angreifer muss also in derselben Broadcast-Domäne sein. Die **IP-Absenderadresse** dagegen steht im IP-Header, wird von IP **nicht geprüft** und von Routern einfach weitergeleitet → ein Angreifer kann von überall Pakete mit gefälschter Absender-IP schicken. Einschränkung: Antworten gehen an die gefälschte Adresse, der Angreifer sieht sie nicht → bei TCP muss er die Sequenznummer **erraten** (daher zufällige ISNs); bei UDP/ICMP (verbindungslos) ist IP-Spoofing viel einfacher → Grundlage für **Amplification-DDoS**. ➕ Schutz: Ingress-Filtering beim Provider (BCP 38).

**H4. Ein Administrator sperrt an der Firewall sämtliche ICMP-Nachrichten. Bewerten Sie diese Maßnahme.**
> **✅ Lösung:**
> **Pro:** Angreifer können das Netz schlechter **ausspähen** (kein Ping-Sweep, kein Traceroute), ICMP-basierte DoS-Angriffe (Ping-Flood, Smurf) und gefälschte **Redirects** werden verhindert.
> **Contra:** ICMP transportiert **wichtige Fehler- und Diagnoseinformationen** (Skript). Ohne „**Fragmentation needed and DF set**" (Type 3 Code 4) funktioniert ➕ Path-MTU-Discovery nicht → manche Verbindungen „hängen" bei großen Paketen. Ping/Traceroute zur Fehlersuche fallen weg. Port-Scans funktionieren trotzdem über TCP.
> **Fazit:** **selektiv** sperren (Skript: „ICMP-Nachrichten lassen sich anhand ihres Typs selektiv sperren"): z. B. Echo Request von außen und Redirects blocken, Destination Unreachable und Time Exceeded erlauben.

**H5. Unterscheiden Sie Fluss- und Staukontrolle bei TCP.**
> **✅ Lösung:** siehe Tabelle in 2.8: Flusskontrolle schützt den **Empfänger** vor Überlastung seines Puffers; gesteuert vom Empfänger über das **Window-Feld** (Zero Window = Stopp). Staukontrolle schützt das **Netz** (überlastete Router); der Sender schätzt die Kapazität aus Paketverlusten (**Slow Start** exponentiell, ab Schwelle **Congestion Avoidance** linear, bei Verlust zurück und Schwelle senken). Problem bei Funk: Verlust ≠ Stau.

**H6. Warum eignet sich UDP für VoIP besser als TCP? Welche Sicherheitsnachteile hat UDP?**
> **✅ Lösung:** Bei TCP wird ein verlorenes Paket **neu angefordert** → Verzögerung schwankt (Jitter), Sprache stockt bzw. Wiedergabepuffer müssen größer sein. Bei UDP fehlt ein Paket einfach → **kurzer Qualitätsverlust**, aber kein Stocken (Skript S. 60–61). Ein spät ankommendes Sprachpaket ist ohnehin nutzlos. Sicherheitsnachteile: **keine Verschlüsselung, keine Authentisierung, keine Integritätssicherung**; verbindungslos → Absender-IP leicht **fälschbar** (Spoofing, Amplification-DDoS); Firewalls können UDP nur über Zeitfenster zustandsbehaftet filtern. ➕ Für VoIP daher SRTP (verschlüsseltes RTP).

**H7. [1,0] Bewerten Sie IPv6 aus Sicht der IT-Sicherheit.**
> **✅ Lösung:**
> **Vorteile:** IPsec von Anfang an vorgesehen (Skript: obligatorisch; ➕ heute „empfohlen") mit getrennten Headern für Authentifizierung und Verschlüsselung; ohne NAT echte **Ende-zu-Ende-Sicherheit**; riesiger Adressraum macht **Scannen** ganzer Netze praktisch unmöglich (2⁶⁴ Adressen pro Subnetz).
> **Nachteile/Risiken:** Interface-ID aus der **MAC** → Geräte und Personen **verfolgbar** (Datenschutz; Gegenmaßnahme Privacy Extensions); ohne NAT sind Geräte **direkt erreichbar** → Firewall-Regeln zwingend; neue Protokolle (NDP, SLAAC) haben eigene Spoofing-Angriffe (wie ARP-Spoofing); **Dual-Stack**-Betrieb verdoppelt die Angriffsfläche; Administratoren/Werkzeuge oft weniger erfahren mit IPv6.

**H8. Erklären Sie, warum ein Webserver Ports und Sockets braucht und wie er mehrere Clients gleichzeitig bedient.**
> **✅ Lösung:** Eine IP-Adresse identifiziert nur den **Rechner**; die **Portnummer** (Schicht 4) bestimmt die **Anwendung** (z. B. 80 = Webserver). Der Server erzeugt einen **Socket**, bindet ihn an IP + Port 80 und **lauscht** (LISTEN). Bei jeder eingehenden Verbindung **akzeptiert** er sie und erzeugt einen **neuen Socket** für genau diese Verbindung, der durch das **4-Tupel** (Server-IP, Server-Port, Client-IP, Client-Port) eindeutig ist. Da jeder Client (bzw. jede parallele Verbindung) einen **anderen Quellport** nutzt, kann der Server beliebig viele Verbindungen auf demselben Port 80 unterscheiden; der Lausch-Socket wartet weiter auf neue Clients.

---

## T. Transfer & Verständnis — Skript anwenden ⭐
> Здесь нет готового ответа в скрипте — нужно соединить механизмы и подумать.

**T1 [1,0]. Ein Admin sagt: „Wir nutzen UDP statt TCP für unsere interne Datenübertragung, das ist schneller." Welche Risiken übersieht er, und wann ist die Entscheidung trotzdem richtig?**
> **✅ Lösungsweg:** UDP ist verbindungslos & unzuverlässig: **keine Bestätigung, keine Reihenfolge, keine Retransmission, keine Fluss-/Staukontrolle** → bei Dateiübertragung drohen **unbemerkter Datenverlust und Reihenfolgefehler** (Integrität/Vollständigkeit). Außerdem ist die Absender-IP leicht **fälschbar** (Spoofing/Amplification). **Richtig** ist UDP nur, wenn **geringer, gleichmäßiger Jitter wichtiger ist als Vollständigkeit** — VoIP, Video, Live-Telemetrie, kurze DNS-Anfragen. Für zuverlässige Datenübertragung gehört TCP (oder UDP + eigene Sicherung in der Anwendung).

**T2 [1,0]. Erklären Sie, warum ein Angreifer mit gefälschter Absender-IP eine TCP-Verbindung kaum übernehmen kann, bei UDP aber leicht spooft. Verbinden Sie Handshake und Sequenznummern.**
> **✅ Lösungsweg:** Bei **TCP** antwortet der Server mit **SYN/ACK an die (gefälschte) IP**, nicht an den Angreifer → der Angreifer sieht die **Server-ISN B nicht** und muss sie **erraten**, um mit `Ack=B+1` fortzufahren. Da ISNs **zufällig** sind, scheitert das praktisch (→ Schutz gegen Blind-Spoofing/Hijacking). Bei **UDP** gibt es **keinen Handshake und keine Sequenznummern** → ein einzelnes gespooftes Datagramm wird sofort akzeptiert; Antworten gehen zwar ins Leere, aber für „feuer-und-vergiss"-Angriffe (DNS-Amplification, Flood) reicht das. Fazit: der TCP-Handshake ist nebenbei ein Spoofing-Schutz.

**T3 [N] 🧮. Ein Rechner hat 10.5.4.130/25 und soll 10.5.4.200 erreichen. Geht das direkt? Und warum sieht man im Internet nie die MAC des Zielservers, im LAN aber schon?**
> **✅ Lösungsweg:** /25 → Blockgröße 128; .130 liegt im Netz **10.5.4.128/25** (.129–.254), .200 ebenfalls → **gleiches Subnetz → direkt per ARP** (kein Routing). — MAC gilt nur auf **Schicht 2 im lokalen Segment**; jeder **Router schreibt den Ethernet-Header neu** (setzt seine eigene MAC), ARP-Broadcasts werden nicht geroutet. Deshalb sieht der Empfänger im Internet nur die MAC des **letzten Routers**, im LAN die echte MAC des Partners. (Verbindet Subnetting + ARP + Routing.)

**T4 [1,0]. Ein Paket hat im IP-Header DF=1 und ist größer als die MTU eines Links unterwegs. Was passiert, und welche Protokolle/Felder spielen zusammen?**
> **✅ Lösungsweg:** Mit **DF=1 (Don't Fragment)** darf der Router das Paket **nicht fragmentieren**; da es die **MTU** überschreitet, **verwirft** er es und meldet per **ICMP „Destination Unreachable, Code 4: fragmentation needed and DF set"** an den Sender (→ Path-MTU-Discovery, der Sender verkleinert dann seine Segmente/MSS). Zusammenspiel: **IP-Header (DF-Flag, Fragment Offset), MTU, ICMP Typ 3 Code 4, TCP-MSS**. Wenn jemand ICMP komplett blockt, „hängt" die Verbindung bei großen Paketen — klassische Fehlerquelle.

## I. Самопроверка перед Kap. 3 (с ответами)

**I1. 7 OSI-Schichten + je ein Protokoll/Gerät**
> 1 Bitübertragung (Hub) · 2 Sicherung (Switch, Ethernet, MAC) · 3 Vermittlung (Router, IP, ICMP) · 4 Transport (TCP, UDP) · 5 Sitzung · 6 Darstellung (Codierung, Kompression, Verschlüsselung) · 7 Anwendung (HTTP, DNS)

**I2. TCP/IP-Modell und Zuordnung zu OSI**
> Netzzugriff (OSI 1+2) · Internet (3) · Transport (4) · Anwendung (5–7)

**I3. Größe und wichtigste Felder: IPv4-Header, TCP-Header, UDP-Header**
> IPv4 20 Byte: Version, IHL, Total Length, ID, Flags (DF/MF), TTL, Protocol (1/6/17), Checksum, Src/Dst. TCP 20 Byte: Ports, Seq, Ack, Offset, Flags, Window, Checksum. UDP 8 Byte: Ports, Länge, Prüfsumme.

**I4. Three-Way-Handshake mit Nummern**
> SYN Seq=A → SYN/ACK Seq=B Ack=A+1 → ACK Seq=A+1 Ack=B+1

**I5. Seq/Ack-Regel**
> Ack = Seq + Len (SYN/FIN = +1); bestätigt kumulativ alles davor.

**I6. 6 TCP-Flags**
> SYN (Aufbau), ACK (Bestätigung), FIN (geordneter Abbau), RST (Abbruch / geschlossener Port), PSH (sofort weiterreichen), URG (dringend)

**I7. Subnetting-Formeln**
> Hosts = 2^(32−n) − 2; Blockgröße = 256 − Maskenoktett; Netz = IP AND Maske; gleiches Netz, wenn IP1 AND M = IP2 AND M.

**I8. Private Adressen, Loopback, APIPA**
> 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 · 127.0.0.0/8 · 169.254.0.0/16

**I9. DNS, DHCP, ARP, ICMP: Zweck, Port/Schicht, Schwäche**
> DNS: Name→IP, UDP/TCP 53, ungesichert → Spoofing · DHCP: automatische Konfiguration, UDP 67/68, keine Authentisierung → Rogue DHCP, Starvation · ARP: IP→MAC im LAN, keine Authentisierung → ARP-Spoofing/MitM · ICMP: Fehler/Diagnose, Schicht 3, ungesichert → Ausspähen, DoS

**I10. Befehle**
> ping (ICMP Echo, Erreichbarkeit) · traceroute (TTL 1,2,3…, ICMP Time Exceeded, Pfad) · netstat -ano (Verbindungen + PID) · ipconfig /all, ifconfig (Konfiguration) · arp -a (ARP-Cache)

**I11. Antworten beim Portscan**
> SYN/ACK = offen · RST = geschlossen · keine Antwort = gefiltert

Если на всё ответил без подсказки, глава закрыта, переходим к `03_Angriffe.md`.
