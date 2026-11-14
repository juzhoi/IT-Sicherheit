# 🎯 Klausurfallen & Szenario-Training
**Где студенты чаще всего теряют баллы + задания «ситуация компании → критерий».**

> Этот файл написан «со стороны профессора». Две части:
> 1. **Szenario-Zuordnung** — то задание, что ты вспомнил: дана ситуация компании → выбрать критерий. Сделано сразу по трём «критериям начала курса» (Schutzziele · Grundfunktionen · Maßnahmenarten), потому что под «критериями» мог иметься в виду любой из них.
> 2. **Professor-Fallen** — пары понятий, которые путают чаще всего, и **по какому признаку** их различать (за это снимают баллы охотнее всего).
>
> Подробно — в главах по ссылкам. Короткие факты — [12_Schnellzugriff.md](12_Schnellzugriff.md). Готовые абзацы — [13_Antwortbank.md](13_Antwortbank.md).

---

# ТЕIL 1 — Szenario-Zuordnung (ситуация компании → критерий)

## 1.1 Firmen-Szenario → Schutzziel ⭐⭐ (самый вероятный формат)
> Задание-тип: «Eine Firma hat folgendes Problem … Welches **Schutzziel** ist primär betroffen/gefordert?» Распредели каждую ситуацию. Критерии: **Vertraulichkeit (V) · Integrität (I) · Verfügbarkeit (Vf) · Authentizität (A) · Verbindlichkeit (Vb) · Anonymität (An)**.

| # | Szenario in der Firma |
|---|---|
| S1 | Ein Onlineshop ist durch eine Flut von Anfragen stundenlang nicht erreichbar, Kunden können nicht bestellen. |
| S2 | In der Angebots-PDF an einen Kunden wurde unterwegs die Kontonummer durch eine fremde ersetzt. |
| S3 | Ein Lieferant streitet ab, eine Bestellung per E-Mail ausgelöst zu haben; die Firma kann es nicht nachweisen. |
| S4 | Mitarbeiter erhalten eine E-Mail, die scheinbar vom Geschäftsführer stammt, in Wahrheit aber vom Angreifer. |
| S5 | Eine Reinigungskraft fotografiert Gehaltslisten, die offen am Drucker liegen. |
| S6 | Nach einem Verschlüsselungs-Trojaner sind alle Konstruktionsdaten unbrauchbar. |
| S7 | Die Marketing-Abteilung veröffentlicht „anonymisierte" Umfragedaten, aus denen sich Einzelpersonen identifizieren lassen. |
| S8 | Ein Techniker meldet sich mit dem Passwort eines Kollegen an und handelt in dessen Namen. |
| S9 | Ein Buchhalter manipuliert nachträglich Rechnungsbeträge in der Datenbank. |
| S10 | Ein einziger Server ohne Backup fällt aus, die Firma steht still. |
| S11 | Der E-Mail-Verkehr zur Rechtsabteilung wird unverschlüsselt über offenes WLAN mitgelesen. |
| S12 | Ein Vertrag wurde digital signiert, damit keine Seite ihn später abstreiten kann. |

> **✅ Lösung + darauf achtet der Prüfer:**

| # | Schutzziel | Begründung / Falle |
|---|---|---|
| S1 | **Verfügbarkeit** | DoS/DDoS. ⚠️ nicht „Integrität" — es wird nichts verändert, nur blockiert. |
| S2 | **Integrität** | Daten **unbemerkt verändert**. ⚠️ nicht Vertraulichkeit — der Inhalt war nicht geheim, er wurde **manipuliert**. |
| S3 | **Verbindlichkeit** (Nichtabstreitbarkeit) | ⚠️ Falle: viele schreiben „Authentizität". Hier geht es ums **spätere Beweisen**, nicht ums Erkennen des Absenders → Verbindlichkeit. Maßnahme: Signatur + Protokoll. |
| S4 | **Authentizität** | Absender gefälscht (Phishing/CEO-Fraud). Maßnahme: signierte Mails. |
| S5 | **Vertraulichkeit** | Unbefugter liest. Interner/physischer Weg, Ursache **organisatorischer Mangel**. |
| S6 | **Verfügbarkeit** (primär) | Ransomware. ⚠️ Daten sind da, aber **nicht nutzbar** → Verfügbarkeit; werden sie zusätzlich kopiert/gedroht → auch Vertraulichkeit (Double Extortion). |
| S7 | **Anonymität** | Anonymitätsmenge zu klein / Quasi-Identifikatoren. ⚠️ nicht „Datenschutz" als Schutzziel — gefragt ist das **Schutzziel** Anonymität. |
| S8 | **Authentizität** (Instanz/Subjekt) | er gibt sich als anderer aus. Folge: auch Verbindlichkeit leidet (Zurechenbarkeit). |
| S9 | **Integrität** | Daten verändert. ⚠️ wenn der Schwerpunkt auf „kann er es abstreiten?" läge → Verbindlichkeit; hier liegt er auf der **Veränderung** → Integrität. |
| S10 | **Verfügbarkeit** | technisches Versagen (Safety-nah). Maßnahme: Redundanz, Backup. |
| S11 | **Vertraulichkeit** | Sniffing, passiver Angriff. Maßnahme: TLS/Verschlüsselung. |
| S12 | **Verbindlichkeit** (gefordert, nicht verletzt) | ⚠️ Falle: hier ist nichts **verletzt**, das Ziel wird **erreicht** — die Frage kann auch „welches Ziel wird sichergestellt?" lauten. |

**🎓 Typische Fehler bei dieser Aufgabe:**
- **Integrität ↔ Verbindlichkeit verwechseln:** Integrität = „wurde geändert?"; Verbindlichkeit = „kann es jemand **abstreiten / nicht beweisen**?".
- **Integrität ↔ Vertraulichkeit:** ändern ≠ lesen.
- **Ransomware → immer nur Verfügbarkeit** eintragen ist oft zu knapp; modernes Erpressen trifft **Verfügbarkeit + Vertraulichkeit**.
- **„Authentizität" als Universalantwort:** nur wenn es ums **Echtheit-Prüfen** einer Instanz/Nachricht geht, nicht ums spätere Beweisen.

## 1.2 Firmen-Szenario → Grundfunktion sicherer Systeme ⭐
> Falls mit „Kriterien am Anfang" die **5 Grundfunktionen** (Grünbuch / IT-Sicherheitskriterien, Skript S. 12–13) gemeint waren. Kriterien: **Identifikation · Authentisierung · Rechteverwaltung · Rechteprüfung · Beweissicherung.** 🔗[01 → 1.3.7](01_Einfuehrung.md)

| # | Situation / Mechanismus | Grundfunktion |
|---|---|---|
| G1 | Der Nutzer tippt seinen Benutzernamen ein. | **Identifikation** |
| G2 | Das System verlangt Passwort/Fingerabdruck als Nachweis. | **Authentisierung** |
| G3 | Der Admin legt fest, wer auf den Ordner „Lohn" zugreifen darf. | **Rechteverwaltung** |
| G4 | Beim Öffnen der Datei prüft das OS, ob der Nutzer das Recht hat. | **Rechteprüfung** |
| G5 | Jeder Zugriff wird in einem manipulationssicheren Log festgehalten. | **Beweissicherung** |

> **🎓 Falle:** **Identifikation (wer behauptet zu sein?) ≠ Authentisierung (Nachweis)**. Login-Name = Identifikation, Passwort = Authentisierung. Und **Rechteverwaltung** (Rechte **vergeben**) ≠ **Rechteprüfung** (bei jedem Zugriff **kontrollieren**). Merke: *Rechtekontrolle ist der Kern jedes sicheren IT-Systems.*

## 1.3 Firmen-Maßnahme → Maßnahmenart ⭐
> Kriterien: **technisch · organisatorisch · personell · infrastrukturell.** 🔗[01 → 1.7](01_Einfuehrung.md)

| # | Maßnahme der Firma | Art |
|---|---|---|
| M1 | Festplatten aller Laptops werden verschlüsselt. | **technisch** |
| M2 | Es gibt eine schriftliche Passwort-Richtlinie. | **organisatorisch** |
| M3 | Neue Mitarbeiter bekommen eine Security-Awareness-Schulung. | **personell** |
| M4 | Der Serverraum bekommt Zutrittskontrolle und Brandmeldeanlage. | **infrastrukturell** |
| M5 | Ein Virenscanner wird installiert. | **technisch** |
| M6 | Zuständigkeiten für Backups werden festgelegt. | **organisatorisch** |

> **🎓 Falle:** Passwort-**Richtlinie** = organisatorisch (Regel), Passwort-**Prüfung durch das System** = technisch. Schulung = personell, nicht organisatorisch.

## 1.4 Vorfall → BSI-Gefährdungskategorie ⭐
> Kriterien: **höhere Gewalt · vorsätzliche Handlung · technisches Versagen · menschliches Fehlverhalten/Fahrlässigkeit · organisatorischer Mangel.** 🔗[01 → 1.3.8](01_Einfuehrung.md)

| # | Vorfall | Kategorie |
|---|---|---|
| K1 | Blitzschlag zerstört den Router. | **höhere Gewalt** |
| K2 | Ein Angreifer schleust Ransomware ein. | **vorsätzliche Handlung** |
| K3 | Die RAID-Platte fällt nach 5 Jahren aus. | **technisches Versagen** |
| K4 | Ein Mitarbeiter löscht versehentlich ein Verzeichnis. | **menschliches Fehlverhalten / Fahrlässigkeit** |
| K5 | Niemand ist für Updates zuständig, ein Patch fehlt seit Monaten. | **organisatorischer Mangel** |

> **🎓 Falle:** ein **ungepatchter** Server ist zuerst ein **organisatorischer Mangel** (Ursache), erst der Angriff darauf ist die vorsätzliche Handlung. Prüfer fragt nach der **Ursachenkategorie**.

## 1.5 Mini-Fallstudie (so sieht die „große" Aufgabe aus) ⭐⭐
> *Die „SchnellLogistik GmbH" hat keine Backups, einen gemeinsamen Admin-Account für alle, offenes Gäste-WLAN im selben Netz wie die Server, und keine Regelung, wer Updates einspielt. Ein Mitarbeiter öffnet einen Mail-Anhang, Ransomware verschlüsselt die Auftragsdatenbank.*
> **Aufgabe:** Nennen Sie je **ein** betroffenes Schutzziel, die Gefährdungskategorie und je eine **passende Maßnahme** (technisch/organisatorisch/personell/infrastrukturell).

> **✅ Lösungsweg:**
> - **Schutzziel:** primär **Verfügbarkeit** (DB verschlüsselt, Betrieb steht). Bei Datenabfluss zusätzlich **Vertraulichkeit**; die manipulierten Daten betreffen **Integrität**.
> - **Ursache / Kategorie:** **organisatorischer Mangel** (keine Backup-/Update-Regelung) + **menschliches Fehlverhalten** (Klick) + **vorsätzliche Handlung** (Angreifer).
> - **Maßnahmen:** technisch = **Backups** (offline/3-2-1) + Virenscanner + Netztrennung Gäste-WLAN; organisatorisch = **Zuständigkeit für Updates**, getrennte Accounts, Patch-Prozess; personell = **Awareness-Schulung** (Phishing); infrastrukturell = Serverraum/Netzsegmentierung.
> - **Verknüpfung:** gemeinsamer Admin-Account verletzt **least privilege** und zerstört die **Zurechenbarkeit** (Verbindlichkeit) → im Log nicht nachvollziehbar, wer klickte.
> **🎓 Punkte gibt es für:** je ein **konkret benanntes** Schutzziel/Maßnahme **mit Bezug zum Szenario** — nicht allgemeines „man sollte sicher sein".

---

# TEIL 2 — Professor-Fallen: Verwechslungen, die Punkte kosten

> Für jede Falle: **das Unterscheidungskriterium**. Wenn eine Frage zwei dieser Begriffe nebeneinanderstellt, ist genau dieses Kriterium gefragt.

### Kapitel 1
- **Safety vs. Security** → **Absicht/Ursache**, nicht Wirkung: Safety = unbeabsichtigt (Fehler, höhere Gewalt), Security = beabsichtigt (Angriff). Gleicher Ausfall kann beides sein. 🔗[01](01_Einfuehrung.md)
- **Authentisierung vs. Authentifizierung** → **wer handelt**: Subjekt **weist nach** (Authentisierung) vs. System **prüft** (Authentifizierung). Autorisierung = was es **darf**.
- **Authentizität vs. Verbindlichkeit** → **erkennen vs. beweisen**: Authentizität = „ich erkenne, von wem"; Verbindlichkeit = „der andere kann es **nicht abstreiten**" (und ich kann es **Dritten beweisen**).
- **Integrität vs. Verbindlichkeit** → **verändert vs. abstreitbar**.
- **Datenschutz vs. Datensicherheit** → **was/wen**: Datenschutz schützt **Personen** (personenbezogene Daten, DSGVO), Datensicherheit schützt **alle Daten** technisch/organisatorisch.
- **Anonymisierung vs. Pseudonymisierung** → **umkehrbar?**: Pseudonym kann mit Zusatzwissen **rückgängig** gemacht werden, echte Anonymisierung nicht.
- **Bedrohung vs. Gefährdung** → Gefährdung = Bedrohung **trifft auf** Schwachstelle (erst dann konkret).
- **Identifikation vs. Authentisierung** (Grundfunktionen) → behaupten vs. nachweisen.

### Kapitel 2
- **IP vs. MAC** → **Schicht 3 (logisch, über Netze)** vs. **Schicht 2 (physisch, im Subnetz)**.
- **TCP vs. UDP** → **zuverlässig/verbindungsorientiert** vs. **schnell/verbindungslos**.
- **SYN/ACK/FIN/RST** → Aufbau / Bestätigung / sauberer Abbau / **harter** Abbruch. (SYN-Flood nutzt SYN ohne ACK.)
- **Netzadresse vs. Broadcast** → Hostbits alle 0 vs. alle 1. 🧮 Maske richtig anlegen, **nicht nur Masken vergleichen**.

### Kapitel 3
- **Sniffing (passiv) vs. Spoofing/MitM (aktiv)** → nur lesen vs. eingreifen/fälschen.
- **ARP-Spoofing vs. DNS-Spoofing** → MAC-Ebene im **lokalen** Netz vs. Namensauflösung.
- **DoS vs. DDoS** → eine Quelle vs. **verteilt (Botnetz)**.
- **SQLi vs. XSS** → Angriff auf die **Datenbank/Server** vs. auf **andere Nutzer im Browser**.
- **Stored vs. Reflected XSS** → gespeichert (trifft alle) vs. über präparierten Link (trifft Opfer, das klickt).

### Kapitel 4
- **Darknet vs. Dark Web** → **Infrastruktur (das Netz)** vs. **Inhalte darauf**.
- **Deep Web vs. Dark Web** → nicht indexiert/hinter Login (**groß**, alltäglich) vs. nur über Darknet + Spezialsoftware (**klein**).
- **VPN vs. TOR** → ein Server (kennt alles) vs. viele Knoten (keiner kennt beides).
- **Guard- vs. Exit-Knoten** → Eintritt (kennt deine IP) vs. Austritt (sieht den Inhalt).

### Kapitel 5
- **Virus vs. Wurm vs. Trojaner** → Wirt+Nutzeraktion / **selbstständig über Netz** / **getarnt**, verbreitet sich nicht selbst.
- **Trojaner vs. Backdoor** → Tarnung (Transportweg) vs. heimlicher **Zugang**.
- **signatur- vs. heuristisch vs. verhaltensbasiert** → bekannte Muster / verdächtige Merkmale (auch Neues, Fehlalarme) / Laufzeitbeobachtung (Sandbox).

### Kapitel 6
- **symmetrisch vs. asymmetrisch** → **ein** Schlüssel (schnell, Austauschproblem) vs. **Schlüsselpaar** (löst Austausch, langsam).
- **Hash vs. Verschlüsselung** → Hash ist **nicht umkehrbar** und schlüssellos (Integrität), Verschlüsselung ist umkehrbar (Vertraulichkeit).
- **MAC vs. digitale Signatur** → symmetrisch, **keine** Verbindlichkeit vs. asymmetrisch, **mit** Verbindlichkeit.
- **ECB vs. CBC** → Blöcke unabhängig (Muster bleiben, unsicher) vs. XOR-Verkettung (Muster weg).
- **Verschlüsselung vs. Signatur (welcher Schlüssel?)** → Verschlüsseln mit **öffentlichem** des Empfängers; Signieren mit **eigenem privatem**.

### Kapitel 7
- **DAC vs. MAC** → **Eigentümer** vergibt vs. **System/Policy** erzwingt.
- **ACL vs. Capability** → pro **Objekt** (wer darf?) vs. pro **Subjekt** (worauf?).
- **Bell-LaPadula vs. Biba** → **Vertraulichkeit** (no read up/no write down) vs. **Integrität** (umgekehrt).
- **Authentifizierung vs. Autorisierung** → Identität prüfen vs. Rechte prüfen.

### Praktika
- **Exploit vs. Payload** → Lücke ausnutzen/Zugang vs. danach ausgeführter Schadcode.
- **reverse vs. bind** → Opfer verbindet zum Angreifer vs. Port am Opfer offen.
- **Portscanner vs. Schwachstellenscanner** → nur offene Ports vs. bekannte Lücken (invasiver).
- **NAT vs. Netzwerkbrücke** (VirtualBox) → Host geschützt vs. VM direkt im echten LAN.

---

# TEIL 3 — Welche Aufgabentypen der Prüfer gern stellt (Checkliste)
- **Definieren/Nennen:** 3 Schutzziele, 5 Grundfunktionen, Malware-Typen, OSI-Schichten, Angriffsphasen.
- **Zuordnen (1a2b3c):** Szenario→Schutzziel · Angriff→OSI-Schicht · Malware→Merkmal · § StGB→Delikt · Werkzeug→Zweck · Modell→Idee → alles in [12_Schnellzugriff.md](12_Schnellzugriff.md).
- **Abgrenzen/Vergleichen (Tabelle!):** die Paare aus Teil 2 → fertige Absätze in [13_Antwortbank.md](13_Antwortbank.md).
- **Richtig/Falsch mit Begründung:** Punkte nur **mit** Begründung (🔗[01 → D](01_Einfuehrung.md)).
- **Rechnen 🧮:** Subnetting, Verfügbarkeit, Risiko, RSA, Seq/Ack, Unix-Rechte → §4 in [00_Index.md](00_Index.md), Formel→Einsetzen→Ergebnis **mit Einheit**.
- **Log/Wireshark 🦈:** [09_Wireshark_Guide.md](09_Wireshark_Guide.md).
- **Fallstudie/Transfer:** Szenario zerlegen → Schutzziel + Angriffstyp + Gegenmaßnahme, immer **mit Bezug zum Text** (Teil 1.5 oben, [11_Uebungen.md](11_Uebungen.md) Teil 5).
