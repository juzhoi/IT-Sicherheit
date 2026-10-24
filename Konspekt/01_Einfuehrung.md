# Kapitel 1 — Einführung in die IT-Sicherheit
**Skript S. 3–28 (PDF S. 9–34)**

> **О чём глава.** Это словарь всего курса. Здесь вводятся понятия, через которые потом объясняется всё остальное: *что* защищаем (Schutzziele), *от чего* (Schwachstelle → Bedrohung → Angriff), *кто* атакует (Angreifertypen), *что говорит закон* (StGB), *как организовать защиту* (Sicherheitsmanagement) и *сколько она стоит* (Risiko).
>
> **Почему важно для оценки 1,0:** на экзамене почти любая задача из других глав заканчивается вопросом «какое Schutzziel нарушено?» или «какая это атака?». Если глава 1 сидит крепко, баллы за такие подпункты получаешь автоматически.
>
> **Обозначения:** ⭐ = очень вероятно на экзамене · 🧮 = расчёт · ⚠️ = ловушка · ➕ = сверх скрипта (полезно для 1,0, но в скрипте этого нет — при ответе сначала даём скриптовую версию).
>
> Нумерация подразделов (1.2.1, 1.3.4 …) — моя, для удобного поиска; основные номера совпадают со скриптом.

---

## Содержание
1.2 Begriff der IT-Sicherheit · 1.3 Schutzziele ⭐ · 1.3.7 Grundfunktionen · 1.3.8 Schwachstelle → Bedrohung → Gefährdung ⭐ · 1.4 Angriffsformen ⭐ · 1.5 Phasen + Angreifertypen · 1.6 Recht ⭐ · 1.7 Sicherheitsmanagement · 1.8 Risiko 🧮⭐ · **Aufgaben A–G** (в конце файла)

---

## 1.2 Begriff der IT-Sicherheit (S. 4–8)

### 1.2.1 Zeichen → Daten → Information (S. 4–5, Abb. 1.1)

**Объяснение.** Представь строку `01.09.2011;32456;HSMW`. Для компьютера это просто набор символов, которые подчиняются правилам записи (формат даты, разделитель `;`). Это **Daten**: есть только **синтаксис** (форма). Когда человек или программа знает *контекст* («это дата зачисления, номер студента и название вуза»), строка получает **смысл** (семантику) и становится **Information**. Ценность для владельца имеет именно информация. Защищаем мы тоже её, но технически можем защищать только данные, в которых она записана.

| Ebene | Определение (DE) | Пример |
|---|---|---|
| **Zeichen** | elementare Muster; physikalisch: zeitliche Änderung einer messbaren Größe | `1`, `0`, `A` |
| **Daten** | Zeichen, die nach festen Regeln (**Syntax**) zur Darstellung von Informationen genutzt werden; maschinenlesbare digitale Repräsentation von Information | `01.09.2011;32456;HSMW` |
| **Information** | im **Kontext interpretierte** Daten: **Syntax + Semantik** → Wert für den Besitzer | «Студент 32456 зачислен в HSMW 1 сентября 2011» |

> **Musterantwort ⭐:** *Daten sind die maschinenlesbare, digitale Repräsentation von Information. Sie bestehen aus Zeichen, deren Aufbau festen Regeln (Syntax) folgt. Erst durch Interpretation in einem Kontext erhalten Daten eine Bedeutung (Semantik) und werden so zu Information, die für ihren Besitzer einen Wert hat. Beispiel: Die Zeichenkette „32456" ist ein Datum; erst mit dem Wissen „das ist eine Matrikelnummer" wird sie zur Information.*

**Следствия для безопасности (S. 5):**
- Одна и та же информация может лежать в **нескольких Datenobjekten**: файл на диске, копия в бэкапе, Nutzdaten IP-пакета при передаче, распечатка. → **Защищать нужно все копии.** Слабейшая копия определяет уровень защиты.
- **IT-System (DIN 44300):** *Funktionseinheit zur Aufnahme, Verarbeitung, Aufbewahrung und Nutzung von Daten* — offenes oder geschlossenes, dynamisches technisches System.
- IT-системы всегда **soziotechnisch** → недостаточно защищать технику, **Faktor Mensch** обязательно учитывать (Social Engineering, ошибки персонала).

### 1.2.2 IT-Sicherheit vs. Informationssicherheit (S. 4–6)
- **Informationssicherheit** = **Oberbegriff**: защита информации в **любой** форме, включая бумажные Akten и рукописи.
- **IT-Sicherheit** = её **Teilgebiet**: защита **электронно** хранимой/обрабатываемой информации и самих ИТ-систем.

> **Def. 1.1 IT-Sicherheit (Eckert 2013) ⭐:** *IT-Sicherheit hat die Aufgabe, Unternehmen und deren Werte zu schützen und wirtschaftliche Schäden, die durch Vertraulichkeitsverletzungen, Manipulationen oder Störungen der Verfügbarkeit von Diensten des Unternehmens entstehen können, zu verhindern.*

Обрати внимание: в определении Eckert зашиты три Schutzziele CIA — *Vertraulichkeitsverletzungen* (C), *Manipulationen* (I), *Störungen der Verfügbarkeit* (A). Это хорошо показать в ответе — выглядит как понимание, а не заучивание.

**Ключевая мысль (S. 6–7):** *Eine vollständige Verhinderung von Angriffen ist nicht möglich.* Цель — **Risiken reduzieren**: атаки рано распознавать, на них реагировать, ущерб минимизировать, системы изначально проектировать безопасно. ISO: Security = **Minimierung der Verwundbarkeit** von Werten und Ressourcen.

**Teildisziplinen der IT-Sicherheit (S. 6):**
| Teildisziplin | Чем занимается |
|---|---|
| Kryptographie | шифрование, подписи, хеши (Kap. 6) |
| Netzwerksicherheit | защита сетей (Kap. 2–3) |
| Kommunikationssicherheit | защита **каналов связи** между партнёрами |
| Softwaresicherheit | безопасная разработка, **начинается уже в процессе разработки** |
| Sicherheitsmanagement | **организационные и пространственные** меры |
| IT-Forensik | работает, «когда уже поздно»: **сохраняет и анализирует цифровые следы** |

### 1.2.3 Safety vs. Security (S. 5–6) ⭐
**Объяснение.** В немецком одно слово *Sicherheit*, в английском два, и они про разное. **Safety** — система работает *правильно* (нет сбоев, нет недопустимых состояний). **Security** — система защищена от *злоумышленника*. Запоминалка: *Safety schützt die Umwelt vor dem System, Security schützt das System vor der Umwelt* (классическая формула из литературы ➕).

| Kriterium | **Safety = Funktionssicherheit** | **Security = Informationssicherheit** |
|---|---|---|
| Definition | Ist-Funktionalität = Soll-Funktionalität; keine funktional unzulässigen Zustände | Schutz vor **unautorisierter** Manipulation und **ungewollter Preisgabe** von Informationen |
| Ursache des Problems | **unbeabsichtigt**: Fehler, Ausfall, Bug | **beabsichtigt**: Angriff |
| Verwandter Begriff | **Zuverlässigkeit** | Vertraulichkeit, Integrität |
| Beispiel | Steuerung einer Bahnschranke schließt zuverlässig | Angreifer kann die Steuerung nicht manipulieren |
| Zusammenhang | Security ist laut Skript eine Eigenschaft **jedes funktionssicheren Systems** → Safety ist Voraussetzung | |

⚠️ **Ловушка:** Sicherheitsmanagement защищает **und** от Security-угроз (beabsichtigt), **und** от Safety-угроз (unbeabsichtigt) — см. Def. 1.6.

### 1.2.4 Datenschutz vs. Datensicherheit (S. 7–8, Abb. 1.2) ⭐
**Объяснение.** **Datenschutz** — про *людей*: право каждого самому решать, кто знает его персональные данные (**informationelle Selbstbestimmung**, выводится из Grundgesetz). **Datensicherheit** — про *данные* любого рода (в том числе коммерческую тайну без связи с личностью): чтобы они не пропали, не исказились, не утекли. Связь: защитить персональные данные невозможно, если они технически не защищены → *Datensicherheit ist Voraussetzung für Datenschutz*.

| Kriterium | **Datenschutz** | **Datensicherheit** |
|---|---|---|
| Schutzobjekt | **Person** / Privatsphäre | **Daten** aller Art (auch ohne Personenbezug: Geschäftsgeheimnisse, Finanzdaten) |
| Ziel | Recht auf **informationelle Selbstbestimmung** | Schutz vor Verlust, Manipulation, unberechtigter Kenntnisnahme, Übertragungsfehlern, Vernichtung, Diebstahl |
| Charakter | **rechtlich** (Gesetze, Normen) | **technisch-organisatorisch** (Maßnahmen) |
| Rechtsgrundlage | Skript: § 9 BDSG + Anlage; ➕ heute **DSGVO** (seit 25.05.2018), v. a. **Art. 32 DSGVO „Sicherheit der Verarbeitung"** | — |
| Ergebnis (Abb. 1.2) | Datenschutzmaßnahmen → **Datenschutz** | Datensicherungsmaßnahmen → **Datensicherheit** + ordnungsgemäßer DV-Betrieb |

> **Def. 1.2 Datenschutz:** *Unter dem Oberbegriff Datenschutz werden Maßnahmen, Normen und Gesetze zusammengefasst, die zum Schutz der Privatsphäre dienen und das Recht jedes Einzelnen auf informationelle Selbstbestimmung sicherstellen.*
>
> **Datensicherheit:** *umfasst alle Maßnahmen zum Schutz von Daten, Datenträgern und Programmen vor Übertragungsfehlern, Verfälschungen, Vernichtung, Diebstahl und unberechtigtem Zugriff.*
>
> *Datensicherheit und Datenschutz bedingen einander.*

⚠️ **Ловушка:** «Datensicherung» (= бэкап) ≠ «Datensicherheit» (= безопасность данных вообще). Бэкап — лишь одна из мер Datensicherheit.

---

## 1.3 Schutzziele (S. 8–12) ⭐

**Объяснение.** Schutzziele отвечают на вопрос «*что именно* значит, что система безопасна?». Они позволяют точно описывать атаки: не «нас взломали», а «нарушена Integrität der Kundendaten». **Verletzung eines Schutzziels = Grundbedrohung.** Система, которая Schutzziele не обеспечивает, считается «unsicher». Цель IT-Sicherheit — исключить нарушение Schutzziele.

### Обзор
| Schutzziel | engl. | Ключевой вопрос | Нарушение (пример) | Maßnahmen |
|---|---|---|---|---|
| **Vertraulichkeit** | confidentiality | Кто может **читать**? | Sniffing пароля, утечка БД | Verschlüsselung, Zugriffskontrolle, Steganografie |
| **Integrität** | integrity | Можно ли **незаметно изменить**? | подмена суммы перевода, дефейс сайта | Zugriffskontrolle, Hash/MAC, digitale Signatur |
| **Verfügbarkeit** | availability | Работает ли, **когда нужно**? | DDoS, Ransomware, пожар | Redundanz, Backups, DoS-Schutz, USV |
| **Verbindlichkeit** (Nichtabstreitbarkeit, Zurechenbarkeit) | non-repudiation, accountability | Можно ли **потом отпереться**? | клиент отрицает заказ | digitale Signatur, Logfiles, Audit |
| **Authentizität** | authenticity | Это **действительно** тот / то? | Phishing-Mail от «банка» | Passwort, Challenge-Response, Signatur, Zertifikat |
| **Anonymisierung / Pseudonymisierung** | anonymity / pseudonymity | Можно ли **связать** с человеком? | деанонимизация опроса | Pseudonyme, Aggregation, TOR |

**CIA-Triade:** **C**onfidentiality – **I**ntegrity – **A**vailability. Это «три кита»; Verbindlichkeit и Authentizität — расширенные цели.

---

### 1.3.1 Verfügbarkeit / Availability (S. 8–9) ⭐🧮
> *Ein System gewährleistet Verfügbarkeit, wenn authentifizierte und autorisierte Subjekte in der Wahrnehmung ihrer Berechtigungen nicht unautorisiert beeinträchtigt werden können.*

**Объяснение.** Обрати внимание на тонкость в определении: доступность нужна только **авторизованным** пользователям, и нарушается она только **неавторизованным** вмешательством. Если админ планово выключил сервер на ночь — это не нарушение Verfügbarkeit. Угроза бывает **absichtlich** (DoS) и **zufällig** (сбой диска).

**Formel (1.1):**
```
                  Gesamtlaufzeit − Ausfallzeit
Verfügbarkeit = ─────────────────────────────
                       Gesamtlaufzeit
```

**Beispiel 1.1 (Skript):** PPS-System (Produktionsplanung und -steuerung), 24/7, 5 h Ausfall/Jahr:
```
(365·24 − 5) / (365·24) = 8755 / 8760 = 0,99943 = 99,943 %
```

**Полезные значения** (1 Jahr = 8760 h = 525 600 min):
| Verfügbarkeit | max. Ausfall/Jahr | Bezeichnung ➕ |
|---|---|---|
| 99 % | 87,6 h ≈ 3,65 Tage | „two nines" |
| 99,9 % | 8,76 h | „three nines" |
| 99,99 % | 52,56 min | „four nines" |
| 99,999 % | 5,26 min | „five nines" (Hochverfügbarkeit) |

**Обратная задача:** Ausfallzeit = Gesamtlaufzeit · (1 − Verfügbarkeit).

➕ **Системы из нескольких компонентов** (нетипичная, но возможная задача):
- **Reihenschaltung** (все компоненты нужны: сервер И сеть И БД): `A_ges = A1 · A2 · … · An` → итог **хуже** самого слабого.
- **Parallelschaltung / Redundanz** (хватает одного из двух): `A_ges = 1 − (1 − A1) · (1 − A2)` → итог **лучше** каждого.
- Поэтому Skript называет **redundante Hardware** главной мерой повышения доступности.

⚠️ **Ловушки:** високосный год = 8784 h (если в задаче указан конкретный год, например 2028); «Wartungsfenster» (плановое обслуживание) иногда не считается простоем — читай условие; ответ давать в **%** с 2–3 знаками после запятой.

---

### 1.3.2 Vertraulichkeit / Confidentiality (S. 10)
> *Ein System gewährleistet Informationsvertraulichkeit, wenn es keine unautorisierte Informationsgewinnung ermöglicht.*

**Объяснение.** Информацию может узнать только уполномоченный. «Информационное извлечение» шире, чем «прочитать файл»: сюда относится и вывод по косвенным признакам (например, по размеру зашифрованных пакетов).

**Zwei Wege zur Vertraulichkeit:**
1. **Inhalt unlesbar machen** → Verschlüsselung (+ Zugriffskontrolle).
2. **Existenz der Nachricht verbergen** → **Covert Channel** (verdeckter Kanal): unverdächtige Datenstruktur übertragen, darin die geheime Information verstecken. Bekanntestes Mittel: **Steganografie** — Text in den Bilddaten eines Fotos verstecken. Angreifer sieht nur ein harmloses Bild.

**Beispiel 1.2:** Brief versiegeln und einem vertrauenswürdigen Kurier übergeben.

⚠️ **Ловушка:** Steganografie ≠ Kryptografie. Криптография прячет **смысл** (видно, что есть шифровка), стеганография прячет **факт существования** сообщения. Лучше всего — комбинация обеих.

---

### 1.3.3 Integrität / Integrity (S. 8)
> *Ein System gewährleistet Datenintegrität, wenn es Subjekten nicht möglich ist, die zu schützenden Daten unautorisiert und unbemerkt zu manipulieren.*

**Объяснение.** Два ключевых слова — **unautorisiert** и **unbemerkt**. Отсюда две стороны защиты:
- **Verhindern** — die Festlegung von Rechten (Zugriffskontrolle: только бухгалтер может менять суммы);
- **Erkennen** — Manipulationen müssen **erkennbar** sein (Hash, MAC, Signatur). Криптография **не может помешать** изменить биты в канале, но может гарантировать, что изменение **заметят**.

⚠️ **Ловушка:** случайное искажение при передаче (bit flip) тоже угроза целостности, но против неё достаточно контрольной суммы (CRC). Против **злоумышленника** CRC не работает: он просто пересчитает её. Нужен MAC или подпись (подробнее в Kap. 6).

---

### 1.3.4 Verbindlichkeit / Non-Repudiation (S. 9) ⭐
> *Das System gewährleistet die Verbindlichkeit einer Menge von Aktionen, wenn es nicht möglich ist, dass ein Subjekt im Nachhinein die Durchführung einer solchen Aktion abstreiten kann.*

- Близкие понятия: **Zurechenbarkeit** (действие однозначно привязано к субъекту), **Abrechenbarkeit (accountability)**, **Überwachung (audit)**, **Protokollierung**.
- Mittel: **elektronische Signaturen**, **Logfiles**, Zugangskontrolle.
- Anwendungsbeispiel (Skript): elektronischer **Vertragsabschluss**, Online-Bestellung.

---

### 1.3.5 Authentizität / Authenticity (S. 10–11) ⭐
> *Authentizität bezeichnet die Echtheit und Glaubwürdigkeit eines Objekts bzw. Subjekts, die anhand einer eindeutigen Identität und charakteristischer Eigenschaften überprüfbar ist.*

| | **Datenauthentizität** | **Instanzauthentizität** |
|---|---|---|
| Что подлинное? | **данные/сообщение** (от кого оно реально) | **субъект/человек/устройство** (кто сейчас логинится) |
| Mittel | **elektronische Signatur**, eigenhändige Unterschrift, persönliche Übergabe, Zugriffskontrolle | **Wissen** (Passwort, PIN am Geldautomaten), **Challenge-Response** |
| Beispiel | Absender einer E-Mail echt? → wichtig gegen **Phishing** | Login am Hochschulrechner |

**Challenge-Response:** система задаёт задачу (challenge), которую может правильно решить только настоящий владелец секрета (response). Преимущество ➕: сам секрет по сети **не передаётся** → его нельзя подслушать и повторить (защита от Replay).

➕ **Faktoren der Authentisierung** (часто спрашивают в смежных темах):
| Faktor | Beispiel |
|---|---|
| **Wissen** (something you know) | Passwort, PIN |
| **Besitz** (something you have) | Chipkarte, Smartphone/TOTP, Hardware-Token |
| **Inhärenz / Sein** (something you are) | Fingerabdruck, Gesichtserkennung |

**Zwei-Faktor-Authentisierung (2FA)** = два **разных** фактора. ⚠️ Passwort + PIN = **zwei Mal Wissen** → **keine** echte 2FA.

#### Authentisierung vs. Authentifizierung (Def. 1.3) ⭐⚠️
> *Unter **Authentisierung** wird der **Nachweis** der behaupteten Identität **durch ein Subjekt** verstanden. **Authentifikation** oder **Authentifizierung** bedeutet die **Prüfung** des Nachweises.*

```
Benutzer ──(Authentisierung: "ich bin Anna, hier mein Passwort")──► System
System   ──(Authentifizierung: prüft das Passwort)──► OK
System   ──(Autorisierung ➕: Anna darf Datei X lesen)──► Zugriff
```
| Schritt | Вопрос | Кто делает |
|---|---|---|
| **Identifikation** | *Wer bist du?* (Benutzername) | Subjekt nennt Identität |
| **Authentisierung** | *Beweise es!* | **Subjekt** legt Nachweis vor |
| **Authentifizierung** | *Stimmt der Beweis?* | **System** prüft |
| **Autorisierung** ➕ | *Was darfst du?* | System vergibt/prüft Rechte (= Rechteprüfung) |

Запоминалка: Authenti**s**ierung — **S**ubjekt; Authenti**f**izierung — **F**eststellung durch das System. (В английском оба слова — *authentication*.)

**Beispiel 1.4 (S. 15):** Login am Hochschulrechner: Name + Passwort eingeben (Authentisierung) → System prüft (Authentifizierung) → erfolgreich → Nutzung erlaubt.

#### Authentizität ≠ Verbindlichkeit (Beispiel 1.3) ⭐
Bob übergibt Alice **persönlich** einen Brief:
- Alice **weiß**, dass der Brief von Bob ist → **Datenauthentizität erreicht** ✅
- Alice kann das einem **Dritten nicht beweisen** → **Verbindlichkeit / Nichtabstreitbarkeit nicht erreicht** ❌ (außer mit Zeugen)

> Суть: Authentizität — *я сам* уверен в источнике. Verbindlichkeit — я могу **доказать это третьему лицу** (суду). ➕ Поэтому MAC (общий ключ) даёт аутентичность, но не неотказуемость, а цифровая подпись (секретный ключ только у отправителя) даёт обе — к этому вернёмся в Kap. 6.

---

### 1.3.6 Anonymisierung und Pseudonymisierung (S. 11–12) ⭐
**Объяснение.** Обе меры изменяют персональные данные так, чтобы человека нельзя было (легко) узнать. Разница в **обратимости**.

| Kriterium | **Anonymisierung** | **Pseudonymisierung** |
|---|---|---|
| Definition | Verändern personenbezogener Daten derart, dass sie **nicht mehr** einer Person zugeordnet werden können (oder nur mit unverhältnismäßig großem Aufwand) | Ersetzen des Namens / Identifikationsmerkmals durch ein **Pseudonym** (Code) mittels einer **Zuordnungsvorschrift** |
| Umkehrbar? | **nein** | **ja**, mit der Zuordnungstabelle |
| Verknüpfung von Datensätzen | geht **verloren** | **bleibt erhalten** (gleiche Person → gleiches Pseudonym) |
| Beispiel | Statistik „23 % der Studierenden …" | Studie: „Patient P-0815" statt Name |
| ➕ DSGVO | anonyme Daten fallen **nicht** unter die DSGVO | pseudonyme Daten sind **weiterhin personenbezogen** |

⚠️ **Anonymität braucht eine ausreichend große Menge** (Anonymitätsmenge): anonyme Umfrage mit 3 Teilnehmern → man kann leicht auf die Urheber schließen. (Это понятие вернётся в Kap. 4 TOR.)

---

### 1.3.7 Grundfunktionen sicherer Systeme (S. 12–13)
**Объяснение.** Schutzziele говорят *что* нужно, Grundfunktionen — *какими функциями* система должна для этого обладать. При этом не говорится, *какими средствами* и *насколько хорошо* они реализованы. Источник — **Grünbuch** des Bundesinnenministeriums (IT-Sicherheitskriterien zur Bewertung von IT-Systemen).

**Anforderungen an die Grundfunktionen:**
| Eigenschaft | Bedeutung |
|---|---|
| **notwendig** | System muss sie besitzen, um die Sicherheitsanforderungen zu erfüllen |
| **vollständig** | richtig implementiert **reichen** sie aus |
| **technisch unabhängig** | nicht an bestimmte Mechanismen / technische Realisierung gebunden |
| **überdeckungsfrei** | sie überlappen sich nicht unnötig |

**Die 5 Grundfunktionen ⭐:**
| # | Grundfunktion | Bedeutung | Пример |
|---|---|---|---|
| 1 | **Identifikation** | Bestimmung der Identität eines Subjekts | ввод логина |
| 2 | **Authentisierung** | Nachweis der angegebenen Identität oder Gruppenzugehörigkeit | ввод пароля |
| 3 | **Rechteverwaltung** | Verwaltung der Rechtebeziehungen zwischen Subjekten und Objekten | админ выдаёт права на папку |
| 4 | **Rechteprüfung** | Prüfung, ob ein Subjekt in der beabsichtigten Art auf ein Objekt zugreifen darf | ОС проверяет при открытии файла |
| 5 | **Beweissicherung** | Verstöße gegen die Sicherheitspolitik nachträglich nachweisen | журнал событий |

> ***Rechtekontrolle ist der Kern jedes sicheren IT-Systems.*** (→ Kap. 7 Zugriffskontrolle)

**3 Problembereiche:** (1) Benutzer- und Rechteverwaltung, (2) Rechnersysteme selbst, (3) Vernetzung (Einfallstor für viele Bedrohungen).

Связь со Schutzzielen ➕: Identifikation + Authentisierung → **Authentizität**; Rechteverwaltung + Rechteprüfung → **Vertraulichkeit, Integrität**; Beweissicherung → **Verbindlichkeit**.

---

### 1.3.8 Schwachstelle → Gefahr → Bedrohung → Gefährdung (S. 13–15) ⭐⚠️

**Объяснение.** Это **самая путаная** терминология главы, а значит, любимая для заданий на соответствие. Логика — от абстрактного к конкретному:

```
Schwachstelle (Weakness)  – слабое место в системе (баг, плохой дизайн)
      │ если через неё можно обойти защиту
      ▼
Verwundbarkeit (Vulnerability) – эксплуатируемая слабость

Gefahr   – абстрактно: «вообще может случиться ущерб» (без привязки ко времени/месту/лицу)
      │ + конкретный Bezug zu einem Schutzziel
      ▼
Bedrohung (Threat) – конкретная потенциальная опасность для Schutzziel
      │ + Bezug auf ein bestimmtes Objekt + Wahrscheinlichkeit
      ▼
Gefährdung – Bedrohung, die auf eine Schwachstelle TRIFFT
      │ wird ausgenutzt
      ▼
Angriff → Schaden    (Risiko = Wahrscheinlichkeit · Schaden, s. 1.8)
```

| Begriff | engl. | Definition (Skript) |
|---|---|---|
| **Schwachstelle** | weakness | (vermeintliche) Schwäche eines Systems oder Punkt, an dem das System verwundbar ist; mögliche Ansatzpunkte für Angriffe. Ursachen: **fehlerhafte Programmierung, falsches Systemdesign, unzureichende Beachtung von Sicherheitsanforderungen** |
| **Verwundbarkeit** | vulnerability | Schwachstelle, über die die Sicherheitsdienste des Systems **umgangen, getäuscht oder unautorisiert modifiziert** werden können |
| **Gefahr** | — | Sachverhalt, bei dem **ohne konkreten zeitlichen, räumlichen oder personellen Bezug** bei ungehindertem Ablauf in absehbarer Zeit mit hinreichender Wahrscheinlichkeit ein Schaden eintreten wird |
| **Bedrohung** | threat | **konkrete, potenzielle Gefahr mit Bezug zu einem Schutzziel** |
| **Gefährdung** | — | Bedrohung bezogen auf ein **bestimmtes Objekt** + Wahrscheinlichkeit des Auftretens; entsteht erst, **wenn eine Schwachstelle auf eine Bedrohung trifft** und ausgenutzt werden kann |

> **Def. 1.4 Bedrohung (Threat) ⭐:** *Als Bedrohungen werden Umstände oder Ereignisse verstanden, die prinzipiell die Schutzziele der IT-Sicherheit verletzen und zu einem Schaden führen können. Eine Bedrohung eines IT-Systems ermöglicht es einem Angreifer, eine oder mehrere Schwachstellen oder Verwundbarkeiten auszunutzen, um einen Verlust der Integrität, Vertraulichkeit oder Verfügbarkeit von Daten zu bewirken, oder um die Authentizität von Subjekten zu gefährden.*

**Аналогия (для понимания и для ответа «Erläutern Sie an einem Beispiel»):**
- Gefahr: «Einbrüche kommen vor.» (вообще)
- Bedrohung: «Ein Einbrecher könnte in unser Büro eindringen und Akten stehlen (→ Vertraulichkeit).»
- Schwachstelle: «Das Fenster im Erdgeschoss schließt nicht richtig.»
- Gefährdung: Bedrohung + Schwachstelle treffen zusammen → reale Gefährdung des Büros.
- IT-Variante: Bedrohung „Ransomware-Kampagne" + Schwachstelle „ungepatchter Server" = Gefährdung.

**BSI-Gefährdungskategorien (IT-Grundschutz, S. 14–15) ⭐ — классика для Zuordnung:**
| Kategorie | Beispiele (Skript) | Absicht? |
|---|---|---|
| **Höhere Gewalt** | Hochwasser, Blitzeinschlag, globaler Stromausfall, Feuer | nein |
| **Vorsätzliche Handlungen** | Abhören / Manipulation von Leitungen, Schadprogramme, Diebstahl | **ja** |
| **Technisches Versagen / technische Fehler** | defekte Datenträger, Ausfall einer Datenbank | nein |
| **Fahrlässigkeit / menschliches Fehlverhalten** | versehentliches Löschen, Nichtbeachtung von Sicherheitsmaßnahmen, Passwort auf Post-it | nein |
| **Organisatorische Mängel** | fehlende oder unzureichende Regelungen, nicht erkannte Sicherheitsvorfälle | nein |

⚠️ **Ловушка:** «Mitarbeiter löscht versehentlich» → **Fahrlässigkeit**; «Mitarbeiter löscht absichtlich aus Rache» → **vorsätzliche Handlung** (+ interner Angriff). «Es gibt keine Regel, wer Backups macht» → **organisatorischer Mangel**. Решает **умысел** и **причина**.

### 1.3.9 Schutzmechanismen (S. 15)
Zwei Grundansätze:
1. **Transformation der Inhalte (Verschlüsselung)** → Vertraulichkeit + Integrität; darauf basieren **digitale Signaturen** → Zurechenbarkeit / Verbindlichkeit.
2. **Zugriffskontrolle** → unberechtigten Zugriff auf Informationen/Ressourcen unterbinden.

> Для Vertraulichkeit **нужно и то, и другое**: шифрование защищает при передаче/хранении, но на работающей системе данные расшифрованы → там нужна Zugriffskontrolle.

---

## 1.4 Angriffsformen (S. 16–19) ⭐

> **Def. 1.5 Angriff (attack):** *Bezeichnet einen nicht autorisierten Zugriff bzw. nicht autorisierten Zugriffsversuch auf ein System.*

⚠️ Уже **попытка** (Zugriffsversuch) — это атака, даже неудачная.

**Современная картина (S. 16):**
- Motivation heute primär **finanziell**, nicht spielerisch.
- Angriffe automatisiert mit gekauften **Exploit-Kits** (Kriminelle handeln wirtschaftlich).
- Teure **manuelle** Angriffe → v. a. **staatliche Akteure**, Ziele: Militär, Regierungen, **kritische Infrastrukturen**. Normales Unternehmen kaum betroffen.
- Verizon Breach Report: ca. **2/3** der Angriffe finanziell motiviert/automatisiert.

**Häufigste Angriffsformen laut BSI:**
1. **Gezieltes Hacking** → Schadsoftware platzieren, Spionage vorbereiten
2. **Drive-by-Exploits** → breitflächige Infektion beim **Surfen** (bloßer Besuch einer Webseite genügt) → Kontrolle über den Rechner
3. **Gezielte Malware per E-Mail + Social Engineering** → Kontrolle + Spionage

**Klassifikationen von Angriffen:**
- nach Zielauswahl: **zielgerichtet (gezielt)** vs. **flächendeckend (breitflächig)**
- nach Angriffskanal (Abb. 1.4): auf **Daten** · auf **Kommunikationskanal** (Abhören/Abfangen) · auf **IT-System**
- nach Art des Eingriffs / Herkunft: **passiv · aktiv · intern** ⭐

| Kriterium | **passiver Angriff** | **aktiver Angriff** | **interner Angriff** |
|---|---|---|---|
| Was passiert | **kein Eingriff** ins System; Kommunikation wird **abgefangen/mitgelesen** | Angreifer **greift direkt** auf das System zu, **verändert** etwas | Angriff durch **Personen mit Bezug zum Unternehmen** |
| Erkennbarkeit | **sehr schwer** (keine Veränderung) | hinterlässt **Spuren** → Erkennung und ggf. Identifizierung möglich | schwer, Insider kennt Systeme |
| Betroffenes Schutzziel | **Vertraulichkeit** (unautorisierte Informations**gewinnung**) | **Integrität, Verfügbarkeit** (unautorisierte Informations**veränderung**) | alle |
| Beispiele | Abhören, Sniffing, *laut Skript:* Man-in-the-Middle | Ausnutzen von Sicherheitslücken, **Brute-Force**, **Maskierung (Spoofing)**, DoS | (ehemalige) Mitarbeiter, **Praktikanten, externe Mitarbeiter, Kooperationspartner, Zulieferer** |
| Schutz | **durchgängige Verschlüsselung + Verifizierung** | Patches, Zugriffskontrolle, IDS, Logging | Rechtekonzept (Need-to-know), Protokollierung, Sensibilisierung |

⚠️ **MitM-Ловушка:** im Skript (S. 17) wird MitM als Beispiel für einen **passiven** Angriff genannt. In vielen anderen Quellen (BSI, Eckert) gilt MitM als **aktiv**, weil der Angreifer sich aktiv dazwischenschaltet (ARP-Spoofing, s. Kap. 3) und Daten verändern kann.
> **Безопасный ответ:** *„Laut Skript zählt der Man-in-the-Middle-Angriff zu den passiven Angriffen, sofern der Angreifer die Kommunikation nur mitliest. Sobald er Daten verändert oder sich aktiv (z. B. per ARP-Spoofing) in die Verbindung einschaltet, handelt es sich um einen aktiven Angriff."* — так получаешь балл при любом мнении проверяющего.

**Sicherheitsvorfall (S. 17):** Ereignis, das die **Vertraulichkeit, Verfügbarkeit und Integrität** von Informationen, Geschäftsprozessen, IT-Diensten, IT-Systemen oder IT-Anwendungen mit **hohem oder sehr hohem Schutzbedarf** so beeinträchtigt, dass ein **großer Schaden** für Unternehmen/Behörde, Kunden oder Geschäftspartner entstehen kann.

**IT-Sicherheitsgesetz (in Kraft 25.07.2015, S. 17–18):**
- Betreiber **kritischer Infrastrukturen (KRITIS)**: Energie, Wasser, Gesundheit, Telekommunikation (u. a.)
- **Meldepflicht** schwerwiegender IT-Sicherheitsvorfälle **an das BSI**
- **Nachweis von Mindeststandards** der IT-Sicherheit
- BSI wertet Meldungen aus, sucht Angriffsmuster, **warnt** potenziell gefährdete Unternehmen
- ➕ Weiterentwickelt durch IT-SiG 2.0 (2021) und die EU-**NIS-2**-Richtlinie (deutlich mehr betroffene Unternehmen). Für den aktuellen Stand im Netz prüfen — im Skript nicht enthalten.

**Bedrohungslage (S. 18–19):**
- KES-Studie: **Malware** ist Bedrohung Nr. 1 (vor Irrtum/Nachlässigkeit eigener Mitarbeiter).
- Infektionswege: **E-Mail** > **WWW** (aktive Inhalte, Drive-by) > Speichermedien, Würmer.
- Gezielter Angriff: Hacker wählt Opfer aus. Malware: wird meist **vom Nutzer selbst unabsichtlich** ausgeführt (Webseite, Mail-Anhang).
- Microsoft Security Intelligence Report: Kriminalität ist **global**; Anfälligkeit hängt stark vom **Betriebssystem** ab (altes Windows XP anfälliger als Windows 8) → Updates/aktuelle Systeme!

---

## 1.5 Phasen eines Angriffs (S. 19–20) ⭐
(nach BSI, Abb. 1.5)

| Phase | Was passiert | Beispiele |
|---|---|---|
| **1. Angriffsinitiierung** | **Zweck** festlegen → **Ziel** bestimmen → **Angriffsart** wählen | Zweck: Vertraulichkeit, Integrität, Verfügbarkeit, Authentizität, **Imageschaden**. Ziel: Informationen, IT-Systeme, Software, Speichermedien, Kommunikationskanäle. Art: gezielt / breitflächig, ein oder mehrere Ziele |
| **2. Angriffsvorbereitung** | möglichst viele **Informationen sammeln**; **Werkzeuge** auswählen; **Tarnung** planen (unbemerkt bleiben, Rückverfolgung verhindern) | Werkzeuge: Schadsoftware, Datenträger, Geräte. Tarnung: **Anonymisierungsdienst, IP-Fälschung, Spoofing** |
| **3. Angriffsdurchführung** | eigentlicher Angriff; danach **Spuren verwischen** | DoS, Malware-Infiltration, Hacking; Angriffspunkte: **Browser, Mailprogramme, Webserver, Betriebssystem**; Logfiles löschen, Software verbergen |

**Связь с другими главами:** информацию собирают **Port-Scanning** (Kap. 3.5), маскируются через **TOR/Proxy** (Kap. 4), выполняют через **Exploits/Metasploit** (Kap. 3), следы ищет **IT-Forensik**.

➕ Für 1,0: verwandt ist die **Cyber Kill Chain** (Lockheed Martin): Reconnaissance → Weaponization → Delivery → Exploitation → Installation → Command & Control → Actions on Objectives. Только как дополнение, основная — BSI-схема из скрипта.

### 1.5.1 Angreifertypen (S. 20–22) ⭐ (Zuordnung)
| Typ | Kenntnisse | Motivation / Merkmal |
|---|---|---|
| **Hacker** | **hoch** (IT-Experte) | ursprünglich **positiv** besetzt; greift gezielt ein System an, um Kontrolle zu erlangen, nutzt alle verfügbaren Infos; folgt oft einer **Hackerethik** (CCC: Informationen sollen frei sein, **nicht in Daten von Privatpersonen schnüffeln**, Misstrauen gegenüber Autoritäten, Dezentralität) |
| **Cracker** | hoch | **kriminelle** Zwecke; Schutzmechanismen umgehen, unautorisierter Zugang, **Kopierschutz knacken**, illegaler **Handel mit Informationen** (von *cracking* = Schlösser knacken) |
| **Scriptkiddie** | **gering** | nutzt **fertige Programme/fremden Code** aus dem Internet, ohne ihn zu verstehen; oft Jugendliche; Motive: **Neugier bis Vandalismus** |
| **Phreaker** | mittel | nimmt die **Identität eines anderen** an, z. B. Nutzung **fremder, ungeschützter Internetzugänge** → persönlicher Vorteil (Gebühren sparen), Rückverfolgung endet beim Opfer |

➕ Ergänzend (Hacker-„Hüte"): **White Hat** (legal, mit Auftrag, z. B. Pentester), **Black Hat** (kriminell ≈ Cracker), **Grey Hat** (ohne Auftrag, aber ohne Schadensabsicht). Плюс **Innentäter**, **Hacktivisten**, **staatliche Akteure (APT)**, **organisierte Kriminalität**.

---

## 1.6 Rechtliche Aspekte (S. 22–23) ⭐

Grundsatz: Der **Versuch**, in ein fremdes Computersystem einzudringen, ist ein **Straftatbestand**.

| Paragraph StGB | Delikt | Суть по-русски |
|---|---|---|
| **§ 202a** | **Ausspähen von Daten** | получить доступ к защищённым данным, **обойдя защиту** |
| **§ 202b** | **Abfangen von Daten** | перехват данных при **передаче** (например, sniffing) *(в скрипте упомянут в цитате BSI)* |
| **§ 202c** | **Vorbereiten des Ausspähens und Abfangens von Daten** = „**Hackerparagraph**" | подготовка: создание, передача, распространение, приобретение хакерских инструментов или паролей для 202a/202b |
| **§ 303a** | **Datenveränderung** | незаконно удалить, подавить, сделать непригодными или изменить данные |
| **§ 303b** | **Computersabotage** | нарушение обработки данных, **важной** для другого (например, DDoS на фирму) |

**Hackerparagraph § 202c:**
- in Kraft seit **11. August 2007**
- betrifft auch Sicherheitsexperten, Webseitenbetreiber, Hersteller
- bestraft „das **Herstellen, Überlassen, Verbreiten oder Verschaffen** von ‚Hackertools', die bereits nach Art und Weise ihres Aufbaus darauf angelegt sind, **illegalen Zwecken** zu dienen"
- **2007: Strafanzeige gegen das BSI** wegen der Toolsammlung **BOSS** (enthielt **Nessus** und **Snort**)
- BSI-Stellungnahme: Nicht das Tool an sich ist strafbar, sondern nur, wer damit eine Straftat nach **§ 202a oder § 202b vorbereitet**.

**Fazit (wichtig für Begründungsfragen):**
- **Besitz** von Hacker-Tools ist **nicht automatisch strafbar** → entscheidend ist der **Vorsatz / Zweck** (Dual-Use).
- Aufspüren von Sicherheitslücken ist **legal**, wenn man **vom Inhaber beauftragt** wurde (Dienstleister, **Penetrationstest**) oder es im Rahmen der **akademischen Ausbildung** geschieht (vom Bundesjustizministerium bestätigt).

**Zuordnungs-Beispiele:**
| Szenario | § |
|---|---|
| Student scannt ohne Erlaubnis das Firmennetz des Nachbarn und liest dort Dateien | 202a |
| Angreifer schneidet im offenen WLAN Datenverkehr mit | 202b |
| Jemand verkauft im Darknet ein Programm zum Knacken fremder Passwörter mit dem Ziel, Konten zu übernehmen | 202c |
| Ransomware verschlüsselt Daten des Opfers | 303a (+ 303b, wenn Betrieb gestört) |
| DDoS legt den Webshop eines Unternehmens lahm | 303b |
| Pentester mit schriftlichem Auftrag nutzt Metasploit | **nicht strafbar** |

---

## 1.7 Sicherheitsmanagement (S. 23–24)

**Объяснение.** Технические меры сами по себе не работают: межсетевой экран бесполезен, если пароль админа висит на стикере. Поэтому нужен **управленческий процесс**.

**Arten von Maßnahmen ⭐ (часто «Nennen Sie je ein Beispiel»):**
| Art | Beispiele |
|---|---|
| **technisch** | Firewall, Verschlüsselung, Virenscanner, Backups |
| **organisatorisch** | Verantwortlichkeiten festlegen, **Schlüsselmanagement**, Passwort-Richtlinie, Notfallplan |
| **personell** | **Schulung, Einweisung, Sensibilisierung** der Mitarbeiter |
| **infrastrukturell** | **Gebäudesicherung**, Zutrittskontrolle zum Serverraum, Brandschutz, USV |

> **Def. 1.6 Sicherheitsmanagement ⭐:** *IT-Sicherheitsmanagement versucht, die mit Hilfe von Informationstechnik realisierten Produktions- und Geschäftsprozesse in Unternehmen und Organisationen systematisch gegen **beabsichtigte Angriffe (Security)** und **unbeabsichtigte Ereignisse (Safety)** zu schützen.*

**Verantwortung:**
- Informationssicherheit ist **Aufgabe der Leitungsebene** (Geschäftsführung): initiieren, organisieren, die **Security Policy** verabschieden.
- Geschäftsführer **haften** nicht selten **persönlich**.
- Operative Umsetzung und Kontrolle → **Sicherheitsmanagementsystem** (➕ ISMS).

**Die 4 Schritte des Sicherheitsmanagements ⭐:**
1. Entwicklung einer **Sicherheitspolitik** (Security Policy) — *что* хотим, цели, ответственность
2. Erstellung eines **Sicherheitskonzepts** — *как*: Bedrohungs- und Risikoanalyse, Maßnahmen, Verantwortlichkeiten
3. **Realisierung** der IT-Sicherheitsmaßnahmen
4. **Erhaltung** der IT-Sicherheit im laufenden Betrieb (Kontrolle, Anpassung, Audits)

➕ Это непрерывный цикл: **PDCA** (Plan – Do – Check – Act), лежащий в основе ISO 27001.

- Auswahl der Maßnahmen nach dem spezifischen **Schutzbedarf** der Institution; Maßnahmen aufeinander abstimmen → **angemessenes Sicherheitsniveau** (nicht maximal!).
- ➕ BSI-Schutzbedarfskategorien: **normal – hoch – sehr hoch**.
- Standardisierte Vorgehensmodelle: **ISO 2700x** (ISO/IEC 27001) und **BSI IT-Grundschutz**. Gemeinsam: strukturiertes Vorgehen, Kern = **IT-Sicherheitskonzept** (Maßnahmen + Vorgehen + Verantwortlichkeiten).

---

## 1.8 Risiko und Risikobewertung (S. 24–27) 🧮⭐

**Объяснение.** Ресурсы ограничены, поэтому нужно решить, **какие** меры принять первыми. Для этого оценивают риск: насколько **вероятно** событие и насколько **дорого** оно обойдётся. Риск возникает при **эксплуатации уязвимости**. Угрозы нельзя устранить полностью, их можно только учесть в плане и в расчётах. Аналогия из скрипта: можно отказаться от страховки домашнего имущества — пока ничего не случилось, это не проблема.

> **Def. 1.7 Risikomanagement ⭐:** *Risikomanagement bezeichnet den Prozess der Festlegung eines vertretbaren Risikowertes, der Beurteilung des aktuellen Risikowertes, der Umsetzung von Schritten, um das Risiko auf ein vertretbares Maß zu minimieren, und der Beibehaltung dieses Risikowertes.*

→ 4 части: **festlegen → beurteilen → minimieren → beibehalten**.

### 1.8.1 Die Risikoformel (DIN VDE 31000-2)
```
R(E) = p(E) · S(E)

R = Risiko (in €)          p = Eintrittswahrscheinlichkeit (in %, als Dezimalzahl rechnen!)
S = Schaden (in €)         E = beliebiges Ereignis
```
- Если p задана «в год» → R — ожидаемый ущерб **в год** (€/Jahr). ➕ В англоязычной литературе это **ALE** (Annual Loss Expectancy).
- Extremfälle (Abb. 1.7): **hohe p + hoher S** → hohes Risiko → **handeln**; **niedrige p + niedriger S** → niedriges Risiko → **akzeptieren**.

**Проблемы на практике (S. 25–26) — для вопросов «Diskutieren Sie die Grenzen»:**
- **p** schwer zu bestimmen: braucht laut Wahrscheinlichkeitstheorie **viele Messungen**.
- **S** schwer zu bestimmen: braucht genaue Kenntnis **interner Verrechnungssätze** (Betriebsstunden, Personal-, Hardwarekosten).
- Häufig wird mit **Annahmen** gearbeitet.
- Schaden: **primär** (Produktionsausfall, Wiederbeschaffungskosten) vs. **sekundär** (**Image- und Vertrauensverlust** — kaum in Euro messbar).
- → Risiko eher **qualitativ** als quantitativ, hilft aber **bei der Priorisierung** von Maßnahmen.

### 1.8.2 Wirtschaftlichkeit einer Maßnahme ➕🧮
Скрипт ставит вопрос «Lohnt sich die Firewall?» — для полного ответа нужна такая логика:
```
Nutzen der Maßnahme = R_vorher − R_nachher        (Risikoreduktion pro Jahr)
Maßnahme lohnt sich, wenn:  Nutzen > Kosten der Maßnahme pro Jahr
Kosten pro Jahr = Anschaffung / Nutzungsdauer + laufende Kosten (Wartung, Lizenzen, Personal)
```

### 1.8.3 Risikostrategien ➕
| Strategie | Bedeutung | Beispiel |
|---|---|---|
| **Vermeiden** | riskante Tätigkeit nicht durchführen | keinen Webshop betreiben |
| **Vermindern / Reduzieren** | p oder S durch Maßnahmen senken | Firewall (p ↓), Backup (S ↓) |
| **Übertragen / Abwälzen** | Risiko an Dritte geben | **Cyber-Versicherung**, Outsourcing |
| **Akzeptieren / Tragen** | bewusst hinnehmen (Restrisiko) | geringe p und S, Maßnahme teurer als Risiko |

⚠️ **Restrisiko** bleibt **immer** (vollständige Sicherheit unmöglich, vgl. 1.2.2).

### 1.8.4 Risikobewertung und Angreifermodell (S. 26–27)
Die Eintrittswahrscheinlichkeit ergibt sich aus:
- geschätztem **Aufwand** für den Angreifer,
- möglichem **Nutzen** für den Angreifer bei Erfolg,
- möglichen **Motiven**.

**Angreifermodell** beschrieben durch:
| Aspekt | Ausprägungen |
|---|---|
| **Angreifertyp** | Scriptkiddie, Hacker, eigener Mitarbeiter |
| **Kenntnisse** | keine … **Insiderwissen** |
| **Budget** | gering … staatlich |
| **Ziele / Motive** | Neugier, Spionage, finanzielle Interessen, Rache |

> Почему важно: от модели нарушителя зависит, *какие* меры осмысленны. Против Scriptkiddie хватит патчей и файрвола; против обиженного админа с Insiderwissen — нужны разделение прав и журналирование; против государства — почти ничего не хватит.

---

# Aufgaben — типичные и нетипичные

Сначала реши сам (закрой решение рукой или прокручивай медленно), потом сверяйся с решением под задачей. Возле каждой задачи указано: **[T]** = типичная, **[N]** = нетипичная/на перенос, **[1,0]** = на высший балл.

## A. Kontrollfragen aus dem Skript (S. 27) [T]

**A1. Erklären Sie den Unterschied zwischen Daten und Informationen.**
> **✅ Lösung:**

Daten sind die maschinenlesbare Repräsentation von Information; sie bestehen aus Zeichen, deren Aufbau einer festen Syntax folgt. Information entsteht erst durch die Interpretation der Daten in einem Kontext — sie besitzt Syntax **und** Semantik und hat dadurch einen Wert für ihren Besitzer. Beispiel: „32456" ist ein Datum; mit dem Kontext „Matrikelnummer von Max" wird es zur Information. Für die IT-Sicherheit heißt das: Eine Information kann in mehreren Datenobjekten vorliegen (Festplatte, Backup, IP-Paket), und alle müssen geschützt werden.


**A2. Der Mailverkehr eines Mitarbeiters wird durch einen Kollegen abgefangen und gelesen. Welches Schutzziel wird unmittelbar verletzt?**
> **✅ Lösung:**

**Vertraulichkeit** — unautorisierte Informationsgewinnung. Zusätzlich für volle Punkte: es ist ein **passiver** (nur lesen) und **interner** Angriff (Kollege = Insider). Strafrechtlich: § 202b StGB (Abfangen von Daten). Gegenmaßnahme: E-Mail-Verschlüsselung (S/MIME, PGP), TLS.


**A3. Was ist das Ziel der IT-Sicherheit?**
> **✅ Lösung:**

Nach Eckert: Unternehmen und ihre Werte schützen und wirtschaftliche Schäden durch Vertraulichkeitsverletzungen, Manipulationen oder Störungen der Verfügbarkeit verhindern. Da eine vollständige Verhinderung von Angriffen nicht möglich ist, geht es konkret um die **Minimierung von Risiken**: Angriffe frühzeitig erkennen und verhindern, und falls das nicht gelingt, das Schadensausmaß gering halten. Dazu gehören technische, organisatorische, personelle und infrastrukturelle Maßnahmen.


**A4. Was ist der Unterschied zwischen einem aktiven und einem passiven Angriff?**
> **✅ Lösung:**

| | passiv | aktiv |
|---|---|---|
| Eingriff | keiner, nur Mithören/Abfangen | direkter Zugriff, Veränderung |
| Spuren | kaum → schwer erkennbar | hinterlässt Spuren → erkennbar |
| Schutzziel | Vertraulichkeit | Integrität, Verfügbarkeit |
| Beispiel | Sniffing im WLAN | Brute-Force, Spoofing, DoS |
| Schutz | durchgängige Verschlüsselung | Patches, Zugriffskontrolle, IDS |


**A5. 🧮 Firewall: Schaden 50.000 €, Eintrittswahrscheinlichkeit 10 %, Firewall verhindert die Angriffe. Berechnen Sie das Risiko.**
> **✅ Lösung:**

R = p · S = 0,10 · 50.000 € = **5.000 €** (pro Jahr).
Bewertung (für volle Punkte): Da die Firewall die Angriffe laut IT-Abteilung verhindert, sinkt das Risiko auf ≈ 0 €. Die Firewall lohnt sich wirtschaftlich, wenn ihre **jährlichen Gesamtkosten** (Anschaffung verteilt auf die Nutzungsdauer + Betrieb + Wartung) **unter 5.000 €** liegen. Einschränkung: p und S sind nur Schätzwerte; sekundäre Schäden (Imageverlust) sind nicht enthalten → Risiko eher unterschätzt.


---

## B. Schutzziele zuordnen [T]
Bestimmen Sie für jedes Szenario das **primär** verletzte Schutzziel.

| # | Szenario |
|---|---|
| B1 | Ein DDoS-Angriff macht den Webshop 4 Stunden unerreichbar. |
| B2 | Ein Angreifer ändert unbemerkt die IBAN in einer Rechnung. |
| B3 | Ein Kunde bestreitet, eine Online-Bestellung aufgegeben zu haben, und der Shop kann es nicht beweisen. |
| B4 | Eine E-Mail scheint von der Hausbank zu kommen, stammt aber von Betrügern. |
| B5 | Ein Praktikant liest Gehaltsdaten in einem falsch freigegebenen Ordner. |
| B6 | Ransomware verschlüsselt alle Daten eines Krankenhauses. |
| B7 | Ein Forscher kann aus „anonymen" Studiendaten (PLZ + Geburtsdatum + Geschlecht) Personen identifizieren. |
| B8 | Die Festplatte des einzigen Servers fällt aus, ein Backup gibt es nicht. |
| B9 | Ein Angreifer loggt sich mit einem erratenen Passwort als Admin ein. |
| B10 | Der Admin löscht Logfiles, um seine Spuren zu verwischen. |

> **✅ Lösung:**

| # | Schutzziel | Kommentar |
|---|---|---|
| B1 | **Verfügbarkeit** | aktiver Angriff, § 303b |
| B2 | **Integrität** | unautorisiert **und** unbemerkt |
| B3 | **Verbindlichkeit** | Maßnahme: Signatur, Protokollierung |
| B4 | **Authentizität** (Datenauthentizität) | Phishing; Maßnahme: signierte Mails |
| B5 | **Vertraulichkeit** | interner Angriff, Ursache organisatorischer Mangel (falsche Rechte) |
| B6 | **Verfügbarkeit** (primär) + Integrität | wenn Daten zusätzlich abgezogen werden → auch Vertraulichkeit („Double Extortion") |
| B7 | **Anonymität** | Anonymitätsmenge zu klein / Quasi-Identifikatoren |
| B8 | **Verfügbarkeit** | Safety-Problem, technisches Versagen; Maßnahme: Redundanz, Backup |
| B9 | **Authentizität** (Instanzauthentizität) → Folgeschäden an C/I/A | Maßnahme: starke Passwörter, 2FA |
| B10 | **Verbindlichkeit / Zurechenbarkeit** (+ Integrität der Logs) | Beweissicherung wird ausgehebelt; Maßnahme: Logs auf separaten Server, Vier-Augen-Prinzip |


---

## C. Zuordnungsaufgaben (1a 2b 3c) [T]

**C1. Ordnen Sie die Begriffe den Definitionen zu.**
| | Begriff | | Definition |
|---|---|---|---|
| 1 | Schwachstelle | a | konkrete, potenzielle Gefahr mit Bezug zu einem Schutzziel |
| 2 | Bedrohung | b | Prüfung des Identitätsnachweises |
| 3 | Gefährdung | c | Schwäche eines Systems bzw. Punkt, an dem es verwundbar ist |
| 4 | Authentisierung | d | entsteht, wenn eine Bedrohung auf eine Schwachstelle trifft |
| 5 | Authentifizierung | e | Nachweis der behaupteten Identität durch ein Subjekt |
| 6 | Verwundbarkeit | f | Schwachstelle, über die Sicherheitsdienste umgangen oder getäuscht werden können |

> **✅ Lösung:** 1c · 2a · 3d · 4e · 5b · 6f

**C2. Ordnen Sie die Beispiele den BSI-Gefährdungskategorien zu.**
| | Beispiel | | Kategorie |
|---|---|---|---|
| 1 | Ein Hochwasser flutet das Rechenzentrum im Keller | a | Vorsätzliche Handlung |
| 2 | Mitarbeiter klickt aus Unachtsamkeit auf „Alle löschen" | b | Technisches Versagen |
| 3 | Ein RAID-Controller fällt aus | c | Höhere Gewalt |
| 4 | Ein Ex-Mitarbeiter stiehlt Kundendaten | d | Organisatorischer Mangel |
| 5 | Niemand ist für das Einspielen von Updates zuständig | e | Fahrlässigkeit |

> **✅ Lösung:** 1c · 2e · 3b · 4a · 5d

**C3. Ordnen Sie die Paragraphen zu.**
| | § StGB | | Delikt |
|---|---|---|---|
| 1 | 202a | a | Computersabotage |
| 2 | 202b | b | Datenveränderung |
| 3 | 202c | c | Ausspähen von Daten |
| 4 | 303a | d | Vorbereiten des Ausspähens und Abfangens von Daten |
| 5 | 303b | e | Abfangen von Daten |

> **✅ Lösung:** 1c · 2e · 3d · 4b · 5a

**C4. Ordnen Sie die Angreifertypen zu.**
| | Typ | | Beschreibung |
|---|---|---|---|
| 1 | Hacker | a | nutzt fremde Internetzugänge unter fremder Identität |
| 2 | Cracker | b | setzt Fähigkeiten kriminell ein, knackt Kopierschutz |
| 3 | Scriptkiddie | c | IT-Experte, ursprünglich positiv, folgt oft einer Ethik |
| 4 | Phreaker | d | nutzt fertige Tools ohne tiefes Verständnis |

> **✅ Lösung:** 1c · 2b · 3d · 4a

**C5. Ordnen Sie die Maßnahmen den Maßnahmenarten zu (technisch / organisatorisch / personell / infrastrukturell).**
Security-Awareness-Schulung · Zutrittskontrolle zum Serverraum · Festplattenverschlüsselung · Passwort-Richtlinie · Brandmeldeanlage · Zuständigkeiten für Backups festlegen · Virenscanner · Einweisung neuer Mitarbeiter

> **✅ Lösung:**

- **technisch:** Festplattenverschlüsselung, Virenscanner
- **organisatorisch:** Passwort-Richtlinie, Zuständigkeiten für Backups
- **personell:** Security-Awareness-Schulung, Einweisung neuer Mitarbeiter
- **infrastrukturell:** Zutrittskontrolle zum Serverraum, Brandmeldeanlage


**C6. Ordnen Sie die Schritte den Phasen eines Angriffs zu (Initiierung / Vorbereitung / Durchführung).**
Portscan des Zielnetzes · Logfiles löschen · Festlegen, dass Kundendaten gestohlen werden sollen · Kauf eines Exploit-Kits · Ausführen eines Exploits auf dem Webserver · Nutzung von TOR zur Verschleierung · Entscheidung für einen gezielten statt breitflächigen Angriff

> **✅ Lösung:**

- **Initiierung:** Ziel „Kundendaten stehlen" festlegen; Entscheidung gezielt statt breitflächig
- **Vorbereitung:** Portscan (Informationen sammeln); Kauf eines Exploit-Kits (Werkzeug); TOR (Tarnung, Rückverfolgung verhindern)
- **Durchführung:** Exploit ausführen; Logfiles löschen (Spuren verwischen)


---

## D. Richtig oder falsch? (mit Begründung) [T/N]
Bei solchen Aufgaben gibt es oft nur Punkte **mit Begründung**.

1. Durch konsequente IT-Sicherheit können Angriffe vollständig verhindert werden.
2. Ein passiver Angriff richtet sich typischerweise gegen die Integrität.
3. Pseudonymisierte Daten können mit Zusatzwissen wieder einer Person zugeordnet werden.
4. Der Besitz eines Portscanners ist nach § 202c StGB grundsätzlich strafbar.
5. Datensicherheit betrifft nur personenbezogene Daten.
6. Bei Authentizität ohne Verbindlichkeit weiß der Empfänger, von wem eine Nachricht stammt, kann es aber Dritten nicht beweisen.
7. Ein Zugriffs*versuch* ohne Erfolg ist kein Angriff.
8. Passwort + PIN ist eine Zwei-Faktor-Authentisierung.
9. Die Verantwortung für die Informationssicherheit liegt bei der IT-Abteilung.
10. Steganografie schützt die Vertraulichkeit, indem sie die Existenz einer Nachricht verbirgt.
11. Eine Gefährdung liegt bereits vor, wenn eine Bedrohung existiert.
12. Safety schützt vor beabsichtigten Angriffen.

> **✅ Lösung:**

1. **Falsch.** Vollständige Verhinderung ist nicht möglich; Ziel ist Risikominimierung, Restrisiko bleibt.
2. **Falsch.** Passive Angriffe verändern nichts → richten sich gegen die **Vertraulichkeit**.
3. **Richtig.** Über die Zuordnungsvorschrift/-tabelle ist die Pseudonymisierung umkehrbar.
4. **Falsch.** Strafbar ist nur, wer damit eine Straftat nach § 202a/b **vorbereitet**; Besitz allein nicht (Dual-Use, Pentest, Lehre).
5. **Falsch.** Datensicherheit betrifft Daten **jeder Art**, auch ohne Personenbezug; das ist gerade der Unterschied zum Datenschutz.
6. **Richtig.** Beispiel 1.3 (Bob übergibt Alice persönlich einen Brief).
7. **Falsch.** Def. 1.5: auch der nicht autorisierte Zugriffs**versuch** ist ein Angriff.
8. **Falsch.** Beides ist Faktor **Wissen** → zwei gleiche Faktoren, keine echte 2FA.
9. **Falsch.** Sie ist Aufgabe der **Leitungsebene/Geschäftsführung** (verabschiedet die Security Policy, haftet ggf. persönlich); die IT setzt operativ um.
10. **Richtig.** Covert Channel / Steganografie: zweiter Weg zur Vertraulichkeit neben der Verschlüsselung.
11. **Falsch.** Gefährdung entsteht erst, wenn die Bedrohung auf eine **Schwachstelle trifft**.
12. **Falsch.** Safety = Funktionssicherheit, Schutz vor **unbeabsichtigten** Fehlern; Security schützt vor beabsichtigten Angriffen.


---

## E. Rechenaufgaben 🧮

**E1 [T]. Ein Mailserver war im Jahr 2026 insgesamt 13 Stunden ausgefallen. Berechnen Sie die Verfügbarkeit.**
> **✅ Lösung:**

2026 ist kein Schaltjahr → 365 · 24 = 8760 h.
V = (8760 − 13) / 8760 = 8747 / 8760 ≈ **0,99852 = 99,852 %**


**E2 [T]. Ein SLA garantiert 99,95 % Verfügbarkeit. Wie viele Stunden Ausfall pro Jahr sind maximal erlaubt? Wie viele pro Monat (30 Tage)?**
> **✅ Lösung:**

Jahr: 8760 h · (1 − 0,9995) = 8760 · 0,0005 = **4,38 h** (≈ 4 h 23 min)
Monat: 720 h · 0,0005 = **0,36 h = 21,6 min**


**E3 [N]. Ein Webdienst besteht aus Webserver (99 %), Datenbank (99,5 %) und Netzwerkanbindung (99,9 %). Alle drei werden benötigt. Wie hoch ist die Gesamtverfügbarkeit? Was passiert, wenn man den Webserver redundant (zwei gleiche Server, einer reicht) auslegt?**
> **✅ Lösung:**

Reihenschaltung: A = 0,99 · 0,995 · 0,999 ≈ **0,98406 = 98,41 %** → schlechter als jede einzelne Komponente.
Webserver redundant: A_Web = 1 − (1 − 0,99)² = 1 − 0,0001 = 0,9999
A_ges = 0,9999 · 0,995 · 0,999 ≈ **0,99391 = 99,39 %**
Fazit: Redundanz der schwächsten Komponente bringt am meisten (Skript: redundante Hardware verbessert die Verfügbarkeit). Jetzt ist die Datenbank der Engpass.


**E4 [T]. Berechnen Sie das Risiko und priorisieren Sie die Maßnahmen.**
| Ereignis | p pro Jahr | Schaden |
|---|---|---|
| Ransomware-Befall | 5 % | 400.000 € |
| Ausfall eines Switches | 30 % | 8.000 € |
| Laptop-Diebstahl (unverschlüsselt) | 20 % | 25.000 € |
| Brand im Serverraum | 0,5 % | 1.000.000 € |

> **✅ Lösung:**

| Ereignis | R = p · S | Rang |
|---|---|---|
| Ransomware | 0,05 · 400.000 = **20.000 €** | 1 |
| Laptop-Diebstahl | 0,20 · 25.000 = **5.000 €** | 2 |
| Brand | 0,005 · 1.000.000 = **5.000 €** | 2 (gleich) |
| Switch | 0,30 · 8.000 = **2.400 €** | 4 |

Priorität: Ransomware zuerst (Backups, Patches, Schulung). Laptop und Brand haben gleiches rechnerisches Risiko, **aber** für 1,0 anmerken: Brand ist ein seltenes Ereignis mit **existenzbedrohendem** Schaden — rein nach R gleich, qualitativ trotzdem kritisch (Matrix: geringe p, sehr hoher S → nicht einfach akzeptieren, sondern z. B. **übertragen** (Versicherung) oder vermindern (Brandschutz, externes Backup)). Laptop-Diebstahl ist mit Festplattenverschlüsselung sehr billig zu reduzieren (Schaden sinkt auf Hardwarewert).


**E5 [N]. Eine Firewall kostet 9.000 € Anschaffung (Nutzungsdauer 3 Jahre) plus 1.500 € Wartung pro Jahr. Ohne Firewall: p = 15 %, S = 60.000 €. Mit Firewall sinkt p auf 3 %. Lohnt sich die Firewall?**
> **✅ Lösung:**

R_vorher = 0,15 · 60.000 = 9.000 €/Jahr
R_nachher = 0,03 · 60.000 = 1.800 €/Jahr
Nutzen = 9.000 − 1.800 = **7.200 €/Jahr**
Kosten/Jahr = 9.000 / 3 + 1.500 = **4.500 €/Jahr**
7.200 € > 4.500 € → **lohnt sich**, Nettonutzen 2.700 €/Jahr. Restrisiko 1.800 €/Jahr bleibt.
Einschränkung: Werte sind Schätzungen; Imageschäden nicht enthalten → in der Realität eher noch vorteilhafter.


**E6 [N]. Ab welcher Eintrittswahrscheinlichkeit lohnt sich ein Backup-System für 2.000 €/Jahr, wenn ein Datenverlust 80.000 € kostet und das Backup den Schaden auf 5.000 € reduziert?**
> **✅ Lösung:**

Nutzen = p · 80.000 − p · 5.000 = p · 75.000
Lohnt sich, wenn p · 75.000 > 2.000 → p > 2.000 / 75.000 ≈ **0,0267 = 2,67 %**
Hier senkt die Maßnahme **S** (nicht p) — gutes Beispiel für „Risiko vermindern" über die Schadenshöhe.


**E7 [N]. Zwei Maßnahmen gegen Ransomware (S = 200.000 €, p = 10 %): (a) Mitarbeiterschulung 3.000 €/Jahr, senkt p auf 6 %; (b) Offline-Backup 5.000 €/Jahr, senkt S auf 30.000 €. Welche Maßnahme(n) empfehlen Sie?**
> **✅ Lösung:**

R_0 = 0,10 · 200.000 = 20.000 €
(a) R = 0,06 · 200.000 = 12.000 → Nutzen 8.000 − 3.000 = **5.000 € netto**
(b) R = 0,10 · 30.000 = 3.000 → Nutzen 17.000 − 5.000 = **12.000 € netto**
(a)+(b): R = 0,06 · 30.000 = 1.800 → Nutzen 18.200 − 8.000 = **10.200 € netto**
Rein rechnerisch ist **(b) allein** am besten (12.000 > 10.200). Für 1,0 begründet ergänzen: (a) bringt rechnerisch nur noch 1.200 € zusätzlich bei 3.000 € Kosten, **reduziert aber Risiken, die nicht im Modell stehen** (Phishing → Datenabfluss/Vertraulichkeit, gegen den ein Backup nicht hilft). Empfehlung: (b) sicher, (a) mit qualitativer Begründung zusätzlich.


---

## F. Offene Fragen und Transfer [N] [1,0]

**F1. Grenzen Sie Safety und Security voneinander ab und erläutern Sie, warum das Sicherheitsmanagement beides berücksichtigen muss.**
> **✅ Lösung:**

Safety (Funktionssicherheit) bedeutet, dass die Ist-Funktionalität eines Systems der Soll-Funktionalität entspricht und keine unzulässigen Zustände auftreten; sie schützt vor unbeabsichtigten Ereignissen wie Hardwaredefekten oder Programmierfehlern und hängt eng mit der Zuverlässigkeit zusammen. Security (Informationssicherheit) schützt dagegen vor beabsichtigten, nicht autorisierten Handlungen wie Datenmanipulation oder unerlaubter Preisgabe. Laut Def. 1.6 schützt das Sicherheitsmanagement die IT-gestützten Geschäftsprozesse gegen beide: Ein Ausfall der Verfügbarkeit schadet dem Unternehmen gleichermaßen, egal ob er durch einen DDoS-Angriff (Security) oder einen Festplattendefekt (Safety) entsteht. Zudem ist Funktionssicherheit eine Voraussetzung für Informationssicherheit: Ein fehlerhaft arbeitendes System kann Sicherheitsmechanismen nicht zuverlässig durchsetzen.


**F2. Erläutern Sie den Zusammenhang zwischen Schwachstelle, Bedrohung, Gefährdung und Risiko an einem selbst gewählten Beispiel.**
> **✅ Lösung:**

Beispiel Webshop:
- **Schwachstelle:** Die Login-Seite ist anfällig für SQL-Injection (fehlerhafte Programmierung).
- **Bedrohung:** Kriminelle wollen Kundendaten stehlen und weiterverkaufen → Bezug zum Schutzziel Vertraulichkeit.
- **Gefährdung:** Diese Bedrohung trifft auf die Schwachstelle des konkreten Webshops → sie kann ausgenutzt werden.
- **Risiko:** Bewertung über R = p · S, z. B. p = 20 % pro Jahr (automatisierte Scanner finden solche Lücken häufig), S = 150.000 € (DSGVO-Bußgeld, Benachrichtigung, Imageschaden) → R = 30.000 €/Jahr.
- **Maßnahme:** Prepared Statements (beseitigt die Schwachstelle), WAF, Penetrationstest.
Ohne Schwachstelle keine Gefährdung — deshalb ist die Identifikation von Schwachstellen so wichtig.


**F3. Ein Unternehmen sagt: „Wir verschlüsseln alle Daten, deshalb brauchen wir keine Zugriffskontrolle." Nehmen Sie Stellung.**
> **✅ Lösung:**

Die Aussage ist falsch. Verschlüsselung schützt Daten bei der **Übertragung** und **Speicherung** vor Unbefugten (Vertraulichkeit) und kann Manipulationen erkennbar machen (Integrität). Zur Verarbeitung müssen Daten aber **entschlüsselt** werden; wer Zugang zum laufenden System hat, sieht sie im Klartext. Außerdem schützt Verschlüsselung nicht vor dem **Löschen** (Verfügbarkeit) und nicht vor Innentätern mit Schlüsselzugriff. Laut Skript sind Verschlüsselung und Zugriffskontrolle die zwei **komplementären** Grundansätze der Schutzmechanismen; „zur Gewährleistung der Vertraulichkeit ist zusätzlich eine Kontrolle des Zugriffs erforderlich". Rechtekontrolle ist der Kern jedes sicheren IT-Systems.


**F4. Warum genügt es nicht, nur technische Maßnahmen umzusetzen? Nennen Sie für jede der vier Maßnahmenarten ein Beispiel.**
> **✅ Lösung:**

IT-Systeme sind soziotechnisch: Der Mensch ist Teil des Systems und oft das schwächste Glied (Social Engineering, Fahrlässigkeit, Phishing ist Infektionsweg Nr. 1). Eine Firewall nützt nichts, wenn ein Mitarbeiter das Admin-Passwort am Telefon herausgibt oder der Serverraum offen steht. Beispiele: technisch — Firewall; organisatorisch — Passwort-Richtlinie, Verantwortlichkeiten; personell — Security-Awareness-Schulung; infrastrukturell — Zutrittskontrolle zum Serverraum.


**F5. Ein Student findet beim Surfen zufällig eine offene Admin-Oberfläche der Hochschule und probiert die Standardpasswörter aus. Bewerten Sie die Situation rechtlich und aus Sicht der IT-Sicherheit.**
> **✅ Lösung:**

**Rechtlich:** Das Ausprobieren von Passwörtern ist ein nicht autorisierter Zugriffsversuch = Angriff (Def. 1.5). Gelingt der Zugriff unter Überwindung einer Zugangssicherung, kommt § 202a StGB (Ausspähen von Daten) in Betracht. Es liegt **kein Auftrag** des Inhabers vor; der akademische Rahmen deckt nur Übungen in dafür vorgesehenen Umgebungen ab, nicht reale Systeme der Hochschule. → Nicht ausprobieren, sondern melden (➕ Responsible Disclosure).
**IT-Sicherheit:** Schwachstellen: öffentlich erreichbare Admin-Oberfläche + ggf. Standardpasswörter (Ursache: unzureichende Beachtung von Sicherheitsanforderungen / organisatorischer Mangel). Bedroht: Authentizität, in der Folge Vertraulichkeit, Integrität, Verfügbarkeit. Maßnahmen: Admin-Zugang nur aus internem Netz/VPN, Standardpasswörter ändern, 2FA, Logging (Beweissicherung).


**F6. Warum lässt sich das Risiko in der Praxis oft nur qualitativ bestimmen? Wozu ist die Formel R = p · S trotzdem nützlich?**
> **✅ Lösung:**

p erfordert nach der Wahrscheinlichkeitstheorie viele Messungen, die für seltene Sicherheitsvorfälle fehlen; S erfordert genaue interne Verrechnungssätze (Personal-, Hardwarekosten, Betriebsstunden), und sekundäre Schäden wie Image- und Vertrauensverlust lassen sich kaum in Euro beziffern. Daher arbeitet man mit Annahmen und Schätzungen. Trotzdem ist die Formel nützlich, weil sie (1) Risiken **vergleichbar** macht und so eine **Priorisierung** der Maßnahmen ermöglicht, (2) eine **Kosten-Nutzen-Abwägung** von Investitionen unterstützt, und (3) die Diskussion über Risiken strukturiert. Außerdem hängt p stark vom zugrunde gelegten **Angreifermodell** ab (Typ, Kenntnisse, Budget, Motive).


**F7. [1,0] Beim elektronischen Vertragsabschluss: Welche Schutzziele müssen erfüllt sein, und warum reicht Authentizität allein nicht?**
> **✅ Lösung:**

- **Integrität:** Der Vertragstext darf nach Unterschrift nicht unbemerkt geändert werden.
- **Authentizität:** Die Vertragsparteien müssen eindeutig identifiziert sein.
- **Verbindlichkeit:** Keine Partei darf später abstreiten können, unterschrieben zu haben — entscheidend vor Gericht.
- **Vertraulichkeit:** je nach Inhalt (Preise, Konditionen).
- **Verfügbarkeit:** des Signatur-/Vertragssystems zum gewünschten Zeitpunkt.
Authentizität allein reicht nicht: Wie in Beispiel 1.3 (Bob übergibt Alice einen Brief) kann der Empfänger zwar selbst sicher sein, von wem die Nachricht stammt, dies aber einem Dritten (Gericht) nicht beweisen. Erst eine **digitale Signatur**, die nur der Unterzeichner erzeugen kann, schafft Verbindlichkeit.


**F8. [N] Eine Arztpraxis speichert Patientendaten auf einem PC am Empfang, ohne Backup, mit dem Passwort „praxis2024", das am Monitor klebt. Führen Sie eine kurze Sicherheitsanalyse durch.**
> **✅ Lösung:**

Struktur für volle Punkte: **Schutzbedarf → Schwachstellen → Bedrohungen/Kategorien → betroffene Schutzziele → Maßnahmen**.
- **Schutzbedarf:** sehr hoch — Gesundheitsdaten sind besonders sensible personenbezogene Daten (Datenschutz, ➕ Art. 9 DSGVO), dazu ärztliche Schweigepflicht.
- **Schwachstellen:** schwaches Passwort, offen sichtbar; kein Backup; PC am öffentlich zugänglichen Empfang.
- **Bedrohungen (BSI-Kategorien):** Fahrlässigkeit (Passwort am Monitor), technisches Versagen (Festplattendefekt), vorsätzliche Handlung (Patient liest Daten / Diebstahl des PCs, Ransomware), organisatorische Mängel (keine Backup- und Passwortregeln), höhere Gewalt (Wasserschaden).
- **Betroffene Schutzziele:** Vertraulichkeit (Einsicht durch Unbefugte), Verfügbarkeit (Datenverlust ohne Backup), Integrität (Manipulation von Befunden), Verbindlichkeit (gemeinsames Passwort → nicht zurechenbar, wer was geändert hat).
- **Maßnahmen:** technisch — Festplattenverschlüsselung, persönliche Accounts, automatische Bildschirmsperre, regelmäßige verschlüsselte Backups (offline/extern); organisatorisch — Passwort-Richtlinie, Backup-Zuständigkeit; personell — Schulung des Personals; infrastrukturell — Bildschirm vom Patienten abgewandt, PC gegen Diebstahl sichern.


**F9. [N] Erklären Sie, warum Anonymität eine „ausreichend große Menge" erfordert, und geben Sie ein Beispiel, wie anonymisierte Daten trotzdem deanonymisiert werden können.**
> **✅ Lösung:**

Anonymität bedeutet, innerhalb einer Menge von Subjekten (Anonymitätsmenge) nicht identifizierbar zu sein. Je kleiner die Menge, desto leichter der Rückschluss: Bei einer anonymen Umfrage mit drei Teilnehmern lässt sich die Antwort oft einer Person zuordnen (Skript). Beispiel Deanonymisierung: Ein Datensatz ohne Namen, aber mit Postleitzahl, Geburtsdatum und Geschlecht — diese Kombination ist für einen großen Teil der Bevölkerung eindeutig und kann mit öffentlichen Daten (z. B. Wählerverzeichnis) verknüpft werden. Daher müssen bei der Anonymisierung auch indirekte Merkmale vergröbert werden (z. B. nur Geburtsjahr, nur die ersten zwei Ziffern der PLZ).


**F10. [N] Ordnen Sie den Grundfunktionen sicherer Systeme jeweils einen konkreten Mechanismus beim Zugriff auf eine Datei in einem Unternehmensnetz zu.**
> **✅ Lösung:**

- Identifikation: Eingabe des Benutzernamens „mueller"
- Authentisierung: Eingabe des Passworts / Smartcard
- Rechteverwaltung: Admin trägt in der ACL des Ordners „Vertrieb: lesen" ein
- Rechteprüfung: Das Betriebssystem prüft beim Öffnen der Datei die ACL
- Beweissicherung: Zugriff wird im Audit-Log mit Zeit und Benutzer protokolliert


---

## T. Transfer & Verständnis — Skript anwenden, nicht auswendig ⭐
> Такие задачи ближе к реальному экзамену: готового ответа в скрипте нет — надо **понять концепт и применить** к новой ситуации, скомбинировать темы, обосновать. Решай рассуждением, а не поиском фразы.

**T1 [1,0]. Ein Start-up will Kosten sparen und sagt: „Wir machen erst Sicherheit, wenn wir den ersten Angriff hatten." Bewerten Sie das mit den Begriffen Risiko, Schaden und Risikostrategie — und wann diese Haltung sogar vertretbar sein kann.**
> **✅ Lösungsweg:** Die Haltung ignoriert, dass **Risiko = p · S** *vor* dem Schaden gemanagt werden muss — nach dem ersten Vorfall ist der (ggf. existenzbedrohende, auch **sekundäre** = Imageschaden) Schaden schon eingetreten. Sinnvoll ist eine **Risikoanalyse vorab** und je nach Ergebnis **vermindern/übertragen/akzeptieren**. **Vertretbar** ist „nichts tun" nur bewusst als **Risiko akzeptieren**, wenn p **und** S nachweislich sehr klein sind und eine Maßnahme teurer wäre als das erwartete Risiko (Restrisiko bleibt immer). Kernpunkt: nicht *ob* man rechnet, sondern *dass* man die Entscheidung begründet trifft.

**T2 [N]. Ein Mitarbeiter nutzt für alles den Admin-Account „weil es praktischer ist". Verbinden Sie das mit drei Konzepten aus Kapitel 1 und erklären Sie die Gefahr.**
> **✅ Lösungsweg:** (1) Verletzt **least privilege / restriktive Rechtevergabe** — er hat mehr Rechte als nötig. (2) Eine Schwachstelle (Fehlklick/Malware) wird so zur **Gefährdung mit maximalem Schaden**, weil Schadcode **alle Admin-Rechte erbt** → trifft Vertraulichkeit, Integrität **und** Verfügbarkeit zugleich. (3) **Beweissicherung/Zurechenbarkeit** leidet: wenn alle unter „admin" arbeiten, ist im Log nicht nachvollziehbar, wer was tat → Verbindlichkeit weg. Maßnahme: getrennte Accounts, Admin nur bei Bedarf.

**T3 [1,0]. Ordnen Sie zu, ob jeweils Safety oder Security betroffen ist, und begründen Sie bei Grenzfällen: a) ein Bug lässt die Produktionssteuerung bei Schaltjahren abstürzen; b) ein Angreifer bringt dieselbe Steuerung per manipulierter Eingabe zum Absturz; c) ein Blitz zerstört den Server.**
> **✅ Lösungsweg:** a) **Safety** — unbeabsichtigter Funktionsfehler (Ist≠Soll). b) **Security** — beabsichtigter Angriff, obwohl das **Symptom** (Absturz, Verfügbarkeit weg) identisch ist → zeigt: entscheidend ist **Absicht/Ursache**, nicht die Wirkung. c) **Safety** (höhere Gewalt, unbeabsichtigt), BSI-Kategorie *höhere Gewalt*. Pointe: dieselbe Wirkung (Ausfall) kann Safety **oder** Security sein — das Sicherheitsmanagement muss beides abdecken (Def. 1.6).

**T4 [N]. Warum ist „der Mensch das schwächste Glied" eine logische Folge aus der Definition des IT-Systems als *soziotechnisch*? Leiten Sie es her.**
> **✅ Lösungsweg:** Ein IT-System ist laut Skript **soziotechnisch** = Technik **+ Mensch**. Die Sicherheit eines Systems ist höchstens so stark wie ihr schwächstes Element. Technische Mittel (Krypto, Firewall) kann man beliebig härten, den Menschen nicht „patchen" — er kann getäuscht werden (Social Engineering/Phishing, Infektionsweg Nr. 1). Also wandert der günstigste Angriffspunkt automatisch zum Menschen → deshalb sind **personelle Maßnahmen (Schulung/Sensibilisierung)** unverzichtbar und nicht nur „nice to have".

## G. Самопроверка перед переходом к Kap. 2 (с ответами)
Сначала попробуй ответить вслух без подсказки, потом сверься.

**G1. 6 Schutzziele (engl.) + Maßnahme**
> Vertraulichkeit (confidentiality) – Verschlüsselung · Integrität (integrity) – Hash/MAC/Signatur, Zugriffskontrolle · Verfügbarkeit (availability) – Redundanz, Backup, DoS-Schutz · Verbindlichkeit (non-repudiation/accountability) – digitale Signatur, Logfiles · Authentizität (authenticity) – Passwort, Challenge-Response, Signatur · Anonymität/Pseudonymität (anonymity) – Pseudonyme, Aggregation, TOR

**G2. Authentisierung / Authentifizierung / Autorisierung**
> Authentisierung = das **Subjekt** weist seine Identität nach (gibt Passwort ein). Authentifizierung = das **System prüft** diesen Nachweis. Autorisierung = das System entscheidet, **was** das Subjekt darf (Rechteprüfung).

**G3. Schwachstelle → Bedrohung → Gefährdung (Beispiel)**
> Schwachstelle: ungepatchter Webserver. Bedrohung: Ransomware-Gruppe, die Daten verschlüsseln will (Bezug zum Schutzziel Verfügbarkeit). Gefährdung: die Bedrohung trifft auf die Schwachstelle dieses konkreten Servers und kann ausgenutzt werden. Risiko = p · S.

**G4. 5 BSI-Gefährdungskategorien + Beispiel**
> Höhere Gewalt (Blitz) · Vorsätzliche Handlungen (Malware, Diebstahl) · Technisches Versagen (defekte Festplatte) · Fahrlässigkeit / menschliches Fehlverhalten (versehentlich gelöscht) · Organisatorische Mängel (keine Regelungen)

**G5. Paragraphen**
> §202a Ausspähen von Daten · §202b Abfangen von Daten · §202c Vorbereiten des Ausspähens und Abfangens („Hackerparagraph", seit 11.08.2007) · §303a Datenveränderung · §303b Computersabotage

**G6. 3 Phasen + 4 Angreifertypen**
> Phasen: Angriffsinitiierung → Angriffsvorbereitung → Angriffsdurchführung. Typen: Hacker, Cracker, Scriptkiddie, Phreaker.

**G7. Formeln**
> Verfügbarkeit = (Gesamtlaufzeit − Ausfallzeit) / Gesamtlaufzeit; rückwärts: Ausfallzeit = Gesamtlaufzeit · (1 − V). Risiko R = p · S. Maßnahme lohnt sich, wenn (R_vorher − R_nachher) > Kosten pro Jahr.

**G8. 4 Schritte Sicherheitsmanagement + 4 Maßnahmenarten**
> Schritte: Sicherheitspolitik → Sicherheitskonzept → Realisierung der Maßnahmen → Erhaltung im laufenden Betrieb. Maßnahmenarten: technisch, organisatorisch, personell, infrastrukturell.

Если на всё ответил без подсказки, глава закрыта, переходим к `02_Rechnernetze.md`.
