# IT-Sicherheit — Главный индекс

> **Экзамен:** ~11.02.2027 · **Open Book** (интернет и свои материалы можно, ИИ нельзя) · **Цель:** 1,0–2,0
>
> **Как пользоваться на экзамене (экзамен: 60–80 вопросов, часть коротких, часть текстом вручную):**
> 1. **Короткий вопрос** (назвать / определить / 1a-2b-3c / верно-неверно) → сразу **[12_Schnellzugriff.md](12_Schnellzugriff.md)**, Cmd+F по слову. Ответ в одну строку.
> 2. **Открытый вопрос текстом** → **[13_Antwortbank.md](13_Antwortbank.md)**: готовый немецкий абзац, подставь сценарий из задания. (Длиннее/глубже — глава по теме.)
> 3. **Вопрос из практикума** → [08_Praktika.md](08_Praktika.md) (блоки ❓ Fragen aus den Anleitungen).
> 4. **Лог/Wireshark** → [09_Wireshark_Guide.md](09_Wireshark_Guide.md). **Расчёт** → §4 ниже.
> 5. Cmd+F работает и по-немецки, и по-русски. Цитата дословно — оригинал в скрипте.
>
> ⏱️ **Тайминг:** 60–80 вопросов → ~1–1,5 мин на вопрос. Первый проход — всё, что знаешь сразу; остальное помечай и ищи во втором проходе через Schnellzugriff. Не гугли то, что быстрее найти у себя.
>
> **Номера страниц:** везде указана страница *скрипта* (как напечатано внизу страницы).
> В PDF-просмотрщике это страница **+6** (стр. скрипта 13 = стр. PDF 19).

---

## 1. Файлы конспекта

| Файл | Тема | Скрипт, стр. | Статус |
|---|---|---|---|
| [01_Einfuehrung.md](01_Einfuehrung.md) | Schutzziele, Bedrohung, Angriffe, Angreifer, Recht, Management, Risiko | 3–28 | ✅ |
| [02_Rechnernetze.md](02_Rechnernetze.md) | OSI, TCP/IP, IP, Subnetting, TCP, UDP, DNS, DHCP, ARP, ICMP, Befehle | 29–74 | ✅ |
| [03_Angriffe.md](03_Angriffe.md) | Sniffing, Spoofing, MitM, DoS, SYN-Flood, SQLi, XSS, Phishing, Pentest, Metasploit | 75–98 | ✅ |
| [04_Anonymitaet_TOR.md](04_Anonymitaet_TOR.md) | Identifizierung, Proxy, TOR, Hidden Services, I2P, JonDo | 99–118 | ✅ |
| [05_Malware.md](05_Malware.md) | Malware-Typen (Virus/Wurm/Trojaner…), Erkennung | 119–140 | ✅ |
| [06_Kryptografie.md](06_Kryptografie.md) | Sym./Asym., AES, Modi, RSA 🧮, Hash, DH 🧮, MAC, Signaturen, Zertifikate, TLS | 141–198 | ✅ |
| [07_Zugriffskontrolle.md](07_Zugriffskontrolle.md) | Zugriffsmatrix, ACL, DAC/MAC, RBAC, Bell-LaPadula, Chinese Wall | Folien | ✅ |
| [08_Praktika.md](08_Praktika.md) | Virtualisierung, MitM/Phishing, Websicherheit/Darkweb, Pentest, Android, CrypTool | Praktika | ✅ |
| [09_Wireshark_Guide.md](09_Wireshark_Guide.md) | Logs lesen, Filter, TCP-Flags, **Angriffssignaturen**, Übungslogs | — | ✅ |
| [10_Glossar_DE_RU.md](10_Glossar_DE_RU.md) | Alphabetisches Glossar + Verwechslungs-Trainer | — | ✅ |
| [11_Uebungen.md](11_Uebungen.md) | **Probeklausur** (100 P, alle Themen, mit Lösungen) | — | ✅ |
| [12_Schnellzugriff.md](12_Schnellzugriff.md) | ⚡ **Kurzantworten** für kurze Fragen + alle Zuordnungstabellen | — | ✅ |
| [13_Antwortbank.md](13_Antwortbank.md) | 📝 **Fertige Musterantworten** für Freitext-Fragen (zum Anpassen) | — | ✅ |

---

## ⚠️ О задачах в конспекте (важно, прочти один раз)
- **Экзамен проверяет понимание и перенос**, а не зубрёжку: часто вопрос прямо в скрипте не написан — надо *понять концепт и применить* его к новой ситуации. Поэтому в каждой главе есть блок **«T. Transfer & Verständnis»** — тренируй именно его, это ближе всего к реальному экзамену.
- **Что откуда:** блоки **«A»** (в гл. 1, 2, 3, 5) — реальные контрольные вопросы из скрипта. **Все остальные задачи** (B–T, Wireshark-логи, Probeklausur) я **составил сам** по материалу — они корректны и в тему, но формулировки реального экзамена Pavlik могут отличаться.
- **При ответе:** сперва давай **скриптовое** определение, потом при желании расширение, помеченное **➕** (это сверх скрипта). На open-book ➕ — плюс, но если препод спрашивает строго по лекции, ориентир — скрипт.
- **Маркеры задач:** [T] типичная · [N] нетипичная/перенос · [1,0] на высший балл · 🧮 расчёт · 🦈 анализ лога.

## 2. Стратегия на экзамене (чтобы получить 1,0)

### 2.1 Operatoren — что именно от тебя хотят
Немецкие задания строятся на «операторах». От оператора зависит, **сколько писать**. Это частая причина потери баллов: человек знает тему, но отвечает не на тот оператор.

| Operator | Что делать | Объём | Пример |
|---|---|---|---|
| **Nennen / Angeben / Aufzählen** | просто перечислить, без объяснений | список | „Nennen Sie drei Schutzziele." → Vertraulichkeit, Integrität, Verfügbarkeit |
| **Beschreiben** | рассказать *как* это выглядит / работает | 2–4 предложения | „Beschreiben Sie einen passiven Angriff." |
| **Erklären / Erläutern** | рассказать *почему / как связано*, + пример | 3–6 предложений + Beispiel | „Erläutern Sie den Unterschied zwischen Gefahr und Bedrohung." |
| **Definieren** | точное определение | 1–2 предложения | „Definieren Sie Risiko." |
| **Vergleichen / Abgrenzen / Unterscheiden** | общее + **различия по критериям** → лучше таблицей | таблица | „Grenzen Sie Safety und Security voneinander ab." |
| **Zuordnen** | 1a 2b 3c | коротко | — |
| **Berechnen** | формула → подстановка → результат **с единицей** → (вывод) | все шаги | „Berechnen Sie das Risiko." |
| **Bewerten / Beurteilen / Begründen** | своё **обоснованное мнение** с аргументами за/против | абзац | „Beurteilen Sie, ob sich die Firewall lohnt." |
| **Analysieren** | разобрать сценарий на части: что произошло, какие Schutzziele, какой тип атаки | структурированно | Fallstudie, Wireshark-Log |

### 2.2 Схема ответа на открытый вопрос (на полный балл)
```
1. Definition       – Begriff präzise definieren (Fachbegriff + ggf. engl. Begriff)
2. Erklärung        – wie / warum, Zusammenhang zu anderen Begriffen
3. Beispiel         – konkret, am besten aus dem Szenario der Aufgabe
4. Bezug / Maßnahme – welches Schutzziel betroffen, welche Gegenmaßnahme
```
Пример: „Was ist ein passiver Angriff?" →
*„Ein passiver Angriff ist ein Angriff, bei dem der Angreifer nicht in das System eingreift, sondern nur Daten mitliest (Definition). Da nichts verändert wird, hinterlässt er kaum Spuren und ist schwer zu erkennen (Erklärung). Beispiel: Mitschneiden unverschlüsselter Logins im WLAN mit Wireshark (Beispiel). Betroffen ist die Vertraulichkeit; Schutz bietet durchgängige Verschlüsselung, z. B. TLS (Bezug/Maßnahme)."*

### 2.2b Für Wireshark-/Log-Aufgaben
→ Direkt **[09_Wireshark_Guide.md](09_Wireshark_Guide.md)** öffnen: dort Filter, TCP-Flags, **Angriffssignaturen** (Port-Scan, SYN-Flood, ARP-/DNS-Spoofing, Klartext-Passwort, Backdoor :4444) und Übungslogs. Analyse-Checkliste: Protokoll → Initiator → Anomalie → Angriff benennen → Schutzziel → Maßnahme.

### 2.3 Тайм-менеджмент в Open Book
- **Сначала** отвечай на то, что знаешь без поиска; гуглить — только то, что реально не помнишь.
- Open Book — ловушка: кажется, что всё можно найти, а времени нет. Поиск по **своему** конспекту быстрее Google.
- На расчётных задачах всегда пиши **формулу → подстановку → результат с единицей**. За ход решения дают баллы даже при ошибке в арифметике.
- На задачах на соответствие сначала расставь **уверенные** пары, остальное — методом исключения.

### 2.4 Formulierungshilfen (готовые фразы)
| Чтобы… | Фраза |
|---|---|
| начать определение | „Unter X versteht man …" / „X bezeichnet …" |
| противопоставить | „Im Gegensatz zu X …" / „Während X …, ist Y …" |
| привести пример | „Ein typisches Beispiel ist …" / „Beispielsweise …" |
| указать последствие | „Dadurch wird das Schutzziel … verletzt." |
| указать меру | „Als Gegenmaßnahme eignet sich …, da …" |
| оценить | „Aus wirtschaftlicher Sicht lohnt sich die Maßnahme, weil …" |
| сослаться на источник | „Laut BSI …" / „Nach Eckert …" |

---

## 3. Stichwortverzeichnis (ключевое слово → где искать)

Колонка «RU» нужна, чтобы искать и по-русски.

### Kapitel 1 — Einführung
| Stichwort | RU | Datei → Abschnitt | Skript S. |
|---|---|---|---|
| Abrechenbarkeit / Accountability | подотчётность | 01 → 1.3 Verbindlichkeit | 9 |
| Aktiver Angriff | активная атака | 01 → 1.4 | 17 |
| Angreifermodell | модель нарушителя | 01 → 1.8.4 | 26–27 |
| Angreifertypen: Hacker, Cracker, Scriptkiddie, Phreaker | типы атакующих | 01 → 1.5.1 | 20–22 |
| Angriff (Definition) | атака | 01 → 1.4 | 16 |
| Angriffsdurchführung / -initiierung / -vorbereitung | фазы атаки | 01 → 1.5 | 19–20 |
| Anonymisierung | анонимизация | 01 → 1.3.6 | 11–12 |
| Authentisierung vs. Authentifizierung | аутентификация | 01 → 1.3.5 | 11 |
| Authentizität (Daten-/Instanz-) | подлинность | 01 → 1.3.5 | 10–11 |
| Autorisierung | авторизация | 01 → 1.3.5 (Erweiterung) | — |
| BDSG / DSGVO | закон о защите данных | 01 → 1.2.4 | 7 |
| Bedrohung / Threat | угроза | 01 → 1.3.8 | 13–14 |
| Beweissicherung | сохранение доказательств | 01 → 1.3.7 | 13 |
| BOSS (BSI-Toolsammlung) | — | 01 → 1.6 | 22 |
| BSI-Gefährdungskategorien | категории угроз BSI | 01 → 1.3.8 | 15 |
| Challenge-Response | запрос-ответ | 01 → 1.3.5 | 11 |
| CIA-Triade | триада CIA | 01 → 1.3 | 8–10 |
| Computersabotage §303b | компьютерный саботаж | 01 → 1.6 | 22 |
| Covert Channel | скрытый канал | 01 → 1.3.2 | 10 |
| Daten vs. Information | данные / информация | 01 → 1.2.1 | 4–5 |
| Datenschutz vs. Datensicherheit | защита перс. данных / безопасность данных | 01 → 1.2.4 | 7–8 |
| Datenveränderung §303a | изменение данных | 01 → 1.6 | 22 |
| Drive-by-Exploit | заражение при посещении сайта | 01 → 1.4 | 16 |
| Exploit-Kit | набор эксплойтов | 01 → 1.4 | 16 |
| Fahrlässigkeit | халатность | 01 → 1.3.8 | 15 |
| Funktionssicherheit (Safety) | функциональная безопасность | 01 → 1.2.3 | 5–6 |
| Gefahr / Gefährdung | опасность | 01 → 1.3.8 | 13–15 |
| Grundbedrohung | базовая угроза | 01 → 1.3 | 8 |
| Grundfunktionen sicherer Systeme | базовые функции | 01 → 1.3.7 | 12–13 |
| Hackerethik (CCC) | этика хакеров | 01 → 1.5.1 | 21 |
| Hackerparagraph §202c | «хакерский параграф» | 01 → 1.6 | 22 |
| Höhere Gewalt | форс-мажор | 01 → 1.3.8 | 15 |
| Informationelle Selbstbestimmung | информ. самоопределение | 01 → 1.2.4 | 7 |
| Informationssicherheit | инфобезопасность | 01 → 1.2.2 | 4–6 |
| Instanzauthentizität | подлинность субъекта | 01 → 1.3.5 | 10–11 |
| Integrität | целостность | 01 → 1.3.3 | 8 |
| Interner Angriff / Innentäter | внутренняя атака | 01 → 1.4 | 17 |
| ISO 27001 / IT-Grundschutz | стандарты | 01 → 1.7 | 24 |
| IT-Sicherheit (Definition Eckert) | ИТ-безопасность | 01 → 1.2.2 | 6 |
| IT-Sicherheitsgesetz / KRITIS | закон об ИТ-безопасности | 01 → 1.4 | 17–18 |
| IT-System (DIN 44300) | ИТ-система | 01 → 1.2.1 | 5 |
| Maßnahmen: technisch / organisatorisch / personell / infrastrukturell | меры | 01 → 1.7 | 23 |
| Nichtabstreitbarkeit / Non-Repudiation | неотказуемость | 01 → 1.3.4 | 9 |
| Passiver Angriff | пассивная атака | 01 → 1.4 | 17 |
| Phreaker | фрикер | 01 → 1.5.1 | 21 |
| Primärer / sekundärer Schaden | прямой / косвенный ущерб | 01 → 1.8 | 26 |
| Pseudonymisierung | псевдонимизация | 01 → 1.3.6 | 11–12 |
| Rechteprüfung / Rechteverwaltung | проверка / управление правами | 01 → 1.3.7 | 13 |
| Risiko R = p · S | риск | 01 → 1.8 | 25 |
| Risikomanagement | управление рисками | 01 → 1.8 | 25 |
| Risikostrategien (vermeiden, vermindern, übertragen, akzeptieren) | стратегии риска | 01 → 1.8.3 (Erweiterung) | — |
| Safety vs. Security | — | 01 → 1.2.3 | 5–6 |
| Schutzbedarf | потребность в защите | 01 → 1.7 | 24 |
| Schutzmechanismen | механизмы защиты | 01 → 1.3.9 | 15 |
| Schutzziele | цели защиты | 01 → 1.3 | 8–12 |
| Schwachstelle / Weakness | слабое место | 01 → 1.3.8 | 13–14 |
| Security Policy / Sicherheitspolitik | политика безопасности | 01 → 1.7 | 23 |
| Sicherheitskonzept | концепция безопасности | 01 → 1.7 | 23–24 |
| Sicherheitsmanagement (4 Schritte) | менеджмент безопасности | 01 → 1.7 | 23–24 |
| Sicherheitsvorfall | инцидент безопасности | 01 → 1.4 | 17 |
| Social Engineering | социальная инженерия | 01 → 1.4 | 16 |
| Steganografie | стеганография | 01 → 1.3.2 | 10 |
| Verbindlichkeit | обязательность | 01 → 1.3.4 | 9 |
| Verfügbarkeit (Formel) | доступность | 01 → 1.3.1 | 8–9 |
| Vertraulichkeit | конфиденциальность | 01 → 1.3.2 | 10 |
| Verwundbarkeit / Vulnerability | уязвимость | 01 → 1.3.8 | 14 |
| Ausspähen von Daten §202a | шпионаж за данными | 01 → 1.6 | 22 |
| Abfangen von Daten §202b | перехват данных | 01 → 1.6 | 22 |

### Kapitel 2 — Rechnernetze
| Stichwort | RU | Datei → Abschnitt | Skript S. |
|---|---|---|---|
| ACK / Quittierungsnummer | подтверждение | 02 → 2.7.1, 2.7.3 | 49, 51–52 |
| Adressklassen A–E | классы адресов | 02 → 2.6.3 | 42–43 |
| APIPA 169.254.x.x | авто-адрес без DHCP | 02 → 2.6.3, 2.12.2 | 43 |
| ARP, ARP-Cache, arp -a/-s/-d | ARP | 02 → 2.12.3 | 70–71 |
| ARP-Spoofing (Erkennung im Log) | подмена ARP | 02 → 2.12.3, Aufgabe G3 | 70 |
| Broadcastadresse | широковещательный адрес | 02 → 2.6.3 | 44–45 |
| CIDR /n | CIDR | 02 → 2.6.3 (Maskentabelle) | 44 |
| Client-Server, HTTP-Request/Response | клиент-сервер | 02 → 2.2 | 31–32 |
| Congestion Control / Slow Start | управление перегрузкой | 02 → 2.8 | 56–57 |
| Delayed ACK, Piggybacking | отложенное подтверждение | 02 → 2.7.4 | 53–54 |
| DF / MF Flags, Fragmentierung, Fragment Offset | фрагментация | 02 → 2.6.1, Aufgabe C3 | 40 |
| DHCP (DORA, Ports 67/68, Rogue DHCP, Starvation) | DHCP | 02 → 2.12.2, Aufgabe G4 | 68–70 |
| DNS (Ablauf, Zonen, Ports 53, TTL, Spoofing) | DNS | 02 → 2.12.1, Aufgabe G7 | 67–69 |
| Duplicate ACK, Retransmission, Fast Retransmit | повторная передача | 02 → 2.7.5 | 54–55 |
| Flusskontrolle / Window / Zero Window | управление потоком | 02 → 2.7.6 | 55–56 |
| Fluss- vs. Staukontrolle | поток vs. перегрузка | 02 → 2.8 (Tabelle) | 55–57 |
| Hex → Dezimal, Header aus Hex lesen | чтение заголовков | 02 → 2.6.1, Aufgaben C | 39–40 |
| ICMP (Typen, Codes, Destination Unreachable, Time Exceeded) | ICMP | 02 → 2.12.4 | 71–72 |
| ifconfig / ipconfig | конфигурация сети | 02 → 2.11 | 66 |
| Instanz, Partner-Instanz, SAP, Dienst | понятия OSI | 02 → 2.3 | 34–35 |
| IP (verbindungslos, zustandslos) | IP | 02 → 2.6 | 39 |
| IP-Header (Felder) | заголовок IP | 02 → 2.6.1 | 39–41 |
| IPv6 (128 Bit, IPsec, kein NAT, Interface-ID/MAC) | IPv6 | 02 → 2.6.4 | 45–47 |
| Kapselung / Schachtelung | инкапсуляция | 02 → 2.5 | 38–39 |
| MAC-Adresse, OUI, MAC-Spoofing | MAC-адрес | 02 → 2.10.1 | 35, 61 |
| netstat (Zustände, -ano, PID) | netstat | 02 → 2.11, Aufgabe G6 | 65 |
| Netzadresse / Netzanteil / Hostanteil | адрес сети | 02 → 2.6.3 | 42–44 |
| OSI-Modell (7 Schichten) | модель OSI | 02 → 2.3 | 32–36 |
| ping (ICMP Echo, TTL → OS) | ping | 02 → 2.11 | 64–65 |
| Ports / Well-known Ports | порты | 02 → 2.10.2 | 61–62 |
| Port-Scan (SYN/ACK, RST, gefiltert) | сканирование портов | 02 → 2.8.2, Aufgabe G2 | 57–58 |
| Private Adressen (RFC 1918) | частные адреса | 02 → 2.6.3 | — |
| Protokoll (Definition) | протокол | 02 → 2.2 | 33 |
| Protokollnummern 1/6/17 | номера протоколов | 02 → 2.6.1 | 41 |
| RST / Verbindungsabbruch | сброс соединения | 02 → 2.8.2 | 57–58 |
| Routing, Default Gateway, Nicht-Determinismus | маршрутизация | 02 → 2.6.2 | 41–42 |
| Sequenznummer / ISN | порядковый номер | 02 → 2.7.1–2.7.3 | 48–52 |
| Sockets, 4-Tupel, Stream/Datagram | сокеты | 02 → 2.10.3 | 62–63 |
| Subnetting (Rezepte) | разбиение на подсети | 02 → 2.6.3, Aufgaben B | 44–45 |
| Subnetzmaske | маска подсети | 02 → 2.6.3 | 42–45 |
| SYN_RECEIVED / halboffene Verbindung | полуоткрытое соединение | 02 → 2.8.3, Aufgabe G6 | 58 |
| TCP (Eigenschaften) | TCP | 02 → 2.7 | 47 |
| TCP-Flags (SYN, ACK, FIN, RST, PSH, URG) | флаги TCP | 02 → 2.7.1 | 49 |
| TCP-Header | заголовок TCP | 02 → 2.7.1 | 48–50 |
| TCP-Prüfsumme schützt nicht vor MitM | контрольная сумма | 02 → 2.7.1 | 50 |
| TCP/IP-Modell (4 Schichten) | модель TCP/IP | 02 → 2.4 | 36–38 |
| TCP-Zustände (LISTEN, ESTABLISHED, TIME_WAIT …) | состояния TCP | 02 → 2.8.3 | 58–60 |
| Three-Way-Handshake | тройное рукопожатие | 02 → 2.7.2 | 50–51 |
| traceroute / tracert | трассировка | 02 → 2.11, Aufgabe G5 | 65–66 |
| TTL (IP) | время жизни пакета | 02 → 2.6.1 | 40 |
| UDP (Header, Vorteile, Nachteile) | UDP | 02 → 2.9 | 60–61 |
| Verbindungsabbau (FIN, half-close) | закрытие соединения | 02 → 2.8.1 | 57 |
| verbindungsorientiert vs. verbindungslos | с установлением соединения / без | 02 → 2.2 | 30–31 |
| Wireshark: relative Sequenznummern | относительные номера | 02 → 2.7.2 | — |
| Skript-Fehler Kap. 2 | ошибки в скрипте | 02 → 🐞 Übersicht | — |

### Kapitel 3 — Angriffe
| Stichwort | RU | Datei → Abschnitt | Skript S. |
|---|---|---|---|
| ARP-Spoofing / ARP-Poisoning / MitM | подмена ARP | 03 → 3.3.3, 3.3.7 | 82–84, 88 |
| Backdoor | бэкдор | 03 → 3.3.4 | 86 |
| Botnetz, Bot, Zombie, C&C | ботнет | 03 → 3.3.4 | 84–86 |
| Buffer Overflow | переполнение буфера | 03 → 3.4.1 | 89 |
| Credential Harvester (SET) | сбор учёток | 03 → 3.4.5 | Prak.02 |
| Deauthentication / Evil Twin (WLAN) | атаки на WLAN | 03 → 3.3.1 | 78–79 |
| DNS-Spoofing / Cache-Poisoning / Pharming | подмена DNS | 03 → 3.3.2, 3.3.8 | 82, 88 |
| DoS / DDoS | отказ в обслуживании | 03 → 3.3.4 | 84–86 |
| Drive-by-Download | заражение при загрузке | 03 → 3.4.2 | 89 |
| Exploit vs. Payload | эксплойт / полезная нагрузка | 03 → 3.5.2 | 93–94 |
| Fail open (Switch), MAC-Flooding | переполнение таблицы switch | 03 → 3.3.3 | 83 |
| Hack the Stack (Angriff→Schicht) | атаки по слоям OSI | 03 → 3.3 | 77 |
| IP-Spoofing | подмена IP | 03 → 3.3.2 | 81 |
| MAC-Spoofing / Port-Security | подмена MAC | 03 → 3.3.2 | 80–81 |
| Meterpreter / Reverse Shell | meterpreter | 03 → 3.5.2 | Prak.04 |
| Metasploit (Module) | Metasploit | 03 → 3.5.2 | 93–96 |
| nmap / Zenmap, Intense Scan -A -T4 | nmap | 03 → 3.5.1 | Prak.04 |
| NOP-Sled, Encoder (IDS umgehen) | обход IDS | 03 → 3.5.2 | 95 |
| OpenVAS / Greenbone (Schwachstellenscanner) | сканер уязвимостей | 03 → 3.5.1 | Prak.04 |
| Penetrationstest (Phasen) | пентест | 03 → 3.5 | 91–92 |
| Phishing / Social Engineering / SET | фишинг | 03 → 3.4.5 | 91, Prak.02 |
| Phishing-Checkliste / Domain-Aufbau | проверка домена | 03 → 3.4.5 | Prak.02 |
| Port-Scanning (open/closed/filtered, Stufen) | сканирование портов | 03 → 3.5.1 | 92–93 |
| promiscuous / monitor mode | режимы сниффера | 03 → 3.3.1 | 79–80 |
| Replay-Angriff | повторное воспроизведение | 03 → Aufgabe A3 | 97 |
| Smurf Attack | smurf | 03 → 3.3.4, Tabelle | 77 |
| Sniffing (Def., Tools) | снифинг | 03 → 3.3.1 | 77–80 |
| Spoofing (allgemein) | спуфинг | 03 → 3.3.2 | 80 |
| SQL-Injection | SQL-инъекция | 03 → 3.4.3 | 89–90 |
| SYN-Flooding | SYN-флуд | 03 → 3.3.5 | 87 |
| TCP-Hijacking / Sequenznummernangriff | перехват TCP | 03 → 3.3.6 | 87–88 |
| Übersicht Angriff→Schutzziel→Maßnahme | атака→цель→мера | 03 → Übersichtstabelle | — |
| Wörterbuch-/Brute-Force-Angriff | словарная атака | 03 → 3.3.1, Aufgabe A3 | 79, 97 |
| XSS (client-/server-seitig) | XSS | 03 → 3.4.4 | 90–91 |

### Kapitel 4 — Anonymität / TOR
| Stichwort | RU | Datei → Abschnitt | Skript S. |
|---|---|---|---|
| Anonymität (Definition) | анонимность | 04 → 4.2 | 99–100 |
| Browser-Fingerprinting | отпечаток браузера | 04 → 4.2 | 102 |
| Cookies | куки | 04 → 4.2 | 100–101 |
| Deanonymisierung (Entry+Exit) | деанонимизация | 04 → 4.6, Aufgabe A4 | 112 |
| DNS-Leak (Port 53, TOR) | утечка DNS | 04 → 4.6.1 | 113 |
| Entry/Guard-, Relay-, Exit-Node | узлы TOR | 04 → 4.4 | 106–107 |
| Exit-Node unverschlüsselt | выход TOR без шифрования | 04 → 4.4, 4.6.2 | 108, 113 |
| Geolocation / Whois | геолокация | 04 → 4.2 | 103 |
| Hidden Service / .onion / HSDir | скрытые сервисы | 04 → 4.5 | 110–111 |
| I2P (P2P, Eepsites) | I2P | 04 → 4.8 | 116 |
| JonDo / Mix-Kaskade (Chaum) | микс-каскад | 04 → 4.8 | 115 |
| Onion-Routing (Mehrfachverschlüsselung) | луковая маршрутизация | 04 → 4.4 | 107–108 |
| Proxy / Proxychain / transparenter Proxy | прокси | 04 → 4.3 | 104–105 |
| Rendezvous-Punkt / Introduction Points | точка встречи | 04 → 4.5 | 111 |
| §8 TMG (Exit-Node-Haftung) | §8 TMG | 04 → 4.7 | 114–115 |
| TOR (The Onion Router) | TOR | 04 → 4.4 | 106 |
| Überwachung am Exit-Node | прослушка на выходе | 04 → 4.6.2 | 113–114 |
| Web-Tracking / Google Analytics | веб-трекинг | 04 → 4.2 | 102–103 |

### Kapitel 5 — Malware
| Stichwort | RU | Datei → Abschnitt | Skript S. |
|---|---|---|---|
| Adware | адварь | 05 → 5.3 | 131 |
| Backdoor | бэкдор | 05 → 5.3 | 133 |
| Bakterie / Rabbit / Fork-Bomb | бактерия/кролик | 05 → 5.3 | 133 |
| Bootsektorvirus / MBR / 0x55AA | загрузочный вирус | 05 → 5.3 | 125–127 |
| Bot / Zombie / Botnetz | бот | 05 → 5.3 | 134 |
| Cybercrime-as-a-Service | киберпреступность как услуга | 05 → 5.2 | 123 |
| Dropper | дроппер | 05 → 5.3 | 131 |
| Erkennung: signaturbasiert | сигнатурное обнаружение | 05 → 5.5.1 | 137–138 |
| Erkennung: heuristisch (statisch/dynamisch) | эвристика | 05 → 5.5.2 | 138–139 |
| Erkennung: verhaltensbasiert / HIPS | поведенческое обнаружение | 05 → 5.5.3 | 139 |
| Infektionsroutine / Trigger / Payload | части вируса | 05 → 5.3 | 124 |
| Keylogger / Password-Grabber | кейлоггер | 05 → 5.3 | 131 |
| Makrovirus / Skriptvirus | макровирус | 05 → 5.3 | 125–126 |
| Malware (Definition) | вредонос | 05 → 5.1 | 119 |
| Malwarefamilien (Zeus, Conficker, Triada) | семейства malware | 05 → 5.4.2 | 136–137 |
| Monokultur (Windows 99,9% / Android) | монокультура | 05 → 5.4 | 123, 134 |
| Polymorphe Viren | полиморфные вирусы | 05 → 5.5.1, Aufgabe A3 | 138 |
| Ransomware / Kryptotrojaner | шифровальщик | 05 → 5.3 | 132 |
| Rootkit | руткит | 05 → 5.3 | 133 |
| Spyware | шпионское ПО | 05 → 5.3 | 131 |
| Stuxnet (Industrie/Hardware) | Stuxnet | 05 → 5.2, 5.3 | 121, 134 |
| Trojaner (Trojanisches Pferd) | троян | 05 → 5.3 | 130 |
| Virus (Definition, Arten) | вирус | 05 → 5.3 | 124–127 |
| Virus vs. Wurm vs. Trojaner | вирус/червь/троян | 05 → Vergleichstabelle | — |
| Wiper | вайпер | 05 → 5.3 | 134 |
| Wurm (Definition, Code Red) | червь | 05 → 5.3 | 128–129 |

### Kapitel 6 — Kryptografie
| Stichwort | RU | Datei → Abschnitt | Skript S. |
|---|---|---|---|
| AES (Eigenschaften, Runden) | AES | 06 → 6.4.4 | 160 |
| Asymmetrisch / Public-Key | асимметричное | 06 → 6.5 | 161 |
| Avalanche-Effekt | лавинный эффект | 06 → 6.4.1 | 153 |
| Betriebsmodi ECB/CBC/CTR/OFB/CFB | режимы блочного шифра | 06 → 6.4.3 | 155–159 |
| Blockchiffre vs. Stromchiffre | блочный/потоковый | 06 → 6.3–6.4 | 148–152 |
| CA / Zertifizierungsstelle | центр сертификации | 06 → 6.8.1 | 180 |
| Chain of Trust / PKI | цепочка доверия | 06 → 6.8.1 | 182–183 |
| Chiffre / Klartext / Geheimtext | шифр/текст | 06 → 6.3 | 146 |
| Diffie-Hellman 🧮 | Диффи-Хеллман | 06 → 6.7 | 174–175 |
| Digitale Signatur 🧮 | цифровая подпись | 06 → 6.8 | 178–180 |
| Diskreter Logarithmus (DLP) | дискретный логарифм | 06 → 6.5 | 163 |
| ECB-Schwäche (Strukturerhaltung) | слабость ECB | 06 → 6.4.3, Aufgabe C1 | 155–156 |
| Einwegfunktion mit Falltür | односторонняя функция с люком | 06 → 6.5 | 163 |
| Faktorisierungsproblem (IFP) | факторизация | 06 → 6.5 | 163 |
| Fingerprint (Schlüssel) | отпечаток ключа | 06 → 6.7.1 | 171 |
| Geburtstagsparadoxon 🧮 | парадокс дней рождения | 06 → 6.6.3 | 169 |
| Hashfunktion (SHA, MD5) | хеш-функция | 06 → 6.6 | 166–169 |
| Hybrides Verfahren | гибридное | 06 → 6.7 | 175 |
| Kerckhoffs'sches Prinzip | принцип Керкгоффса | 06 → 6.2 | 144–145 |
| Kollision / Kollisionsresistenz | коллизия | 06 → 6.6.2 | 169 |
| Konfusion & Diffusion | конфузия и диффузия | 06 → 6.4.1 | 153 |
| Kryptologie/-grafie/-analyse | криптология | 06 → 6.2 | 142 |
| MAC (vs. Signatur) | MAC | 06 → 6.7.4 | 176–177 |
| One-Time-Pad / Vernam | одноразовый блокнот | 06 → 6.3 | 149–150 |
| RSA (Setup, c=mᵉ, m=cᵈ) 🧮 | RSA | 06 → 6.5 | 163–165 |
| Schlüsseltauschproblem n(n-1)/2 | проблема обмена ключами | 06 → 6.3.2 | 147–148 |
| Security by Obscurity | безопасность через неизвестность | 06 → 6.2 | 145 |
| Symmetrische Verschlüsselung | симметричное | 06 → 6.3.1 | 146–147 |
| TLS/SSL (Handshake, 4 Phasen) | TLS | 06 → 6.9 | 183–187 |
| Cipher Suite | набор шифров | 06 → 6.9 | 186 |
| X.509 / CRL | X.509/отзыв | 06 → 6.8.1 | 180–182 |
| XOR-Verschlüsselung 🧮 | XOR | 06 → 6.3 | 148–149 |

### Kapitel 7 — Zugriffskontrolle
| Stichwort | RU | Datei → Abschnitt | Folie |
|---|---|---|---|
| ACL (Access Control List) / ACE | список контроля доступа | 07 → Zugriffsmatrix | 30 |
| Bell-LaPadula (BLP) | модель Белла-Лападулы | 07 → Bell-LaPadula | 23–26 |
| Capabilities (Berechtigungen) | возможности | 07 → Zugriffsmatrix | 9 |
| Chinese Wall / COI-Regel | китайская стена | 07 → Chinese Wall | 27–29 |
| chmod / oktal (rwx=421) 🧮 | chmod | 07 → Unix-Rechte | 36–39 |
| DAC (Discretionary Access Control) | дискреционный доступ | 07 → 4 Ansätze | 5–6 |
| DAC vs. MAC | DAC против MAC | 07 → MAC (Tabelle) | 5–8 |
| HRU-Modell (6 Elementaroperationen) | модель HRU | 07 → HRU | 13–14 |
| IBAC (Identity Based) | по идентичности | 07 → 4 Ansätze | 7 |
| least privilege / need-to-know | мин. привилегии | 07 → Einleitung | 4 |
| MAC (Mandatory Access Control) | мандатный доступ | 07 → 4 Ansätze | 8 |
| No-Read-Up / No-Write-Down / Tranquility | правила BLP | 07 → Bell-LaPadula | 23 |
| RBAC (Rollenmodell) | ролевой доступ | 07 → RBAC | 16–22 |
| Referenzmonitor | эталонный монитор | 07 → Modelle | 5 |
| restriktive Rechtevergabe | ограничительная выдача прав | 07 → Einleitung | 4 |
| Rollenhierarchie (R1⇒R2) | иерархия ролей | 07 → RBAC | 17 |
| Schutzstatus vs. Berechtigungen (Spalte/Zeile) | столбец/строка матрицы | 07 → Zugriffsmatrix | 12 |
| Session (RBAC) | сессия | 07 → RBAC | 20 |
| Sicherheitsklasse SC = (Label, Kategorien) | класс безопасности | 07 → Bell-LaPadula | 23 |
| suid/sgid/sticky (Spezialbits) | спецбиты | 07 → Unix-Rechte | 37 |
| Unix-Rechte Datei vs. Verzeichnis | права файл/каталог | 07 → Unix-Rechte | 34–35 |
| Zugriffsmatrix (access matrix) | матрица доступа | 07 → Zugriffsmatrix | 10–11 |

### Praktika 01–06
| Stichwort | RU | Datei → Abschnitt |
|---|---|---|
| apktool / Smali / Dalvik / .dex | декомпиляция APK | 08 → Praktikum 05 |
| arpspoof / ip_forward / promisc | MitM-инструменты | 08 → Praktikum 02 |
| Caesar / Vigenère / Playfair | классические шифры | 08 → Praktikum 06 |
| CrypTool 2 | CrypTool | 08 → Praktikum 06 |
| Darknet vs. Dark Web vs. Deep Web vs. Surface Web | термины сети | 08 → Praktikum 03 |
| Hooking (Android Backdoor) | хук | 08 → Praktikum 05 |
| msfvenom / reverse_tcp vs. bind_tcp | генерация payload | 08 → Praktikum 05 |
| NAT-Netzwerk / VirtualBox-Modi | режимы сети VirtualBox | 08 → Praktikum 01 |
| nikto | сканер веб-сервера | 08 → Praktikum 03 |
| OWASP-BWA / Mutillidae / DVWA | тренировочные веб-приложения | 08 → Praktikum 03 |
| Sandboxing / Virtualisierung | песочница | 08 → Praktikum 01 |
| SET (Social Engineering Toolkit) | фишинг-набор | 08 → Praktikum 02 |
| SQL-Injection ' OR 1=1 -- - | SQL-инъекция логина | 08 → Praktikum 03 |
| sqlmap | авто-SQLi | 08 → Praktikum 03 |
| TOR-Browser / Letterboxing | TOR-браузер | 08 → Praktikum 03 |

*(таблица будет расти с каждой новой главой)*

---

## 4. Формулы (всё, что считается)
| Formel | Wo | Skript S. |
|---|---|---|
| Verfügbarkeit = (Gesamtlaufzeit − Ausfallzeit) / Gesamtlaufzeit | 01 → 1.3.1 | 9 |
| Risiko R = p · S | 01 → 1.8 | 25 |
| Nutzen einer Maßnahme = R_vorher − R_nachher − Kosten | 01 → 1.8.2 | (Erweiterung) |
| Reihen-/Parallelschaltung Verfügbarkeit | 01 → 1.3.1 | (Erweiterung) |
| Hosts im Subnetz = 2^(32 − n) − 2 | 02 → 2.6.3 | 44 |
| Blockgröße = 256 − Maskenoktett; Netz = IP AND Maske | 02 → 2.6.3 | 44–45 |
| gleiches Netz ⇔ IP1 AND M = IP2 AND M | 02 → 2.6.2 | 45 |
| Ack = Seq + Len (SYN/FIN = +1) | 02 → 2.7.3 | 51–52 |
| TCP-Nutzdaten = Total Length − IHL·4 − Offset·4 | 02 → 2.7.3 | 52 |
| Headerlänge = IHL·4 bzw. Offset·4 Byte | 02 → 2.6.1, 2.7.1 | 39, 49 |
| Fragment-Offset in Byte = Offset · 8 | 02 → 2.6.1 | (Erweiterung) |
| Symmetrische Schlüssel = n·(n−1)/2 | 06 → 6.3.2 | 147 |
| RSA: n=p·q, φ=(p−1)(q−1), e·d≡1 mod φ | 06 → 6.5 | 164 |
| RSA: c=mᵉ mod n, m=cᵈ mod n | 06 → 6.5 | 165 |
| Diffie-Hellman: A'=gᵃ mod p, K=B'ᵃ mod p | 06 → 6.7 | 174 |
| Geburtstagsparadoxon: Kollision bei ~1,2·√n | 06 → 6.6.3 | 169 |
| Hash-Kollisionsstärke ≈ 2^(N/2) (N = Hashlänge) | 06 → 6.6.3 | 169 |
| Unix-Rechte: r=4, w=2, x=1 (Summe je Gruppe) | 07 → Unix-Rechte | Folie 36 |
| BLP: lesen SC(S)≥SC(O), schreiben SC(S)≤SC(O) | 07 → Bell-LaPadula | Folie 25 |
| Hops ≈ Start-TTL (64/128/255) − empfangene TTL | 02 → 2.11 | (Erweiterung) |

---

## 5. Закладки для экзамена (официальные источники, не ИИ)
- BSI IT-Grundschutz-Kompendium / Glossar: https://www.bsi.bund.de
- StGB §202a–c, §303a–b: https://www.gesetze-im-internet.de/stgb/
- DSGVO (Art. 4, 5, 32): https://dsgvo-gesetz.de
- Wireshark Display Filter Reference: https://www.wireshark.org/docs/dfref/
- TCP RFC 9293: https://www.rfc-editor.org/rfc/rfc9293
- OWASP Top 10: https://owasp.org/Top10/
- Nmap Reference Guide: https://nmap.org/book/man.html
