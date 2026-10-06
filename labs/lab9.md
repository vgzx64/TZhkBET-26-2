# Лабораторная работа / Зертханалық жұмыс / Laboratory

**Тақырыбы / Тақырыбы / Topic:** Шлюздің резервтелуі және жүктемені бөлу. HSRP, VRRP, GLBP, LACP және PAgP  
**Ұзақтығы / Ұзақтығы / Duration:** 45–60 минут  
**Бағдарлама / Бағдарлама / Software:** Cisco Packet Tracer  
**Деңгейі / Деңгейі / Level:** Бастапқы / Бастапқы / Beginner

> Негізгі жұмыста HSRP және LACP баптаймыз. PAgP — LACP-ке балама, оны бөлек тексереміз; VRRP пен GLBP үшін соңында қосымша тапсырма берілген. Packet Tracer-дегі құрылғы моделі мен IOS нұсқасына қарай кейбір командаларға қолдау болмауы мүмкін. Екі физикалық қосылымды бір логикалық EtherChannel ретінде қолдану — зертхананың негізгі бөлігі. <citation src="3"></citation>

## 1. Мақсаты / Мақсаты / Objective

- **KK:** HSRP арқылы резервтік виртуалды шлюз құрып, LACP көмегімен екі коммутатор арасында EtherChannel баптау.
- **RU:** Создать резервный виртуальный шлюз с помощью HSRP и настроить EtherChannel между двумя коммутаторами с помощью LACP.
- **EN:** Create a redundant virtual gateway with HSRP and configure an EtherChannel between two switches using LACP.

## 2. Құрылғылар / Құрылғылар / Devices

Packet Tracer-ге мына құрылғыларды орналастырыңыз:

| Саны / Количество / Qty | Құрылғы / Устройство / Device |
|---:|---|
| 2 | Router 1941 немесе 2911 |
| 2 | Switch 2960 |
| 2 | PC |

Егер интерфейс атаулары өзгеше болса, мысалы, `G0/0` орнына `G0/0/0` болса, командаларда өз құрылғыңыздағы интерфейс атауын қолданыңыз.

## 3. Топология және жалғау / Топология и подключение / Topology and cabling

Жалғаңыз:

- **PC1 FastEthernet0** ↔ **SW1 Fa0/2**
- **R1 G0/0** ↔ **SW1 Fa0/1**
- **SW1 Fa0/23** ↔ **SW2 Fa0/23**
- **SW1 Fa0/24** ↔ **SW2 Fa0/24**
- **R2 G0/0** ↔ **SW2 Fa0/1**
- **PC2 FastEthernet0** ↔ **SW2 Fa0/2**

SW1 мен SW2 арасындағы екі кабель кейін бір EtherChannel тобына біріктіріледі.

```text
PC1 — SW1 ——— SW2 — PC2
        |  \   /  |
        |   \ /   |
        R1         R2
```

## 4. IP мекенжайлар жоспары / План IP-адресов / IP addressing plan

| Құрылғы / Устройство / Device | IP мекенжайы / IP-адрес / IP address | Маска / Маска / Mask | Әдепкі шлюз / Шлюз / Default gateway |
|---|---|---|---|
| R1 G0/0 | 192.168.10.2 | 255.255.255.0 | — |
| R2 G0/0 | 192.168.10.3 | 255.255.255.0 | — |
| HSRP виртуалды IP / виртуальный IP / virtual IP | 192.168.10.1 | 255.255.255.0 | — |
| PC1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1 |

## 5. Жұмыс барысы / Порядок работы / Procedure

### 1-қадам. Компьютерлерге IP беру / Шаг 1. Назначить IP-адреса ПК / Step 1. Assign IP addresses to PCs

**KK:** Әр компьютерді ашыңыз: **Desktop → IP Configuration**. Кестедегі IP, маска және әдепкі шлюзді енгізіңіз.

**RU:** Откройте каждый ПК: **Desktop → IP Configuration**. Введите IP-адрес, маску и шлюз из таблицы.

**EN:** Open each PC: **Desktop → IP Configuration**. Enter its IP address, subnet mask, and default gateway from the table.

### 2-қадам. R1 маршрутизаторын баптау / Шаг 2. Настроить маршрутизатор R1 / Step 2. Configure router R1

R1 құрылғысында **CLI** ашып, енгізіңіз:

```text
enable
configure terminal
interface gigabitEthernet0/0
ip address 192.168.10.2 255.255.255.0
standby 1 ip 192.168.10.1
standby 1 priority 110
standby 1 preempt
no shutdown
end
```

**Түсіндірме / Пояснение / Explanation:**

- **KK:** `standby 1 ip` — HSRP тобының виртуалды шлюзін орнатады. `priority 110` R1-ге жоғары басымдық береді. `preempt` R1 қалпына келгенде негізгі рөлді қайта алуына мүмкіндік береді.
- **RU:** `standby 1 ip` задает виртуальный шлюз HSRP-группы. `priority 110` назначает R1 более высокий приоритет. `preempt` позволяет R1 вернуть активную роль после восстановления.
- **EN:** `standby 1 ip` sets the HSRP group’s virtual gateway. `priority 110` gives R1 a higher priority. `preempt` allows R1 to resume the active role after recovery.

### 3-қадам. R2 маршрутизаторын баптау / Шаг 3. Настроить маршрутизатор R2 / Step 3. Configure router R2

R2 құрылғысында **CLI** ашып, енгізіңіз:

```text
enable
configure terminal
interface gigabitEthernet0/0
ip address 192.168.10.3 255.255.255.0
standby 1 ip 192.168.10.1
no shutdown
end
```

**Түсіндірме / Пояснение / Explanation:**

- **KK:** R2-де де сол HSRP тобы мен виртуалды IP қолданылады. Басымдық көрсетілмегендіктен, R1-де бапталған `110` мәнінен төмен болады. Сондықтан әдетте R1 белсенді, R2 резервтік маршрутизатор болады.
- **RU:** На R2 используются та же HSRP-группа и виртуальный IP. Поскольку приоритет не задан, он ниже установленного на R1 значения `110`. Обычно R1 становится активным, а R2 — резервным маршрутизатором.
- **EN:** R2 uses the same HSRP group and virtual IP. Since no priority is set, its priority is lower than R1’s `110`. R1 should normally be active and R2 standby.

### 4-қадам. SW1-де LACP EtherChannel баптау / Шаг 4. Настроить LACP EtherChannel на SW1 / Step 4. Configure LACP EtherChannel on SW1

SW1 коммутаторының **CLI** терезесінде:

```text
enable
configure terminal
interface range fastEthernet0/23 - 24
switchport mode access
channel-group 1 mode active
no shutdown
end
```

### 5-қадам. SW2-де LACP EtherChannel баптау / Шаг 5. Настроить LACP EtherChannel на SW2 / Step 5. Configure LACP EtherChannel on SW2

SW2 коммутаторының **CLI** терезесінде де дәл сондай командаларды енгізіңіз:

```text
enable
configure terminal
interface range fastEthernet0/23 - 24
switchport mode access
channel-group 1 mode active
no shutdown
end
```

**Түсіндірме / Пояснение / Explanation:**

- **KK:** `channel-group 1 mode active` LACP келіссөзін бастайды және порттарды 1-нөмірлі EtherChannel тобына қосады. Екі коммутаторда да порт нөмірлері мен негізгі параметрлер сәйкес болуы керек.
- **RU:** `channel-group 1 mode active` запускает согласование LACP и добавляет порты в EtherChannel-группу 1. Номера портов и основные параметры на обоих коммутаторах должны совпадать.
- **EN:** `channel-group 1 mode active` starts LACP negotiation and adds the ports to EtherChannel group 1. Port numbers and key settings should match on both switches.

### 6-қадам. Конфигурацияны тексеру / Шаг 6. Проверить конфигурацию / Step 6. Verify the configuration

R1 маршрутизаторында:

```text
show standby brief
```

SW1 және SW2 коммутаторларында:

```text
show etherchannel summary
```

**Не іздеу керек? / Что проверить? / What to look for?**

- **KK:** R1 HSRP-да `Active`, ал R2 `Standby` болуы тиіс. EtherChannel тексерісінде Port-channel көрініп, арнаға екі физикалық порт қосылғанын қараңыз.
- **RU:** В HSRP R1 должен иметь состояние `Active`, а R2 — `Standby`. В выводе EtherChannel проверьте наличие Port-channel и двух входящих в него физических портов.
- **EN:** R1 should show `Active` and R2 `Standby` in HSRP. In the EtherChannel output, check that the Port-channel exists and includes both physical ports.

### 7-қадам. Байланысты тексеру / Шаг 7. Проверить соединение / Step 7. Test connectivity

PC1-де **Desktop → Command Prompt** ашып, орындаңыз:

```text
ping 192.168.10.1
```

Содан кейін PC2 мекенжайын тексеріңіз:

```text
ping 192.168.10.20
```

**Түсіндірме / Пояснение / Explanation:**  
Бірінші ping виртуалды шлюзге жететінін тексереді. Екінші ping екі компьютердің бір LAN ішінде байланысын тексереді. Алғашқы ping сұрауы жауап бермесе, тағы бір рет орындаңыз: құрылғыларға ARP кестесін толтыруға уақыт керек болуы мүмкін.

### 8-қадам. HSRP резервін тексеру / Шаг 8. Проверить резервирование HSRP / Step 8. Test HSRP failover

R1-де оның желілік интерфейсін уақытша өшіріңіз:

```text
configure terminal
interface gigabitEthernet0/0
shutdown
end
```

PC1-де шлюзге ping орындаңыз:

```text
ping 192.168.10.1
```

R2 резервтік маршрутизатор ретінде трафикті өткізуі керек. Кейін R1 интерфейсін қайта қосыңыз:

```text
configure terminal
interface gigabitEthernet0/0
no shutdown
end
```

R1 күйін қайта тексеріңіз:

```text
show standby brief
```

**Түсіндірме / Пояснение / Explanation:**

- **KK:** Негізгі маршрутизатордың интерфейсі істен шыққанда, HSRP резервтік маршрутизаторға ауысады. Бірнеше ping пакеті жоғалуы мүмкін — бұл ауысу уақытымен байланысты.
- **RU:** При отключении интерфейса основного маршрутизатора HSRP переключает обслуживание на резервный. Несколько пакетов ping могут потеряться во время переключения.
- **EN:** When the active router’s interface goes down, HSRP switches service to the standby router. A few ping packets may be lost during the switchover.

### 9-қадам. LACP арнасының резервін тексеру / Шаг 9. Проверить резервирование канала LACP / Step 9. Test LACP link redundancy

**KK:** SW1 мен SW2 арасындағы екі кабельдің біреуін ажыратыңыз немесе екі жақтағы бір мүше портты уақытша өшіріңіз. `show etherchannel summary` арқылы арна күйін тексеріп, PC1-ден шлюзге ping-ті қайталаңыз. Содан соң портты немесе кабельді қалпына келтіріңіз.

**RU:** Отключите один из двух кабелей между SW1 и SW2 или временно выключите по одному порту-участнику на обоих концах. Проверьте состояние командой `show etherchannel summary` и повторите ping от PC1 до шлюза. Затем восстановите порт или кабель.

**EN:** Disconnect one of the two cables between SW1 and SW2, or temporarily shut down one member port at both ends. Check the state with `show etherchannel summary` and repeat the ping from PC1 to the gateway. Then restore the port or cable.

**Нәтиже / Результат / Expected result:** бір физикалық байланыс ажыратылса да, екінші арна жұмыс істеп тұруы керек / при отключении одной физической линии вторая продолжает работать / the remaining physical link should continue carrying traffic.

## 6. Қосымша: PAgP сынағы / Дополнительно: тест PAgP / Optional: test PAgP

Бұл бөлімде LACP конфигурациясын алып тастап, PAgP режимін екі коммутаторда да қолданып көріңіз.

SW1 және SW2 құрылғыларында:

```text
configure terminal
interface range fastEthernet0/23 - 24
no channel-group 1
channel-group 1 mode desirable
end
```

**Түсіндірме / Пояснение / Explanation:**

- **KK:** `desirable` PAgP келіссөзін бастайды. PAgP үшін екі коммутаторда да `desirable` режимін қолдануға болады.
- **RU:** `desirable` инициирует согласование PAgP. Для проверки можно задать `desirable` на обоих коммутаторах.
- **EN:** `desirable` initiates PAgP negotiation. You can use `desirable` on both switches for this test.

PAgP пен LACP — EtherChannel құрудың екі бөлек нұсқасы; бір арнаның екі шетінде бір протоколды қолданыңыз. Қолжетімді болса, күйді `show etherchannel summary` командасымен қайта тексеріңіз.

## 7. Қосымша тапсырма: VRRP және GLBP / Дополнительное задание: VRRP и GLBP / Optional task: VRRP and GLBP

Бұл тапсырманы HSRP орнына немесе оқытушының нұсқауымен орындаңыз. Бір уақытта бір интерфейсте бірнеше FHRP хаттамасын қатар баптамаңыз. Packet Tracer команданы қабылдамаса, онда қолданылып жатқан модель немесе IOS нұсқасы бұл функцияны қолдамауы мүмкін.

**VRRP үлгісі / Пример VRRP / VRRP example:**

```text
interface gigabitEthernet0/0
ip address 192.168.10.2 255.255.255.0
vrrp 1 ip 192.168.10.1
vrrp 1 priority 110
vrrp 1 preempt
```

**GLBP үлгісі / Пример GLBP / GLBP example:**

```text
interface gigabitEthernet0/0
ip address 192.168.10.2 255.255.255.0
glbp 1 ip 192.168.10.1
glbp 1 load-balancing round-robin
```

- **KK:** HSRP және VRRP виртуалды шлюздің резервтелуіне бағытталған; GLBP резервтеумен қатар жүктемені бірнеше маршрутизаторға бөле алады.
- **RU:** HSRP и VRRP обеспечивают резервирование виртуального шлюза; GLBP также может распределять нагрузку между маршрутизаторами.
- **EN:** HSRP and VRRP provide virtual-gateway redundancy; GLBP can also distribute traffic across multiple routers.

## 8. Қорытынды сұрақтар / Контрольные вопросы / Review questions

1. HSRP виртуалды IP мекенжайы не үшін керек?  
   **Для чего нужен виртуальный IP-адрес HSRP? / What is the HSRP virtual IP address used for?**

2. R1-де `priority 110` командасы қандай рөл атқарады?  
   **Для чего задан `priority 110` на R1? / What does `priority 110` do on R1?**

3. LACP арнасындағы бір физикалық кабель ажыратылса, не болады?  
   **Что произойдет при отключении одного кабеля в канале LACP? / What happens if one physical cable in the LACP bundle fails?**

4. LACP пен PAgP-тің бір айырмашылығын атаңыз.  
   **Назовите одно различие между LACP и PAgP. / Name one difference between LACP and PAgP.**

## 9. Есеп тапсыру / Отчет / Submission

**KK:** Есепке топологияның скриншотын, IP кестесін, `show standby brief` және `show etherchannel summary` нәтижелерін, сондай-ақ істен шығу сынағының қысқаша қорытындысын қосыңыз.

**RU:** В отчет добавьте скриншот топологии, таблицу IP-адресов, вывод команд `show standby brief` и `show etherchannel summary`, а также краткий результат проверки отказоустойчивости.

**EN:** Include a topology screenshot, the IP addressing table, the output of `show standby brief` and `show etherchannel summary`, and a short summary of the failover test.
