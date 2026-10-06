## Негізгі ой / Главная идея / Key idea

- **KK:** HSRP, VRRP және GLBP маршрутизатор істен шыққанда әдепкі шлюзді резервтеуге көмектеседі. LACP пен PAgP бірнеше физикалық портты бір логикалық EtherChannel арнасына біріктіреді.
- **RU:** HSRP, VRRP и GLBP обеспечивают резервирование шлюза при отказе маршрутизатора. LACP и PAgP объединяют несколько физических портов в один логический канал EtherChannel.
- **EN:** HSRP, VRRP, and GLBP provide gateway redundancy if a router fails. LACP and PAgP bundle multiple physical ports into one logical EtherChannel.

> **Есте сақта / Запомни / Remember:** FHRP-хаттамалары шлюз деңгейіндегі ақауды, ал EtherChannel физикалық арна ақауын өңдейді. Бұл — бірін-бірі толықтыратын, бірақ әртүрлі технологиялар. <citation src="1"></citation>

## 1. HSRP, VRRP және GLBP

| Хаттама / Протокол / Protocol | KK — Қазақша | RU — Русский | EN — English |
|---|---|---|---|
| **HSRP** | Бір маршрутизатор `Active`, екіншісі `Standby` болады. Белсенді маршрутизатор істен шықса, резервтік маршрутизатор оның орнын басады. | Один маршрутизатор становится `Active`, другой — `Standby`. При отказе активного его заменяет резервный. | One router is `Active`, while another is `Standby`. If the active router fails, the standby router takes over. |
| **VRRP** | Бір маршрутизатор `Master`, басқалары `Backup` болады. `Master` істен шықса, резервтік маршрутизаторлардың бірі оның міндетін қабылдайды. | Один маршрутизатор становится `Master`, остальные — `Backup`. Если `Master` отказывает, его роль принимает резервный маршрутизатор. | One router becomes `Master`; the others are `Backup`. If the Master fails, a backup router takes over. |
| **GLBP** | Резервтеумен бірге трафикті бірнеше маршрутизатор арасында бөле алады. | Помимо резервирования, может распределять трафик между несколькими маршрутизаторами. | In addition to redundancy, it can distribute traffic across multiple routers. |

HSRP, VRRP және GLBP пайдаланушыларға бір **виртуалды шлюз IP-мекенжайын** ұсынады. Оны компьютерлердің әдепкі шлюзі ретінде орнатуға болады. GLBP топтағы маршрутизаторлардың біреуі істен шықса да трафикті қайта бағыттай алады. <citation src="1"></citation>

**Трафикті бөлу туралы ескерту / Важно о распределении нагрузки / Note on load balancing:**

- **KK:** HSRP пен VRRP-дің бір тобында әдетте бір маршрутизатор белсенді болады. Бірнеше VLAN не топ құрып, оларды маршрутизаторлар арасында бөліп баптауға болады. GLBP бір топта трафикті бірнеше маршрутизаторға бөле алады.
- **RU:** В одной группе HSRP или VRRP обычно активен один маршрутизатор. Распределить работу между устройствами можно, например, настроив разные группы для разных VLAN. GLBP умеет распределять трафик между маршрутизаторами в одной группе.
- **EN:** Typically, one router is active in an HSRP or VRRP group. You can share the load across routers by using different groups on different VLANs. GLBP can distribute traffic across routers within one group.

## 2. LACP және PAgP

EtherChannel құрамындағы физикалық порттардың негізгі параметрлері үйлесімді болуы керек: мысалы, порт режимі мен VLAN баптаулары. Арнадағы бір кабель істен шықса, қалған желі байланысы жұмысын жалғастыра алады.

Параметры физических портов EtherChannel должны быть согласованы — например, режим работы и настройки VLAN. Если одна линия откажет, оставшиеся соединения могут продолжить работу.

Physical ports in an EtherChannel should have compatible settings, such as port mode and VLAN configuration. If one link fails, the remaining links can continue carrying traffic.

| Протокол / Protocol | Режимдер / Modes | Келіссөз нәтижесі / Результат согласования / Negotiation result |
|---|---|---|
| **LACP** | `active`, `passive` | `active` + `active` — арна құрылады / канал создается / channel forms. `active` + `passive` — арна құрылады / канал создается / channel forms. `passive` + `passive` — келіссөз басталмайды / согласование не начинается / no negotiation starts. |
| **PAgP** | `desirable`, `auto` | `desirable` + `desirable` — арна құрылады / канал создается / channel forms. `desirable` + `auto` — арна құрылады / канал создается / channel forms. `auto` + `auto` — келіссөз басталмайды / согласование не начинается / no negotiation starts. |

**Есте сақтайтын нәрсе / Что запомнить / Remember:**

- **KK:** LACP пен PAgP — EtherChannel келіссөзінің екі бөлек нұсқасы. Бір арнаның екі жағында бірдей протокол қолданыңыз.
- **RU:** LACP и PAgP — два разных протокола согласования EtherChannel. На обоих концах одного канала используйте один и тот же протокол.
- **EN:** LACP and PAgP are different EtherChannel negotiation protocols. Use the same protocol at both ends of a channel.

EtherChannel ішіндегі жүктеме физикалық желілерге ағындар бойынша бөлінуі мүмкін. Бірақ бұл бір ғана ағынның жылдамдығы бірнеше есе артады дегенді білдірмейді.

Трафик EtherChannel может распределяться по физическим линиям между потоками. Но это не означает, что скорость одного отдельного потока обязательно возрастет в несколько раз.

EtherChannel may distribute traffic across its physical links by flow. This does not necessarily multiply the speed of a single individual flow.

## 3. Негізгі терминдер / Основные термины / Key terms

| Термин / Term | Қазақша | Русский | English |
|---|---|---|---|
| **Virtual IP** | Пайдаланушылар қолданатын ортақ шлюз мекенжайы. | Общий адрес шлюза, используемый клиентами. | A shared gateway address used by clients. |
| **Priority** | Маршрутизатордың белсенді болу мүмкіндігіне әсер ететін басымдық. | Приоритет, влияющий на выбор активного маршрутизатора. | A value that influences which router becomes active. |
| **Preempt** | Басымдығы жоғары маршрутизатор қалпына келгенде белсенді рөлді қайта алуына мүмкіндік беретін баптау. | Настройка, позволяющая маршрутизатору с более высоким приоритетом вернуть активную роль после восстановления. | A setting that allows a higher-priority router to reclaim the active role after recovery. |
| **EtherChannel** | Бір логикалық арна ретінде жұмыс істейтін бірнеше физикалық порт тобы. | Группа физических портов, работающая как один логический канал. | A group of physical ports that operates as one logical channel. |

## 4. Командалар мысалы / Примеры команд / Command examples

Төмендегі үлгілер — Cisco IOS үшін. Құрылғы моделі мен Packet Tracer нұсқасына қарай кейбір командалар қолжетімсіз болуы мүмкін.

Примеры ниже предназначены для Cisco IOS. Доступность отдельных команд зависит от модели устройства и версии Packet Tracer.

The examples below are for Cisco IOS. Some commands may not be available in every device model or Packet Tracer version.

### HSRP мысалы / Пример HSRP / HSRP example

Берілгендер / Дано / Example values: R1 — `192.168.10.2`, R2 — `192.168.10.3`, virtual IP — `192.168.10.1`.

R1 маршрутизаторында / На маршрутизаторе R1 / On router R1:

```text
interface gigabitEthernet0/0
 ip address 192.168.10.2 255.255.255.0
 standby 1 ip 192.168.10.1
 standby 1 priority 110
 standby 1 preempt
 no shutdown
```

R2-де де сол HSRP тобы мен виртуалды IP мекенжайын орнатыңыз, бірақ физикалық IP мекенжайы `192.168.10.3` болсын. `priority 110` R1-ге жоғары басымдық береді; `preempt` қалпына келген R1-ге белсенді рөлді қайта алуға мүмкіндік береді.

На R2 задайте ту же группу HSRP и виртуальный IP, но физический адрес интерфейса укажите `192.168.10.3`. `priority 110` дает R1 более высокий приоритет; `preempt` позволяет ему вернуть активную роль после восстановления.

On R2, configure the same HSRP group and virtual IP, but use `192.168.10.3` as its physical interface address. `priority 110` gives R1 a higher priority; `preempt` allows it to reclaim the active role after recovery.

### VRRP немесе GLBP баламалары / Варианты VRRP и GLBP / VRRP or GLBP alternatives

Бұларды HSRP орнына бөлек зерттеу үшін қолданыңыз; бәрін бір интерфейсте қатар баптамаңыз.

Используйте эти варианты отдельно вместо HSRP; не настраивайте все протоколы одновременно на одном интерфейсе.

Use these as separate alternatives to HSRP; do not configure all of them on the same interface at once.

VRRP:

```text
interface gigabitEthernet0/0
 vrrp 1 ip 192.168.10.1
 vrrp 1 priority 110
 vrrp 1 preempt
```

GLBP:

```text
interface gigabitEthernet0/0
 glbp 1 ip 192.168.10.1
 glbp 1 load-balancing round-robin
```

### LACP үлгісі / Пример LACP / LACP example

Екі коммутаторда да бірдей физикалық порттарды көрсетіңіз:

На обоих коммутаторах укажите одинаковые физические порты:

On both switches, select the corresponding physical ports:

```text
interface range fastEthernet0/23 - 24
 channel-group 1 mode active
```

LACP үшін бір жақта `active`, екінші жақта `active` немесе `passive` қолдануға болады. Екі жақ та `passive` болса, арна келіссөзі басталмайды.

Для LACP на одной стороне задайте `active`, на другой — `active` или `passive`. Если с обеих сторон стоит `passive`, согласование не начнется.

For LACP, use `active` on one side and `active` or `passive` on the other. If both sides are `passive`, negotiation will not start.

### PAgP үлгісі / Пример PAgP / PAgP example

```text
interface range fastEthernet0/23 - 24
 channel-group 1 mode desirable
```

**KK:** PAgP арнасы үшін қарсы жақта `desirable` немесе `auto` қолданыңыз.  
**RU:** На другом конце для PAgP задайте `desirable` или `auto`.  
**EN:** Configure `desirable` or `auto` at the other end for PAgP.

## 5. Тексеру командалары / Команды проверки / Verification commands

| Не тексеріледі / Проверка / Check | Команда / Command |
|---|---|
| HSRP күйі / Состояние HSRP / HSRP status | `show standby brief` |
| VRRP күйі / Состояние VRRP / VRRP status | `show vrrp brief` |
| GLBP күйі / Состояние GLBP / GLBP status | `show glbp brief` |
| EtherChannel күйі / Состояние EtherChannel / EtherChannel status | `show etherchannel summary` |
| LACP көршісі / Сосед LACP / LACP neighbor | `show lacp neighbor` |
| PAgP көршісі / Сосед PAgP / PAgP neighbor | `show pagp neighbor` |

HSRP тексергенде бір маршрутизатордан `Active`, екіншісінен `Standby` күйін іздеңіз. EtherChannel тексергенде порт-арнаның құрылғанын және күтілген порттардың оған қосылғанын қараңыз.

При проверке HSRP найдите состояние `Active` на одном маршрутизаторе и `Standby` на другом. В выводе EtherChannel проверьте, что порт-канал сформирован и нужные интерфейсы входят в его состав.

For HSRP, look for `Active` on one router and `Standby` on the other. In the EtherChannel output, check that the port-channel formed and includes the expected interfaces.

## 6. Жиі кездесетін қателер / Частые ошибки / Common mistakes

- **KK:** Компьютерде шлюз ретінде виртуалды IP емес, физикалық маршрутизатор мекенжайы көрсетілген.  
  **RU:** На компьютере указан физический адрес маршрутизатора вместо виртуального шлюза.  
  **EN:** A PC is configured with a router’s physical address instead of the virtual gateway.

- **KK:** LACP және PAgP режимдері не топ нөмірлері екі жақта сәйкес емес.  
  **RU:** Режимы LACP/PAgP или номера групп на двух концах не совпадают.  
  **EN:** The LACP/PAgP modes or group numbers do not match at both ends.

- **KK:** Екі жақта да LACP `passive` немесе PAgP `auto` орнатылған — ешбір құрылғы келіссөзді бастамайды.  
  **RU:** С обеих сторон настроен LACP `passive` или PAgP `auto` — ни одно устройство не инициирует согласование.  
  **EN:** Both sides are set to LACP `passive` or PAgP `auto`, so neither side initiates negotiation.

- **KK:** Бір арнаның екі шетінде LACP және PAgP араластырылған.  
  **RU:** На разных концах одного канала настроены LACP и PAgP.  
  **EN:** LACP is configured at one end of a channel and PAgP at the other.

- **KK:** EtherChannel порттарының VLAN, trunk/access режимі немесе басқа негізгі параметрлері үйлеспейді.  
  **RU:** Не совпадают настройки VLAN, режим trunk/access или другие основные параметры портов EtherChannel.  
  **EN:** VLAN settings, trunk/access mode, or other key EtherChannel port settings are inconsistent.

- **KK:** Бір LAN ішіндегі екі компьютер арасындағы ping HSRP-дің дұрыс жұмысын міндетті түрде дәлелдемейді. Виртуалды шлюзге ping жасап, негізгі маршрутизаторды өшіріп тексеріңіз.  
  **RU:** Ping между двумя компьютерами в одной LAN не обязательно подтверждает работу HSRP. Проверьте виртуальный шлюз и отключите основной маршрутизатор.  
  **EN:** A ping between two PCs on the same LAN does not necessarily test HSRP. Ping the virtual gateway and simulate a failure of the active router.

## 7. Қысқаша қорытынды / Краткий вывод / Quick recap

- **KK:** HSRP/VRRP — шлюз резерві; GLBP — шлюз резерві және жүктемені бөлу; LACP/PAgP — порттарды EtherChannel-ға біріктіру.
- **RU:** HSRP/VRRP — резервирование шлюза; GLBP — резервирование и распределение нагрузки; LACP/PAgP — объединение портов в EtherChannel.
- **EN:** HSRP/VRRP provide gateway redundancy; GLBP provides gateway redundancy and load sharing; LACP/PAgP bundle ports into an EtherChannel.
