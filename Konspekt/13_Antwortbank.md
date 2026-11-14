# 📝 Antwortbank — fertige Musterantworten für Freitext-Fragen
**Для вопросов, которые нужно писать текстом вручную.**

> **Как пользоваться:**
> 1. Найди вопрос через **Cmd+F** (по немецкому термину или по-русски).
> 2. Возьми готовый абзац и **подставь сценарий из задания** — без привязки к сценарию ответ выглядит шаблонным и теряет баллы.
> 3. Схема каждого ответа: **Definition → Erklärung (wie/warum) → Beispiel → Schutzziel/Maßnahme**. Можно сокращать под оператор (Nennen → только определение; Erläutern → весь абзац).
>
> Короткие вопросы — в [12_Schnellzugriff.md](12_Schnellzugriff.md). Операторы и фразы-клише — в [00_Index.md](00_Index.md) §2. ➕ = сверх скрипта.

---

## Kapitel 1 — Grundlagen

**F: Erläutern Sie die drei Schutzziele der CIA-Triade.**
> Die drei Kern-Schutzziele der Informationssicherheit sind **Vertraulichkeit, Integrität und Verfügbarkeit** (CIA). **Vertraulichkeit** bedeutet, dass nur berechtigte Subjekte Informationen lesen können; sie wird z. B. durch Sniffing verletzt und durch Verschlüsselung und Zugriffskontrolle geschützt. **Integrität** bedeutet, dass Daten nicht unbemerkt verändert werden können; sie wird z. B. durch Manipulation einer Überweisungssumme verletzt und durch Hashwerte, MACs und digitale Signaturen geschützt. **Verfügbarkeit** bedeutet, dass ein System funktioniert, wenn berechtigte Nutzer es brauchen; sie wird z. B. durch einen DDoS-Angriff oder Ransomware verletzt und durch Redundanz und Backups geschützt.

**F: Grenzen Sie Safety und Security voneinander ab.**
> Im Deutschen steht ein Wort „Sicherheit" für zwei Begriffe. **Safety (Funktionssicherheit)** schützt die Umwelt **vor** dem System und bezieht sich auf **unbeabsichtigte** Fehler und Ausfälle (verwandt mit Zuverlässigkeit), z. B. dass eine Maschine niemanden verletzt. **Security (Informationssicherheit)** schützt das System **vor** Angreifern und bezieht sich auf **beabsichtigte** Angriffe auf Vertraulichkeit, Integrität und Verfügbarkeit, z. B. Schutz vor einem Hacker. Laut Skript ist Security eine Eigenschaft jedes funktionssicheren Systems, Safety ist also Voraussetzung.

**F: Erläutern Sie den Unterschied zwischen Schwachstelle, Bedrohung und Gefährdung.**
> Eine **Schwachstelle** (vulnerability) ist eine Schwäche oder Lücke in einem System, z. B. eine veraltete Software. Eine **Bedrohung** (threat) ist ein mögliches Ereignis, das eine Schwachstelle ausnutzen könnte, z. B. ein Angreifer mit einem passenden Exploit. Eine **Gefährdung** entsteht, wenn eine Bedrohung auf eine passende Schwachstelle trifft — erst dann besteht konkrete Gefahr. Wird die Gefährdung aktiv ausgenutzt, spricht man von einem **Angriff**.

**F: Was ist ein passiver, was ein aktiver Angriff?**
> Bei einem **passiven Angriff** greift der Angreifer nicht in das System ein, sondern liest nur Daten mit. Da nichts verändert wird, hinterlässt er kaum Spuren und ist schwer zu erkennen; betroffen ist die **Vertraulichkeit**. Beispiel: Mitschneiden unverschlüsselter Logins im WLAN mit Wireshark. Bei einem **aktiven Angriff** verändert, verzögert oder unterbindet der Angreifer Daten; betroffen sind **Integrität** und/oder **Verfügbarkeit**. Beispiel: ein MitM, der Pakete manipuliert, oder ein DoS-Angriff. Schutz bietet durchgängige Verschlüsselung (gegen passiv) sowie Integritätssicherung und Redundanz (gegen aktiv).

**F: Definieren Sie Risiko und erläutern Sie die Berechnung. 🧮**
> **Risiko** ist das Produkt aus **Eintrittswahrscheinlichkeit p** eines Schadensereignisses und der **Schadenshöhe S**: R = p · S. Ein Ereignis mit kleiner Wahrscheinlichkeit, aber hohem Schaden kann also ein ähnliches Risiko haben wie ein häufiges Ereignis mit kleinem Schaden. Eine Maßnahme lohnt sich wirtschaftlich, wenn die Risikoreduktion (R_vorher − R_nachher) größer ist als ihre Kosten. Beispiel: p = 0,1/Jahr, S = 10.000 € → R = 1.000 €/Jahr; eine Maßnahme für 300 €/Jahr, die das Risiko halbiert, spart 500 € − 300 € = 200 €.

**F: Warum ist vollständige Sicherheit nicht erreichbar, und was folgt daraus?**
> Laut Skript ist eine vollständige Verhinderung von Angriffen nicht möglich, weil jedes reale System Schwachstellen hat, Angreifer neue Methoden finden und absolute Sicherheit unwirtschaftlich wäre. Ziel der IT-Sicherheit ist daher nicht Null-Risiko, sondern die **Reduktion von Risiken auf ein akzeptables Maß** durch angemessene, wirtschaftlich vertretbare Maßnahmen (Risikomanagement). Das wird im Sicherheitsmanagement (ISMS, PDCA-Zyklus) organisiert.

## Kapitel 2 — Rechnernetze

**F: Erläutern Sie Aufbau und Zweck des OSI-Modells.**
> Das **OSI-Modell** gliedert die Netzwerkkommunikation in **sieben aufeinander aufbauende Schichten**: 1 Bitübertragung, 2 Sicherung, 3 Vermittlung, 4 Transport, 5 Sitzung, 6 Darstellung, 7 Anwendung. Jede Schicht erfüllt eine abgegrenzte Aufgabe und stellt der darüberliegenden Schicht ihre Dienste bereit, ohne dass diese die Details kennen muss (Kapselung). Vorteil: Hersteller können einzelne Schichten unabhängig entwickeln und austauschen, und Protokolle lassen sich klar einordnen. Beispiel: eine IP-Adresse gehört zu Schicht 3, eine MAC-Adresse zu Schicht 2, TCP zu Schicht 4.

**F: Vergleichen Sie TCP und UDP.**
> **TCP** ist **verbindungsorientiert** und **zuverlässig**: es baut vor der Übertragung mit dem Drei-Wege-Handshake (SYN, SYN/ACK, ACK) eine Verbindung auf, nummeriert Pakete, bestätigt sie, überträgt verlorene erneut und sorgt für die richtige Reihenfolge sowie Fluss- und Staukontrolle. Das kostet Zeit, eignet sich aber für Web, E-Mail, Dateiübertragung. **UDP** ist **verbindungslos** und **unzuverlässig**: es sendet Datagramme ohne Handshake, ohne Bestätigung und ohne Reihenfolgegarantie, ist dafür schnell und schlank. Es eignet sich für DNS, Streaming, VoIP und Spiele, wo Geschwindigkeit wichtiger ist als der Verlust einzelner Pakete.

**F: Beschreiben Sie den TCP-Verbindungsaufbau (Drei-Wege-Handshake).**
> Der TCP-Verbindungsaufbau erfolgt in drei Schritten. (1) Der Client sendet ein **SYN** mit seiner initialen Sequenznummer. (2) Der Server antwortet mit **SYN/ACK**: er bestätigt die Client-Nummer (ACK) und sendet seine eigene Sequenznummer (SYN). (3) Der Client bestätigt mit **ACK**. Danach ist die Verbindung in beide Richtungen aufgebaut. Dieser Mechanismus wird beim **SYN-Flooding** ausgenutzt, indem viele SYNs ohne abschließendes ACK gesendet werden und so halboffene Verbindungen die Ressourcen des Servers erschöpfen.

**F: Was macht ARP und warum ist es unsicher?**
> Das **Address Resolution Protocol (ARP)** löst innerhalb eines lokalen Subnetzes eine IP-Adresse (Schicht 3) in die zugehörige MAC-Adresse (Schicht 2) auf: ein Host fragt per Broadcast „wer hat IP x?", der Besitzer antwortet per Unicast mit seiner MAC, das Paar wird im ARP-Cache gespeichert. ARP ist unsicher, weil es **nicht authentifiziert** ist und viele Systeme auch **ungefragte** ARP-Antworten akzeptieren. Dadurch kann ein Angreifer per **ARP-Spoofing** gefälschte Antworten senden und sich als Man-in-the-Middle zwischen zwei Hosts schalten. Betroffen ist zunächst die Vertraulichkeit; Schutz bieten statische ARP-Einträge, Monitoring und verschlüsselte Protokolle.

## Kapitel 3 — Angriffe

**F: Was ist ein Man-in-the-Middle-Angriff? Welche Schutzziele sind betroffen?**
> Bei einem **Man-in-the-Middle-Angriff** schaltet sich ein Angreifer unbemerkt zwischen zwei kommunizierende Parteien und kontrolliert deren Datenverkehr — er kann ihn mitlesen, verändern oder unterbinden. Im lokalen Netz wird das oft per ARP-Spoofing erreicht. Primär verletzt ist die **Vertraulichkeit** (Mitlesen); verändert der Angreifer Daten, zusätzlich die **Integrität**, und unterbindet er sie, die **Verfügbarkeit**. Schutz bietet durchgängige Verschlüsselung (TLS), die den Inhalt auch bei abgefangenem Verkehr unlesbar hält, sowie Zertifikatsprüfung gegen Identitätsfälschung.

**F: Erklären Sie SQL-Injection und eine Gegenmaßnahme.**
> **SQL-Injection** ist ein Angriff auf Anwendungsebene, bei dem ein Angreifer über ein ungeprüftes Eingabefeld eigenen SQL-Code in eine Datenbankabfrage einschleust. Weil die Eingabe ungeprüft in den Query-String eingesetzt wird, wird sie als **Code statt als Daten** interpretiert. Beispiel: Die Eingabe `' OR 1=1 -- -` als Nutzername macht die Bedingung immer wahr und kommentiert die Passwortprüfung aus, sodass man sich ohne gültiges Passwort einloggt. Betroffen sind Vertraulichkeit und Integrität der Datenbank. Wirksamste Gegenmaßnahme sind **Prepared Statements** (parametrisierte Abfragen), bei denen Eingaben strikt von Code getrennt bleiben, zusätzlich Eingabevalidierung und minimale DB-Rechte.

**F: Was ist Cross-Site-Scripting (XSS)?**
> **Cross-Site-Scripting** ist eine Schwachstelle, bei der ein Angreifer clientseitigen Schadcode (meist JavaScript) in eine Webseite einschleust, der dann im Browser **anderer Nutzer** ausgeführt wird. Ursache sind ungeprüfte Eingaben, die ohne Encoding ausgegeben werden. Bei **Stored XSS** wird der Code dauerhaft gespeichert (z. B. in einem Gästebuch) und bei jedem Seitenaufruf aller Nutzer ausgeführt; bei **Reflected XSS** steckt er in einem präparierten Link. Folgen reichen von Verfälschung der Seite (Integrität) bis zu Session- und Datendiebstahl (Vertraulichkeit). Schutz: serverseitige Validierung und **Output-Encoding**, ergänzt durch eine Content-Security-Policy.

**F: Was unterscheidet DoS von DDoS?**
> Ein **Denial-of-Service-Angriff (DoS)** überlastet ein System oder einen Dienst mit Anfragen, bis er für legitime Nutzer nicht mehr erreichbar ist — verletzt wird die **Verfügbarkeit**. Ein **DDoS (Distributed DoS)** führt denselben Angriff **verteilt** von vielen gekaperten Rechnern aus, die als **Botnetz** über einen C&C-Server gesteuert werden. Dadurch ist der Angriff viel stärker und schwerer abzuwehren, weil der Verkehr von vielen verschiedenen IP-Adressen kommt. Gegenmaßnahmen sind Überdimensionierung, Filter, Rate-Limiting und spezialisierte Anti-DDoS-Dienste.

**F: Was ist ein Penetrationstest und welche Phasen hat ein gezielter Angriff?**
> Ein **Penetrationstest** ist die systematische, **autorisierte** Überprüfung eines Systems oder Netzwerks auf Schwachstellen, die ein unbefugtes Eindringen ermöglichen. Ziel ist, Lücken vor echten Angreifern zu finden. Ein gezielter Angriff läuft typischerweise in Phasen ab: (1) **Reconnaissance** (Informationsbeschaffung: Hosts, Ports, Dienste, OS — der wichtigste Schritt), (2) **Exploit** (Ausnutzen einer Schwachstelle, Eindringen), (3) **Zugangssicherung und Ausbreitung** im Netz, (4) **Loot** (Sammeln wichtiger Informationen) und (5) **Spuren verschleiern**. Im Praktikum: nmap → Greenbone → Metasploit → Meterpreter.

## Kapitel 4 — Anonymität / TOR

**F: Erklären Sie die Arbeitsweise des TOR-Netzwerks (Onion Routing).**
> **TOR** anonymisiert Verbindungen nach dem Prinzip des **Onion Routing**. Der Verkehr läuft über mindestens drei zufällig gewählte Knoten: **Entry/Guard**, **Middle** und **Exit**. Die Daten werden mehrfach verschlüsselt („Zwiebelschichten"), und jeder Knoten entfernt nur seine Schicht und kennt dadurch jeweils nur Vorgänger und Nachfolger, nie Quelle und Ziel zugleich. So wird die **Identität (IP-Adresse) vom Inhalt getrennt**. Der Guard bleibt in einer Sitzung gleich; der Exit-Knoten stellt die Verbindung zum Ziel her und sieht den Inhalt, falls er nicht zusätzlich (z. B. per HTTPS) verschlüsselt ist. Betroffenes Schutzziel: Anonymität/Vertraulichkeit der Metadaten.

**F: Nennen und erläutern Sie Vor- und Nachteile von TOR.**
> **Vorteile:** Die dynamische Route über weltweit verteilte Knoten erschwert die Nachverfolgung; Identität und Inhalt sind getrennt; es gibt keinen zentralen Betreiber, den man zur Überwachung zwingen könnte (anders als beim Kaskadenansatz). **Nachteile:** Wer ausreichend viele Knoten überwacht, kann Kommunikation nachvollziehen; kontrolliert ein Angreifer **ersten und letzten** Knoten, ist die Anonymität gefährdet; manipulierte Exit-Knoten können unverschlüsselten Verkehr mitlesen; und durch den langen Weg ist der Datendurchsatz niedrig. Nur TOR zu nutzen reicht nicht — der Nutzer muss auch Browser-Fingerprinting vermeiden (englische Version, Fenstergröße bzw. Letterboxing nicht ändern).

**F: Vergleichen Sie VPN und TOR.**
> Beide verbergen die eigene IP-Adresse, aber unterschiedlich. Ein **VPN** leitet den gesamten Verkehr über **einen** Server, der stellvertretend seine IP verwendet; dieser Server kennt jedoch **Quell-IP, Ziel und Inhalt** und kann alles protokollieren — man muss ihm also vertrauen. **TOR** verteilt den Verkehr über **mehrere Knoten**, von denen keiner Quelle und Ziel gleichzeitig kennt; dafür ist es deutlich langsamer. Maximale Anonymität erreicht man durch **Onion over VPN** (erst VPN, dann TOR): der TOR-Eintrittsknoten sieht nur die VPN-IP, und der VPN-Anbieter sieht weder Ziel noch Inhalt.

## Kapitel 5 — Malware

**F: Grenzen Sie Virus, Wurm und Trojaner voneinander ab.**
> Alle drei sind Schadsoftware, unterscheiden sich aber in der Verbreitung. Ein **Virus** hängt sich an eine **Wirtsdatei** und wird erst durch eine **Nutzeraktion** (Öffnen der Datei) ausgeführt, mit der er sich weiterverbreitet. Ein **Wurm** ist **eigenständig** und verbreitet sich **selbstständig ohne Wirt und ohne Nutzeraktion** über das Netzwerk, oft durch Ausnutzen von Sicherheitslücken. Ein **Trojaner** tarnt sich als **nützliches Programm**, enthält aber eine versteckte Schadfunktion; er verbreitet sich **nicht selbst**, sondern wird vom getäuschten Nutzer installiert. Häufig öffnet ein Trojaner eine **Backdoor** für dauerhaften Zugriff.

**F: Erläutern Sie die drei Ansätze der Malware-Erkennung.**
> **Signaturbasierte Erkennung** vergleicht Dateien mit einer Datenbank bekannter Schadcode-Muster (Signaturen). Sie ist schnell und präzise, erkennt aber **nur bereits bekannte** Malware und versagt bei neuen oder polymorphen Varianten. **Heuristische Erkennung** sucht nach **verdächtigen Merkmalen und Code-Mustern** und kann so auch unbekannte Malware erkennen, erzeugt aber mehr **Fehlalarme** (False Positives). **Verhaltensbasierte Erkennung** beobachtet das Programm **zur Laufzeit** (oft in einer Sandbox) und schlägt bei schädlichem Verhalten an, z. B. massenhaftem Verschlüsseln von Dateien; sie erkennt auch neue Bedrohungen, kostet aber Ressourcen und greift teils erst, wenn schon Schaden entsteht.

## Kapitel 6 — Kryptografie

**F: Vergleichen Sie symmetrische und asymmetrische Verschlüsselung.**
> Bei der **symmetrischen** Verschlüsselung nutzen Sender und Empfänger **denselben geheimen Schlüssel** zum Ver- und Entschlüsseln (z. B. AES). Sie ist sehr schnell, hat aber das **Schlüsselaustauschproblem**: der Schlüssel muss sicher übermittelt werden, und für n Teilnehmer braucht man n·(n−1)/2 Schlüssel. Bei der **asymmetrischen** Verschlüsselung (Public-Key, z. B. RSA) hat jeder ein **Schlüsselpaar**: mit dem öffentlichen Schlüssel wird verschlüsselt, nur mit dem privaten entschlüsselt. Das löst den Schlüsselaustausch, ist aber deutlich langsamer. In der Praxis kombiniert man beides **hybrid**: asymmetrisch wird ein symmetrischer Sitzungsschlüssel ausgetauscht, mit dem dann die eigentlichen Daten schnell verschlüsselt werden (z. B. in TLS).

**F: Vergleichen Sie die Betriebsmodi ECB und CBC.**
> **ECB (Electronic Code Book)** verschlüsselt jeden Block **unabhängig** mit demselben Schlüssel. Dadurch werden **gleiche Klartextblöcke zu gleichen Chiffreblöcken** — Strukturen bleiben erkennbar, bei einem Bild sieht man die Umrisse trotz Verschlüsselung. ECB ist deshalb unsicher. **CBC (Cipher Block Chaining)** verknüpft jeden Klartextblock vor der Verschlüsselung per **XOR mit dem vorherigen Chiffreblock** (der erste mit einem Initialisierungsvektor). So ergeben gleiche Klartextblöcke **verschiedene** Chiffreblöcke und Muster verschwinden. Der Verkettungsmodus ist also entscheidend für die Sicherheit einer Blockchiffre.

**F: Was ist eine kryptografische Hashfunktion und wozu dient sie?**
> Eine **kryptografische Hashfunktion** bildet eine beliebig lange Eingabe auf einen **Wert fester Länge** ab (z. B. SHA-256). Sie ist eine **Einwegfunktion** (aus dem Hash lässt sich die Eingabe nicht zurückrechnen), **kollisionsresistent** (es ist praktisch unmöglich, zwei Eingaben mit gleichem Hash zu finden) und **lawinenartig** (kleine Änderung → völlig anderer Hash). Damit dient sie der **Integritätsprüfung**: ändert sich die Nachricht, ändert sich der Hash. Beispiel: Prüfsumme eines Downloads. ⚠️ MD5 und SHA-1 gelten als gebrochen und sollten nicht mehr verwendet werden. Die Kollisionssicherheit liegt wegen des Geburtstagsparadoxons nur bei ca. 2^(N/2).

**F: Erklären Sie digitale Signatur und was sie von einem MAC unterscheidet.**
> Bei einer **digitalen Signatur** berechnet der Absender den **Hash** der Nachricht und verschlüsselt ihn mit seinem **privaten** Schlüssel. Jeder kann mit dem **öffentlichen** Schlüssel prüfen, ob Hash und Nachricht zusammenpassen. Das garantiert **Integrität**, **Authentizität** und **Verbindlichkeit** (der Absender kann die Signatur nicht abstreiten, weil nur er den privaten Schlüssel hat). Ein **MAC** nutzt dagegen einen **gemeinsamen geheimen Schlüssel** (symmetrisch); er sichert Integrität und Authentizität zwischen den zwei Parteien, bietet aber **keine Verbindlichkeit**, weil beide denselben Schlüssel kennen und jede Partei den MAC erzeugen könnte.

**F: Wozu dienen Zertifikate und eine PKI?**
> Bei der asymmetrischen Verschlüsselung muss man sicher sein, dass ein öffentlicher Schlüssel wirklich zur angegebenen Identität gehört — sonst könnte ein Angreifer seinen eigenen Schlüssel unterschieben (MitM). Ein **digitales Zertifikat** (Format X.509) bindet einen öffentlichen Schlüssel an eine Identität und wird von einer vertrauenswürdigen **Zertifizierungsstelle (CA)** digital signiert. Die **PKI (Public-Key-Infrastruktur)** ist das Gesamtsystem aus CAs, Zertifikaten und Sperrlisten, das dieses Vertrauen organisiert. Beispiel: das HTTPS-Zertifikat einer Webseite, das der Browser gegen die bekannten Root-CAs prüft.

**F: Warum ist ein kurzer DES-Schlüssel ein Problem und wie löst man es?**
> **DES** arbeitet nur mit einer **Schlüssellänge von 56 Bit**. Dadurch gibt es „nur" 2^56 mögliche Schlüssel, die sich mit heutiger Rechenleistung per **Brute-Force** in akzeptabler Zeit durchprobieren lassen — DES gilt daher als unsicher. Eine einfache Abhilfe ist die mehrfache Anwendung, insbesondere **3DES (Triple-DES)**, das durch die Verkettung eine effektiv größere Schlüssellänge erreicht. Der moderne Standard-Nachfolger ist **AES** mit Schlüssellängen von 128, 192 oder 256 Bit, der als sehr sicher gilt und u. a. in TLS, IPSec und SSH eingesetzt wird.

## Kapitel 7 — Zugriffskontrolle

**F: Vergleichen Sie DAC, MAC und RBAC.**
> Die drei Modelle regeln, wer auf welche Objekte zugreifen darf. Bei **DAC (Discretionary Access Control)** vergibt der **Eigentümer** eines Objekts die Rechte nach eigenem Ermessen (z. B. Unix-Dateirechte). Das ist flexibel, aber anfällig, weil ein Trojaner mit den Rechten des Nutzers Rechte weitergeben kann. Bei **MAC (Mandatory Access Control)** erzwingt das **System** die Rechte anhand einer zentralen Richtlinie und fester Sicherheitsstufen/Labels; der Nutzer kann sie nicht selbst ändern — sicher, aber unflexibel (z. B. militärische Systeme). Bei **RBAC (Role-Based Access Control)** werden Rechte an **Rollen** gebunden und Nutzern Rollen zugewiesen; das vereinfacht die Verwaltung in großen Organisationen.

**F: Erläutern Sie das Bell-LaPadula-Modell.**
> **Bell-LaPadula** ist ein MAC-Modell zum Schutz der **Vertraulichkeit**. Subjekte und Objekte haben Sicherheitsstufen (z. B. öffentlich < vertraulich < geheim). Es gilt die **No-Read-Up-Regel** („simple security property"): ein Subjekt darf nur Objekte **gleicher oder niedrigerer** Stufe lesen (SC(S) ≥ SC(O)). Und die **No-Write-Down-Regel** („*-property"): ein Subjekt darf nur auf **gleicher oder höherer** Stufe schreiben (SC(S) ≤ SC(O)), damit keine Information aus einer hohen in eine niedrige Stufe abfließt. Merksatz: **read down, write up**. Das komplementäre **Biba-Modell** kehrt dies zum Schutz der Integrität um (no read down, no write up).

**F: Was ist der Unterschied zwischen einer ACL und einer Capability?**
> Beide beschreiben die Einträge einer **Zugriffsmatrix** (Subjekte × Objekte × Rechte), aber aus unterschiedlicher Richtung. Eine **Access Control List (ACL)** ist **objektzentriert** (spaltenweise): zu jedem Objekt wird gespeichert, welche Subjekte welche Rechte haben — z. B. die Rechteliste einer Datei. Eine **Capability** ist **subjektzentriert** (zeilenweise): zu jedem Subjekt wird gespeichert, auf welche Objekte es mit welchen Rechten zugreifen darf — wie ein Schlüsselbund. ACLs erleichtern die Frage „wer darf auf dieses Objekt?", Capabilities die Frage „worauf darf dieses Subjekt?".

## Praktika-bezogene Freitext-Fragen

**F: Warum nutzt die Testumgebung ein NAT-Netzwerk und Sandboxing?**
> **Sandboxing** bedeutet, sicherheitskritische Software in einer isolierten Testumgebung auszuführen, damit der echte Rechner keinen Schaden nimmt. Realisiert wird das mit **virtuellen Maschinen** in VirtualBox. Die VMs liegen in einem **NAT-Netzwerk**, weil nur dieser Modus alle drei Anforderungen erfüllt: die Gäste können **untereinander** kommunizieren, der **Host bleibt geschützt** (kein Gast→Host-Zugriff, da die VMs absichtlich verwundbar sind), und die Gäste haben bei Bedarf **Internetzugang**. Eine Netzwerkbrücke würde die verwundbaren VMs direkt ins echte LAN hängen und Host sowie andere Rechner gefährden.

**F: Erklären Sie Exploit und Payload am Beispiel des Pentest-Praktikums.**
> Ein **Exploit** nutzt eine konkrete **Schwachstelle** aus, um sich Zugriff auf ein System zu verschaffen; allein richtet er keinen weiteren Schaden an. Im Praktikum wurde die SMB-Lücke an Port 445 des Windows-XP-Systems mit dem Metasploit-Exploit `ms08_067_netapi` ausgenutzt. Der **Payload** ist der danach ausgeführte **Schadcode** — hier `windows/meterpreter/reverse_tcp`, der eine Meterpreter-Fernsteuer-Session aufbaut, bei der sich das Opfer zum Angreifer verbindet (Reverse). Bild: der Exploit öffnet die Tür, der Payload ist das, was der Angreifer dann im System tut (Screenshots, Keylogger, Rechte-Eskalation).

**F: Woran erkennt das Opfer im Praktikum einen ARP-Spoofing-Angriff?**
> Beim ARP-Spoofing sendet der Angreifer gefälschte ARP-Antworten, sodass im **ARP-Cache** des Opfers (`ip neighbour show`) **zwei verschiedene IP-Adressen dieselbe MAC-Adresse** — die des Angreifers — tragen. Das ist ein starkes Indiz für einen laufenden Angriff. Mit dem Live-Monitoring (`ip monitor neigh`) sieht man zusätzlich, wie die MAC zu einer bekannten IP plötzlich wechselt. In Wireshark meldet die Expertinfo „duplicate use of … detected". ⚠️ Zu beachten: Die virtuelle NAT-Infrastruktur (.1/.2/.3) kann aus technischen Gründen ebenfalls gleiche MACs zeigen, ohne dass ein Angriff vorliegt.
