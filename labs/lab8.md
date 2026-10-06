# Packet Tracer Lab: VLANs, STP, EtherChannel, and Routing Protocols (OSPF, EIGRP, BGP)

# Packet Tracer зертханалық жұмысы: VLAN, STP, EtherChannel және маршруттау протоколдары (OSPF, EIGRP, BGP)

# Лабораторная работа Packet Tracer: VLAN, STP, EtherChannel и протоколы маршрутизации (OSPF, EIGRP, BGP)

> **Note / Ескерту / Примечание:**  
> This lab is trilingual. Cisco IOS commands remain in English. Explanations are provided in English, Kazakh, and Russian.  
> Бұл зертханалық жұмыс үш тілде. Cisco IOS командалары ағылшын тілінде қалады. Түсіндірмелер ағылшын, қазақ және орыс тілдерінде берілген.  
> Эта лабораторная работа трехъязычная. Команды Cisco IOS остаются на английском. Пояснения даны на английском, казахском и русском языках.

---

## 1. Learning Objectives / Оқу мақсаттары / Цели обучения

### English
After completing this lab, students will be able to:
- Configure VLANs, trunks, Rapid-PVST, and EtherChannel on Cisco 2960 switches.
- Implement router-on-a-stick for inter-VLAN routing.
- Explain and distinguish **interior routing protocols** (IGP) and **exterior routing protocols** (EGP).
- **Understand interior and exterior routing protocols and configure OSPF, BGP, and EIGRP protocols.**
- Configure **OSPF** as an internal link-state IGP.
- Configure **EIGRP** for a branch network.
- Configure **eBGP** between an enterprise edge router and an ISP router.
- Verify routing protocol neighbors, routes, and end-to-end connectivity.

### Қазақша
Осы зертханалық жұмысты орындағаннан кейін студенттер:
- Cisco 2960 коммутаторларында VLAN, транк, Rapid-PVST және EtherChannel конфигурациялай алады.
- VLAN аралық маршруттау үшін router-on-a-stick іске асыра алады.
- **Ішкі маршруттау протоколдарын** (IGP) және **сыртқы маршруттау протоколдарын** (EGP) түсіндіріп, ажырата алады.
- **Ішкі және сыртқы маршруттау протоколдарын түсінеді және OSPF, BGP, EIGRP протоколдарын конфигурациялайды.**
- **OSPF**-ті ішкі link-state IGP ретінде конфигурациялайды.
- **EIGRP**-ті филиал желісі үшін конфигурациялайды.
- Кәсіпорынның шекаралық маршрутизаторы мен ISP маршрутизаторы арасында **eBGP** конфигурациялайды.
- Маршруттау протоколдарының көршілерін, маршруттарын және шеттен шетке байланысын тексереді.

### Русский
После выполнения этой лабораторной работы студенты смогут:
- Настраивать VLAN, транки, Rapid-PVST и EtherChannel на коммутаторах Cisco 2960.
- Реализовывать router-on-a-stick для маршрутизации между VLAN.
- Объяснять и различать **протоколы внутренней маршрутизации** (IGP) и **протоколы внешней маршрутизации** (EGP).
- **Понимает внутренние и внешние протоколы маршрутизации и настраивает протоколы OSPF, BGP, EIGRP.**
- Настраивать **OSPF** как внутренний link-state IGP.
- Настраивать **EIGRP** для филиальной сети.
- Настраивать **eBGP** между пограничным маршрутизатором предприятия и маршрутизатором ISP.
- Проверять соседей протоколов маршрутизации, маршруты и сквозную связность.

---

## 2. Topology Overview / Топологияға шолу / Обзор топологии

### English
- 4 × Cisco 2960 switches: SW1–SW4
- 1 × Router R1 (2911 or 4331) for router-on-a-stick
- 4 × PCs: 2 per VLAN
- Additional routers for routing protocols: R2 (core/edge), R3 (ISP), R4 (branch)

### Қазақша
- 4 × Cisco 2960 коммутаторы: SW1–SW4
- 1 × R1 маршрутизаторы (2911 немесе 4331) router-on-a-stick үшін
- 4 × ДК: әр VLAN-да 2-ден
- Маршруттау протоколдары үшін қосымша маршрутизаторлар: R2 (ядро/шекара), R3 (ISP), R4 (филиал)

### Русский
- 4 × коммутатора Cisco 2960: SW1–SW4
- 1 × маршрутизатор R1 (2911 или 4331) для router-on-a-stick
- 4 × ПК: по 2 на VLAN
- Дополнительные маршрутизаторы для протоколов маршрутизации: R2 (ядро/граница), R3 (ISP), R4 (филиал)

```text
                         [R3 ISP]
                        203.0.113.2/30
                         Lo0: 8.8.8.8/32
                             |
                         eBGP AS 65002
                             |
[PC1]--SW3--\                |
[PC2]--SW3---SW1=====SW2---SW4--[PC3]
               |       |     |
               |       |     +--[PC4]
               |       |
              R1------R2------R4--[Branch LAN 192.168.30.0/24]
           router-on-  |      10.0.1.0/30
           -a-stick    |
                    OSPF area 0
                   10.0.0.0/30
```

- SW1–SW2: EtherChannel, 2 links, trunk
- SW3–SW4: redundant trunk, blocked by STP
- R1–R2: OSPF area 0
- R2–R4: EIGRP AS 100
- R2–R3: eBGP between AS 65001 and AS 65002

---

## 3. VLAN and IP Addressing Plan / VLAN және IP мекенжайлау жоспары / План VLAN и IP-адресации

| VLAN | Name / Атауы / Имя | Subnet / Субнет / Подсеть | Gateway / Шлюз / Шлюз |
|------|-------------------|---------------------------|----------------------|
| 10   | TEACHERS          | 192.168.10.0/24           | 192.168.10.1         |
| 20   | STUDENTS          | 192.168.20.0/24           | 192.168.20.1         |

### PC IP Configuration / ДК IP конфигурациясы / IP-конфигурация ПК

| PC  | VLAN | IP Address / IP мекенжайы / IP-адрес | Mask / Маска / Маска | Gateway / Шлюз / Шлюз |
|-----|------|--------------------------------------|----------------------|----------------------|
| PC1 | 10   | 192.168.10.10                        | 255.255.255.0        | 192.168.10.1         |
| PC2 | 20   | 192.168.20.10                        | 255.255.255.0        | 192.168.20.1         |
| PC3 | 10   | 192.168.10.11                        | 255.255.255.0        | 192.168.10.1         |
| PC4 | 20   | 192.168.20.11                        | 255.255.255.0        | 192.168.20.1         |

### WAN / Routed Links / WAN / Маршрутталатын байланыстар / WAN / Маршрутизируемые каналы

| Link / Байланыс / Канал | Network / Желі / Сеть | Router A / A маршрутизаторы / Маршрутизатор A | Router B / B маршрутизаторы / Маршрутизатор B |
|--------------------------|----------------------|-----------------------------------------------|-----------------------------------------------|
| R1–R2                    | 10.0.0.0/30          | R1 .1                                         | R2 .2                                         |
| R2–R3                    | 203.0.113.0/30       | R2 .1                                         | R3 .2                                         |
| R2–R4                    | 10.0.1.0/30          | R2 .1                                         | R4 .2                                         |
| R4 LAN                   | 192.168.30.0/24      | R4 .1                                         | —                                             |
| R3 Loopback              | 8.8.8.8/32           | R3                                            | —                                             |

---

## 4. Part A: VLANs, STP, and EtherChannel / A бөлімі: VLAN, STP және EtherChannel / Часть A: VLAN, STP и EtherChannel

### SW1 Configuration / SW1 конфигурациясы / Конфигурация SW1

```text
enable
configure terminal
hostname SW1

! Create VLANs
vlan 10
 name TEACHERS
vlan 20
 name STUDENTS
exit

! VTP transparent
vtp mode transparent

! Trunk to Router R1
interface fa0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

! Trunk to SW3
interface fa0/3
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

! EtherChannel to SW2
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 channel-group 1 mode active
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
exit

! STP: Rapid-PVST, root for VLAN 10, secondary for VLAN 20
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary

end
write memory
```

### SW2 Configuration / SW2 конфигурациясы / Конфигурация SW2

```text
enable
configure terminal
hostname SW2

vlan 10
 name TEACHERS
vlan 20
 name STUDENTS
exit

vtp mode transparent

! Trunk to SW4
interface fa0/3
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

! EtherChannel to SW1
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 channel-group 1 mode active
 no shutdown
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
exit

! STP: Rapid-PVST, root for VLAN 20, secondary for VLAN 10
spanning-tree mode rapid-pvst
spanning-tree vlan 20 root primary
spanning-tree vlan 10 root secondary

end
write memory
```

### SW3 Configuration / SW3 конфигурациясы / Конфигурация SW3

```text
enable
configure terminal
hostname SW3

vlan 10
 name TEACHERS
vlan 20
 name STUDENTS
exit

vtp mode transparent

! PC1 - Teacher
interface fa0/1
 switchport mode access
 switchport access vlan 10
 no shutdown
exit

! PC2 - Student
interface fa0/2
 switchport mode access
 switchport access vlan 20
 no shutdown
exit

! Trunk to SW1
interface fa0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

! Redundant trunk to SW4
interface fa0/23
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

spanning-tree mode rapid-pvst

end
write memory
```

### SW4 Configuration / SW4 конфигурациясы / Конфигурация SW4

```text
enable
configure terminal
hostname SW4

vlan 10
 name TEACHERS
vlan 20
 name STUDENTS
exit

vtp mode transparent

! PC3 - Teacher
interface fa0/1
 switchport mode access
 switchport access vlan 10
 no shutdown
exit

! PC4 - Student
interface fa0/2
 switchport mode access
 switchport access vlan 20
 no shutdown
exit

! Trunk to SW2
interface fa0/3
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

! Redundant trunk to SW3
interface fa0/23
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
exit

spanning-tree mode rapid-pvst

end
write memory
```

---

## 5. Part B: Routing Protocols – OSPF, EIGRP, BGP / B бөлімі: Маршруттау протоколдары – OSPF, EIGRP, BGP / Часть B: Протоколы маршрутизации – OSPF, EIGRP, BGP

### R1 – Router-on-a-Stick + OSPF / R1 – Router-on-a-Stick + OSPF / R1 – Router-on-a-Stick + OSPF

```text
enable
configure terminal
hostname R1

! Trunk to SW1
interface g0/0
 no shutdown
exit

! VLAN 10 gateway
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

! VLAN 20 gateway
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit

! Link to R2
interface g0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown
exit

! OSPF – internal routing protocol
router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
exit

end
write memory
```

### R2 – OSPF + EIGRP + BGP / R2 – OSPF + EIGRP + BGP / R2 – OSPF + EIGRP + BGP

```text
enable
configure terminal
hostname R2

! Link to R1
interface g0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit

! Link to ISP R3
interface g0/1
 ip address 203.0.113.1 255.255.255.252
 no shutdown
exit

! Link to Branch R4
interface g0/2
 ip address 10.0.1.1 255.255.255.252
 no shutdown
exit

! OSPF – internal IGP toward R1
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
 redistribute eigrp 100 subnets
 default-information originate
exit

! EIGRP – internal IGP toward branch R4
router eigrp 100
 network 10.0.1.0 0.0.0.3
 network 192.168.30.0 0.0.0.255
 redistribute ospf 1 metric 10000 100 255 1 1500
 no auto-summary
exit

! BGP – external routing protocol toward ISP
router bgp 65001
 neighbor 203.0.113.2 remote-as 65002
 redistribute ospf 1
 redistribute eigrp 100
exit

end
write memory
```

### R3 – ISP Router with eBGP / R3 – ISP маршрутизаторы eBGP-мен / R3 – ISP-маршрутизатор с eBGP

```text
enable
configure terminal
hostname R3

! Link to R2
interface g0/0
 ip address 203.0.113.2 255.255.255.252
 no shutdown
exit

! ISP loopback representing Internet
interface loopback0
 ip address 8.8.8.8 255.255.255.255
exit

! eBGP to enterprise AS 65001
router bgp 65002
 neighbor 203.0.113.1 remote-as 65001
 neighbor 203.0.113.1 default-originate
 network 8.8.8.8 mask 255.255.255.255
exit

end
write memory
```

### R4 – Branch Router with EIGRP / R4 – Филиал маршрутизаторы EIGRP-мен / R4 – Филиальный маршрутизатор с EIGRP

```text
enable
configure terminal
hostname R4

! Link to R2
interface g0/0
 ip address 10.0.1.2 255.255.255.252
 no shutdown
exit

! Branch LAN
interface g0/1
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit

! EIGRP – internal IGP
router eigrp 100
 network 10.0.1.0 0.0.0.3
 network 192.168.30.0 0.0.0.255
 no auto-summary
exit

end
write memory
```

---

## 6. Verification Commands / Тексеру командалары / Команды проверки

### VLAN / Trunk / EtherChannel

```text
show vlan brief
show interfaces trunk
show etherchannel summary
! Expected: Po1(SU) with Fa0/1(P) and Fa0/2(P)
```

### STP Load Balancing / STP жүктемесін теңестіру / Балансировка нагрузки STP

```text
show spanning-tree vlan 10
! SW1 should show "This bridge is the root"

show spanning-tree vlan 20
! SW2 should show "This bridge is the root"

show spanning-tree
! On SW3/SW4, one uplink should be Blocking/Alternate for each VLAN
```

### OSPF

```text
show ip ospf neighbor
show ip route ospf
show ip protocols
! R1 and R2 should form FULL adjacency in area 0
```

### EIGRP

```text
show ip eigrp neighbors
show ip route eigrp
! R2 and R4 should be EIGRP neighbors in AS 100
```

### BGP

```text
show ip bgp summary
show ip bgp
show ip route bgp
! R2 and R3 should establish eBGP session between AS 65001 and AS 65002
```

### End-to-End Connectivity / Шеттен шетке байланыс / Сквозная связность

From PC1 / PC1-ден / С PC1:

```text
ping 192.168.10.11   ! PC3 – same VLAN, different switch
ping 192.168.20.10   ! PC2 – different VLAN, via router-on-a-stick
ping 192.168.30.1    ! Branch LAN via EIGRP
ping 8.8.8.8         ! ISP loopback via BGP default route
traceroute 8.8.8.8
```

### Fault Tolerance Tests / Ақауға төзімділік тесттері / Тесты отказоустойчивости

- Unplug one EtherChannel member: traffic continues.
- Unplug SW1–SW3 trunk: STP converges through SW4.
- Shut down R2–R4 link: EIGRP neighbor drops; branch connectivity is lost until restored.
- Shut down R2–R3 link: BGP session drops; ISP reachability is lost.

- Бір EtherChannel мүшесін ажыратыңыз: трафик жалғасады.
- SW1–SW3 транкін ажыратыңыз: STP SW4 арқылы қайта жинақталады.
- R2–R4 байланысын өшіріңіз: EIGRP көршісі жоғалады; филиал байланысы қалпына келгенше жоғалады.
- R2–R3 байланысын өшіріңіз: BGP сессиясы жоғалады; ISP қолжетімділігі жоғалады.

- Отключите одного члена EtherChannel: трафик продолжается.
- Отключите транк SW1–SW3: STP перестроится через SW4.
- Отключите канал R2–R4: сосед EIGRP пропадет; связь с филиалом потеряется до восстановления.
- Отключите канал R2–R3: сессия BGP пропадет; доступность ISP потеряется.

---

## 7. What Students Should Observe / Студенттер не байқауы керек / Что должны наблюдать студенты

### English
1. **EtherChannel** bundles two physical links into one logical link. `show etherchannel summary` shows `SU` and `P`.
2. **Rapid-PVST** blocks the SW3–SW4 redundant link. One end is in `Alt/Blocking` state.
3. **Per-VLAN load balancing** works because SW1 is root for VLAN 10 and SW2 is root for VLAN 20.
4. **Router-on-a-stick** enables inter-VLAN routing without an L3 switch.
5. **OSPF** is an interior link-state protocol used between R1 and R2.
6. **EIGRP** is an interior advanced distance-vector protocol used between R2 and R4.
7. **BGP** is an exterior path-vector protocol used between the enterprise AS 65001 and ISP AS 65002.
8. **Fault tolerance** is provided by EtherChannel, STP redundancy, and routing protocol reconvergence.

### Қазақша
1. **EtherChannel** екі физикалық байланысты бір логикалық байланысқа біріктіреді. `show etherchannel summary` `SU` және `P` көрсетеді.
2. **Rapid-PVST** SW3–SW4 резервтік байланысын бұғаттайды. Бір ұшы `Alt/Blocking` күйінде болады.
3. **Әр VLAN үшін жүктемені теңестіру** жұмыс істейді, өйткені SW1 VLAN 10 үшін root, ал SW2 VLAN 20 үшін root.
4. **Router-on-a-stick** L3 коммутаторсыз VLAN аралық маршруттауды қамтамасыз етеді.
5. **OSPF** — R1 мен R2 арасында қолданылатын ішкі link-state протоколы.
6. **EIGRP** — R2 мен R4 арасында қолданылатын ішкі кеңейтілген distance-vector протоколы.
7. **BGP** — кәсіпорын AS 65001 мен ISP AS 65002 арасында қолданылатын сыртқы path-vector протоколы.
8. **Ақауға төзімділік** EtherChannel, STP резервтеу және маршруттау протоколдарының қайта жинақталуы арқылы қамтамасыз етіледі.

### Русский
1. **EtherChannel** объединяет два физических канала в один логический. `show etherchannel summary` показывает `SU` и `P`.
2. **Rapid-PVST** блокирует резервный канал SW3–SW4. Один конец находится в состоянии `Alt/Blocking`.
3. **Балансировка нагрузки по VLAN** работает, так как SW1 — корень для VLAN 10, а SW2 — корень для VLAN 20.
4. **Router-on-a-stick** обеспечивает маршрутизацию между VLAN без L3-коммутатора.
5. **OSPF** — внутренний link-state протокол, используемый между R1 и R2.
6. **EIGRP** — внутренний advanced distance-vector протокол, используемый между R2 и R4.
7. **BGP** — внешний path-vector протокол, используемый между предприятием AS 65001 и ISP AS 65002.
8. **Отказоустойчивость** обеспечивается EtherChannel, резервированием STP и перестроением протоколов маршрутизации.

---

## 8. Common Student Pitfalls / Жиі кездесетін қателер / Типичные ошибки студентов

### English
- Forgetting `switchport trunk allowed vlan 10,20` → VLANs pruned.
- Mismatched EtherChannel modes: use `active/active` (LACP) or `desirable/desirable` (PAgP). Avoid `on/on` in Packet Tracer.
- Forgetting `no shutdown` on port-channel members.
- Wrong STP priority if using `root primary/secondary` — verify with `show spanning-tree`.
- Native VLAN mismatch → CDP warnings. Keep native VLAN 1 on both ends.
- Forgetting `no auto-summary` under EIGRP.
- Missing `default-information originate` under OSPF on R2 → R1 will not learn the default route.
- Forgetting `default-originate` on R3 BGP neighbor → R2 will not receive a default route toward the ISP.
- Redistribution without metrics under EIGRP → routes may not be installed. Use `redistribute ospf 1 metric 10000 100 255 1 1500`.

### Қазақша
- `switchport trunk allowed vlan 10,20` ұмыту → VLAN-дар кесіледі.
- EtherChannel режимдерінің сәйкессіздігі: `active/active` (LACP) немесе `desirable/desirable` (PAgP) қолданыңыз. Packet Tracer-де `on/on`-нан аулақ болыңыз.
- Port-channel мүшелерінде `no shutdown` ұмыту.
- `root primary/secondary` қолданғанда STP басымдылығы дұрыс емес — `show spanning-tree` арқылы тексеріңіз.
- Native VLAN сәйкессіздігі → CDP ескертулері. Екі жақта да native VLAN 1 ұстаңыз.
- EIGRP астында `no auto-summary` ұмыту.
- R2-де OSPF астында `default-information originate` жоқ → R1 әдепкі маршрутты үйренбейді.
- R3 BGP көршісінде `default-originate` ұмыту → R2 ISP жаққа әдепкі маршрутты алмайды.
- EIGRP астында метрикасыз redistribution → маршруттар орнатылмауы мүмкін. `redistribute ospf 1 metric 10000 100 255 1 1500` қолданыңыз.

### Русский
- Забыли `switchport trunk allowed vlan 10,20` → VLAN обрезаются.
- Несовпадение режимов EtherChannel: используйте `active/active` (LACP) или `desirable/desirable` (PAgP). Избегайте `on/on` в Packet Tracer.
- Забыли `no shutdown` на членах port-channel.
- Неверный приоритет STP при использовании `root primary/secondary` — проверьте через `show spanning-tree`.
- Несовпадение native VLAN → предупреждения CDP. Оставляйте native VLAN 1 с обеих сторон.
- Забыли `no auto-summary` под EIGRP.
- Отсутствует `default-information originate` под OSPF на R2 → R1 не узнает маршрут по умолчанию.
- Забыли `default-originate` на BGP-соседе R3 → R2 не получит маршрут по умолчанию в сторону ISP.
- Redistribution без метрик под EIGRP → маршруты могут не установиться. Используйте `redistribute ospf 1 metric 10000 100 255 1 1500`.

---

## 9. Key Terms / Негізгі терминдер / Ключевые термины

| English | Қазақша | Русский | Protocol Type |
|--------|---------|---------|---------------|
| Interior Gateway Protocol (IGP) | Ішкі шлюздік протокол | Внутренний протокол маршрутизации | OSPF, EIGRP |
| Exterior Gateway Protocol (EGP) | Сыртқы шлюздік протокол | Внешний протокол маршрутизации | BGP |
| OSPF | OSPF | OSPF | Link-state IGP |
| EIGRP | EIGRP | EIGRP | Advanced distance-vector IGP |
| BGP | BGP | BGP | Path-vector EGP |
| Autonomous System | Автономды жүйе | Автономная система | AS 65001, AS 65002 |

---

**End of Lab / Зертханалық жұмыстың соңы / Конец лабораторной работы**
