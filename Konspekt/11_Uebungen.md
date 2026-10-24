# Probeklausur IT-Sicherheit — alle Themen gemischt
**Pробный экзамен · все 3 типа заданий + расчёты**

> **Как пользоваться.** Это тренировочный экзамен по всем главам, со всеми тремя типами заданий, которые будут на реальном: **анализ логов**, **открытые вопросы (Freitext)**, **соответствия (1a 2b 3c)** + **расчёты**. Условия идут сверху с баллами, **решения — в самом конце** (раздел «Lösungen»), чтобы можно было честно прорешать на время и потом сверить.
>
> **Режим тренировки:** 90 минут, открытый конспект + интернет (как на реальном), **без ИИ**. Цель для оценки 1,0–2,0: **≥ 80 %** баллов.
> Всего **100 Punkte**. Распределение времени ≈ 1 минута на балл + 10 минут на проверку.
>
> 🔗 При затруднении — ссылка на главу рядом с заданием, но сначала попробуй без неё.

---

# TEIL 1 — Zuordnung (1a 2b 3c) · 20 P

### A1. Schutzziele (5 P) 🔗01
Ordnen Sie jedem Szenario das **primär** verletzte Schutzziel zu (Vertraulichkeit / Integrität / Verfügbarkeit / Verbindlichkeit / Authentizität).
| # | Szenario |
|---|---|
| 1 | Ransomware verschlüsselt die Dateien eines Krankenhauses. |
| 2 | Ein Angreifer ändert unbemerkt die IBAN in einer Online-Rechnung. |
| 3 | Ein Kollege liest mit Wireshark unverschlüsselte Logins mit. |
| 4 | Eine Mail gibt vor, von der Hausbank zu stammen. |
| 5 | Ein Kunde bestreitet nachträglich seine Online-Bestellung. |

### A2. Angriff → OSI-Schicht (4 P) 🔗03
ARP-Spoofing · SYN-Flooding · SQL-Injection · ICMP-Flood
Schichten: 2 · 3 · 4 · 7

### A3. Malware-Typ → Beschreibung (5 P) 🔗05
| # | Typ | | Beschreibung |
|---|---|---|---|
| 1 | Wurm | a | tarnt sich als nützlich, führt heimlich Payload aus |
| 2 | Trojaner | b | verschlüsselt Daten, fordert Lösegeld |
| 3 | Rootkit | c | eigenständig, verbreitet sich selbst über Netz/Lücken |
| 4 | Ransomware | d | übermittelt heimlich Nutzerdaten an Dritte |
| 5 | Spyware | e | verschleiert Prozesse/Malware vor dem AV |

### A4. Krypto-Begriff → Definition (6 P) 🔗06
| # | Begriff | | Definition |
|---|---|---|---|
| 1 | Diffie-Hellman | a | symmetrische Prüfsumme mit Schlüssel (Integrität + Authentizität) |
| 2 | RSA | b | Schlüsselaustausch, kein Verschlüsseln, kein MitM-Schutz |
| 3 | MAC | c | asymmetrisch, Sicherheit = Faktorisierung von n=p·q |
| 4 | Hashfunktion | d | bindet öffentlichen Schlüssel an Identität, von CA signiert |
| 5 | digitale Signatur | e | beliebige Länge → feste Länge, einweg, kollisionsresistent |
| 6 | Zertifikat | f | privat signieren/öffentlich prüfen → + Verbindlichkeit |

---

# TEIL 2 — Log-Analyse 🦈 · 20 P
🔗 09_Wireshark_Guide.md

### B1. tshark-Mitschnitt (10 P)
```
1  10.0.0.50 → 10.0.0.9  TCP 45000 → 22   [SYN]
2  10.0.0.50 → 10.0.0.9  TCP 45000 → 80   [SYN]
3  10.0.0.50 → 10.0.0.9  TCP 45000 → 443  [SYN]
4  10.0.0.50 → 10.0.0.9  TCP 45000 → 3389 [SYN]
5  10.0.0.9  → 10.0.0.50 TCP 22 → 45000   [SYN, ACK]
6  10.0.0.9  → 10.0.0.50 TCP 80 → 45000   [RST, ACK]
7  10.0.0.9  → 10.0.0.50 TCP 443 → 45000  [SYN, ACK]
8  10.0.0.9  → 10.0.0.50 TCP 3389 → 45000 [RST, ACK]
9  10.0.0.50 → 10.0.0.9  TCP 45000 → 22   [RST]
```
a) Um welchen Vorgang handelt es sich? Nennen Sie zwei Indizien. (4 P)
b) Welche Ports sind offen, welche geschlossen? Welche Dienste laufen darauf? (4 P)
c) Ist das bereits ein Angriff im engeren Sinne? Begründen Sie. (2 P)

### B2. ARP-Mitschnitt (10 P)
```
1  0.000  aa:11:.. → ff:ff:ff:ff:ff:ff  ARP  Who has 192.168.0.1? Tell 192.168.0.40
2  0.001  bb:22:.. → aa:11:..           ARP  192.168.0.1 is at bb:22:..
3  4.500  c0:ff:ee → aa:11:..           ARP  192.168.0.1 is at c0:ff:ee
4  6.500  c0:ff:ee → bb:22:..           ARP  192.168.0.40 is at c0:ff:ee
5  8.500  c0:ff:ee → aa:11:..           ARP  192.168.0.1 is at c0:ff:ee
```
a) Was ist auffällig? (3 P) b) Welcher Angriff, in welche Richtung(en)? (3 P) c) Schutzziel + eine Gegenmaßnahme? (2 P) d) Warum funktioniert dieser Angriff nur im lokalen Netz? (2 P)

---

# TEIL 3 — Rechenaufgaben 🧮 · 25 P

### C1. Subnetting (6 P) 🔗02
Gegeben `172.20.50.130/26`.
a) Subnetzmaske (dezimal)? (1) b) Netzadresse? (1) c) Broadcastadresse? (1) d) Hostbereich? (1) e) Anzahl nutzbarer Hosts? (1) f) Liegt `172.20.50.200` im selben Subnetz? (1)

### C2. TCP Seq/Ack (5 P) 🔗02
Client-ISN = 500, Server-ISN = 9000. Nach dem Handshake sendet der Client 300 Byte, der Server antwortet mit 1000 Byte. Geben Sie Seq und Ack aller Segmente bis einschließlich der Bestätigung der Server-Antwort an.

### C3. RSA (7 P) 🔗06
p = 5, q = 11, gewählt e = 3.
a) n? (1) b) φ(n)? (1) c) Ist e gültig? (1) d) Bestimmen Sie d. (2) e) Verschlüsseln Sie m = 4. (2)

### C4. Risiko & Verfügbarkeit (4 P) 🔗01
a) Schaden 80.000 €, Eintrittswahrscheinlichkeit 15 %/Jahr. Risiko? Eine Maßnahme kostet 9.000 €/Jahr und senkt p auf 3 %. Lohnt sie sich? (2)
b) Ein Server fiel 2025 insgesamt 20 Stunden aus. Berechnen Sie die Verfügbarkeit in %. (2)

### C5. Caesar & Geburtstagsparadoxon (3 P) 🔗06,08
a) Entschlüsseln Sie den Caesar-Text `WHVW` mit Schlüssel k=3. (1)
b) Eine Hashfunktion liefert 80-Bit-Werte. Wie viele Hashwerte braucht man grob für eine 50-%-Kollision, und wie stark ist die Kollisionsresistenz damit? (2)

---

# TEIL 4 — Offene Fragen (Freitext) · 35 P

### D1. (5 P) 🔗01
Erklären Sie den Unterschied zwischen **Gefahr, Bedrohung und Gefährdung** an einem selbst gewählten Beispiel.

### D2. (5 P) 🔗02,03
Beschreiben Sie den Ablauf eines **Man-in-the-Middle-Angriffs per ARP-Spoofing**: Warum ist ARP anfällig, was tut der Angreifer, welche Schutzziele sind betroffen?

### D3. (6 P) 🔗06
a) Worin unterscheiden sich **symmetrische und asymmetrische** Verschlüsselung (je ein Vor-/Nachteil)? (3)
b) Was ist ein **hybrides Verfahren** und warum wird es z. B. in TLS eingesetzt? (3)

### D4. (5 P) 🔗06
Erklären Sie den Unterschied zwischen einem **MAC** und einer **digitalen Signatur**. Welches Schutzziel kann nur die Signatur, und warum?

### D5. (6 P) 🔗07
a) Nennen und erklären Sie die zwei Kernregeln von **Bell-LaPadula**. (3)
b) Ein Subjekt der Stufe `confidential` möchte auf ein `top-secret`-Objekt und auf ein `public`-Objekt zugreifen. Was ist jeweils erlaubt (lesen/schreiben)? (3)

### D6. (4 P) 🔗04
Ein Nutzer surft über TOR, loggt sich aber in sein E-Mail-Konto ein, und meint, er sei „völlig anonym". Nehmen Sie begründet Stellung (mind. zwei Argumente).

### D7. (4 P) 🔗03,06
Warum ist **HTTP mit einem Login-Formular** unsicher, und wie behebt man das? Nennen Sie das betroffene Schutzziel und die konkrete Maßnahme.

---

# TEIL 5 — Transfer / Fallanalyse (Bonus) · 15 P
> Тип заданий «подумай и примени несколько тем сразу» — то, что чаще всего отличает 1,0 от 2,0.

### E1. Fallstudie (9 P)
Eine Firma betreibt einen **Webshop über HTTP** (Port 80). Die Mitarbeiter arbeiten alle unter **einem gemeinsamen Admin-Account**. Der Webserver läuft in **keiner isolierten Umgebung**. Ein Angreifer sitzt im selben WLAN.
a) Nennen Sie **drei** verschiedene Schwachstellen und je das bedrohte Schutzziel. (3)
b) Beschreiben Sie **eine** realistische Angriffskette aus mindestens drei Schritten über zwei Kapitel hinweg. (3)
c) Priorisieren Sie **drei** Gegenmaßnahmen und begründen Sie die Reihenfolge. (3)

### E2. Entscheidung begründen (6 P)
Ein Administrator fragt: „Reicht es, wenn wir alles mit **AES verschlüsseln**? Dann sind wir doch sicher." Nehmen Sie Stellung: Welche Schutzziele deckt AES, welche **nicht**, und welche zwei weiteren Bausteine braucht er mindestens — mit Begründung über die Schutzziele. (6)

---
---

# ✅ LÖSUNGEN

## Teil 1 — Zuordnung
**A1:** 1 **Verfügbarkeit** · 2 **Integrität** · 3 **Vertraulichkeit** · 4 **Authentizität** · 5 **Verbindlichkeit**.
**A2:** ARP-Spoofing → **Schicht 2** · ICMP-Flood → **Schicht 3** · SYN-Flooding → **Schicht 4** · SQL-Injection → **Schicht 7**.
**A3:** 1c · 2a · 3e · 4b · 5d.
**A4:** 1b · 2c · 3a · 4e · 5f · 6d.

## Teil 2 — Log-Analyse
**B1:**
a) **Port-Scan** (SYN-/Half-Open-Scan, nmap -sS). Indizien: **ein Quellhost/-port (45000) auf mehrere Zielports** in Folge; der Scanner **beantwortet SYN/ACK mit RST** (Paket 9) statt den Handshake abzuschließen (stealth); außerdem die hohe Geschwindigkeit.
b) **Offen:** 22 (SSH) und 443 (HTTPS) — Antwort **SYN/ACK**. **Geschlossen:** 80 (HTTP) und 3389 (RDP) — Antwort **RST/ACK**.
c) **Nein, im engeren Sinne kein Angriff** — Port-Scanning ist „Anklopfen", **Angriffsvorbereitung/Reconnaissance**. Es wird nichts verändert oder eingedrungen. (Ohne Auftrag aber rechtlich heikel; dient der Vorbereitung echter Angriffe.)

**B2:**
a) IP **192.168.0.1** (Gateway) erscheint mit **zwei verschiedenen MACs**: echt `bb:22:..` (Paket 2) und `c0:ff:ee` (Pakete 3,5, **unaufgefordert**, wiederholt); zusätzlich fälscht `c0:ff:ee` gegenüber dem Gateway die IP **192.168.0.40** (Paket 4).
b) **ARP-Spoofing / Cache-Poisoning → Man-in-the-Middle**, **in beide Richtungen** (dem Opfer .40 wird die Gateway-IP, dem Gateway die Opfer-IP mit der Angreifer-MAC c0:ff:ee vorgegaukelt).
c) **Vertraulichkeit** (+ Integrität/Verfügbarkeit, falls Pakete verändert/verworfen werden). Maßnahme: **statische ARP-Einträge** (`arp -s`), Dynamic ARP Inspection, TLS gegen die Folgen.
d) Weil **ARP mit Broadcasts auf Schicht 2** arbeitet, die **nicht geroutet** werden; ARP-Caches gelten nur im eigenen Subnetz, und jeder Router schreibt den Ethernet-Header neu. Der Angreifer muss also in derselben Broadcast-Domäne sein.

## Teil 3 — Rechenaufgaben
**C1:** /26 → Blockgröße 256−192 = 64; 130 → abrunden auf 128.
a) **255.255.255.192** · b) Netz **172.20.50.128** · c) Broadcast **172.20.50.191** · d) Hosts **172.20.50.129 – 172.20.50.190** · e) 2⁶ − 2 = **62** · f) 200 > 191 → liegt im nächsten Netz (172.20.50.192) → **nein**.

**C2:**
| # | Richtung | Flags | Seq | Ack | Len |
|---|---|---|---|---|---|
| 1 | C→S | SYN | 500 | — | 0 |
| 2 | S→C | SYN,ACK | 9000 | **501** | 0 |
| 3 | C→S | ACK | 501 | **9001** | 0 |
| 4 | C→S | PSH,ACK | 501 | 9001 | 300 |
| 5 | S→C | ACK | 9001 | **801** | 0 |
| 6 | S→C | PSH,ACK | 9001 | 801 | 1000 |
| 7 | C→S | ACK | 801 | **10001** | 0 |

**C3:** a) n = 5·11 = **55** · b) φ = 4·10 = **40** · c) ggT(3,40) = 1 → **gültig** · d) 3·d ≡ 1 mod 40 → **d = 27** (3·27 = 81 = 80+1 ≡ 1) · e) c = 4³ mod 55 = 64 mod 55 = **9**. (Probe: 9²⁷ mod 55 = 4 ✓.)

**C4:**
a) R = 0,15 · 80.000 = **12.000 €/Jahr**. Mit Maßnahme: R_neu = 0,03 · 80.000 = 2.400 €. Nutzen = 12.000 − 2.400 = **9.600 €/Jahr** > Kosten 9.000 € → **lohnt sich** (Nettonutzen 600 €/Jahr).
b) 2025 ist kein Schaltjahr: 8760 h. V = (8760 − 20)/8760 = 8740/8760 ≈ **0,99772 = 99,772 %**.

**C5:** a) `WHVW` − 3 → **TEST**. b) ~1,2·√(2⁸⁰) ≈ 1,2·2⁴⁰ ≈ **1,3·10¹²** Werte (Größenordnung **2⁴⁰**); die Kollisionsresistenz entspricht damit nur **~40 Bit** (≈ halbe Hashlänge, Geburtstagsparadoxon).

## Teil 4 — Offene Fragen (Musterantworten)
**D1:** **Gefahr** = abstrakte Möglichkeit eines Schadens ohne konkreten Bezug (z. B. „Einbrüche kommen vor"). **Bedrohung** = konkrete, potenzielle Gefahr mit Bezug zu einem Schutzziel (z. B. „eine Ransomware-Gruppe will unsere Daten verschlüsseln" → Verfügbarkeit). **Gefährdung** entsteht, wenn die Bedrohung auf eine **Schwachstelle trifft** und ausnutzbar ist (z. B. die Gruppe + unser **ungepatchter Server**). Ohne Schwachstelle keine Gefährdung.

**D2:** ARP ist **nicht authentifiziert** und viele Systeme übernehmen **unaufgeforderte** ARP-Replies. Der Angreifer sendet gefälschte Replies: dem Opfer „die Gateway-IP gehört zu **meiner** MAC", dem Gateway „die Opfer-IP gehört zu meiner MAC". Beide tragen das in ihren **ARP-Cache** ein → der Verkehr fließt über den Angreifer, der ihn (mit aktiviertem IP-Forwarding) **weiterleitet und dabei mitliest/verändert** (MitM, unbemerkt als Proxy). Betroffen: **Vertraulichkeit** (Mitlesen), bei Manipulation **Integrität**, bei Verwerfen **Verfügbarkeit**. Nur im lokalen Netz möglich.

**D3:**
a) **Symmetrisch:** gleicher Schlüssel; **+** sehr schnell/kurze Schlüssel, **−** Schlüsselaustauschproblem (n²/2 Schlüssel). **Asymmetrisch:** Schlüsselpaar; **+** kein Austauschproblem, ermöglicht Signaturen, **−** langsam, lange Schlüssel.
b) **Hybrid:** man tauscht/erzeugt **asymmetrisch** (bzw. per DH) einen Sitzungsschlüssel und verschlüsselt die eigentlichen Daten **symmetrisch**. In TLS kombiniert das die **Sicherheit/Authentizität** der asymmetrischen Verfahren (Zertifikat, Schlüsselaustausch) mit der **Geschwindigkeit** der symmetrischen (AES für die Nutzdaten).

**D4:** **MAC** ist **symmetrisch** (ein gemeinsamer geheimer Schlüssel), **digitale Signatur** ist **asymmetrisch** (privat signieren, öffentlich verifizieren). Beide liefern **Integrität + Authentizität**. **Nur die Signatur** liefert **Verbindlichkeit/Nichtabstreitbarkeit**, weil beim MAC **beide** Partner denselben Schlüssel kennen — Bob kann einem Dritten (Gericht) nicht beweisen, dass Alice und nicht er selbst den MAC erzeugt hat. Bei der Signatur kann nur der Inhaber des privaten Schlüssels sie erzeugen.

**D5:**
a) **No-Read-Up** (simple security): ein Subjekt darf nur Objekte **≤ seiner** Stufe **lesen** (nichts Höheres/Geheimeres). **No-Write-Down** (*-Property): ein Subjekt darf nur auf Objekte **≥ seiner** Stufe **schreiben** (damit nichts nach unten „durchsickert"). Kurz: read down, write up.
b) `confidential`-Subjekt: **top-secret-Objekt** → **lesen verboten** (no-read-up), **schreiben erlaubt** (write up). **public-Objekt** → **lesen erlaubt** (read down), **schreiben verboten** (no-write-down).

**D6:** Falsch. TOR verschleiert **nur die IP-Adresse**. Durch den **Login** gibt der Nutzer seine Identität direkt preis — der Dienst kennt ihn namentlich, die IP ist zweitrangig. Zusätzlich können **Cookies und Browser-Fingerprinting** ihn wiedererkennen; und ohne **HTTPS** kann der **Exit-Node** den Klartext inkl. Zugangsdaten mitlesen. Anonymität ist ein Gesamtpaket aus Technik und Verhalten.

**D7:** Bei **HTTP** werden Daten **im Klartext** übertragen → jeder, der mitschneiden kann (Sniffing, MitM per ARP-Spoofing), liest **Benutzername und Passwort** mit. Betroffenes Schutzziel: **Vertraulichkeit** (bei GET zusätzlich landen die Daten in Logs/Verlauf). Maßnahme: **HTTPS (TLS)** erzwingen (Port 443, Zertifikat), Login per **POST** statt GET, HTTP→HTTPS umleiten (➕ HSTS).

## Teil 5 — Transfer / Fallanalyse
**E1:**
a) Drei Schwachstellen (Beispiele): **HTTP statt HTTPS** → Vertraulichkeit (Passwörter im Klartext); **gemeinsamer Admin-Account** → Verbindlichkeit/Zurechenbarkeit (keiner weiß, wer was tat) + Verletzung least privilege; **Webserver nicht isoliert** → bei Kompromittierung volle Ausbreitung → alle Schutzziele; (Bonus) **offenes WLAN** → Sniffing/ARP-Spoofing möglich.
b) Angriffskette (Kap. 2/3 + 6): (1) Angreifer macht **ARP-Spoofing** im WLAN → MitM; (2) liest den **HTTP-Login im Klartext** mit (Sniffing) → Admin-Passwort; (3) loggt sich als **Admin** ein; da der Server **nicht isoliert** ist und alle unter Admin laufen, hat er **volle Kontrolle** und kann einen **Backdoor** platzieren (reverse shell :4444). Betroffen am Ende: Vertraulichkeit, Integrität, Verfügbarkeit.
c) Priorisierung (Begründung = größter Risikoabbau pro Aufwand): **1. HTTPS/TLS** erzwingen — stoppt Sniffing/MitM-Login sofort, billig, bricht die ganze Kette an Stufe 2. **2. Getrennte Accounts + least privilege** — begrenzt Schaden und stellt Zurechenbarkeit her. **3. Server isolieren/härten** (eigene VM, minimale Rechte, Monitoring) — begrenzt Ausbreitung. (Man beginnt mit der Maßnahme, die die wahrscheinlichste/billigste Kette unterbricht.)

**E2:** **AES** (symmetrisch) deckt **Vertraulichkeit** der verschlüsselten Daten — und **nur** die. **Nicht** gedeckt: **Integrität/Authentizität** (ein MitM kann Chiffretext verändern; AES allein erkennt das nicht → man braucht **MAC/HMAC oder Signatur**), **Verbindlichkeit** (→ nur **digitale Signatur**, da symmetrischer Schlüssel geteilt ist), und das **Schlüsselaustauschproblem** (wie kommt der AES-Schlüssel sicher zum Partner? → **Diffie-Hellman** oder **RSA**, also asymmetrisch). Mindestens zwei weitere Bausteine: **(1) ein Verfahren zur Integritäts-/Authentizitätssicherung (MAC/Signatur)** und **(2) ein sicherer Schlüsselaustausch (DH/RSA)** — genau das kombiniert ein **hybrides Verfahren wie TLS**. Fazit: „alles mit AES" sichert nur ein Schutzziel, nicht Sicherheit insgesamt.

---

## Bewertung
| Teil | Punkte | erreicht |
|---|---|---|
| 1 Zuordnung | 20 | ___ |
| 2 Log-Analyse | 20 | ___ |
| 3 Rechnen | 25 | ___ |
| 4 Freitext | 35 | ___ |
| 5 Transfer (Bonus) | 15 | ___ |
| **Summe** | **100 (+15)** | ___ |

**≥ 85 → 1,x · 70–84 → 2,x · 55–69 → 3,x** (von 100; Teil 5 als Bonus/Übung für das 1,0-Niveau). Für die Zielnote 1,0–2,0: mind. **80**.
→ Schwache Teile notieren und das zugehörige Kapitel + dessen Aufgaben wiederholen (🔗 in jeder Aufgabe).

---
## Weitere Übungen
Mehr Aufgaben mit Lösungen stehen **am Ende jedes Kapitels** (Abschnitte „Aufgaben"):
[01](01_Einfuehrung.md) Schutzziele/Risiko · [02](02_Rechnernetze.md) Subnetting/Seq-Ack/Logs · [03](03_Angriffe.md) Angriffe/Pentest · [04](04_Anonymitaet_TOR.md) TOR · [05](05_Malware.md) Malware · [06](06_Kryptografie.md) RSA/DH/Hash · [07](07_Zugriffskontrolle.md) BLP/Unix · [08](08_Praktika.md) Praktika · [09](09_Wireshark_Guide.md) 6 Übungslogs.
