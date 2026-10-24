# Kapitel 7 — Zugriffs- und Informationsflusskontrolle
**Quelle: Foliensatz `EITS_Zugriffskontrolle` (41 Folien)**

> **О чём глава.** Как система решает, **кто (Subjekt)** может **что (Operation)** делать с **чем (Objekt)**. Модели: Zugriffsmatrix (+HRU), ACL/Capabilities, четыре подхода (DAC/IBAC/MAC/RBAC), и формальные модели безопасности — **Bell-LaPadula** (защита Vertraulichkeit через уровни секретности) и **Chinese Wall** (конфликт интересов). Плюс Unix-права (chmod, 12 бит).
>
> **Почему важно для оценки 1,0:** классика экзамена — заполнить/прочитать **Zugriffsmatrix**, отличить **DAC vs. MAC**, применить правила **Bell-LaPadula (no-read-up / no-write-down)** к сценарию с уровнями секретности, разобрать **Chinese-Wall** по истории доступа, посчитать **Unix-права в восьмеричном виде** (rwx = 421). Это сильно «задачный» материал.
>
> **Связь:** Zugriffskontrolle = одна из 5 Grundfunktionen ([01 → 1.3.7](01_Einfuehrung.md)); «Rechtekontrolle ist der Kern jedes sicheren IT-Systems»; restriktive Rechtevergabe = least privilege. Referenzmonitor проверяет каждый запрос.
>
> **Обозначения:** ⭐ очень вероятно · 🧮 расчёт · ⚠️ ловушка · ➕ сверх слайдов.

## Содержание
Einleitung (least privilege) · Zugriffskontrollmodelle (Subjekt/Objekt/Referenzmonitor) · 4 Ansätze: **DAC/IBAC/MAC/RBAC** ⭐ · Zugriffsmatrix + ACL/Capabilities ⭐ · HRU-Modell · **RBAC** (+ Rollenhierarchie) ⭐ · **Bell-LaPadula** ⭐ · **Chinese Wall** ⭐ · **Unix-Rechte / chmod** 🧮⭐ · **Aufgaben A–G**

---

## Einleitung — zentrales Prinzip (Folie 4)

**Restriktive Rechtevergabe (restriktive Rechtevergabe) ⭐:**
> *Einem Prozess sollen nur diejenigen Zugriffe erlaubt sein, die für die Erfüllung seiner Aufgabe **unverzichtbar** sind.*
> = **principle of least privilege** + **need-to-know principle**.

- ⚠️ Dieses Prinzip wird durch **Autorisierung verletzt**: Beispiel — ein **Editor** kann durch seinen Benutzer nicht daran gehindert werden, heimlich Daten in eine fremde Datei zu kopieren („Trojanisches Pferd"). → DAC allein reicht gegen Informationsflüsse nicht (→ dafür MAC/Bell-LaPadula).

**Zugriffskontrollmodelle (Folie 3):**
- beschreiben **auf logischer Ebene**, auf welche **Art und Weise** Benutzer Zugriff auf IT-Systeme erhalten.
- bestehen aus: **Objekten** (Benutzer, Prozesse, ACLs, Risikoklassen, Gruppen, Rollen), **Beziehungen** zwischen ihnen und **Regeln** über den **Zugriffsentscheid**.

**Grundbegriffe ⭐ (immer sauber benennen):**
| Begriff | Bedeutung |
|---|---|
| **Subjekt (S)** | aktiver Akteur: Benutzer / Prozess, der zugreifen will |
| **Objekt (O)** | passive Ressource: Datei, Drucker, Verzeichnis, Prozess … |
| **Operation / Recht (r)** | was getan wird: read, write, execute … |
| **Referenzmonitor** | Instanz, die **jeden** Zugriff gegen die **Richtlinie (Policy)** prüft → gewährt oder lehnt ab (Folie 5) |

---

## Die 4 Ansätze der Zugriffskontrolle (Folie 5) ⭐⭐

| Ansatz | voll | Steuerung durch | Kerngedanke |
|---|---|---|---|
| **DAC** — Discretionary Access Control | *Ermessensbezogen* | der **Eigentümer** des Objekts | Besitzer entscheidt nach **eigenem Ermessen**, wer zugreift |
| **IBAC** — Identity Based Access Control | *Identitätsgesteuert* | die **Identität** des Benutzers | Rechte an die konkrete **Identität** gebunden |
| **MAC** — Mandatory Access Control | *zwingend/per Verpflichtung* | das **System** (zentrale Regeln) | **systembestimmte** Regeln, der Benutzer kann sie nicht ändern |
| **RBAC** — Role Based Access Control | *mit Rollenmodell* | zugewiesene **Rollen** | Rechte hängen an **Rollen**, nicht an Personen |

### Discretionary Access Control (DAC) (Folie 6) ⭐
- **Besitzer bestimmt** die Zugriffsrechte seiner Objekte (engl. *discretionary* = nach Ermessen) und kann Rechte **weitergeben** (an andere Benutzer übertragen).
- Realisiert meist über **Gruppen und Eigentümer**; Kontrolle via Benutzeridentifikation (Login mit Name + Passwort).
- ⚠️ **Nachteile:** anfällig für **Schadsoftware** (Trojaner erbt die Rechte des Benutzers!), hoher Wartungsaufwand für ACLs/Capabilities, begrenzte negative Berechtigungen.
- → typisch für **Unix/Windows-Dateirechte**.

### Identity Based Access Control (IBAC) (Folie 7)
- Zugriff basiert auf der **Identität** des Benutzers; Rechte sind konkreten Identitäten auf bestimmte Objekte zugeordnet.
- Ein **Prozess**, der im Namen eines Benutzers läuft, erhält **dessen Berechtigungen**. (Oft als Spezialfall/Grundlage von DAC gesehen.)

### Mandatory Access Control (MAC) (Folie 8) ⭐
- *„zwingend erforderliche Zugangskontrolle"* — **regelbasierte, systembestimmte** Strategie.
- Entscheidung **nicht nur** über die Identität von Subjekt/Objekt, sondern über **zusätzliche Regeln und Eigenschaften**: **Kategorisierungen, Kennzeichnungen (Labels), Code-Wörter**.
- ⚠️ Benutzer/Besitzer **kann die Regeln nicht ändern** — im Gegensatz zu DAC. → Grundlage von **Bell-LaPadula**.

> **DAC vs. MAC ⭐ (die wichtigste Abgrenzung):**
> | | **DAC** | **MAC** |
> |---|---|---|
> | Wer vergibt Rechte? | der **Eigentümer** (nach Ermessen) | das **System** (feste Regeln/Labels) |
> | Kann Besitzer Rechte weitergeben? | **ja** | **nein** |
> | Schutz gegen Informationsfluss (Trojaner)? | **nein** | **ja** |
> | Beispiel | Unix-Dateirechte | Bell-LaPadula, SELinux, militärische Einstufung |

---

## Zugriffsmatrix-Modell (Folien 10–13) ⭐

**Объяснение.** Простейшая и старейшая модель (= **Zugriffsmatrix / access matrix / Referenzmonitor-Modell**). Таблица: **строки = Subjekte**, **столбцы = Objekte**, в ячейке **M(s,o) = множество прав** субъекта s на объект o. Это и есть **Schutzzustand** системы.

**Beispiel-Matrix (Folie 11):**
| S\O | s1 :Segment | pr2 :Printer | d3 :Directory | m4 :Mailbox | p3 :Process |
|---|---|---|---|---|---|
| **S1** | | write | search, enter | | |
| **S2** | | write | search, update, delete | put | |
| **S3** | read, delete | | | put | block |
| **S4** | read, write | write, block, unblock | | put, get | wakeup |

> **Matrixelement = Menge von Rechten.** Rechte sind **objektspezifisch** (у Printer — `write/block`, у Mailbox — `put/get`).

**Varianten (Folie 10):** **statisch** (напр. Paketfilter в Firewall) vs. **dynamisch** (матрица меняется командами из элементарных операций → HRU).

### Zwei Sichten der Matrix ⭐⭐ (Folie 12 — очень частый вопрос)
| | **Schutzstatus (ACL)** | **Berechtigungen (Capabilities)** |
|---|---|---|
| = Teil der Matrix | **Spalte** (столбец) | **Zeile** (строка) |
| gespeichert beim | **Objekt** | **Subjekt** |
| Gruppierung | typischerweise Subjekte zu **Gruppen** | — |
| Weitergabe von Rechten | **schlechter** | **besser** |
| Zurücknahme (Entzug) von Rechten | **besser** | **schlechter** |

- **ACL (Access Control List)** = Liste beim **Objekt**: «кто и что может с этим объектом». Содержит **ACEs** (Access Control Entries), которые права **выдают или отбирают** (positive/negative ACEs). Пример (Folie 30):
  ```
        O1    O2   O3
  S1    rwx    -    -
  S2     -    w     -
  S3    rx     -    x
  ```
  Auswertung einer ACL: zuerst **negative ACEs** (was ist entzogen?), dann **positive ACEs** (was ist erlaubt?) → unlogische Kombinationen verhindert.
- **Capability** = Liste beim **Subjekt**: «что мне разрешено». Концептуально это **abstrakter Datenwert vom Typ Capability**, «fälschungssicher/unforgeable» (Folie 9). Операции над Capabilities: **Erzeugen, Kopieren/Weitergabe, Weitergabe mit Einschränkung (restriction), Weitergabe mit Erweiterung (amplification — siehe DAC), Entzug (revocation), Löschen** (перезапись null-правом).

> **Запоминалка:** **ACL — по столбцу (у Objekt)**, **Capability — по строке (у Subjekt)**. ACL хорош для «кто имеет доступ к файлу», Capability — для «что может пользователь».

### HRU-Modell (Folien 13–14) ⭐
**Harrison-Ruzzo-Ullman** — моделирует **динамическую** матрицу. Изменения через **6 Elementaroperationen**:
| Operation | Bedeutung |
|---|---|
| `enter r into M(s,o)` | Recht r **eintragen** |
| `delete r from M(s,o)` | Recht r **entfernen** |
| `create subject s` | Subjekt erzeugen |
| `delete subject s` | Subjekt löschen |
| `create object o` | Objekt erzeugen |
| `delete object o` | Objekt löschen |

Из них строятся **Kommandos** (Folie 14):
```
COMMAND com (s1,…,sn, o1,…,on)
  IF r1 in M(s1,o1) AND … AND rm in M(sm,om)
  THEN op1 , … , opn
END;
```
Семантика элементарных операций: `enter` → `M'(s,o) := M(s,o) ∪ {r}`; `delete` → `M'(s,o) := M(s,o) \ {r}` (при условии s∈S, o∈O).
➕ Теоретический результат: для HRU **Safety-Problem** (может ли право когда-либо «утечь») в общем случае **неразрешимо** (unentscheidbar).

---

## Role-Based Access Control (RBAC) (Folien 16–22) ⭐

**Объяснение.** Права привязаны не к людям, а к **ролям**. Человек получает роль, роль несёт права. Это сильно упрощает администрирование: сменился сотрудник — просто переназначил роль.

**Элементы и отношения (Folie 20):**
- **Benutzer (U)** ←→ **Rollen (R)** через **Benutzer-Rollen-Zuweisung (UA)**.
- **Rollen (R)** ←→ **Zugriffsrechte (P)** через **Rollen-Zugriffsrechte-Zuweisung (PA)**.
- Право **P** = доступные для **Objekt (O)** **Methoden (M)**.
- **Session (S)** — пользователь активирует для сессии нужные роли и получает связанные с ними права. Session = **zeitgebundenes Konstrukt**, связывающее Benutzer, Rollen, Rechte.

```
Nutzer ──Zuweisung(UA)──► Rolle ──Rollen-Berechtigung(PA)──► Berechtigungen (Methoden/Objekte)
   └──── Sitzung ────────────┘           ▲
                                    Rollen-Hierarchie
```

### Rollenhierarchie (Folie 17) ⭐
- Роли образуют **иерархию**: старшая роль **наследует** права младших.
- **Нотация:** `R1 ⇒ R2` означает, что **R2 тоже обладает правами R1**, то есть `Rj ≤ Ri` (нижняя роль «меньше»).
- Пример (клиника): Basis → {Verwaltung, Forschung, Med. Personal, Patient}; Forschung → Statistiker; Med. Personal → Schwester → {Tag-, Nacht-} → Oberschwester → Arzt → **Chefarzt** (наследует всё по цепочке вверх). Второй пример: Projekt-Mitglied → {Tester, Programmierer} → Projektleiter.

> **Преимущество RBAC:** меньше назначений (не «каждому пользователю каждое право», а «пользователь → роль → права»), легко менять. ⚠️ **Минус:** RBAC **не контролирует информационные потоки** (Folie 23) → для этого нужен Bell-LaPadula.

---

## Bell-LaPadula-Modell (BLP) (Folien 23–26) ⭐⭐

**Объяснение.** RBAC контролирует доступ, но не **куда утекает информация**. BLP (Bell & LaPadula, **1973**, первая формальная модель) — это **MAC-модель**, защищающая **Vertraulichkeit** через **уровни секретности (Multi-Level Security, MLS)** и метки (Labeling).

**Элементы:**
- **Zugriffsrechte R** = {read-only, append, execute, read-write, control}.
- **Sicherheitsklasse SC**, X ∈ SC, **X = (A, B)**:
  - **A = Sicherheitsmarke (Label)**, напр. *public < confidential < secret < top-secret*;
  - **B = Menge von Kategorien (compartments)**, напр. «Arzt», «Projekt X».
- Устанавливается **частичный порядок (partielle Ordnung)** над метками. **Информационные потоки разрешены только снизу вверх** (вдоль порядка).

### Die zwei Kernregeln ⭐⭐ (выучить железно)
| Regel | Bedeutung | Интуиция |
|---|---|---|
| **No-Read-Up** (simple security property) | Subjekt darf nur Objekte **≤ seiner** Stufe **lesen** | нельзя читать *выше* своего уровня (секрет не прочитаешь) |
| **No-Write-Down** (*-property, „Star") | Subjekt darf nur auf Objekte **≥ seiner** Stufe **schreiben** | нельзя писать *ниже* своего уровня (чтобы секрет не «стёк» вниз) |
| **Tranquility** | Sicherheitsklassen ändern sich **nicht** während des Betriebs | метки не меняются на ходу |

> **Почему no-write-down (контринтуитивно, но ключевое ⭐):** если бы субъект top-secret мог писать в public-файл, он (или троянец в его процессе) **перенёс бы секрет вниз** → утечка. Поэтому писать можно только «вверх или на свой уровень».
> Запоминалка: **«read down, write up»** (читай вниз, пиши вверх). Также: **No Read Up + No Write Down**.

**BLP-Beispiel (Folie 24):**
- Subjekt на уровне **confidential**: **читает** объект confidential (≤ своего — ✅ no-read-up соблюдён), **пишет** вверх к top-secret-объекту (✅ write up разрешён).
- Subjekt на уровне **top-secret** пишет вниз к confidential-объекту — помечено **«???»** → это и есть **запрещённый no-write-down**.

### BLP-Policies in UNIX (Folie 25) 🧮 — применение правил к операциям
`SC(S)` = уровень субъекта, `SC(O)` = уровень объекта:
| Operation | Bedingung |
|---|---|
| **read (file)** | SC(S) **≥** SC(O) → *no read up* |
| **exec (file)** | SC(S) ≥ SC(O) |
| **write (file)** | SC(S) **=** SC(O) |
| **append (file)** | SC(S) **≤** SC(O) → *no write down* (писать можно вверх) |
| search (directory) | SC(S) ≥ SC(O) |
| create (directory) | SC(S) = SC(O) |
| read/kill (signal/ipc) | SC(S) ≥ / = SC(O) |

### Grenzen des BLP (Folie 26) ⚠️
- **Проблема:** информация/объекты **постепенно получают всё более высокий гриф** (только вверх → всё ползёт в top-secret). Решение — ввести дополнительные правила/категории.
- BLP защищает только **Vertraulichkeit**, не Integrität (для целостности — модель **Biba** ➕, зеркальная: no-read-down, no-write-up).

---

## Chinese Wall Modell (Folien 27–29) ⭐

**Объяснение.** Моделирует **конфликт интересов** (напр. консультант, работающий с конкурирующими фирмами). Доступ зависит от **истории доступа (Zugriffshistorie)**: раз посмотрел данные одной фирмы отрасли — больше нельзя смотреть конкурента.

**Структура (двухуровневая иерархия объектов, Folie 27):**
```
Interessenskonfliktklassen (Branchen):   Ölgesellschaft        Bank
                                          /       \           /      \
Unternehmen:                            BP       Aral     Dresdner   Hypo
                                       /  \       / \       / | \      / \
Objekte:                             o1   o2    o3  o4    o5 o6 o7   o8  o9
```
- **COI (Conflict of Interest)-Klasse K** = отрасль (Banken, Ölgesellschaften).
- Фирмы одной отрасли (Dresdner и Hypo) — в одной COI-классе.

### COI-Regel (Folie 27) ⭐
> *Wer auf Objekte eines Unternehmens U in einer Konfliktklasse K zugreift, darf **nicht mehr** auf Objekte eines Unternehmens U' ≠ U (derselben Klasse K) zugreifen.*

→ После первого доступа вокруг субъекта «строится стена» (**Zugriffsmauer**, Folie 28): он заперт в выбранной фирме каждой отрасли.
- **Schreibzugriffe (запись)** разрешены только если **все предыдущие чтения** были на объекты **того же предприятия** или на **interessensfreie** объекты (Folie 29).
- ⚠️ **Проблемы:** снятие конфликта интересов **не предусмотрено** (однажды закрыто — навсегда); **история растёт неограниченно**.

---

## Zugriffsrechte unter Unix/Linux (Folien 31–40) 🧮⭐

**Объяснение.** Unix — классический **DAC**: у каждого файла есть **владелец (UID)** и **группа (GID)**, права заданы для трёх категорий: **владелец (user)**, **группа (group)**, **остальные (others)**.

### 12 Bits, rwx и восьмеричное представление (Folie 36) 🧮⭐
```
[ s  g  t ] [ r w x ] [ r w x ] [ r w x ]
 Spezialbits  Besitzer   Gruppe   andere
 suid sgid sticky
```
- Каждый символ права даёт восьмеричное значение: **r = 4, w = 2, x = 1** (сумма → цифра 0–7).
  | rwx | двоично | восьм. |
  |---|---|---|
  | rwx | 111 | **7** |
  | rw- | 110 | 6 |
  | r-x | 101 | **5** |
  | r-- | 100 | **4** |
  | -wx | 011 | 3 |
  | -w- | 010 | 2 |
  | --x | 001 | 1 |
  | --- | 000 | 0 |
- **Примеры (Folie 36):** `755` = `rwx r-x r-x`; `400` = `r-- --- ---`; `510` = `r-x --x ---`.
- **Spezialbits (Folie 37) ➕:** **suid** (при запуске — права владельца файла), **sgid** (права группы), **sticky bit** (в каталоге: удалять файл может только его владелец, напр. `/tmp`).

### Значение прав — файл vs. каталог ⭐⚠️ (Folien 34–35)
| Recht | für **Datei** | для **Verzeichnis** |
|---|---|---|
| **r** (read) | содержимое файла читать | **список** файлов каталога читать (`ls`) |
| **w** (write) | содержимое менять | файлы в каталоге **создавать/удалять/переименовывать** |
| **x** (execute) | файл **запускать** | в каталог **заходить** (`cd`), обращаться к файлам по имени |

> ⚠️ Частая ловушка: на каталоге `x` ≠ «выполнить», а «войти/пройти насквозь». Можно иметь `r` без `x` на каталоге (видишь имена, но не можешь зайти) и наоборот.

### chmod (Folien 38–39) 🧮
- **Variante 1 — numerisch:** `chmod 755 datei` · `chmod 471 passwd` · `chmod 700 ..` · `chmod 722 ./a/a1/dat.x`.
- **Variante 2 — symbolisch:** `u` (user/Besitzer), `g` (group), `o` (others), `a` (all); операции `+`/`−`/`=`; напр. `chmod g+w datei`, `chmod o-rwx datei`.
- ➕ Начальные права новых файлов задаёт **umask** (вычитается из базовых прав).

---

# Aufgaben — типичные и нетипичные
**[T]** типичная · **[N]** перенос · **[1,0]** высший балл · 🧮 расчёт. Решения сразу под задачей. *(Контрольных заданий в слайдах нет — блоки ниже я составил по материалу; восьмеричные расчёты перепроверены.)*

## A. Grundbegriffe & Zuordnung [T]

**A1. Ordnen Sie die vier Ansätze zu ihrer Steuerung zu: DAC, IBAC, MAC, RBAC.**
> **✅ Lösung:** DAC → Eigentümer entscheidet nach Ermessen · IBAC → Rechte an die Identität gebunden · MAC → System bestimmt über feste Regeln/Labels · RBAC → Rechte über zugewiesene Rollen.

**A2. Was besagt die „restriktive Rechtevergabe" und wie heißen die zugehörigen Prinzipien?**
> **✅ Lösung:** Einem Prozess sollen nur die für seine Aufgabe **unverzichtbaren** Zugriffe erlaubt sein. Prinzipien: **principle of least privilege** und **need-to-know**. Ziel: Schaden durch Fehler/Malware begrenzen.

**A3. Was ist die Aufgabe des Referenzmonitors?**
> **✅ Lösung:** Er prüft **jeden** Zugriffswunsch eines Subjekts auf ein Objekt gegen die hinterlegte **Richtlinie (Policy)** und **gewährt** oder **lehnt ab**. Er ist die zentrale, nicht umgehbare Kontrollinstanz.

## B. DAC vs. MAC [T]

**B1. Erklären Sie den zentralen Unterschied zwischen DAC und MAC.**
> **✅ Lösung:** Bei **DAC** vergibt der **Eigentümer** des Objekts die Rechte nach eigenem Ermessen und kann sie weitergeben; bei **MAC** entscheidet das **System** anhand fester Regeln und Kennzeichnungen (Labels/Kategorien), die der Benutzer **nicht ändern** kann. DAC schützt daher nicht vor unerwünschten Informationsflüssen (Trojaner erbt die Benutzerrechte), MAC schon.

**B2. Warum verletzt ein Editor mit einem Trojanischen Pferd das Prinzip der restriktiven Rechtevergabe unter DAC?**
> **✅ Lösung:** Unter DAC läuft der Editor mit **allen Rechten des Benutzers**. Ein darin versteckter Trojaner kann daher heimlich Daten in eine fremde Datei kopieren, ohne dass der Benutzer es verhindern kann — der Prozess hat mehr Rechte, als er für seine eigentliche Aufgabe braucht. MAC/Bell-LaPadula unterbinden solche Informationsflüsse über Systemregeln.

## C. Zugriffsmatrix, ACL, Capabilities 🧮 [T]

**C1. Gegeben die Matrix. a) Was ist die ACL von O1? b) Was ist die Capability-Liste von S3?**
```
      O1     O2    O3
S1    rwx    -     -
S2    -      w     -
S3    rx     -     x
```
> **✅ Lösung:** a) **ACL(O1)** = Spalte zu O1 = {S1: rwx, S3: rx} (S2 hat keine Rechte). b) **Capability(S3)** = Zeile zu S3 = {O1: rx, O3: x}.

**C2. Ordnen Sie zu: ACL / Capability.** Spalte · Zeile · beim Objekt gespeichert · beim Subjekt gespeichert · Weitergabe besser · Entzug besser
> **✅ Lösung:** **ACL:** Spalte, beim Objekt gespeichert, **Entzug** (Zurücknahme) besser. **Capability:** Zeile, beim Subjekt gespeichert, **Weitergabe** besser.

**C3 [N]. Warum ist der Entzug eines Rechts bei Capabilities schwieriger als bei ACLs?**
> **✅ Lösung:** Bei einer **ACL** liegen alle Rechte zentral beim **Objekt** — man streicht einfach den Eintrag. Bei **Capabilities** sind die Rechte über viele **Subjekte** verteilt (jedes hat seine Liste, Kopien können weitergegeben worden sein). Um ein Recht zu entziehen, müsste man alle Kopien aufspüren → Zurücknahme (revocation) ist aufwendig. Dafür ist die Weitergabe bei Capabilities einfach.

## D. HRU [T]

**D1. Nennen Sie die 6 Elementaroperationen des HRU-Modells.**
> **✅ Lösung:** `enter r into M(s,o)`, `delete r from M(s,o)`, `create subject s`, `delete subject s`, `create object o`, `delete object o`.

**D2 [N]. Schreiben Sie ein HRU-Kommando „grant_read", mit dem der Besitzer o eines Objekts einem Subjekt s2 das Leserecht gibt (Besitzer hat Recht `own`).**
> **✅ Lösung:**
> ```
> COMMAND grant_read (s1, s2, o)
>   IF own in M(s1,o)
>   THEN enter read into M(s2,o)
> END;
> ```
> (Nur wenn s1 das Recht `own` am Objekt o hat, darf es s2 das Leserecht eintragen — so funktioniert die kontrollierte Weitergabe in DAC.)

## E. RBAC [T]

**E1. Beschreiben Sie die Beziehungen U, R, P, S im RBAC-Modell.**
> **✅ Lösung:** **Benutzer (U)** werden über die Benutzer-Rollen-Zuweisung (UA) **Rollen (R)** zugewiesen. Rollen werden über die Rollen-Zugriffsrechte-Zuweisung (PA) **Zugriffsrechte (P)** zugeordnet (= verfügbare Methoden M auf Objekten O). In einer **Session (S)** aktiviert ein Benutzer Rollen und erhält deren Rechte. Die Session ist ein **zeitgebundenes** Konstrukt, das Benutzer, Rollen und Rechte verbindet.

**E2. Was bedeutet in einer Rollenhierarchie `R1 ⇒ R2`?**
> **✅ Lösung:** Dass **R2 auch die Rechte von R1 besitzt** (R2 erbt die Rechte von R1), formal `R1 ≤ R2`. Beispiel: Ein **Chefarzt** erbt über die Hierarchie u. a. die Rechte von Arzt, Oberschwester und Schwester.

**E3 [N]. Vorteil von RBAC gegenüber einer direkten Benutzer-Rechte-Zuordnung?**
> **✅ Lösung:** Rechte hängen an **Rollen**, nicht an einzelnen Personen. Wechselt ein Mitarbeiter die Stelle, ändert man nur seine Rollenzuweisung statt vieler Einzelrechte; neue Mitarbeiter bekommen sofort das passende Rechtepaket. Das reduziert Administrationsaufwand und Fehler und setzt least privilege leichter um.

## F. Bell-LaPadula ⭐ [T][1,0]

**F1. Nennen und erklären Sie die zwei Kernregeln von Bell-LaPadula.**
> **✅ Lösung:** **No-Read-Up** (simple security): ein Subjekt darf nur Objekte **lesen**, deren Stufe **≤ seiner eigenen** ist (kann nichts Höheres/Geheimeres lesen). **No-Write-Down** (*-Property): ein Subjekt darf nur auf Objekte **schreiben**, deren Stufe **≥ seiner eigenen** ist (kann nichts nach unten „durchsickern" lassen). Kurz: **read down, write up**. Zusätzlich **Tranquility**: Sicherheitsklassen ändern sich zur Laufzeit nicht.

**F2 🧮. Ein Subjekt hat die Stufe `secret`. Objekte: A=public, B=confidential, C=secret, D=top-secret. Welche Objekte darf es lesen, welche beschreiben?**
> **✅ Lösung:** Stufenordnung: public < confidential < secret < top-secret.
> **Lesen** (no-read-up, SC(S) ≥ SC(O)): A (public), B (confidential), C (secret) — **nicht** D (top-secret).
> **Schreiben** (no-write-down, SC(S) ≤ SC(O)): C (secret), D (top-secret) — **nicht** A, B.
> (Reines „write" setzt laut Unix-Policy SC(S)=SC(O) voraus → nur C; „append" erlaubt nach oben → C und D.)

**F3 [1,0]. Warum ist gerade die No-Write-Down-Regel kontraintuitiv, aber für die Vertraulichkeit entscheidend?**
> **✅ Lösung:** Intuitiv würde man einem hoch eingestuften Subjekt erlauben, „nach unten" zu schreiben. Genau das wäre aber gefährlich: Ein top-secret-Subjekt (oder ein Trojaner in seinem Prozess) könnte geheime Informationen in ein öffentlich lesbares Objekt schreiben → **Informationsabfluss nach unten**. No-Write-Down verbietet das: Informationen dürfen nur „aufsteigen". So wird die **Vertraulichkeit** auch gegen verdeckte Informationsflüsse (die DAC/RBAC nicht kontrollieren) geschützt.

**F4 [N]. Welche Grenze/welches Problem hat das BLP-Modell im Dauerbetrieb?**
> **✅ Lösung:** Da Informationen nur **nach oben** fließen dürfen, werden Objekte **sukzessive immer höher eingestuft** — mit der Zeit landet fast alles auf hoher Stufe und wird unbrauchbar restriktiv. Zudem schützt BLP nur die **Vertraulichkeit**, nicht die Integrität. Abhilfe: zusätzliche Regeln/Kategorien bzw. ergänzende Modelle (Integrität → Biba).

## G. Chinese Wall & Unix 🧮 [T][1,0]

**G1. Formulieren Sie die COI-Regel des Chinese-Wall-Modells.**
> **✅ Lösung:** Greift ein Subjekt auf Objekte eines Unternehmens U in einer Konfliktklasse (COI) K zu, darf es **nicht mehr** auf Objekte eines **anderen** Unternehmens U' ≠ U **derselben** Klasse K zugreifen. Zugriffe hängen also von der **Zugriffshistorie** ab; um das Subjekt wird eine „Zugriffsmauer" gebaut.

**G2 [N]. Ein Berater hat heute Objekte der Dresdner Bank gelesen. Darf er danach a) Objekte der HypoVereinsbank, b) Objekte von BP (Ölgesellschaft) lesen? Begründung.**
> **✅ Lösung:** a) **Nein** — Dresdner und Hypo liegen in **derselben** Konfliktklasse „Bank"; nach dem Zugriff auf Dresdner ist Hypo durch die Mauer gesperrt (COI-Regel). b) **Ja** — BP gehört zur **anderen** Klasse „Ölgesellschaft", dort hat der Berater noch kein Unternehmen gewählt. Ab dann ist er aber innerhalb „Ölgesellschaft" auf BP festgelegt (Aral gesperrt).

**G3 🧮. Wandeln Sie um: a) `rwxr-x--x` in oktal, b) `640` in Symbole, c) `chmod 751`?**
> **✅ Lösung:** a) rwx=7, r-x=5, --x=1 → **751**. b) 6=rw-, 4=r--, 0=--- → **rw-r-----**. c) 751 = Besitzer **rwx**, Gruppe **r-x**, andere **--x**.

**G4 🧮. Ein Skript soll vom Besitzer les-/schreib-/ausführbar sein, die Gruppe soll es nur ausführen, alle anderen gar nichts. Welcher chmod-Wert?**
> **✅ Lösung:** Besitzer rwx = 7, Gruppe --x = 1, andere --- = 0 → **chmod 710**.

**G5 [N]. Worin unterscheidet sich die Bedeutung des x-Rechts bei einer Datei und bei einem Verzeichnis?**
> **✅ Lösung:** Bei einer **Datei** bedeutet `x`, dass man sie **ausführen** (als Programm starten) darf. Bei einem **Verzeichnis** bedeutet `x`, dass man in das Verzeichnis **wechseln/hindurchgehen** (`cd`) und auf benannte Dateien darin zugreifen darf — ohne `x` kann man selbst mit `r` nicht auf die Dateien zugreifen. `r` am Verzeichnis erlaubt nur das Auflisten der Namen (`ls`).

**G6 [T]. Unix-Rechte sind ein Beispiel für welchen Zugriffskontroll-Ansatz? Begründung.**
> **✅ Lösung:** **DAC** — der **Eigentümer** einer Datei legt per `chmod` nach eigenem Ermessen fest, wer (user/group/others) welche Rechte hat, und kann diese ändern/weitergeben. Das System erzwingt keine übergeordnete, unveränderliche Label-Policy (das wäre MAC).

## T. Transfer & Verständnis — Skript anwenden ⭐

**T1 [1,0]. Ein Krankenhaus will: (a) Ärzte dürfen Akten ihrer Station lesen/ändern, (b) niemand darf geheime Diagnosen in öffentliche Bereiche kopieren, (c) ein Wechsel der Rolle „Assistenzarzt → Oberarzt" soll die Rechte automatisch erweitern. Welches Modell deckt welche Anforderung, und warum reicht eines allein nicht?**
> **✅ Lösungsweg:** (a) + (c) = **RBAC**: Rechte an **Rollen** gebunden, **Rollenhierarchie** (Oberarzt erbt Arzt-Rechte) → Wechsel = nur Rollenzuweisung ändern. (b) = **Informationsfluss-Kontrolle**, die RBAC **nicht** leistet → dafür **Bell-LaPadula (MAC)** mit **no-write-down** (geheime Diagnose darf nicht in „public" geschrieben werden). Fazit: **RBAC regelt den Zugriff, BLP den Fluss** — man kombiniert sie. Zeigt Verständnis, dass RBAC und MAC unterschiedliche Probleme lösen.

**T2 [1,0]. Warum ist die No-Write-Down-Regel genau die Verteidigung gegen das „Trojanisches Pferd"-Problem aus der Einleitung (ein Editor kopiert heimlich Daten in eine fremde Datei)?**
> **✅ Lösungsweg:** Das Trojaner-Problem ([07 Einleitung](07_Zugriffskontrolle.md)): unter **DAC** erbt der Editor-Prozess alle Rechte des Nutzers und kann geheime Daten **heimlich nach außen schreiben**. **Bell-LaPadula (MAC)** verhindert das **systemseitig**: ein hoch eingestuftes Subjekt darf per **No-Write-Down** gar nicht auf ein niedriger eingestuftes Objekt schreiben — der Trojaner **kann** die Daten also nicht „nach unten" kopieren, selbst wenn er wollte. Pointe: DAC vertraut dem Programm, **MAC erzwingt** den Informationsfluss unabhängig vom Programm — deshalb löst MAC, was DAC offenlässt.

**T3 [N] 🧮. Erklären Sie, warum man bei einer ACL „negative ACEs zuerst" auswertet, an einem Konflikt: Nutzer ist in Gruppe „Mitarbeiter" (darf lesen), aber als Person explizit gesperrt.**
> **✅ Lösungsweg:** Würde man **positive zuerst** nehmen, bekäme der Nutzer über die Gruppe „lesen" und die Sperre liefe ins Leere. Durch **negative ACEs zuerst** greift die **explizite Sperre** und überschreibt die Gruppenerlaubnis → der Nutzer wird korrekt ausgeschlossen. Prinzip: **Verbot schlägt Erlaubnis**, damit gezielte Sperren nicht durch breite Gruppenrechte ausgehebelt werden („unlogische Kombinationen werden verhindert"). Verbindet ACL-Auswertung mit least privilege.

**T4 [1,0] 🧮. Ein Skript `deploy.sh` soll: Besitzer darf alles, die Gruppe darf es ausführen und lesen (aber nicht ändern), alle anderen nichts. a) Welcher chmod-Wert? b) Warum ist `chmod 777` hier ein Sicherheitsproblem? c) Was bedeutet es, wenn jemand nur `x` (ohne `r`) auf dem Verzeichnis setzt, in dem das Skript liegt?**
> **✅ Lösungsweg:** a) Besitzer rwx=7, Gruppe r-x=5, andere ---=0 → **chmod 750**. b) **777** = jeder darf **schreiben/ändern** → jeder könnte Schadcode ins Skript einschleusen, das dann (evtl. mit höheren Rechten) läuft → verletzt **least privilege** und **Integrität**. c) `x` ohne `r` auf dem **Verzeichnis**: man kann **nicht auflisten** (`ls` scheitert), aber bei **bekanntem Dateinamen trotzdem zugreifen/durchgehen** (`cd`, Datei öffnen) — nützlich, um Dateien gezielt freizugeben, ohne den Verzeichnisinhalt preiszugeben.

## H. Самопроверка перед den Praktika/Wireshark-Guide (с ответами)
**H1. 4 Ansätze + Steuerung?** DAC (Eigentümer), IBAC (Identität), MAC (System/Labels), RBAC (Rollen).
**H2. Subjekt/Objekt/Referenzmonitor?** aktiver Akteur / passive Ressource / prüft jeden Zugriff gegen die Policy.
**H3. ACL vs. Capability?** ACL = Spalte beim Objekt (Entzug leicht); Capability = Zeile beim Subjekt (Weitergabe leicht).
**H4. DAC vs. MAC in einem Satz?** DAC: Besitzer vergibt Rechte, weitergebbar; MAC: System erzwingt feste Label-Regeln, kein Informationsabfluss.
**H5. BLP-Regeln?** No-Read-Up (lesen nur ≤), No-Write-Down (schreiben nur ≥), Tranquility; „read down, write up"; schützt Vertraulichkeit.
**H6. Chinese Wall COI-Regel?** Nach Zugriff auf ein Unternehmen einer Konfliktklasse kein Zugriff auf Konkurrenten derselben Klasse (historienabhängig).
**H7. Unix: rwx-Werte und 755/640?** r=4,w=2,x=1; 755=rwxr-xr-x; 640=rw-r-----.
**H8. x-Recht bei Datei vs. Verzeichnis?** Datei: ausführen; Verzeichnis: hineinwechseln (cd).
**H9. RBAC-Hierarchie `R1⇒R2`?** R2 erbt die Rechte von R1.

Следующие файлы: `08_Praktika.md` (сводка всех лаб), `09_Wireshark_Guide.md`, `10_Glossar_DE_RU.md`, `11_Uebungen.md` (пробный экзамен).
