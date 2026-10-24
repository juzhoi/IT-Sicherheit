# Kapitel 4 — Anonymität im Datenverkehr
**Skript S. 99–118 (PDF S. 105–124)** · dazu **Praktikum 03 (Websicherheit + Darkweb)**

> **О чём глава.** Как тебя опознают в интернете (IP, Cookies, Fingerprinting, Tracking) и как от этого прячутся: Proxy, **TOR (Onion-Routing)**, Hidden Services, альтернативы (JonDo, I2P). Плюс где у TOR слабые места и что говорит закон.
>
> **Почему важно для оценки 1,0:** глава короткая и очень «вопросно-ориентированная» — 5 контрольных вопросов в скрипте почти наверняка всплывут на экзамене (что анонимизируется, сколько узлов, какой узел прослушивается, как деанонимизируют). Темы на соответствие: техники идентификации и узлы TOR.
>
> **Связь с другими главами:** анонимизация = одно из Schutzziele ([01 → 1.3.6](01_Einfuehrung.md)); Proxy как MitM ([03 → 3.3.3](03_Angriffe.md)); шифрование для слоёв (TLS) понадобится полнее в [06](06_Kryptografie.md); DNS-утечки через порт 53 ([02 → 2.12.1](02_Rechnernetze.md)).
>
> **Обозначения:** ⭐ очень вероятно · ⚠️ ловушка · ➕ сверх скрипта · 🔧 из Praktikum.

## Содержание
4.2 Anonymität & Identifizierungstechniken (IP, Cookies, Fingerprinting, Tracking, Geolocation) ⭐ · 4.3 Proxy ⭐ · 4.4 TOR / Onion-Routing ⭐ · 4.5 Hidden Services (.onion) ⭐ · 4.6 Vor-/Nachteile, DNS-Leak, Exit-Node-Überwachung ⭐ · 4.7 Recht (TMG §8) · 4.8 Alternativen (JonDo/Mix-Kaskade, I2P) · 4.9 Fazit · **Aufgaben A–E**

---

## 4.2 Anonymität und Identifizierungstechniken (S. 99–103) ⭐

> **Anonymität** (Def. aus [01 → 1.3.6](01_Einfuehrung.md)): *Eigenschaft eines Systems, dass es anderen nur mit **unverhältnismäßig großem Aufwand** möglich ist, auf die Identität eines Subjekts beim Datenaustausch zu schließen.* Oft: Feststellung der tatsächlichen Identität praktisch **nicht möglich**.

- Hintergrund: Datensammelwut der Konzerne + staatliche Zensur → Wunsch nach Schutz der **informationellen Selbstbestimmung** (Grundrecht).
- **Wichtigstes Identifizierungsmerkmal: die IP-Adresse** — darunter sind Rechner und Nutzer jederzeit für die Gegenseite identifizierbar; sie ist eine **personenbezogene Information** (Datenschutz). Zweiter Aspekt: **Nichtsichtbarkeit genutzter Dienste** gegenüber ISP/Nachrichtendienst.

**Techniken, mit denen Betreiber Nutzer identifizieren ⭐ (Zuordnung!):**
| Technik | Prinzip | Besonderheit |
|---|---|---|
| **IP-Adresse** | eindeutige Kennung des Rechners | **Whois** → ISP; **Geolocation** → Stadt/Stadtteil; für regionale Werbung, Preisdiskriminierung, Bonitätsschätzung |
| **Cookies** | kleine Info im Browser-Cache, beim Seitenbesuch gesetzt | ~90 % der Webseiten (Berkeley 2012); problematisch, sobald die **Cookie-ID mit einem Account** verknüpft wird (z. B. Google/Android) → Profil wird einer Person zuordenbar; **Do-not-track** reduziert Akzeptanz |
| **Browser-Fingerprinting** | Skript liest **Systemeinstellungen** aus: Browser-ID, OS, Bildschirmauflösung, Farbtiefe, Plugins, Schriftarten, DNT-Parameter | ergibt ein **individuelles Profil** ohne Cookie; Test: EFF **Panopticlick** |
| **Web-Tracking** | mitgeladene **JavaScript-Programme** melden beim Seitenaufruf Infos an eine feste Adresse | z. B. **Google Analytics**; Fraunhofer SIT: Tracker auf 1100 Seiten gleichzeitig; Schutz: JavaScript deaktivieren |

⚠️ **Kernbotschaft (Fazit vorweggenommen):** IP-Verschleierung (z. B. TOR) **allein reicht nicht** — über **Cookies, Fingerprinting, aktive Inhalte oder Login** kommt die Gegenseite trotzdem an die Identität.

**Technische Anonymisierung** erreicht man, indem man **Zwischenstationen (Proxies)** zwischen Client und Server schaltet → verschleiert die IP. Auch Kriminelle (**Darkweb/Darknet**) nutzen dieselben Techniken.

---

## 4.3 Anonymisierung der IP-Adresse — Proxy-Server (S. 104–105) ⭐

**Ziel:** Gegenseite/Beobachter bekommt **nicht die echte IP**, sondern die eines **Stellvertreters** → Rückverfolgung endet beim Proxy (Sackgasse).

**Funktionsweise eines Proxys (Abb. 4.3):**
- A will Nachricht M an B senden. Statt direkt → M geht an den **Proxy**. Der analysiert das Paket, erstellt eine **eigene Anfrage M′** (Absender = Proxy!) und sendet sie an B. Antwort N: B → Proxy → A.
- **Absender und Empfänger ändern sich** an jedem Proxy. B kann **nicht feststellen, wer ursprünglich** angefragt hat.
- Heißt **socket-basiert**: zwischen Proxy und je einem Teilnehmer ein Socket → **für einen Proxy zwei Socket-Paare**.

**Eigenschaften und Nachteile:**
- **HTTP-Proxy**: anonymisiert Web-Traffic; kann oft Anfragen **zwischenspeichern (Caching)** → weniger Last am Ziel.
- **Transparenter Proxy:** von den Teilnehmern **unbemerkt** dazwischengeschaltet → sie glauben, direkt zu kommunizieren. Diese Rolle ist ein **Man-in-the-Middle** (vgl. [03](03_Angriffe.md)).
- ⚠️ **Nachteile:** Proxy leitet meist nur Web-Traffic weiter und — ohne SSL — **unverschlüsselt**. Dem oft **unbekannten Betreiber** muss man **vertrauen**: als MitM kann er alles **mitlesen**.
- **Proxychain / Proxy-Kaskade:** Verbindung über **mehrere** Proxies → bessere Anonymität. Der bekannteste solche Dienst ist **TOR**.

---

## 4.4 TOR — The Onion Router (S. 106–109) ⭐

**Grundidee.** TOR = **Echtzeit-Anonymisierungsdienst**. Datenpakete werden **nie direkt** ausgetauscht, sondern über **mehrere nicht-kooperierende Zwischenknoten (Relays/Mixe)** geleitet. Jeder Knoten ersetzt die **Absender-IP durch seine eigene** → die echte IP wird verschleiert.
- Netzwerk: > **8000 TOR-Knoten** weltweit. Ursprünglich vom **US Naval Research Laboratory** gefördert, dann **EFF**-Projekt; **kostenlos, gemeinnützig**.
- Zugang über den **TOR-Browser** (angepasste Mozilla/Firefox-Engine): holt beim Start vom **Verzeichnisserver** die Node-Liste und baut eine **zufällige Route**.

**Eine Route = genau 3 Knoten ⭐:**
| Knoten | Rolle |
|---|---|
| **Entry- / Guard-Node** | Eingang ins TOR-Netz; kennt die echte IP des Nutzers |
| **Relay- / Middle-Node** | Zwischenknoten |
| **Exit-Node** | verlässt das TOR-Netz, stellt die Anfrage an das Ziel; aus Sicht des Ziels kommt der Zugriff **von hier** |

**Verschlüsselung — das „Zwiebel"-Prinzip (Onion) ⭐:** Der Client verschlüsselt das Paket **in mehreren Schichten**, und zwar **von innen nach außen** (Reihenfolge wichtig!):
1. zuerst mit dem **öffentlichen Schlüssel des Exit-Nodes** (inkl. Zieladresse),
2. dann mit dem des **Relay-Nodes**,
3. zuletzt mit dem des **Entry-Nodes**.

Beim Durchlauf „schält" jeder Knoten **eine Schicht** ab (entschlüsselt mit seinem privaten Schlüssel) und sieht nur die **nächste Station**. → **Jeder Knoten kennt nur Vorgänger und Nachfolger**, nie den ganzen Weg. (Jeder Knoten stellt seinen öffentlichen Schlüssel per **Zertifikat** bereit — asymmetrische Krypto, [06](06_Kryptografie.md).)

⚠️ **Wichtigste Falle:** Die Verbindungen **innerhalb** des TOR-Netzes sind verschlüsselt (grüne Pfeile), **aber die Strecke Exit-Node → Zielserver ist UNVERSCHLÜSSELT** — es sei denn, man erzwingt zusätzlich **HTTPS**. → Der Exit-Node-Betreiber kann den Klartext mitlesen/manipulieren.

**Nutzen:** anonymes Surfen, **Zensur/Firewalls umgehen** (Dissidenten in Überwachungsstaaten). Deanonymisierung bräuchte die Überwachung **großer Teile** des Netzes → nahezu unmöglich.

---

## 4.5 Hidden Services (.onion) (S. 110–111) ⭐
- **Hidden Service** = Dienst/Webseite, die **nur über TOR** erreichbar ist → anonyme Inhalte, **das TOR-Netz wird nicht verlassen** (daher **kein unverschlüsselter Exit**).
- Erkennbar an der URL mit Endung **`.onion`**; die kryptische Adresse ist zugleich ein **Hashwert** eines Schlüsselsystems → stellt sicher, dass man wirklich mit dem richtigen Dienst verbunden ist. Auch der **Betreiber** ist schwer zu ermitteln.
- Beispiele: **Silk Road** (abgeschalteter Drogenmarktplatz), **Wikileaks** (Submission-Plattform), Facebook, Suchmaschinen (DuckDuckGo, grams).

**Wie man einen Hidden Service findet (Abb. 4.7):**
- Nutzer fragt einen Verzeichnisserver an — **HSDir (Hidden Service Directory)**.
- Zuständigkeit per **Hash-Berechnung** (enthält das **aktuelle Datum**) bestimmt: die 3 Server, deren Hashes dem Service-Hash folgen, sind zuständig → über zwei Berechnungswege **6 Server**; der Client wählt einen zufällig.
- Durch das **Datum** wechseln die zuständigen Server **täglich**.
- Dann wird ein **Rendezvous-Punkt** ausgehandelt — ein vom Client gewählter TOR-Knoten, über den beide Seiten kommunizieren (die im Schema als **Introduction Points** markierten Server vermitteln den Kontakt).
- ⚠️ **Angriff auf Hidden Services über die Verzeichnisdienste ist deutlich einfacher** als die Deanonymisierung normaler Nutzer.
- Darkweb-Marktplätze: volle E-Commerce-Lösungen, Zahlung in **Bitcoin**; Problem für Betreiber: schwer **Reputation** aufzubauen → viele **betrügerisch nachgeahmte** .onion-Namen (keine Domain-Kontrolle).

---

## 4.6 Vor- und Nachteile von TOR (S. 112–113) ⭐

| Vorteile | Nachteile |
|---|---|
| verschleiert die IP, hohe Anonymität | **langsamer** (Umweg über mehrere Knoten) |
| Zensur/Firewalls umgehen | **Exit-Node unverschlüsselt** → Betreiber kann mitlesen/manipulieren (außer bei HTTPS) |
| kostenlos, ausgereift, EFF-empfohlen | **Deanonymisierung** möglich, wenn eine Partei **Entry- UND Exit-Node** kontrolliert |
| Hidden Services, .onion | IP-Verschleierung nutzlos bei Cookies/Fingerprinting/aktiven Inhalten/Login |
| | nur für **versierte Nutzer**; Hidden Services über Verzeichnisdienste angreifbar |

**Deanonymisierung (Prinzip):** Angreifer beobachtet **Eingangs- und Ausgangspunkt** und **vergleicht** den Verkehr (Timing/Volumen) → **Correlation-/End-to-End-Confirmation-Angriff ➕**. Braucht Kontrolle über viele Exit-Nodes bzw. deren Traffic; mit hohem technischem/finanziellem Aufwand schon gelungen.

### 4.6.1 Problembereich DNS-Auflösung ⭐
- Ruft der Browser `www.google.de` auf, entsteht ein **DNS-Request** ([02 → 2.12.1](02_Rechnernetze.md)).
- **Früher** gingen DNS-Anfragen **nicht** über den TOR-Proxy-Port → ein Beobachter konnte sehen, **welche Seite** der Nutzer als Nächstes besuchen wollte = **DNS-Leak**.
- **Heute** standardmäßig über den **SOCKS-Proxy** verschlüsselt ins TOR-Netz. ⚠️ Aber **Browser-Erweiterungen** mit vollem Rechnerzugriff können **eigene DNS-Anfragen an TOR vorbei** stellen.
- **🔧 Leak-Test:** mit Wireshark einen Filter auf **DNS-Port 53** setzen — läuft man anonym und ohne Hintergrunddienste, dürfen dort **keine** Anfragen auftauchen.

### 4.6.2 Überwachung des Datenverkehrs ⭐
- Interessant ist der **Exit-Node**: alle anderen Strecken sind verschlüsselt.
- Am **Entry-Node** sieht ein Lauscher nur, **dass** der Nutzer mit einem TOR-Knoten spricht — **nicht** Inhalt oder Ziel.
- Erst am **Exit-Node**, wo das Paket TOR verlässt, ist die **eigentliche Anfrage sichtbar** (z. B. Mitschnitt von HTTP/POP3/IMAP). Ein Großteil der Exit-Nodes wird vermutlich von staatlichen Stellen überwacht → stellt den Wert der Anonymität teils infrage.
- ⚠️ **Wer** die Anfrage stellt, verrät das i. d. R. **nicht** — außer der Inhalt verrät den Nutzer **oder** dieselbe Partei kontrolliert auch den **Entry-Node**.

---

## 4.7 Rechtlicher Hintergrund (S. 114–115)
- Der **Exit-Node-Betreiber ist nicht anonym** → bei Straftaten Dritter über TOR fragt die Polizei zuerst bei ihm nach (ggf. Beschlagnahmung, Verdächtigung).
- **Schutz: §8 TMG (Telemediengesetz) — „Durchleitung von Informationen":** Diensteanbieter sind für **fremde** durchgeleitete Informationen **nicht verantwortlich**, sofern sie (1) die Übermittlung **nicht veranlasst**, (2) den **Adressaten nicht ausgewählt**, (3) die Informationen **nicht ausgewählt/verändert** haben. Gilt in Zivil- und Strafrecht.
- Ausnahme: wenn der Anbieter **absichtlich** mit einem Nutzer zu rechtswidrigen Zwecken zusammenarbeitet.
- In Deutschland wurde **noch nie** ein Exit-Node-Betreiber verurteilt (in Österreich schon — Beihilfe).

---

## 4.8 Alternative Anonymisierungsdienste (S. 115–116) ⭐

### JonDo — Mix-Kaskade (David Chaum, 1981)
- Braucht eigene Client-Software, basiert wie TOR auf dem **SOCKS-Protokoll**.
- **Mix-Kaskade:** 2–3 **Mix-Server**; Prinzip = **Sammeln und Mischen** von Nachrichten (gegen Verkehrsanalyse). **Mehrfachverschlüsselung** schützt vor den Betreibern.
- Unterschied zu TOR: **feste Kaskade** mit **fest definierten, authentifizierten** Servern (TOR = zufällige Route aus vielen Knoten). **Mix-Pakete** sind **gleich lang (998 Byte)** → erschwert statistische Analyse. Jeder Knoten kennt nur Vorgänger/Nachfolger.

### I2P — Invisible Internet Project
- **Overlay-Netzwerk** auf Anwendungsebene; **Peer-to-Peer**: jeder Teilnehmer ist **Client UND Server** → Anonymität hängt von der **Nutzerzahl** ab.
- **Geschlossenes** Netzwerk: Daten werden **nicht** an öffentliche Server durchgeleitet; nur **interne** Adressen ansprechbar; Dienste = **Eepsites** (anonyme Blogs/Seiten).
- **Sicherer/zensurresistenter als TOR**, weil **vollständig dezentral** (TOR braucht zentrale Directory-Server). Nachteil: Client muss mindestens eine Adresse im Netz vorab kennen (aus publizierten Listen).

| | **TOR** | **JonDo (Mix-Kaskade)** | **I2P** |
|---|---|---|---|
| Route | **zufällig**, 3 Knoten aus >8000 | **feste** Kaskade, 2–3 authentifizierte Mixe | P2P, jeder ist Knoten |
| Architektur | zentraler **Directory-Server** | feste Mix-Server | **dezentral** |
| offen/geschlossen | offen (Exit ins normale Web) | offen | **geschlossen** (nur intern, Eepsites) |
| Besonderheit | Hidden Services, .onion | gleich lange Pakete (998 B), Mischen | sicherer, aber kleineres Netz |

---

## 4.9 Fazit (S. 117) ⭐
- Anonymität gibt es **nicht zum Nulltarif** → kostet v. a. **Geschwindigkeit**. Je aufwendiger die IP-Verschleierung, desto langsamer.
- TOR ist für **versierte** Nutzer, die die Schwachstellen kennen.
- **IP-Verschleierung hilft nichts**, wenn die Gegenseite über **aktive Inhalte, Cookies oder Browser-Fingerprints** an die Identität kommt → Anonymität ist immer ein **Gesamtpaket** aus Technik und Nutzerverhalten.

---

# Aufgaben — типичные и нетипичные
**[T]** типичная · **[N]** перенос · **[1,0]** высший балл. Решения сразу под задачей.

## A. Kontrollfragen aus dem Skript (S. 117) [T] ⭐

**A1. Welches Identifikationsmerkmal wird bei TOR anonymisiert?**
> **✅ Lösung:** Die **IP-Adresse** des Nutzers (das wichtigste Identifizierungsmerkmal im Internet). Jeder TOR-Knoten ersetzt die Absender-IP durch seine eigene, sodass das Ziel nur die IP des **Exit-Nodes** sieht. ⚠️ Andere Merkmale (Cookies, Browser-Fingerprint, Login-Daten) werden **nicht** anonymisiert.

**A2. Wie viele Rechner sind i. d. R. am Aufbau einer Route beteiligt?**
> **✅ Lösung:** **Drei** TOR-Knoten: **Entry-/Guard-Node → Relay-/Middle-Node → Exit-Node** (Proxy-Kaskade aus 3 Relays).

**A3. Nennen Sie mindestens ein weiteres Verfahren zum anonymen Surfen.**
> **✅ Lösung:** **JonDo** (Mix-Kaskade nach Chaum) oder **I2P** (Peer-to-Peer-Overlay); allgemein auch einfache **Proxys / Proxychains** oder ein **VPN** ➕.

**A4. Unter welchen Umständen kann die Anonymität trotz TOR aufgedeckt werden? (mind. zwei)**
> **✅ Lösung (mehrere nennen):**
> 1. **Entry- UND Exit-Node werden von derselben Partei kontrolliert** → Verkehrsvergleich (Timing/Volumen) → Deanonymisierung (Correlation-Angriff).
> 2. **Exit-Node-Überwachung** + der Nutzer **verrät sich durch den Inhalt** der (unverschlüsselten) Daten.
> 3. **DNS-Leak** / Browser-Erweiterungen stellen Anfragen an TOR vorbei.
> 4. **Cookies, Browser-Fingerprinting, aktive Inhalte (JavaScript)** identifizieren den Nutzer unabhängig von der IP.
> 5. **Login** bei einem Dienst (dann ist die IP zweitrangig, vgl. Facebook).

**A5. Welcher Teil einer TOR-Route kann am besten abgehört werden?**
> **✅ Lösung:** Der **Exit-Node**. Nur die Strecke Exit-Node → Ziel ist **unverschlüsselt** (außer bei HTTPS); alle Verbindungen innerhalb des Netzes sind verschlüsselt. Am Entry-Node sieht man nur, *dass* jemand TOR nutzt, nicht *was*.

## B. Zuordnung [T]
**B1. Identifizierungstechnik → Beschreibung.**
| | Technik | | Beschreibung |
|---|---|---|---|
|1|Cookie|a|Skript liest Systemeinstellungen (OS, Auflösung, Plugins) aus|
|2|Browser-Fingerprinting|b|von IP-Adresse auf Stadt/Region schließen|
|3|Web-Tracking|c|kleine Info im Browser-Cache, beim Besuch gesetzt|
|4|Geolocation|d|mitgeladene JavaScripts melden Infos an feste Adresse|
|5|Whois|e|Abfrage, die zum ISP einer IP führt|
> **✅ Lösung:** 1c · 2a · 3d · 4b · 5e

**B2. TOR-Knoten → Eigenschaft.**
| | Knoten | | Eigenschaft |
|---|---|---|---|
|1|Entry/Guard|a|Verbindung zum Ziel, unverschlüsselt, am besten abhörbar|
|2|Relay/Middle|b|kennt die echte IP des Nutzers|
|3|Exit|c|reiner Zwischenknoten, kennt nur Vor-/Nachfolger|
> **✅ Lösung:** 1b · 2c · 3a

**B3. Dienst → Merkmal.** TOR · JonDo · I2P
Merkmale: zufällige Route aus >8000 Knoten · feste Kaskade mit gleich langen 998-Byte-Paketen · dezentrales P2P, geschlossenes Netz mit Eepsites
> **✅ Lösung:** TOR → zufällige Route · JonDo → feste Kaskade, 998-Byte-Pakete · I2P → dezentrales P2P, Eepsites.

## C. Richtig oder falsch? (mit Begründung) [T/N]
1. Mit TOR ist der gesamte Weg vom Nutzer bis zum Zielserver verschlüsselt.
2. Eine TOR-Route besteht normalerweise aus drei Knoten.
3. Ein einzelner Proxy bietet dieselbe Anonymität wie TOR.
4. Der Exit-Node kennt die echte IP-Adresse des Nutzers.
5. Hidden Services sind an der Endung .onion erkennbar und verlassen das TOR-Netz nicht.
6. IP-Verschleierung macht einen Nutzer vollständig anonym.
7. In Deutschland haftet ein Exit-Node-Betreiber grundsätzlich für fremde Straftaten über seinen Knoten.
8. I2P ist ein zentral organisiertes Netzwerk mit Directory-Servern.
9. Ein transparenter Proxy ist eine Form des Man-in-the-Middle.
10. Jeder TOR-Knoten kennt die komplette Route.
> **✅ Lösung:**
> 1. **Falsch** — die Strecke **Exit-Node → Ziel** ist unverschlüsselt (außer HTTPS).
> 2. **Richtig** — Entry, Relay, Exit.
> 3. **Falsch** — ein Proxy ist ein einzelner, oft unbekannter Vertrauenspunkt (MitM-Risiko); TOR = Kaskade aus 3 nicht-kooperierenden Knoten mit Mehrfachverschlüsselung.
> 4. **Falsch** — nur der **Entry/Guard-Node** kennt die echte IP; der Exit kennt sie nicht.
> 5. **Richtig.**
> 6. **Falsch** — Cookies, Fingerprinting, aktive Inhalte, Login können identifizieren.
> 7. **Falsch** — **§8 TMG**: nicht verantwortlich für fremde durchgeleitete Informationen (nie verurteilt in D).
> 8. **Falsch** — I2P ist **dezentral/P2P**; TOR hat die zentralen Directory-Server.
> 9. **Richtig** — unbemerkt dazwischengeschaltet, liest/leitet weiter.
> 10. **Falsch** — jeder Knoten kennt nur **Vorgänger und Nachfolger**.

## D. Offene Fragen und Transfer [N] [1,0]

**D1. Erklären Sie das Onion-Prinzip Schritt für Schritt und warum dadurch kein einzelner Knoten die ganze Verbindung kennt.**
> **✅ Lösung:** Der Client verschlüsselt das Paket in drei Schichten **von innen nach außen**: zuerst mit dem öffentlichen Schlüssel des **Exit-Nodes** (inkl. Zieladresse), dann des **Relay-Nodes**, zuletzt des **Entry-Nodes**. Jeder Knoten entschlüsselt mit seinem privaten Schlüssel **genau eine Schicht** und erfährt nur die **nächste Station**. Der Entry-Node kennt den Nutzer, aber nicht das Ziel; der Exit-Node kennt das Ziel, aber nicht den Nutzer; der Relay-Node kennt beide nicht. So kann kein einzelner Knoten Absender und Ziel gleichzeitig zuordnen — Anonymität entsteht aus der **Trennung des Wissens**.

**D2. [1,0] Ein Nutzer surft über TOR und loggt sich dabei in sein E-Mail-Konto ein und meldet, er fühle sich „völlig anonym". Nehmen Sie Stellung.**
> **✅ Lösung:** Falsch. TOR verschleiert nur die **IP-Adresse**. Durch den **Login** gibt der Nutzer seine Identität **direkt preis** — der Dienst kennt ihn namentlich, die IP ist zweitrangig (vgl. Facebook im Skript). Zusätzlich können **Cookies, Browser-Fingerprinting und aktive Inhalte** ihn wiedererkennen. Ist die Verbindung zum Dienst nicht **HTTPS**, kann außerdem der **Exit-Node** den gesamten Klartext inkl. Zugangsdaten mitlesen. Anonymität ist ein Gesamtpaket aus Technik **und** Verhalten.

**D3. Warum ist gerade der Exit-Node sowohl rechtlich als auch sicherheitstechnisch der kritische Punkt?**
> **✅ Lösung:** **Sicherheitstechnisch:** Die Strecke Exit-Node → Ziel ist **unverschlüsselt** → der Betreiber kann den Klartext **mitlesen und manipulieren** (außer bei HTTPS); viele Exit-Nodes werden vermutlich staatlich überwacht. **Rechtlich:** Aus Sicht des Ziels kommt der Verkehr **vom Exit-Node** → bei Straftaten Dritter gerät dessen (nicht anonymer) Betreiber ins Visier der Ermittler. Geschützt wird er in Deutschland durch **§8 TMG** (keine Haftung für fremde durchgeleitete Informationen).

**D4. [N] Vergleichen Sie Proxy, TOR und I2P hinsichtlich Vertrauen, Architektur und Anwendungsfall.**
> **✅ Lösung:** **Einzelner Proxy:** ein Vertrauenspunkt (der Betreiber kann als MitM alles mitlesen), minimale Anonymität, schnell — Anwendung: einfache IP-Verschleierung, Geo-Sperren umgehen. **TOR:** Vertrauen auf **viele nicht-kooperierende** Knoten verteilt, zentrale Directory-Server, zufällige 3-Knoten-Route, Exit ins offene Web + Hidden Services — Anwendung: anonymes Surfen, Zensurumgehung. **I2P:** vollständig **dezentral/P2P**, jeder ist Knoten, **geschlossenes** Netz (Eepsites), kein Exit — Anwendung: anonyme interne Dienste, höhere Zensurresistenz, aber kleineres Netz. Grundregel: mehr Anonymität → mehr Knoten/Verschlüsselung → **langsamer**.

## T. Transfer & Verständnis — Skript anwenden ⭐

**T1 [1,0]. Ein Journalist nutzt TOR, um anonym eine Datei auf eine `https://`-Plattform hochzuladen. An welchen Stellen ist er geschützt, an welchen trotzdem angreifbar? Gehen Sie die Route durch.**
> **✅ Lösungsweg:** **Geschützt:** innerhalb des TOR-Netzes (3 Knoten) ist alles mehrfach verschlüsselt; der Zielserver sieht nur die IP des **Exit-Nodes**, nicht seine echte. Da die Plattform **HTTPS** nutzt, kann **auch der Exit-Node den Inhalt nicht lesen** (sonst wäre der Exit-Node die kritische Stelle). **Angreifbar:** (a) wenn eine Partei **Entry- UND Exit-Node** kontrolliert → Korrelationsangriff; (b) **Browser-Fingerprinting / Cookies / aktive Inhalte** identifizieren ihn unabhängig von der IP; (c) **Metadaten in der Datei** (EXIF, Autor) verraten ihn; (d) wenn er sich **einloggt**, ist die Anonymität weg. Kern: TOR + HTTPS decken IP und Transport, aber **nicht Verhalten und Inhalt**.

**T2 [1,0]. Warum ist der Exit-Node sowohl das größte Sicherheitsrisiko für den Nutzer als auch das größte rechtliche Risiko für den Betreiber? Ein Zusammenhang, zwei Perspektiven.**
> **✅ Lösungsweg:** Beides folgt aus **derselben Tatsache**: am Exit-Node **verlässt der Verkehr das TOR-Netz und wird (ohne HTTPS) wieder Klartext**, und aus Sicht des Ziels **stammt er vom Exit-Node**. Für den **Nutzer**: der Exit-Betreiber (oder ein Überwacher dort) kann Inhalte **mitlesen/manipulieren** → Vertraulichkeit/Integrität. Für den **Betreiber**: Straftaten Dritter erscheinen unter **seiner** IP → Ermittler klopfen bei ihm an (geschützt durch **§ 8 TMG**, keine Haftung für fremde Durchleitung). Eine technische Eigenschaft, zwei Risiken.

**T3 [N]. Vergleichen Sie TOR und JonDo (Mix-Kaskade) hinsichtlich der Frage „Wem muss ich vertrauen?" — und warum macht I2P es nochmal anders?**
> **✅ Lösungsweg:** **TOR:** Vertrauen auf **viele, zufällig gewählte, nicht-kooperierende** Knoten verteilt — kein einzelner kennt Anfang+Ende; Schwäche: zentrale **Directory-Server**. **JonDo:** **feste Kaskade** aus wenigen, **authentifizierten** Mix-Servern → man muss genau diesen Betreibern vertrauen (kleine, bekannte Gruppe — auch rechtlich angreifbarer). **I2P:** **vollständig dezentral/P2P**, jeder Teilnehmer ist selbst Knoten → kein zentraler Vertrauenspunkt, Anonymität skaliert mit der Nutzerzahl; dafür geschlossen (nur interne Eepsites). Pointe: mehr Dezentralität = weniger einzelne Vertrauenspunkte, aber kleineres Netz/langsamer.

## E. Самопроверка перед Kap. 5 (с ответами)
**E1. Wichtigstes Identifizierungsmerkmal + 3 weitere Techniken?** IP-Adresse; dazu Cookies, Browser-Fingerprinting, Web-Tracking (+ Geolocation/Whois).
**E2. Was ist ein Proxy, was eine Proxy-Kaskade?** Vermittler, der die Absender-IP ersetzt; Kaskade = mehrere Proxies hintereinander.
**E3. TOR-Route: wie viele und welche Knoten?** 3: Entry/Guard, Relay/Middle, Exit.
**E4. Onion-Verschlüsselung — Reihenfolge?** Von innen nach außen: Exit, dann Relay, dann Entry; jeder schält eine Schicht.
**E5. Welcher Knoten kennt was?** Entry → echte IP; Exit → Ziel + unverschlüsselt; keiner die ganze Route.
**E6. .onion / Hidden Service — Besonderheit?** Nur über TOR erreichbar, verlässt das Netz nicht, Adresse = Hash, HSDir + Rendezvous-Punkt.
**E7. Deanonymisierung (2 Wege)?** Entry+Exit kontrollieren (Correlation); Identität über Cookies/Fingerprint/Login/DNS-Leak.
**E8. Rechtsgrundlage für Exit-Node-Betreiber?** §8 TMG — keine Haftung für fremde Durchleitung.
**E9. Kernaussage des Fazits?** Anonymität kostet Geschwindigkeit; IP-Verschleierung allein reicht nicht.

Если на всё ответил без подсказки — переходим к `05_Malware.md`.
