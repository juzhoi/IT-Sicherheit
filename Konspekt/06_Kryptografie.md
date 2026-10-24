# Kapitel 6 — Verschlüsselung (Kryptografie)
**Skript S. 141–192 (PDF S. 147–198)** · dazu **Praktikum 06 (CrypTool)**

> **О чём глава.** Это самая большая и самая «техничная» глава. Здесь — весь крипто-инструментарий: симметричное шифрование (AES, Betriebsmodi), асимметричное (RSA), хеш-функции, обмен ключами (Diffie-Hellman), MAC и цифровые подписи, сертификаты и PKI, защищённые каналы (TLS/SSL).
>
> **Почему важно для оценки 1,0:** здесь максимальная концентрация **расчётных задач** (RSA шаг за шагом, Diffie-Hellman, XOR, Geburtstagsparadoxon) и «понятийных» различий, которые любят на экзамене (симметрично vs. асимметрично, MAC vs. Signatur, Hash vs. MAC, какое Schutzziel чем достигается). Плюс Kerckhoffs-принцип и сравнение режимов (ECB-ловушка).
>
> **Сквозная идея главы:** каждое Schutzziel достигается своим средством:
> - **Vertraulichkeit** → Verschlüsselung (AES, RSA) + Schlüsselaustausch (DH)
> - **Integrität** → Hashfunktion / MAC / Signatur
> - **(Daten-)Authentizität** → MAC / Signatur
> - **Verbindlichkeit (Nichtabstreitbarkeit)** → **nur** digitale Signatur (nicht MAC!)
>
> **Обозначения:** ⭐ очень вероятно · 🧮 расчёт · ⚠️ ловушка · ➕ сверх скрипта · 🔧 Praktikum.

## Содержание
6.2 Kryptologie/-grafie/-analyse, Kerckhoffs ⭐ · 6.3 Symmetrisch, Schlüsseltauschproblem 🧮, Stromchiffre/XOR/One-Time-Pad 🧮 · 6.4 Blockchiffren, Konfusion/Diffusion, **Betriebsmodi (ECB/CBC/CTR/OFB/CFB)** ⭐, AES ⭐ · 6.5 Asymmetrisch, Einwegfunktion mit Falltür, **RSA** 🧮⭐ · 6.6 Hashfunktionen, Einwegeigenschaft, Kollision, **Geburtstagsparadoxon** 🧮⭐ · 6.7 Schlüsselmanagement, **Diffie-Hellman** 🧮⭐, hybrid, **MAC** ⭐ · 6.8 Digitale Signatur ⭐, Zertifikate/X.509/PKI/Chain of Trust ⭐ · 6.9 Sichere Kanäle, **TLS/SSL** ⭐ · **Aufgaben A–H**

---

## 6.2 Kryptografie, Kryptologie, Kryptoanalyse (S. 142–145) ⭐

**Объяснение.** В цифровом мире данные копируются без следов, а открытая сеть — как **открытка** (Postkarte): кто вклинится в путь, тот всё прочитает. Поэтому в открытых сетях шифрование практически обязательно.

| Begriff | Определение (DE) | Суть |
|---|---|---|
| **Kryptografie** | *Lehre von den Methoden zur Ver- und Entschlüsselung von Nachrichten zum Zweck der Geheimhaltung* (Def. 6.1) | как **защищать** (строить шифры) |
| **Kryptoanalyse** | *Wissenschaft von Methoden zur Entschlüsselung ohne Zugriff auf den Schlüssel* (Def. 6.2) | как **ломать** шифры |
| **Kryptologie** | **Oberbegriff** für beide | вся наука целиком |

**Действующие лица (модель связи, Abb. 6.2):** отправитель **Alice**, получатель **Bob**, противник **Mallory / Eve** (passiv = подслушивает, aktiv = изменяет/подделывает/блокирует).

**Какое Schutzziel чем достигается (это ядро всей главы) ⭐:**
| Schutzziel | Mittel |
|---|---|
| **Vertraulichkeit** | Verschlüsselung (AES, DES, RSA, ECC) + Schlüsselaustausch (DH, ECDH, RSA, KDC) |
| **Integrität** | Hashfunktionen (SHA), **MAC** oder Signaturen |
| **Datenauthentizität** | **MAC** oder Signaturen |
| **Instanzauthentizität** | Challenge-Response-Protokoll |
| **Nichtabstreitbarkeit** | **Signaturen** (nur asymmetrisch!) |

### Kerckhoffs'sches Prinzip ⭐ (очень частый вопрос)
> **Def. 6.3:** *Die gesamte Sicherheit eines Kryptografie-Verfahrens soll **nur auf der Geheimhaltung des Schlüssels** beruhen und **nicht** auf der Geheimhaltung des Algorithmus.* (Auguste Kerckhoffs, 1883)

- Противоположность — **Security by Obscurity** («безопасность через неизвестность алгоритма»): не прижилось, т.к. без открытой экспертной проверки легко просмотреть фатальную ошибку дизайна.
- Пример следования принципу: **AES** выбирался в многолетнем **открытом** конкурсе; каждая деталь AES известна, секретен только ключ.
- Доп. принципы Kerckhoffs: система должна быть практически безопасной, простой в использовании, хорошо проанализированной экспертами, и её можно без опасности отдать в руки врага (кроме ключа).

---

## 6.3 Verschlüsselungsverfahren (S. 146–152)

**Базовые понятия ⭐:**
- **Klartext (plaintext) m** → через **Verschlüsselung E(k, m)** → **Geheimtext/Chiffrat (ciphertext) c**; обратно **Entschlüsselung D(k, c) = m**.
- **Drei Klassen кriptoverfahren:**
  | Klasse | Schlüssel |
  |---|---|
  | **symmetrisch** | **gleicher** Schlüssel zum Ver- und Entschlüsseln (e = d) |
  | **asymmetrisch (Public-Key)** | öffentlicher Schlüssel **e** zum Verschlüsseln, geheimer **d** zum Entschlüsseln |
  | **hybrid** | kombiniert beide (Vorteile beider) |

### 6.3.1 Symmetrische Verschlüsselung ⭐
> **Def. 6.4:** *Verfahren heißen symmetrisch, wenn für Ver- und Entschlüsselung die **gleichen** Schlüssel verwendet werden.* Bedingung: `D(e, E(e, m)) = m`.

| ✅ Vorteile | ❌ Nachteile |
|---|---|
| **sehr schnell**, hoher Datendurchsatz (große Datenmengen) | **Schlüsselaustauschproblem** (Verteilung) |
| **kurze Schlüssel** (z. B. 128–256 Bit) | aufwendiges **Schlüsselmanagement** |
| | **keine Nichtabstreitbarkeit** (beide haben denselben Schlüssel) |

### 6.3.2 Schlüsseltauschproblem 🧮⭐
**Объяснение.** Чтобы симметрично общаться, каждой паре участников нужен свой общий ключ, переданный по безопасному каналу. В сети из n человек число ключей растёт **квадратично**.

**Formel ⭐:** число ключей = **n·(n−1)/2** (≈ n²/2).
- 10 человек → 10·9/2 = **45** ключей
- 100 человек → 100·99/2 = **4950** ключей
- Удвоение участников ≈ **учетверение** ключей (2² = 4). Асимметричные методы эту проблему решают (каждому нужна лишь **одна** пара ключей).

### Stromchiffren (Stream Cipher) 🧮
- Из ключа генерируется **Schlüsselstrom** (поток битов), которым **побитово XOR** шифруется открытый текст. Это частный случай **polyalphabetische Substitution**.
- **XOR (⊕, Kontravalenz)** — обратимая операция (= сложение по модулю 2):
  | A | B | A⊕B |
  |---|---|---|
  | 1 | 1 | 0 |
  | 1 | 0 | 1 |
  | 0 | 1 | 1 |
  | 0 | 0 | 0 |
- **Свойство обратимости** (одна и та же функция для шифрования и дешифрования):
  ```
  Klartext ⊕ Schlüssel = Chiffre
  Chiffre  ⊕ Schlüssel = Klartext
  Chiffre  ⊕ Klartext  = Schlüssel
  ```
  **Beispiel 6.1:** `101 ⊕ 011 = 110` (шифр); `110 ⊕ 011 = 101` (обратно текст); `110 ⊕ 101 = 011` (вышел ключ).

**One-Time-Pad (Vernam-Chiffre) ⭐** — единственный **beweisbar sicher** (математически невзламываемый) шифр при двух условиях:
1. ключ состоит из **настоящих случайных** чисел;
2. ключ **ровно такой же длины**, как текст, и используется **только один раз**.
- ❌ Практический минус: ключ длиной со всё сообщение надо как-то передать → непрактично. Поэтому на практике из короткого ключа генерируют **псевдослучайный** поток (Pseudozufallsbitgenerator), напр. методом **линейной конгруэнции** `x_{n+1} = (x_n·a + c) mod m`.
- Примеры потоковых шифров: **RC4** (Rivest, 1987; уже небезопасен, с 2015 **запрещён в TLS** по RFC 7465), SEAL.
- ⚠️ Потоковые шифры (как RC4) **не дают Integritätsschutz**: поменяешь бит шифротекста — поменяется тот же бит открытого текста.

---

## 6.4 Blockchiffren (S. 152–160) ⭐

**Объяснение.** Блочный шифр делит текст на **блоки** фиксированной длины **n** (типично **128 бит**, старые — 64) и шифрует каждый блок ключом в блок той же длины. Последний неполный блок дополняют (**Padding**).

### 6.4.1 Konfusion und Diffusion ⭐ (Claude Shannon)
| Begriff | Определение (Def. 6.5/6.6) | достигается через |
|---|---|---|
| **Konfusion** | *Verschleierung des Zusammenhangs zwischen Klartext- und Geheimtextzeichen* | **Substitution** (S-Box: напр. 6 бит → 4 бита) |
| **Diffusion** | *die im Klartext enthaltene Information wird über den Geheimtext verteilt* | **Permutation** (переупорядочивание битов) |

- **SPN (Substitutions-Permutations-Netzwerk):** чередование Substitution и Permutation в **несколько раундов**.
- **Avalanche-Effekt (лавинный эффект) ⭐:** при хорошей реализации изменение **одного бита** открытого текста/ключа меняет **каждый бит** шифротекста с вероятностью ~**50 %** → результат непредсказуемый.
- **Produktchiffre:** шифр из комбинации Substitution + Permutation.

### 6.4.3 Betriebsmodi для Blockchiffren ⭐⭐ (ключевая таблица)
Один блочный шифр можно применять по-разному. Режим определяет, как цепляются блоки.

| Modus | Прицип | IV/Nonce? | Плюсы/минусы |
|---|---|---|---|
| **ECB** (Electronic Code Book) | каждый блок шифруется **независимо**: `cᵢ = E(k, mᵢ)` | нет | ⚠️ **слабый!** одинаковые блоки → одинаковые шифроблоки → видны **узоры** (Strukturerhaltung); возможна манипуляция (вставка/перестановка блоков). Только для очень коротких сообщений |
| **CBC** (Cipher Block Chaining) | каждый блок перед шифрованием **XOR с предыдущим шифроблоком**; первый — с **IV**: `cᵢ = E(k, mᵢ ⊕ cᵢ₋₁)` | **IV** (случайный c₀) | одинаковый текст → разный шифр (контекстозависимо); перестановка/вставка блоков ломает расшифровку; стоит лишь +1 XOR на блок |
| **CTR** (Counter) | шифруется **Nonce + счётчик**, результат XOR с блоком: `cᵢ = mᵢ ⊕ E(k, Nonce‖counter)` | **Nonce** | блочный шифр работает как генератор потока; счётчик +1 на блок. ⚠️ **Nonce нельзя повторять при том же ключе** (иначе тот же keystream); Nonce **не секретна** |
| **OFB** (Output Feedback) | выход шифра подаётся обратно на вход → поток битов, XOR с текстом (**Pseudo-One-Time-Pad**) | **IV** | битовая ошибка в шифре → ошибка **только в том же бите** текста (не распространяется); ver=entschl. идентичны; не самосинхронизируется |
| **CFB** (Cipher Feedback) | как CBC, но зависимость через предыдущий **шифроблок**; самосинхронизируется | **IV** | |

> **ECB-ловушка ⭐:** классический вопрос — «почему ECB небезопасен?». Ответ: одинаковые открытые блоки дают одинаковые шифроблоки → сохраняются **узоры/структура** открытого текста (знаменитый «ECB-пингвин»), и можно вставлять/переставлять блоки незаметно. CBC/CTR это устраняют через IV/Nonce.

### 6.4.4 AES (Advanced Encryption Standard) ⭐
**История:** DES устарел (слишком короткий ключ) → NIST объявил открытый конкурс (1997) → AES стандартизован **26.11.2001** как **FIPS 197**, открытый и бесплатный.

**Ключевые свойства AES (на «Nennen Sie Eigenschaften»):**
1. **симметричный** алгоритм, **блочный шифр**, **Blocklänge 128 Bit**;
2. три длины ключа: **128 / 192 / 256 Bit**;
3. число **раундов** зависит от длины ключа: **10 / 12 / 14** (при 128 бит → 10);
4. это **не** Feistel-шифр (в отличие от DES) — в каждом раунде шифруется **весь блок**; текст в сетке **4×4 байта (State)**;
5. безопасен ≥ как Triple-DES, но **эффективнее**;
6. рассчитан на использование ~до **2030**, данные должны оставаться защищёнными до ~**2100**;
7. применяется в **TLS, IPsec, 802.11i (WPA2), SSH, Skype**.

**4 шага каждого раунда (Abb. 6.13):** `SubBytes` (байтовая подстановка, S-Box → Konfusion) → `ShiftRows` (циклический сдвиг строк) → `MixColumns` (умножение столбцов на полином → Diffusion) → `AddRoundKey` (XOR с раундовым ключом).

---

## 6.5 Asymmetrische Verschlüsselung (S. 161–166) ⭐

**Объяснение.** У каждого участника — **пара ключей**: публичный **e** (можно раздать всем) и приватный **d** (держать в тайне). Отправитель шифрует **публичным ключом получателя**, а расшифровать может **только** получатель своим приватным. Публичный ключ передаётся по **аутентичному** каналу (его можно читать, но нельзя подменять). Отсюда название **Public-Key-Verfahren**.

**Einwegfunktion mit Falltür (Trapdoor one-way function) ⭐** — основа всех Public-Key:
- функция **легко** считается вперёд, но её **обратную практически невозможно** вычислить…
- …**кроме** как с дополнительным знанием — это **приватный ключ** (= «потайная дверь», Falltür).
- Опирается на «тяжёлую» математическую задачу:
  | Проблема | Асимметрия | Используют |
  |---|---|---|
  | **Faktorisierungsproblem (IFP)** | умножить легко, разложить на простые множители тяжело | **RSA** |
  | **Diskreter Logarithmus (DLP)** | возвести в степень легко, взять логарифм тяжело | ElGamal, DH, DSA |
  | **Elliptische Kurven (ECC)** | — | ECDH, ECDSA |

| ✅ Vorteile asymmetrisch | ❌ Nachteile |
|---|---|
| **kein Schlüsselaustauschproblem** (nur eigenen public key verteilen) | **langsam** (für große Daten ungeeignet) |
| nur **1 Schlüsselpaar pro Person** (statt n²/2) | **lange Schlüssel** (RSA ~2048–4096 Bit) |
| ermöglicht **Signaturen/Nichtabstreitbarkeit** | |

### RSA (Rivest, Shamir, Adleman, 1978) 🧮⭐⭐
Безопасность RSA = сложность **факторизации** `n = p·q` на простые `p, q`.

**Setup (делает получатель Bob):**
1. выбрать два разных простых **p, q** (держать в тайне);
2. **n = p·q** (RSA-Modul, публикуется);
3. **φ(n) = (p−1)·(q−1)** (функция Эйлера, держать в тайне);
4. выбрать публичный экспонент **e** c условием **1 < e < φ(n)** и **ggT(e, φ(n)) = 1** (взаимно просты);
5. вычислить приватный экспонент **d** как `e·d ≡ 1 mod φ(n)` (через расширенный алгоритм Евклида).

- **Öffentlicher Schlüssel: (n, e)** · **Privater Schlüssel: (p, q, d)**.
- **Verschlüsselung** (Alice, публичным ключом Боба): `c ≡ mᵉ mod n`.
- **Entschlüsselung** (Bob, приватным): `m ≡ cᵈ mod n`.
- ⚠️ Шифровать можно только **m < n**; длиннее — через блочные режимы.
- **Falltür:** d может вычислить только тот, кто знает **p и q** (т.е. φ(n)).

#### 🧮 Разобранный пример из скрипта (Beispiel 6.2) — выучить наизусть схему
```
Setup:  p = 11, q = 17
        n = p·q = 11·17 = 187
        φ(n) = (11−1)(17−1) = 10·16 = 160       (= 2⁵·5)
        e: ggT(e,160)=1 → e=7 passt   → Public Key (n,e) = (187, 7)
        d: 7·d ≡ 1 mod 160 → d = 23   (Probe: 7·23 = 161 = 160+1 ≡ 1 mod 160 ✓)
        → Private Key (p,q,d) = (11, 17, 23)
```
**Verschlüsselung z. B. m = 2:** `c = 2⁷ mod 187 = 128`.
**Entschlüsselung:** `m = 128²³ mod 187 = 2` ✓.
> 🧮 Трюк для степени по модулю вручную — **быстрое возведение** через промежуточные `mod`: `128² = 16384 ≡ 16384 − 87·187 = 16384−16269 = 115 mod 187`; дальше по шагам квадратами. На экзамене при маленьких числах считай пошагово, каждый раз беря `mod n`.

#### 🧮 Как вычислить `d` вручную — erweiterter Euklidischer Algorithmus ⭐
Это **важный навык**: нужно найти `d` c `e·d ≡ 1 mod φ(n)` — т.е. **обратное к e по модулю φ(n)**. Два способа.

**Способ 1 — подбор (быстро при малых числах):** ищи k, при котором `(k·φ(n) + 1)` делится на e.
Пример `e=7, φ=160`: пробуем `(k·160 + 1)/7`: k=1→161/7=**23** ✓ → **d=23**.

**Способ 2 — erweiterter Euklid (надёжно всегда):** находим коэффициенты в `e·d + φ·y = ggT(e,φ) = 1`; тогда `d` — множитель при e.
Пример `e=7, φ=160`:
```
Прямой ход (обычный Евклид, остатки):
160 = 22·7 + 6        (160 mod 7 = 6)
  7 =  1·6 + 1        (7 mod 6 = 1)
  6 =  6·1 + 0        → ggT = 1  ✓ (значит обратное существует)
Обратный ход (выражаем 1 через 7 и 160):
  1 = 7 − 1·6
  6 = 160 − 22·7   →  1 = 7 − 1·(160 − 22·7) = 7 − 160 + 22·7 = 23·7 − 1·160
→ 23·7 ≡ 1 mod 160  →  d = 23  ✓
```
Если d получится отрицательным — прибавь φ(n), пока не станет в диапазоне `1…φ(n)`.
⚠️ На экзамене метод (оба хода Евклида) приносит баллы, даже если в арифметике ошибёшься — показывай шаги.

---

## 6.6 Kryptografische Hashfunktionen (S. 166–169) ⭐

**Объяснение.** Хеш-функция **H** отображает вход **любой длины** на выход **фиксированной длины N** (Hashwert = «цифровой отпечаток», Fingerabdruck). Всего возможно **2^N** разных хешей. Центральное Schutzziel — **Integrität** (узнать, менялись ли данные; напр. контрольные суммы ISO-образов).

**Три требования к криптографической хеш-функции ⭐:**
| Eigenschaft | Что значит |
|---|---|
| **effizient** | H(m) быстро считается вперёд |
| **Einwegeigenschaft (Preimage-Resistenz)** | по хешу **практически невозможно** найти подходящий вход (хотя входов бесконечно много, т.к. функция не инъективна) |
| **Kollisionsresistenz** | практически невозможно найти два разных входа с одинаковым хешем |

- **Kollision (Def. 6.7):** два **разных** входа дают **одинаковый** хеш.
- **Sicherheitsschwelle:** сейчас ~**100 бит** (эффективнейшая атака требует ≥ 2¹⁰⁰ операций).

**Важные хеш-алгоритмы (Tabelle 6.2) ⭐:**
| Name | Ausgabe | Статус |
|---|---|---|
| **MD5** | 128 Bit | **unsicher**, Kollisionen (2¹⁸-Angriff) — nicht mehr verwenden! |
| **SHA-1** | 160 Bit | Kollisionen (2⁵²-Angriff) — veraltet |
| **SHA-2** | 256/384/512 Bit | sicher, keine bekannten Kollisionen |
| **SHA-3** | 256/384/512 Bit | sicher |

> ⚠️ **Hash ≠ MAC ≠ Verschlüsselung.** Хеш сам по себе **не защищает от подделки**: атакующий тоже умеет считать H(m). Хеш даёт только обнаружение случайных изменений (Integrität), но не Authentizität. Для аутентичности нужен **ключ** → MAC или подпись.

### 6.6.3 Geburtstagsparadoxon (Birthday Paradox) 🧮⭐
**Объяснение.** Чтобы найти человека с **твоим** днём рождения, нужно ~183 человека. А чтобы найти **любых двух** с совпадающими — достаточно **23** (вероятность 50 %). Это и есть парадокс: коллизию найти гораздо легче, чем кажется.

**Formel ⭐:** при хеше с **n** возможными значениями коллизия с вероятностью 50 % наступает примерно при
```
  k ≈ 1,2·√n  Werten
```
- Дни рождения: n = 365 → 1,2·√365 ≈ **22,9 ≈ 23** человека.
- Хеш длиной **40 бит** → n = 2⁴⁰ → уже при ~**1 млн** входов 50 % шанс коллизии → для крипто **непригодно**.
- **Вывод для безопасности:** чтобы найти коллизию, нужно ~**√(2^N) = 2^(N/2)** попыток. Поэтому для стойкости ~100 бит хеш должен быть **~200+ бит**. SHA-1 (160 бит) → коллизионная стойкость всего ~2⁸⁰ → считается устаревшим.

---

## 6.7 Schlüsselmanagement (S. 170–177) ⭐

### 6.7.1 Public-Key + Fingerprint-Problem
- Публичный ключ кладут на **Key-Server** (напр. pgp.mit.edu), любой скачивает и шифрует сообщение владельцу приватного ключа.
- ⚠️ **Главная проблема:** откуда Боб знает, что скачанный «ключ Alice» **действительно принадлежит Alice**, а не подменён MitM?
- **Решение 1 — Fingerprint:** хеш публичного ключа (напр. `F49A 821B ... CBF4 9231`), который сверяют по **другому каналу** (телефон, личная встреча, своя домашняя страница).
- **Решение 2 (надёжнее) — Zertifikat** (см. 6.8.1).

- **Классы ключей:** **Sitzungsschlüssel** (сеансовый, короткоживущий — напр. Kerberos v4: 21 ч), **Masterschlüssel** (долгоживущий, порождает сеансовые), Austauschschlüssel.
- **PFS (Perfect Forward Secrecy):** параметры ключа меняются **в ходе соединения** → даже перехватив сеансовый ключ, атакующий не восстановит остальные/будущие.

### Diffie-Hellman-Schlüsselaustausch (DH) 🧮⭐⭐
**Объяснение.** DH позволяет двум сторонам **совместно вычислить общий секретный ключ** по открытому каналу, **ничего секретного не передавая**. ⚠️ **DH — это не шифрование и не асимметричный шифр**, а **протокол обмена ключами**. Основан на сложности **дискретного логарифма**.

**Ablauf ⭐:**
```
A wählt: Primzahl p, primitive Wurzel g, geheime Zufallszahl a (< p)
  A berechnet A' = gᵃ mod p   →  schickt (p, g, A') an B
B wählt geheime Zufallszahl b
  B berechnet B' = gᵇ mod p   →  schickt B' an A
A berechnet K = B'ᵃ mod p
B berechnet K = A'ᵇ mod p
  → beide erhalten denselben Schlüssel K = gᵃᵇ mod p
```
- **Публичны:** p, g, A', B'. **Секретны:** a, b, K.
- ⚠️ **DH не защищает от Man-in-the-Middle** (не проверяет Authentizität!) → нужно дополнительно **сертификатами** подтвердить, с кем говоришь. Атакующий посередине может навязать два отдельных ключа.
- ✅ Преимущество: **на каждую сессию новый ключ** (→ PFS). Даже перехват одного сеансового ключа не компрометирует другие сессии.
- Применяется в **TLS, IPsec, SSH**.

#### 🧮 Разобранный пример DH (маленькие числа)
```
p = 23, g = 5
A: a = 6  → A' = 5⁶ mod 23 = 15625 mod 23 = 8    (schickt 8)
B: b = 15 → B' = 5¹⁵ mod 23 = 19                 (schickt 19)
A: K = B'ᵃ = 19⁶ mod 23 = 2
B: K = A'ᵇ = 8¹⁵ mod 23 = 2
→ gemeinsamer Schlüssel K = 2
```

### Hybride Verfahren ⭐
Комбинируют оба подхода: **асимметрично** безопасно передают (или вырабатывают) сеансовый ключ → дальше **симметрично** быстро шифруют данные. → **скорость симметрии + удобство/безопасность асимметрии**. Примеры: **SSH, PGP, TLS**.

### 6.7.4 Message Authentication Code (MAC) ⭐⭐
**Объяснение.** Простой хеш не защищает от подделки (атакующий пересчитает H(m)). Если в хеш **втянуть общий секретный ключ k**, получится **MAC** = «контрольная сумма с ключом». Упрощённо: `MAC(m, k) = H(m + k)`.

**Ablauf:**
- Alice: `s = MAC(m, k)` → шлёт `m` и `s` по небезопасному каналу.
- Bob: сам считает `s' = MAC(m, k)`; если **s = s'** → сообщение **от Alice и не изменено**; если `s ≠ s'` → подделка/искажение.

**MAC достигает: Datenauthentizität + Integrität.** ⚠️ **MAC НЕ даёт Verbindlichkeit/Zurechenbarkeit!** Потому что **оба** (Alice и Bob) знают **один и тот же** ключ k → Bob не докажет третьей стороне (суду), что MAC сделала Alice, а не он сам.

| | **MAC** | **Hash (bloß)** |
|---|---|---|
| Schlüssel | **ja** (geheim, geteilt) | nein |
| Integrität | ja | ja |
| Authentizität | **ja** | nein |
| Verbindlichkeit | **nein** | nein |

- ✅ MAC: быстрый (симметричный), короткие ключи. ❌ Schlüsselaustauschproblem (как у симметрии). **HMAC** ➕ — стандартный MAC на основе хеша (используется в TLS).

---

## 6.8 Digitale Signaturen (S. 178–182) ⭐⭐

**Объяснение.** Цифровая подпись = **асимметричный** аналог MAC. Подписывают **приватным** ключом, проверяют **публичным** → теперь только **один** человек мог создать подпись, и это можно **доказать третьим лицам**.

**Цифровая подпись обеспечивает 3 Schutzziele ⭐:**
- **Authentizität** — сообщение действительно от заявленного автора;
- **Integrität** — не было изменено;
- **Verbindlichkeit (Nichtabstreitbarkeit)** — автор не отопрётся.
- ⚠️ **Vertraulichkeit — НЕ задача подписи** (документ может быть открытым, подпись лишь подтверждает подлинность).

**Ablauf (асимметрично + хеш) ⭐:**
```
Signieren (Absender):
  1. Hashwert h = H(Dokument)              ← хешируют, т.к. Public-Key медленный
  2. Signatur = Verschlüsselung von h mit PRIVATEM Schlüssel des Absenders
  3. Dokument + Signatur-Datei senden
Verifizieren (Empfänger):
  1. h' = H(empfangenes Dokument)
  2. h  = Entschlüsselung der Signatur mit ÖFFENTLICHEM Schlüssel des Absenders
  3. h == h'?  → echt und unverändert : gefälscht/verändert
```
> ⚠️⚠️ **Ключевая ловушка:** при **Verschlüsselung** шифруют **публичным** ключом **получателя**; при **Signatur** «шифруют» (кодируют хеш) **приватным** ключом **отправителя**. Направление ключей **противоположное**! Хешируют перед подписью, потому что подписывать весь большой файл через медленный Public-Key невыгодно.

**MAC vs. digitale Signatur ⭐ (частый вопрос):**
| | **MAC** | **Digitale Signatur** |
|---|---|---|
| Krypto | symmetrisch (geteilter Schlüssel k) | asymmetrisch (privat signieren, öffentlich prüfen) |
| Geschwindigkeit | schnell | langsam |
| Integrität + Authentizität | ja | ja |
| **Verbindlichkeit** | **nein** | **ja** |
| Schlüssel | 1 geheimer, geteilt | Schlüsselpaar |

- EU-eIDAS ➕: три уровня — **einfache / fortgeschrittene / qualifizierte** elektronische Signatur.

### 6.8.1 Zertifikate, X.509, PKI, Chain of Trust ⭐
**Объяснение.** Сертификат **связывает публичный ключ с личностью** (человеком/организацией/сервером) и **заверяет** эту связь подписью доверенной стороны → решает проблему «чей это ключ».

- **CA (Certification Authority, Zertifizierungsstelle):** выдаёт сертификаты, заверяет публичные ключи своей **цифровой подписью**, отзывает недействительные.
- **X.509** — стандарт формата сертификата. Содержит: **Versionsnummer, Seriennummer** (уникальна!), **Signatur(algorithmus)**, **Aussteller (Issuer)**, **Gültigkeitsdauer**, **Benutzername (Subject)**, **öffentlicher Schlüssel + Algorithmen**, расширения (v3).
- **Отзыв:** **CRL (Certificate Revocation List)** — список отозванных сертификатов (часто на LDAP-сервере). ➕ современнее — OCSP.
- **Chain of Trust / Zertifizierungskette (Abb. 6.21) ⭐:**
  ```
  Root CA (z.B. Bundesnetzagentur) — selbstsigniert
        │ signiert
  Zwischen-CA (z.B. Telekom AG)
        │ signiert
  Endnutzer-Zertifikat (z.B. HS Mittweida)
  ```
  Браузер доверяет корневому CA → по цепочке подписей доверие передаётся конечному сертификату. **PKI (Public-Key-Infrastruktur)** = вся эта система CA + сертификаты.
- В Германии высший CA — **Bundesnetzagentur** (как Root CA); для qualifizierte Signaturen нужна личная идентификация по паспорту (SigG/eIDAS).

---

## 6.9 Sichere Kanäle — SSL/TLS (S. 183–187) ⭐

**Sicherer Kanal** = **vertraulich + authentisch**. Три способа комбинировать шифрование и MAC (⭐ порядок важен):
| Variante | Reihenfolge | Protokoll |
|---|---|---|
| **Encrypt-then-MAC** | erst verschlüsseln, dann MAC über **Geheimtext** | **IPsec** — **einzig nachgewiesen sicher** (Krawczyk 2001) |
| **MAC-then-Encrypt** | MAC über Klartext, dann Klartext verschlüsseln | SSH |
| MAC-then-Encrypt (beides) | MAC über Klartext, dann Klartext **und** MAC verschlüsseln | SSL |

### SSL/TLS ⭐
- **SSL (Secure Socket Layer)** / **TLS (Transport Layer Security)**: шифрованный **«туннель»** между партнёрами + взаимная **аутентификация** через асимметрию и **сертификаты**. Порты: **HTTPS → 443**.
- Две части:
  | Teil | Aufgaben |
  |---|---|
  | **Record-Protokoll** | Fragmentierung, Kompression, **MAC-Berechnung** (+ Change Cipher Spec, Alert) |
  | **Handshake-Protokoll** | **Authentifikation** der Partner, **Aushandeln der Verfahren**, **Austausch geheimer Informationen** |

**TLS-Handshake — 4 Phasen ⭐:**
1. **Phase 1 (Client → Server, unverschlüsselt):** Zeitstempel, Session ID, Zufallszahl, **Cipher-Suite-Liste** (поддерживаемые методы).
2. **Phase 2 (Server → Client):** Zeitstempel, Zufallszahl, Session ID; сервер **выбирает** приоритетную Cipher Suite, шлёт **свой Zertifikat**, (опц. Certificate Request, ServerKeyExchange), «Hello Done».
3. **Phase 3 (Client):** проверяет данные/сертификат сервера, (опц. шлёт свой сертификат), «Change Cipher Spec» = «принимаю методы».
4. **Phase 4 (Server):** квитирует «Change Cipher Spec» → соединение согласовано → дальше идут **зашифрованные данные приложения**.

- **Cipher Suite** — набор методов, напр. `TLS_ECDH_RSA_WITH_AES_128_GCM_SHA256` = `TLS_`**метод обмена ключами (ECDH)**`_`**аутентификация (RSA)**`_WITH_`**шифр данных (AES-128-GCM)**`_`**хеш (SHA256)**.
- **TLS** = развитие SSL 3.1; использует **HMAC** для MAC, улучшенную генерацию ключей и расширенный Alert.

---

## Итоговая карта: Schutzziel → Mittel ⭐ (повесить над столом)
| Schutzziel | Mittel | Beispiele |
|---|---|---|
| **Vertraulichkeit** | Verschlüsselung + Schlüsselaustausch | AES, RSA, DES, ChaCha / DH, ECDH |
| **Integrität** | Hashfunktion (allein nur gegen Zufall), MAC, Signatur | SHA-2/3, HMAC |
| **(Daten-)Authentizität** | MAC oder Signatur | HMAC, RSA-Signatur |
| **Instanzauthentizität** | Challenge-Response | — |
| **Verbindlichkeit** | **nur digitale Signatur** (+ PKI) | RSA, DSA, X.509 |
| «с кем я говорю» | Zertifikat / Chain of Trust | CA, X.509 |

---

# Aufgaben — типичные и нетипичные
**[T]** типичная · **[N]** перенос · **[1,0]** высший балл · 🧮 расчёт. Решения сразу под задачей. *(В скрипте к Kap. 6 нет Kontrollaufgaben — блоки ниже я составил по материалу главы; расчёты перепроверены.)*

## A. Grundbegriffe & Zuordnung [T]

**A1. Erklären Sie den Zusammenhang von Kryptologie, Kryptografie und Kryptoanalyse.**
> **✅ Lösung:** **Kryptologie** ist der Oberbegriff und umfasst zwei Teildisziplinen: die **Kryptografie** (Methoden zum Ver-/Entschlüsseln, also zum **Schützen** von Nachrichten) und die **Kryptoanalyse** (Methoden zum **Brechen** von Verschlüsselung **ohne** Kenntnis des Schlüssels).

**A2. Was besagt das Kerckhoffs'sche Prinzip, und was ist der Gegensatz dazu?**
> **✅ Lösung:** Die Sicherheit eines Verfahrens darf **nur von der Geheimhaltung des Schlüssels** abhängen, **nicht** von der Geheimhaltung des Algorithmus. Gegensatz: **Security by Obscurity** (Geheimhaltung des Verfahrens) — hat sich nicht durchgesetzt, weil ohne öffentliche Expertenprüfung Designfehler übersehen werden. Beispiel für korrektes Vorgehen: AES wurde in einem offenen Wettbewerb ausgewählt.

**A3. Ordnen Sie zu: Schutzziel → Mittel.** Vertraulichkeit · Integrität · Datenauthentizität · Verbindlichkeit
Mittel: digitale Signatur · Verschlüsselung · Hashfunktion/MAC · MAC oder Signatur
> **✅ Lösung:** Vertraulichkeit → Verschlüsselung · Integrität → Hashfunktion/MAC · Datenauthentizität → MAC oder Signatur · Verbindlichkeit → digitale Signatur.

**A4. Ordnen Sie Verfahren ihrer Klasse zu:** AES · RSA · DES · Diffie-Hellman · RC4 · SHA-256 · HMAC · ECC
> **✅ Lösung:** **symmetrisch:** AES, DES, RC4 (Stromchiffre) · **asymmetrisch:** RSA, ECC · **Schlüsselaustausch:** Diffie-Hellman · **Hashfunktion:** SHA-256 · **MAC:** HMAC.

## B. Symmetrisch, XOR, Schlüsseltausch 🧮 [T]

**B1 🧮. XOR: Klartext m = 1 1 0 1, Schlüssel k = 1 0 1 1. Bestimmen Sie c. Zeigen Sie die Rückgewinnung von m.**
> **✅ Lösung:** `c = m ⊕ k = 1101 ⊕ 1011 = 0110`. Zurück: `c ⊕ k = 0110 ⊕ 1011 = 1101 = m` ✓.

**B2 🧮. In einem Netz mit 20 Personen: wie viele symmetrische Schlüssel sind nötig? Und bei 40?**
> **✅ Lösung:** n·(n−1)/2. Bei 20: 20·19/2 = **190**. Bei 40: 40·39/2 = **780**. (Verdopplung der Teilnehmer → ca. Vervierfachung der Schlüssel.)

**B3. Nennen Sie je zwei Vor- und Nachteile symmetrischer gegenüber asymmetrischer Verschlüsselung.**
> **✅ Lösung:** Symmetrisch — **Vorteile:** sehr schnell/hoher Durchsatz, kurze Schlüssel; **Nachteile:** Schlüsselaustauschproblem, n²/2 Schlüssel, keine Nichtabstreitbarkeit. Asymmetrisch — Vorteile: kein Austauschproblem, nur 1 Paar/Person, ermöglicht Signaturen; Nachteile: langsam, lange Schlüssel.

**B4. Warum ist das One-Time-Pad beweisbar sicher, aber praktisch unpraktisch?**
> **✅ Lösung:** Beweisbar sicher, weil der Schlüssel (1) echt zufällig, (2) genauso lang wie der Text und (3) nur **einmal** verwendet wird — dann passt zu jedem Geheimtext jeder beliebige Klartext, der Angreifer gewinnt keine Information. Unpraktisch, weil der Schlüssel **so lang wie die Nachricht** ist und sicher ausgetauscht werden muss — das verlagert nur das Problem. Lösung in der Praxis: pseudozufälliger Schlüsselstrom aus kurzem Schlüssel (Stromchiffre).

## C. Blockchiffren & Modi [T]

**C1. Warum ist der ECB-Modus unsicher? Welcher Modus behebt das und wie?**
> **✅ Lösung:** Im **ECB** wird jeder Block unabhängig verschlüsselt → **gleiche Klartextblöcke ergeben gleiche Geheimtextblöcke** → Muster/Struktur des Klartextes bleiben im Geheimtext sichtbar (Strukturerhaltung), und Blöcke lassen sich unbemerkt einfügen/umordnen. **CBC** behebt das: jeder Klartextblock wird vor der Verschlüsselung per **XOR mit dem vorherigen Geheimtextblock** verknüpft (erster mit einem zufälligen **IV**) → gleiche Blöcke werden kontextabhängig verschieden verschlüsselt.

**C2. Was sind Konfusion und Diffusion und wodurch werden sie erreicht? Was ist der Avalanche-Effekt?**
> **✅ Lösung:** **Konfusion** = Verschleierung des Zusammenhangs Klartext↔Geheimtext, erreicht durch **Substitution** (S-Boxen). **Diffusion** = Verteilung der Klartextinformation über den Geheimtext, erreicht durch **Permutation**. **Avalanche-Effekt:** Ändert man ein einzelnes Bit im Klartext oder Schlüssel, ändert sich nach mehreren Runden **jedes** Geheimtextbit mit ~50 % Wahrscheinlichkeit.

**C3. Nennen Sie die wichtigsten Eigenschaften von AES (mind. 5).**
> **✅ Lösung:** symmetrisch; Blockchiffre mit **128 Bit Blocklänge**; Schlüssellängen **128/192/256 Bit**; **10/12/14 Runden**; keine Feistel-Chiffre (ganzer Block pro Runde, 4×4-Byte-State); 4 Schritte je Runde (SubBytes, ShiftRows, MixColumns, AddRoundKey); FIPS 197 (2001), offen/lizenzfrei; eingesetzt in TLS, IPsec, WPA2, SSH.

**C4 [N]. Nonce im CTR-Modus: Warum darf sie bei gleichem Schlüssel nicht wiederholt werden, und muss sie geheim sein?**
> **✅ Lösung:** Wird dieselbe Nonce mit demselben Schlüssel erneut benutzt, entsteht derselbe **Schlüsselstrom** → XOR zweier Geheimtexte = XOR der Klartexte, und der Angreifer erkennt u. a., ob gleiche Klartexte verschlüsselt wurden (Informationsverlust). **Geheim muss die Nonce nicht sein** — sie wird sogar mitübertragen; die Sicherheit beruht allein auf der Geheimhaltung des Schlüssels (Kerckhoffs).

## D. RSA 🧮 [T][1,0]

**D1 🧮. RSA-Setup: p = 3, q = 11. Bestimmen Sie n, φ(n), einen gültigen öffentlichen Exponenten e und den passenden privaten Exponenten d.**
> **✅ Lösung:** n = 3·11 = **33**; φ(n) = (3−1)(11−1) = 2·10 = **20**. e teilerfremd zu 20 → z. B. **e = 7** (ggT(7,20)=1). d: 7·d ≡ 1 mod 20 → **d = 3** (7·3 = 21 = 20+1 ≡ 1 mod 20 ✓). Public (n,e) = (33,7), Private (p,q,d) = (3,11,3).

**D2 🧮. Mit (n,e) = (33,7) und d = 3: verschlüsseln Sie m = 4 und entschlüsseln Sie das Ergebnis.**
> **✅ Lösung:** `c = 4⁷ mod 33`. Schrittweise: 4²=16; 4³=64 mod 33 = 31; 4⁶ = 31² = 961 mod 33 = 961−29·33 = 961−957 = 4; 4⁷ = 4⁶·4 = 4·4 = 16 → **c = 16**. Entschlüsseln: `m = 16³ mod 33 = 4096 mod 33 = 4096−124·33 = 4096−4092 = 4` → **m = 4** ✓.

**D3 [T]. Warum gilt RSA als sicher, und was ist die „Falltür"?**
> **✅ Lösung:** Die Sicherheit beruht auf der (vermuteten) Schwierigkeit, den **RSA-Modul n in seine Primfaktoren p und q zu zerlegen** (Faktorisierungsproblem). n und e sind öffentlich, aber den privaten Exponenten **d kann nur berechnen, wer φ(n) = (p−1)(q−1) kennt** — und das setzt die Kenntnis von p und q voraus. Dieses Wissen (p, q) ist die **Falltür** der Einwegfunktion.

**D4 [1,0]. Alice will Bob eine vertrauliche Nachricht schicken und zusätzlich beweisen, dass sie von ihr stammt. Welche Schlüssel verwendet sie in welcher Reihenfolge?**
> **✅ Lösung:** Für **Vertraulichkeit** verschlüsselt Alice die Nachricht (bzw. den Sitzungsschlüssel, hybrid) mit **Bobs öffentlichem Schlüssel** — nur Bob kann mit seinem privaten entschlüsseln. Für **Authentizität/Verbindlichkeit** bildet sie den **Hash** der Nachricht und signiert ihn mit **ihrem eigenen privaten Schlüssel**; Bob prüft die Signatur mit **Alices öffentlichem Schlüssel**. Merke: **verschlüsseln mit dem öffentlichen Schlüssel des Empfängers, signieren mit dem eigenen privaten Schlüssel** — gegenläufige Schlüsselrichtungen.

## E. Hash & Geburtstagsparadoxon 🧮 [T]

**E1. Was sind die drei Sicherheitsanforderungen an eine kryptografische Hashfunktion?**
> **✅ Lösung:** (1) **effizient** berechenbar; (2) **Einwegeigenschaft** (zu gegebenem Hash praktisch kein passender Input findbar); (3) **Kollisionsresistenz** (praktisch keine zwei verschiedenen Inputs mit gleichem Hash findbar).

**E2 🧮. Eine Hashfunktion liefert 64-Bit-Werte. Nach wie vielen Eingaben ist eine Kollision mit ~50 % Wahrscheinlichkeit zu erwarten? Wie stark ist sie damit gegen Kollisionen?**
> **✅ Lösung:** ~1,2·√(2⁶⁴) ≈ 1,2·2³² ≈ **5,2 Milliarden** Eingaben, grob in der Größenordnung **2³²**. Die **Kollisionsresistenz** entspricht also nur ~**2³² = 32 Bit** (≈ √(2⁶⁴)) — viel zu wenig (Schwelle ~100 Bit). Deshalb muss ein Hash etwa **doppelt so lang** wie die gewünschte Sicherheit sein (Geburtstagsparadoxon).

**E3. Warum gilt MD5 als unsicher und SHA-1 als veraltet?**
> **✅ Lösung:** Für beide sind **Kollisionen** praktisch berechenbar (MD5: 2¹⁸-Angriff; SHA-1: 2⁵²-Angriff, 160 Bit → Kollisionsresistenz nur ~2⁸⁰ wegen Geburtstagsparadoxon). Dadurch lassen sich zwei verschiedene Dokumente mit gleichem Hash erzeugen → Signaturen/Integritätsschutz werden unterlaufen. Empfohlen: **SHA-2/SHA-3**.

## F. Diffie-Hellman 🧮 [T][1,0]

**F1 🧮. DH mit p = 11, g = 2, a = 4, b = 3. Berechnen Sie A', B' und den gemeinsamen Schlüssel K.**
> **✅ Lösung:** A' = 2⁴ mod 11 = 16 mod 11 = **5**. B' = 2³ mod 11 = **8**. K bei A: B'ᵃ = 8⁴ mod 11 = 4096 mod 11 = 4096−372·11 = 4096−4092 = **4**. K bei B: A'ᵇ = 5³ mod 11 = 125 mod 11 = 125−121 = **4**. → gemeinsamer Schlüssel **K = 4** ✓.

**F2 [1,0]. Warum ist Diffie-Hellman anfällig für Man-in-the-Middle, obwohl die geheimen Zahlen a und b nie übertragen werden? Wie behebt man das?**
> **✅ Lösung:** DH **authentifiziert die Partner nicht**. Ein Angreifer in der Mitte kann **zwei getrennte DH-Austausche** führen — einen mit A (gibt sich als B aus), einen mit B (gibt sich als A aus) — und so **zwei** Schlüssel aushandeln, die er beide kennt. A und B merken nichts, weil nie geprüft wird, **mit wem** sie den Schlüssel bilden. Behebung: vorher **Authentizität per Zertifikat/Signatur** prüfen (authentisiertes DH, wie in TLS).

**F3. Nennen Sie Vorteil und Einschränkung von DH und ein Protokoll, das es nutzt.**
> **✅ Lösung:** Vorteil: gemeinsamer Schlüssel wird **dezentral** ohne Übertragung des Geheimnisses gebildet, **pro Sitzung neu** (Forward Secrecy). Einschränkung: **keine Verschlüsselung** und **kein MitM-Schutz** (nur Schlüsselaustausch). Protokolle: **TLS, IPsec, SSH**.

## G. MAC, Signatur, Zertifikate, TLS [T][1,0]

**G1. Worin unterscheiden sich ein MAC und eine digitale Signatur? Welches Schutzziel kann nur die Signatur?**
> **✅ Lösung:** MAC ist **symmetrisch** (gemeinsamer geheimer Schlüssel), schnell; Signatur ist **asymmetrisch** (privat signieren, öffentlich verifizieren), langsamer. Beide liefern **Integrität + Authentizität**. **Nur die Signatur** liefert **Verbindlichkeit/Nichtabstreitbarkeit**, weil beim MAC **beide** Partner denselben Schlüssel kennen und Bob daher einem Dritten nicht beweisen kann, dass Alice (und nicht er selbst) den MAC erzeugt hat.

**G2. Warum reicht ein einfacher Hash nicht, um die Echtheit einer Nachricht zu sichern?**
> **✅ Lösung:** Ein Hash erkennt nur **Veränderungen** (Integrität), aber **jeder** — auch ein Angreifer — kann H(m) neu berechnen. Verändert der Angreifer m zu m' und hängt H(m') an, merkt der Empfänger nichts. Erst ein **geheimer Schlüssel** (MAC) oder ein **privater Schlüssel** (Signatur) bindet den Hash an den Absender → Authentizität.

**G3. Erklären Sie Chain of Trust und wozu Zertifikate dienen.**
> **✅ Lösung:** Ein **Zertifikat (X.509)** bindet einen öffentlichen Schlüssel an eine Identität und wird von einer **CA** mit deren privatem Schlüssel signiert → beglaubigt „dieser Schlüssel gehört wirklich zu X". Die **Chain of Trust** ist die Kette: **Root-CA** (selbstsigniert) → **Zwischen-CA** → **Endnutzer-Zertifikat**; wer der Root-CA vertraut, vertraut über die Signaturkette dem Endzertifikat. Das löst das Problem „gehört dieser öffentliche Schlüssel wirklich dem angeblichen Besitzer?". Die Gesamtstruktur heißt **PKI**.

**G4 [N]. Cipher Suite `TLS_ECDH_RSA_WITH_AES_128_GCM_SHA256` — erklären Sie die vier Bestandteile.**
> **✅ Lösung:** **ECDH** = Schlüsselaustausch (Elliptic-Curve-Diffie-Hellman); **RSA** = Authentifizierung/Signatur (Zertifikat); **AES_128_GCM** = symmetrische Verschlüsselung der Nutzdaten (AES mit 128-Bit-Schlüssel im GCM-Modus); **SHA256** = Hashfunktion für den MAC/die Integrität. Das zeigt den **hybriden** Aufbau: asymmetrisch (ECDH/RSA) zum sicheren Aushandeln, symmetrisch (AES) für die schnellen Daten.

**G5 [1,0]. Beschreiben Sie, wie beim TLS-Handshake aus asymmetrischer und symmetrischer Kryptografie ein sicherer Kanal entsteht.**
> **✅ Lösung:** Im Handshake einigen sich Client und Server zunächst **unverschlüsselt** auf eine **Cipher Suite** und tauschen Zufallszahlen aus (Phase 1–2). Der Server weist seine **Identität per Zertifikat** nach (asymmetrisch, CA-signiert) → Schutz vor MitM. Über ein **asymmetrisches Verfahren bzw. authentisiertes Diffie-Hellman** wird ein gemeinsames **Geheimnis** sicher ausgehandelt, aus dem der **symmetrische Sitzungsschlüssel** abgeleitet wird. Nach „Change Cipher Spec" (Phase 3–4) werden alle Anwendungsdaten **symmetrisch (z. B. AES)** verschlüsselt und per **MAC/HMAC** integritätsgesichert. So kombiniert TLS die **Sicherheit/Authentizität** der asymmetrischen Verfahren mit der **Geschwindigkeit** der symmetrischen (hybrides Verfahren).

## H. Richtig oder falsch? (mit Begründung) [T/N]
1. Bei symmetrischer Verschlüsselung kennen beide Partner denselben Schlüssel.
2. RSA wird typischerweise verwendet, um große Dateien direkt zu verschlüsseln.
3. Ein MAC gewährleistet Nichtabstreitbarkeit.
4. Beim Signieren verschlüsselt der Absender den Hash mit dem öffentlichen Schlüssel des Empfängers.
5. Diffie-Hellman ist ein asymmetrisches Verschlüsselungsverfahren.
6. Im ECB-Modus werden gleiche Klartextblöcke zu gleichen Geheimtextblöcken.
7. Eine Hashfunktion ist injektiv.
8. Das One-Time-Pad ist beweisbar sicher.
9. Die Nonce im CTR-Modus muss geheim gehalten werden.
10. Die Sicherheit von AES hängt von der Geheimhaltung des Algorithmus ab.
11. Zur Kollisionssuche braucht man wegen des Geburtstagsparadoxons nur ca. die Wurzel aller möglichen Hashwerte.
12. Eine digitale Signatur sorgt für Vertraulichkeit.
> **✅ Lösung:**
> 1. **Richtig** (e = d). 2. **Falsch** — RSA ist langsam; große Daten → symmetrisch, RSA nur für den Schlüssel (hybrid). 3. **Falsch** — nur Integrität+Authentizität; beide kennen k. 4. **Falsch** — mit dem **eigenen privaten** Schlüssel des Absenders. 5. **Falsch** — DH ist ein **Schlüsselaustausch**verfahren, keine Verschlüsselung. 6. **Richtig** (Strukturerhaltung → unsicher). 7. **Falsch** — nicht injektiv (Kollisionen existieren immer, da Eingaben länger als Ausgaben). 8. **Richtig** (bei zufälligem, gleich langem, einmaligem Schlüssel). 9. **Falsch** — nur eindeutig (einmalig), nicht geheim. 10. **Falsch** — nur vom Schlüssel (Kerckhoffs); AES ist offen. 11. **Richtig** (~1,2·√n). 12. **Falsch** — Authentizität/Integrität/Verbindlichkeit, **nicht** Vertraulichkeit.

## T. Transfer & Verständnis — Skript anwenden ⭐
> Крипто-задачи на экзамене часто просят *выбрать и обосновать* средство под цель, а не пересказать алгоритм.

**T1 [1,0]. Alice will Bob eine große Datei schicken, die (a) nur Bob lesen kann, (b) nachweislich von Alice stammt und unverändert ist. Welche Verfahren kombiniert sie in welcher Reihenfolge und mit welchen Schlüsseln? Warum nicht alles mit RSA?**
> **✅ Lösungsweg:** **Hybrid + Signatur.** (1) Alice bildet **Hash** der Datei und **signiert** ihn mit **ihrem privaten** Schlüssel → Authentizität, Integrität, **Verbindlichkeit** (b). (2) Sie erzeugt einen **symmetrischen Sitzungsschlüssel** (AES) und verschlüsselt die große Datei damit → schnell. (3) Den AES-Schlüssel verschlüsselt sie mit **Bobs öffentlichem** Schlüssel → nur Bob entschlüsselt ihn (a). **Nicht alles mit RSA**, weil asymmetrisch **zu langsam** für große Daten ist — darum nur der kleine Schlüssel/Hash asymmetrisch, die Masse symmetrisch. Merke die **gegenläufigen Schlüssel**: verschlüsseln mit Empfänger-public, signieren mit eigenem private.

**T2 [1,0]. Ein Entwickler speichert Passwörter „sicher" als MD5-Hash. Nennen Sie zwei unabhängige Gründe, warum das heute unsicher ist, und was man stattdessen tut.**
> **✅ Lösungsweg:** (1) **MD5 ist kollisions-/angreifbar** und **extrem schnell** → Milliarden Hashes/s per Brute-Force/**Wörterbuch** (vgl. Praktikum: `5f4dcc…` = „password"). (2) **Kein Salt** → gleiche Passwörter geben gleichen Hash → **Rainbow Tables** und Wiedererkennung. Richtig: **langsame, gesalzene** Passwort-Hashes (bcrypt/scrypt/Argon2 ➕), pro Nutzer eigenes **Salt**. (Zeigt Verständnis, dass „Hash ≠ automatisch sicher" — es kommt auf Algorithmus, Geschwindigkeit und Salt an.)

**T3 [1,0]. Warum bietet Diffie-Hellman „Forward Secrecy", aber keinen Schutz gegen Man-in-the-Middle — und wie behebt man Letzteres, ohne DH aufzugeben?**
> **✅ Lösungsweg:** **Forward Secrecy:** DH erzeugt **pro Sitzung einen neuen** gemeinsamen Schlüssel aus frischen Zufallszahlen a,b; selbst wenn ein späterer Schlüssel/Langzeitschlüssel kompromittiert wird, bleiben **frühere Sitzungen** unlesbar. **Kein MitM-Schutz:** DH **authentifiziert die Partner nicht** → ein MitM kann zwei getrennte DH-Austausche führen. **Behebung ohne DH aufzugeben:** DH **authentisieren** — die DH-Werte per **Zertifikat/Signatur** bestätigen (so macht es TLS: „authenticated DH/ECDHE"). Man kombiniert also DH (für Forward Secrecy) mit PKI (für Authentizität).

**T4 [N] 🧮. RSA mit p=7, q=13, e=5. Bestimmen Sie φ(n), prüfen Sie e und berechnen Sie d (zeigen Sie den Weg).**
> **✅ Lösungsweg:** n = 91, **φ = 6·12 = 72**. ggT(5,72)=1 → e gültig. d: `5·d ≡ 1 mod 72`. Подбор: `(k·72+1)/5` → k=2 → 145/5 = **29** → **d = 29** (Probe: 5·29 = 145 = 2·72+1 ≡ 1 mod 72 ✓). Или Евклид: 72=14·5+2; 5=2·2+1 → 1 = 5−2·2 = 5−2·(72−14·5) = 29·5 − 2·72 → d=29.

**T5 [1,0]. Begründen Sie mit dem Geburtstagsparadoxon, warum ein 128-Bit-Hash für Kollisionsresistenz „nur" ~64-Bit-Sicherheit bietet — und was das für die Wahl der Hashlänge heißt.**
> **✅ Lösungsweg:** Eine Kollision tritt mit 50 % schon bei **~√(2¹²⁸) = 2⁶⁴** Versuchen auf (Geburtstagsparadoxon: ~1,2·√n). Also ist die **Kollisionsresistenz ≈ halbe Hashlänge**. Für eine Zielsicherheit von ~128 Bit gegen Kollisionen braucht man daher einen **~256-Bit-Hash (SHA-256)**. Deshalb gilt SHA-1 (160 Bit → nur ~80-Bit-Kollisionsresistenz) als veraltet. Regel: **Hash doppelt so lang wie die gewünschte Kollisionssicherheit**.

## I. Самопроверка перед Kap. 7 (с ответами)
**I1. Symmetrisch vs. asymmetrisch — je 1 Vor-/Nachteil + Beispiel?** Sym: schnell, aber Schlüsseltauschproblem (AES). Asym: kein Austauschproblem, aber langsam (RSA).
**I2. Kerckhoffs?** Sicherheit nur im Schlüssel, nicht im Algorithmus.
**I3. Schlüsselanzahl bei n Teilnehmern (symmetrisch)?** n(n−1)/2.
**I4. ECB vs. CBC?** ECB: Blöcke unabhängig → Muster sichtbar. CBC: XOR mit Vorgängerblock + IV → kontextabhängig.
**I5. AES: Typ, Blocklänge, Schlüssellängen, Runden?** symmetrisch, 128 Bit Block, 128/192/256 Bit, 10/12/14 Runden.
**I6. RSA-Schritte?** Setup (p,q,n,φ,e,d) → c = mᵉ mod n → m = cᵈ mod n; Sicherheit = Faktorisierung.
**I7. Hash — 3 Anforderungen + Schutzziel?** effizient, Einweg, kollisionsresistent; Integrität.
**I8. Geburtstagsparadoxon-Formel?** Kollision bei ~1,2·√n (n = Zahl möglicher Hashwerte) → Hash doppelt so lang wie Zielsicherheit.
**I9. Diffie-Hellman — was, Schwäche?** gemeinsamer Schlüssel ohne Übertragung; keine Verschlüsselung, kein MitM-Schutz.
**I10. MAC vs. Signatur?** beide Integrität+Authentizität; nur Signatur (asymmetrisch) → Verbindlichkeit.
**I11. Signieren — welche Schlüssel?** privat signieren, öffentlich verifizieren (umgekehrt zur Verschlüsselung).
**I12. Zertifikat/Chain of Trust/PKI?** CA bindet Schlüssel an Identität per Signatur; Root-CA → Zwischen-CA → Endzertifikat.
**I13. TLS — welche Kryptoarten und warum hybrid?** asymmetrisch (Handshake/Zertifikat/Schlüssel) + symmetrisch (AES-Daten) → Sicherheit + Geschwindigkeit.

Если на всё ответил без подсказки — переходим к `07_Zugriffskontrolle.md` (RBAC, Bell-LaPadula, Chinese Wall).
