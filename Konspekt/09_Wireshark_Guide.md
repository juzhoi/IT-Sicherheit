# Wireshark- & Log-Analyse — Prüfungs-Spickzettel 🦈
**Für die Aufgaben „analysieren Sie das Log im Terminal"**

> **О чём файл.** Это рабочий справочник под экзаменационное задание «прочитай лог/вывод Wireshark/tshark и скажи, что происходит». Собрано всё, что нужно: как читать строку, фильтры, флаги TCP, порты, и — главное — **сигнатуры атак** (как каждая атака выглядит в логе) с готовыми формулировками ответа. Плюс чтение вывода команд (ping/netstat/nmap/arp).
>
> **Как пользоваться на экзамене:**
> 1. Определи **протокол** (столбец Protocol) и **кто с кем** (Source → Destination, порты).
> 2. Сопоставь картину с разделом **«Сигнатуры атак»** ниже.
> 3. Назови: **что это за атака/процесс**, **какое Schutzziel**, **признаки в логе**, **мера защиты**.
>
> 🔗 Теория: [02 Rechnernetze](02_Rechnernetze.md), [03 Angriffe](03_Angriffe.md).

## Содержание
Строка лога · Фильтры Wireshark · Флаги TCP (hex) · Seq/Ack · Порты/протоколы · **Сигнатуры атак** ⭐⭐ · Чтение команд (ping/netstat/nmap/arp/traceroute) · Чек-лист анализа · Тренировочные логи A–F

---

## 1. Как читать строку лога

**Формат Wireshark (графический / `tshark`):**
```
No.  Time      Source          Destination     Protocol  Length  Info
5    0.004100  192.168.1.20    10.0.0.5        HTTP      145     GET /login.php HTTP/1.1
```
- **Source / Destination** — IP (или имя). Кто инициатор — смотри, кто шлёт первый SYN.
- **Protocol** — верхний распознанный протокол (ARP, TCP, HTTP, DNS, ICMP…).
- **Info** — самое важное: флаги, Seq/Ack, запрос/ответ.
- **Length** — размер кадра в байтах (голый TCP-ACK ≈ 54–66 байт).

**Окно деталей = слои (сверху вниз) 🔗 [02 → 2.5](02_Rechnernetze.md):**
```
Frame 5: 145 bytes
Ethernet II, Src: 00:21:cc:.. Dst: 00:1a:4f:..     ← Schicht 2 (MAC)
Internet Protocol Version 4, Src: .., Dst: ..      ← Schicht 3 (IP, TTL, Protocol)
Transmission Control Protocol, Src Port.., Dst..   ← Schicht 4 (Ports, Flags, Seq/Ack)
Hypertext Transfer Protocol                         ← Schicht 7 (Nutzdaten)
```

**Hex-Dump внизу** — для задач «прочитай заголовок из hex» 🔗 [02 → Aufgaben C](02_Rechnernetze.md): байты IP/TCP-заголовка. Hex→Dez: первая цифра · 16 + вторая (a=10…f=15).

---

## 2. Anzeigefilter (Display Filter) — то, что набирают в строке фильтра ⭐
| Фильтр | Показывает |
|---|---|
| `ip.addr == 192.168.1.20` | весь трафик этого хоста (src ИЛИ dst) |
| `ip.src == x` / `ip.dst == x` | только как источник / получатель |
| `tcp.port == 80` / `udp.port == 53` | по порту |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | только **SYN** (поиск сканов/SYN-flood) |
| `tcp.flags.reset == 1` | **RST** (закрытые порты, обрывы) |
| `http` / `http.request` | HTTP / только запросы |
| `dns` | DNS-запросы и ответы |
| `arp` | ARP (поиск спуфинга) |
| `icmp` | ping, Time Exceeded, Unreachable |
| `tcp.analysis.retransmission` | повторные передачи |
| `tcp.analysis.flags` | все замечания анализатора (Dup ACK, ZeroWindow…) |
| `frame contains "password"` | кадры с подстрокой (поиск паролей в Klartext) |

➕ **tshark** (командная строка): `tshark -r datei.pcap -Y "http.request"` (`-Y` = display filter), `-z io,phs` (иерархия протоколов), `-q -z conv,tcp` (список соединений).

---

## 3. TCP-Flags ⭐⭐ (самое нужное для чтения логов)
🔗 [02 → 2.7.1](02_Rechnernetze.md)
| Flag | Значение | Когда видно |
|---|---|---|
| **SYN** | начало соединения, синхр. Seq | пакет 1–2 рукопожатия |
| **ACK** | поле Ack действительно | почти в каждом пакете после первого SYN |
| **FIN** | штатное закрытие | конец соединения |
| **RST** | сброс / отказ | ошибка, закрытый порт, отклонённое соединение |
| **PSH** | сразу отдать приложению | сегменты с данными `[PSH, ACK]` |
| **URG** | срочные данные | редко |

**Flags-Byte из hex (биты слева: CWR ECE URG ACK PSH RST SYN FIN):**
| Hex | Flags | Смысл |
|---|---|---|
| `02` | SYN | 1-й пакет рукопожатия |
| `12` | SYN, ACK | ответ сервера (порт **открыт**) |
| `10` | ACK | подтверждение |
| `18` | PSH, ACK | данные |
| `11` | FIN, ACK | закрытие |
| `04` / `14` | RST / RST, ACK | порт **закрыт** / сброс |
| `29` | URG, PSH, FIN | **Xmas-Scan** (nmap) |
| `00` | — | **Null-Scan** (nmap) |

**Three-Way-Handshake в Wireshark (относительные Seq):**
```
1  A → B  [SYN]      Seq=0            ← инициатор = клиент (эфемерный порт), B = сервер (well-known порт)
2  B → A  [SYN, ACK] Seq=0 Ack=1
3  A → B  [ACK]      Seq=1 Ack=1      ← ESTABLISHED
```

---

## 4. Seq/Ack быстро 🧮
🔗 [02 → 2.7.3](02_Rechnernetze.md)
```
Ack_Empfänger = Seq_empfangen + Len   (SYN и FIN считаются за 1 байт, чистый ACK Len=0 → ничего не меняет)
```
Признаки проблем: `[TCP Retransmission]`, `[TCP Dup ACK]`, `[TCP ZeroWindow]` (буфер получателя полон, 🔗 Flusskontrolle), `[TCP Previous segment not captured]`.

---

## 5. Порты и протоколы ⭐ (для опознания сервиса)
🔗 [02 → 2.10.2](02_Rechnernetze.md)
| Порт | Сервис | Klartext? |
|---|---|---|
| 20/21 | FTP | **да** ⚠️ |
| 22 | SSH | нет |
| 23 | Telnet | **да** ⚠️ |
| 25 | SMTP | да |
| 53 | DNS (UDP запросы, TCP зонный трансфер) | да |
| 67/68 | DHCP | да |
| 80 | HTTP | **да** ⚠️ |
| 110/143 | POP3/IMAP | да |
| 443 | HTTPS (TLS) | **нет** ✅ |
| 445 | SMB | — (частая мишень) |
| 3389 | RDP | — |
| 4444 | **Metasploit** (reverse shell) ⚠️ | — |

→ Трафик на **80/21/23/110** = потенциально читаемые пароли (Schutzziel **Vertraulichkeit**). Исходящее на **4444** от сервера = подозрение на **Backdoor**.

---

## 6. Сигнатуры атак в логе ⭐⭐⭐ (ядро экзамена)
Для каждой — **как выглядит**, **признак**, **Schutzziel**, **мера**.

### Port-Scan (SYN-/Half-Open-Scan, nmap -sS)
```
10.0.0.66 → 10.0.0.5  TCP 41000 → 21  [SYN]
10.0.0.66 → 10.0.0.5  TCP 41000 → 22  [SYN]
10.0.0.66 → 10.0.0.5  TCP 41000 → 23  [SYN]   ← много SYN, РАЗНЫЕ целевые порты, один источник
10.0.0.5  → 10.0.0.66 TCP 22 → 41000  [SYN, ACK]  ← порт 22 ОТКРЫТ
10.0.0.5  → 10.0.0.66 TCP 21 → 41000  [RST, ACK]  ← порт 21 ЗАКРЫТ
10.0.0.66 → 10.0.0.5  TCP 41000 → 22  [RST]       ← сканер НЕ завершает handshake (stealth)
```
- **Признак:** один источник, одинаковый src-порт, **много разных целевых портов** (вертикальный скан) ИЛИ один порт на **многих хостах** (горизонтальный скан), в микросекундах; маленький Window (1024). SYN/ACK = open, RST = closed, нет ответа = filtered.
- **Это:** Angriffsvorbereitung (Reconnaissance). **Schutzziel:** пока не нарушено (разведка). **Мера:** Firewall (только нужные порты), IDS.

### SYN-Flooding (DoS)
```
203.0.113.17 → 10.0.0.5  TCP [SYN] 80
198.51.100.4 → 10.0.0.5  TCP [SYN] 80
192.0.2.200  → 10.0.0.5  TCP [SYN] 80   ← тысячи SYN от РАЗНЫХ (поддельных) IP, БЕЗ 3-го пакета ACK
```
В `netstat`: множество `SYN_RECV`. 🔗 [03 → 3.3.5](03_Angriffe.md)
- **Признак:** лавина SYN с разных (spoofed) адресов, рукопожатие **не завершается** → полуоткрытые соединения. **Schutzziel: Verfügbarkeit.** **Мера:** SYN-Cookies, rate-limiting.

### ARP-Spoofing / MitM ⭐
```
00:1a:4f:.. → ff:ff:ff:ff:ff:ff  ARP  Who has 192.168.1.1? Tell 192.168.1.20   ← нормальный запрос
de:ad:be:ef:00:01 → 00:21:cc:..  ARP  192.168.1.1 is at de:ad:be:ef:00:01       ← НЕЗАПРОШЕННЫЙ reply
de:ad:be:ef:00:01 → 00:1a:4f:..  ARP  192.168.1.20 is at de:ad:be:ef:00:01      ← и в другую сторону
```
🔗 [03 → 3.3.3](03_Angriffe.md)
- **Признак:** **unaufgeforderte ARP-Replies** (без предшествующего request), **две разные IP с одной MAC**, повторяются каждые ~2 с. Wireshark: *„duplicate use of … detected"*. **Schutzziel:** Vertraulichkeit (+ Integrität/Verfügbarkeit). **Мера:** статические ARP-записи, Dynamic ARP Inspection, TLS против последствий.

### DNS-Spoofing / Cache-Poisoning
```
192.168.1.20 → resolver  DNS  query 0x7c11 A www.bank.de
resolver → 192.168.1.20  DNS  response 0x7c11 A 6.6.6.6         ← подложный, приходит ПЕРВЫМ
resolver → 192.168.1.20  DNS  response 0x7c11 A 193.17.42.10    ← настоящий, опоздал
```
🔗 [03 → 3.3.8](03_Angriffe.md)
- **Признак:** на **один** запрос (та же Transaction-ID) **два ответа с разными IP**, подложный быстрее. **Schutzziel:** Integrität/Authentizität → Phishing. **Мера:** DNSSEC, HTTPS + проверка сертификата.

### Rogue DHCP / APIPA
```
PC:  IPv4 169.254.88.12   (kein Gateway)   ← APIPA: DHCP-Server НЕ найден (сбой/Starvation)
PC:  Gateway=DNS=192.168.1.66, echter Server=192.168.1.1  ← навязанный Rogue-DHCP → MitM
```
🔗 [02 → 2.12.2](02_Rechnernetze.md) — **Мера:** DHCP Snooping.

### Klartext-Passwort (Sniffing) ⭐
```
192.168.1.20 → 10.0.0.5  HTTP  GET /login.php?user=anna&pw=Sommer2026 HTTP/1.1
```
- **Признак:** логин/пароль в **HTTP (80), FTP, Telnet** в открытом виде; фильтр `frame contains "pw"`. **Schutzziel: Vertraulichkeit.** **Мера:** HTTPS/TLS, POST вместо GET.

### ICMP-основанное
```
192.168.1.20 → 8.8.8.8  ICMP  Echo (ping) request          ← разведка/ping-sweep
10.0.0.1 → 192.168.1.20 ICMP  Time-to-live exceeded          ← traceroute (TTL дошёл до 0)
10.0.0.5 → 192.168.1.20 ICMP  Destination unreachable (Port) ← UDP на закрытый порт
```
Лавина Echo = **Ping-/ICMP-Flood (DoS)**. 🔗 [02 → 2.12.4](02_Rechnernetze.md)

### SQL-Injection / Web-атаки в HTTP-логе (Apache access.log)
```
194.38.105.19 - - [12/Jan/2014:09:12:25] "GET /sql/index.php HTTP/1.1" 404 ...
194.38.105.19 - - [12/Jan/2014:09:12:25] "GET /phpmyadmin/index.php HTTP/1.1" 404 ...
```
- **Признак:** много `GET` к админ-/БД-путям (`phpmyadmin`, `mysql`, `/admin`) с кодом **404** → **сканирование** (ещё не успех). `200` на такой путь → успех. SQLi видна как `' OR 1=1` / `UNION SELECT` в параметрах. 🔗 [08 → Praktikum 03](08_Praktika.md)
- Коды: **200** OK (успех), **404** not found, **403** forbidden, **500** server error, **501** (TLS-рукопожатие на HTTP-порт).

---

## 7. Чтение вывода команд
🔗 [02 → 2.11](02_Rechnernetze.md)

**ping** — `ttl=53` → Start-TTL (64 Linux / 128 Windows / 255 сетевое) − ttl ≈ число хопов; `0% packet loss` = доступен; нет ответа ≠ выключен (может фильтроваться ICMP).
**netstat -ano** — `LISTEN` ждёт; `ESTABLISHED` активно; много `SYN_RECV` → SYN-Flood; исходящее на :4444 → Backdoor; `-o` даёт PID процесса.
**nmap** — `80/tcp open http`, `Not shown: 991 closed ports`; порты 135/139/445 → Windows (SMB); `MAC Address` виден только в локальной сети.
**arp -a** — две IP с одной MAC → ARP-Spoofing.
**traceroute** — `* * *` = роутер не отвечает (фильтрует ICMP), пакет идёт дальше; хоп 1 = свой шлюз.

---

## 8. Чек-лист анализа лога (порядок действий на экзамене)
1. **Протокол?** (ARP / ICMP / TCP / UDP / DNS / HTTP)
2. **Кто инициатор?** (первый SYN; эфемерный порт = клиент, well-known = сервер)
3. **Нормальный обмен или аномалия?** (много SYN? unsolicited ARP? два DNS-ответа? Klartext-пароль? исходящее на 4444?)
4. **Назвать атаку/процесс** + **признак в логе** (цитировать строку!).
5. **Schutzziel** (mitlesen→Vertraulichkeit, verändern→Integrität, blockieren→Verfügbarkeit, Identität→Authentizität).
6. **Gegenmaßnahme.**

---

# Übungslogs — тренировка 🦈
Сначала ответь сам, потом сверяйся.

### Log A [T]
```
1  10.0.0.9 → 10.0.0.5  TCP  53122 → 80   [SYN]      Seq=0 Win=64240
2  10.0.0.5 → 10.0.0.9  TCP  80 → 53122   [SYN, ACK] Seq=0 Ack=1
3  10.0.0.9 → 10.0.0.5  TCP  53122 → 80   [ACK]      Seq=1 Ack=1
4  10.0.0.9 → 10.0.0.5  HTTP 53122 → 80   GET /index.html HTTP/1.1   Len=120
5  10.0.0.5 → 10.0.0.9  TCP  80 → 53122   [ACK] Seq=1 Ack=121
```
Что происходит? Кто клиент/сервер? Что в пакете 5 означает Ack=121?
> **✅ Lösung:** Нормальный **HTTP-запрос** по TCP. **Клиент** = 10.0.0.9 (первый SYN, эфемерный порт 53122), **сервер** = 10.0.0.5 (порт 80). Пакеты 1–3 = Three-Way-Handshake. Пакет 4: клиент шлёт GET (120 байт). Пакет 5: сервер подтверждает **Ack = 1 + 120 = 121** (следующий ожидаемый байт). Никакой атаки. ⚠️ HTTP (80) → при логине пароль был бы в открытом виде.

### Log B [T]
```
1  172.16.0.50 → 172.16.0.9  TCP 40000 → 21   [SYN]
2  172.16.0.50 → 172.16.0.9  TCP 40000 → 22   [SYN]
3  172.16.0.50 → 172.16.0.9  TCP 40000 → 23   [SYN]
4  172.16.0.50 → 172.16.0.9  TCP 40000 → 80   [SYN]
5  172.16.0.9  → 172.16.0.50 TCP 22 → 40000   [SYN, ACK]
6  172.16.0.9  → 172.16.0.50 TCP 80 → 40000   [SYN, ACK]
7  172.16.0.9  → 172.16.0.50 TCP 21 → 40000   [RST, ACK]
8  172.16.0.9  → 172.16.0.50 TCP 23 → 40000   [RST, ACK]
```
Что это? Какие порты открыты/закрыты? Почему нет 3-го ACK?
> **✅ Lösung:** **Port-Scan (SYN-/Half-Open-Scan, nmap -sS)** от 172.16.0.50 на 172.16.0.9. **Открыты:** 22 (SSH), 80 (HTTP) — ответ SYN/ACK. **Закрыты:** 21 (FTP), 23 (Telnet) — ответ RST/ACK. Сканер **не шлёт финальный ACK** → соединение не завершается (stealth, часто не логируется приложением). Это Angriffsvorbereitung; мера — Firewall/IDS.

### Log C [T] ⭐
```
1  00:11:22:33:44:55 → ff:ff:ff:ff:ff:ff  ARP  Who has 192.168.0.1? Tell 192.168.0.50
2  aa:bb:cc:dd:ee:ff → 00:11:22:33:44:55  ARP  192.168.0.1 is at aa:bb:cc:dd:ee:ff
3  13:37:13:37:13:37 → 00:11:22:33:44:55  ARP  192.168.0.1 is at 13:37:13:37:13:37
4  13:37:13:37:13:37 → 00:11:22:33:44:55  ARP  192.168.0.1 is at 13:37:13:37:13:37
```
Was ist auffällig? Welcher Angriff, welches Schutzziel, welche Maßnahme?
> **✅ Lösung:** IP **192.168.0.1** (шлюз) появляется с **двумя разными MAC**: настоящей `aa:bb:..` (пакет 2, ответ на запрос) и `13:37:..` (пакеты 3–4, **незапрошенные**, повторяются). Это **ARP-Spoofing / ARP-Cache-Poisoning** → **Man-in-the-Middle**: атакующий выдаёт себя за шлюз, трафик жертвы пойдёт через него. **Schutzziel:** Vertraulichkeit (+ Integrität, если меняет пакеты). **Мера:** статические ARP-записи, Dynamic ARP Inspection, TLS. Wireshark покажет «duplicate use of 192.168.0.1 detected».

### Log D [N] 🧮
```
1  10.1.1.2 → 10.1.1.9  TCP [SYN]       Seq=0
2  10.1.1.9 → 10.1.1.2  TCP [SYN, ACK]  Seq=0 Ack=1
3  10.1.1.2 → 10.1.1.9  TCP [ACK]       Seq=1 Ack=1
4  10.1.1.2 → 10.1.1.9  TCP [PSH, ACK]  Seq=1 Ack=1 Len=200
5  10.1.1.9 → 10.1.1.2  TCP [ACK]       Seq=1 Ack=?    Len=0
6  10.1.1.9 → 10.1.1.2  TCP [PSH, ACK]  Seq=1 Ack=201 Len=500
7  10.1.1.2 → 10.1.1.9  TCP [ACK]       Seq=? Ack=?    Len=0
```
Заполни ? в пакетах 5 и 7.
> **✅ Lösung:** Пакет 5: Ack = 1 + 200 = **201**. Пакет 7: Seq = **201** (клиент отправил 200 байт), Ack = 1 + 500 = **501**.

### Log E [N] ⭐
```
1  192.168.1.33 → 192.168.1.1  DNS  query 0x4a1f A www.sparkasse.de
2  192.168.1.1  → 192.168.1.33 DNS  response 0x4a1f A 45.11.99.2     (time 0.003)
3  192.168.1.1  → 192.168.1.33 DNS  response 0x4a1f A 193.17.42.10   (time 0.044)
```
Was ist hier faul?
> **✅ Lösung:** На **один** DNS-запрос (Transaction-ID 0x4a1f) приходят **два ответа с разными IP**. Подложный **45.11.99.2** приходит первым (3 мс) — клиент примет его и отбросит настоящий (44 мс). Это **DNS-Spoofing** (вероятно после ARP-Spoofing злоумышленник в локальной сети подделал ответ резолвера). Пользователь при заходе на банк попадёт на фальшивый сайт → **Phishing**. **Schutzziel:** Integrität/Authentizität. **Мера:** DNSSEC, DoH/DoT, HTTPS с проверкой сертификата (браузер предупредит — у атакующего нет валидного сертификата на sparkasse.de).

### Log F [N] [1,0] — netstat на веб-сервере
```
Proto Lokale Adresse     Remoteadresse         Status        PID
TCP   0.0.0.0:443        0.0.0.0:0            LISTENING     900
TCP   10.0.0.5:443       198.51.100.7:52001  ESTABLISHED   900
TCP   10.0.0.5:443       203.0.113.9:40210   SYN_RECEIVED  900
TCP   10.0.0.5:443       192.0.2.4:61002     SYN_RECEIVED  900
...   (≈3000 Zeilen SYN_RECEIVED)
TCP   10.0.0.5:49200     185.220.101.7:4444  ESTABLISHED   4071
```
Zwei Probleme — welche?
> **✅ Lösung:**
> 1. **Tausende `SYN_RECEIVED`** от разных IP → **SYN-Flooding (DoS)**: полуоткрытые соединения забивают буфер, легитимные клиенты не проходят → **Verfügbarkeit**. Мера: SYN-Cookies, rate-limiting, DDoS-защита.
> 2. **Исходящее** соединение сервера (эфемерный порт 49200) на внешний IP, порт **4444** (стандарт Metasploit) → подозрение на **Backdoor/компрометацию** (reverse shell). Веб-сервер не должен сам инициировать внешние соединения. Действия: найти процесс **PID 4071** (`tasklist`/`ps`), изолировать систему, сохранить вывод для форензики (`netstat -ano > beweis.txt`).

---
Следующие файлы: `10_Glossar_DE_RU.md` (алфавитный глоссарий под Zuordnung), `11_Uebungen.md` (Probeklausur).
