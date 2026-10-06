# Зертханалық жұмыс: Packet Tracer-де Site-to-Site IPSec VPN конфигурациялау / Лабораторная работа: Настройка Site-to-Site IPSec VPN в Packet Tracer / Lab Assignment: Site-to-Site IPSec VPN Configuration in Packet Tracer

**Болжалды уақыт:** 30-45 минут / **Примерное время:** 30-45 минут / **Estimated Time:** 30-45 minutes  
**Технология:** IPSec Site-to-Site VPN / **Технология:** IPSec Site-to-Site VPN / **Technology:** IPSec Site-to-Site VPN

---

## 1. Зертханаға шолу / Обзор лабораторной работы / Lab Overview

**KZ:** Бұл зертханалық жұмыста сіз Packet Tracer-де Cisco маршрутизаторларын пайдаланып, екі кеңсе (Office A және Office B) арасында site-to-site IPSec VPN туннелін конфигурациялайсыз. IPSec деректерді шифрлап, сенімсіз желі (интернет) арқылы қауіпсіз байланыс құрады.

**RU:** В этой лабораторной работе вы настроите туннель site-to-site IPSec VPN между двумя офисами (Office A и Office B) с использованием маршрутизаторов Cisco в Packet Tracer. IPSec шифрует данные и обеспечивает безопасное соединение через недоверенную сеть (интернет).

**EN:** In this lab, you will configure a site-to-site IPSec VPN tunnel between two offices (Office A and Office B) using Cisco routers in Packet Tracer. IPSec encrypts data and provides secure communication over an untrusted network (the internet).

---

## 2. Оқу мақсаттары / Цели обучения / Learning Objectives

**KZ:** Осы жұмысты аяқтағаннан кейін сіз:
1. IPSec VPN параметрлерін конфигурациялай аласыз (IKE Phase 1 және Phase 2)
2. Crypto map-ты маршрутизатор интерфейстеріне қолдана аласыз
3. IPSec туннелінің орнатылғанын тексере аласыз
4. Екі сайт арасындағы шифрланған байланысты тексере аласыз

**RU:** После завершения этой работы вы сможете:
1. Настроить параметры IPSec VPN (IKE Phase 1 и Phase 2)
2. Применить crypto map к интерфейсам маршрутизатора
3. Проверить установку туннеля IPSec
4. Протестировать зашифрованную связь между двумя сайтами

**EN:** By completing this lab, you will be able to:
1. Configure IPSec VPN parameters (IKE Phase 1 and Phase 2)
2. Apply crypto maps to router interfaces
3. Verify IPSec tunnel establishment
4. Test encrypted communication between two sites

---

## 3. Желілік топология / Сетевая топология / Network Topology

```
┌─────────────────┐                                         ┌─────────────────┐
│   Office A      │                                         │   Office B      │
│                 │                                         │                 │
│  PC-A           │         ┌──────────────┐                │           PC-B  │
│  192.168.1.2    │         │   "Internet" │                │    192.168.2.2  │
│       │         │         │              │                │         │       │
│       │         │         │              │                │         │       │
│  ┌────┴────┐    │         │              │                │    ┌────┴────┐  │
│  │   R1    │────┼─────────┤   ISP Router ├────────────────┼────│   R2    │  │
│  │         │    │         │              │                │    │         │  │
│  └─────────┘    │         └──────────────┘                │    └─────────┘  │
│  G0/0: 192.168.1.1                           G0/0: 192.168.2.1              │
│  S0/0/0: 10.1.1.1                           S0/0/0: 10.2.2.2                │
└─────────────────┘                                         └─────────────────┘
```

**Ескерту:** ISP маршрутизаторы қоғамдық интернетті имитациялайды. Packet Tracer-де кез келген маршрутизаторды (мысалы, 2811) пайдалануға болады.  
**Примечание:** Маршрутизатор ISP имитирует публичный интернет. В Packet Tracer можно использовать любой маршрутизатор (например, 2811).  
**Note:** The ISP router simulates the public internet. In Packet Tracer, you can use any router (e.g., 2811) for this purpose.

---

## 4. IP адрестеу кестесі / Таблица IP-адресации / IP Addressing Table

| Құрылғы / Устройство / Device | Интерфейс / Интерфейс / Interface | IP мекенжайы / IP-адрес / IP Address | Масқа / Маска / Subnet Mask | Сипаттама / Описание / Description |
|---|---|---|---|---|
| R1 | G0/0 | 192.168.1.1 | 255.255.255.0 | LAN (Office A) |
| R1 | S0/0/0 | 10.1.1.1 | 255.255.255.252 | WAN to ISP |
| R2 | G0/0 | 192.168.2.1 | 255.255.255.0 | LAN (Office B) |
| R2 | S0/0/0 | 10.2.2.2 | 255.255.255.252 | WAN to ISP |
| ISP | S0/0/0 | 10.1.1.2 | 255.255.255.252 | To R1 |
| ISP | S0/0/1 | 10.2.2.1 | 255.255.255.252 | To R2 |
| PC-A | NIC | 192.168.1.2 | 255.255.255.0 | Default GW: 192.168.1.1 |
| PC-B | NIC | 192.168.2.2 | 255.255.255.0 | Default GW: 192.168.2.1 |

---

## 5. Алдын ала конфигурация / Предварительная конфигурация / Pre-Lab Configuration

### Қадам 0: Негізгі конфигурация / Шаг 0: Базовая конфигурация / Step 0: Basic Configuration

**KZ:** IPSec конфигурациясын бастамас бұрын, келесіні орындаңыз:  
**RU:** Перед началом настройки IPSec выполните следующее:  
**EN:** Before starting IPSec configuration, ensure the following:

**R1 құрылғысында / На R1 / On R1:**
```
R1(config)# interface G0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown

R1(config)# interface S0/0/0
R1(config-if)# ip address 10.1.1.1 255.255.255.252
R1(config-if)# no shutdown

R1(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2
```

**R2 құрылғысында / На R2 / On R2:**
```
R2(config)# interface G0/0
R2(config-if)# ip address 192.168.2.1 255.255.255.0
R2(config-if)# no shutdown

R2(config)# interface S0/0/0
R2(config-if)# ip address 10.2.2.2 255.255.255.252
R2(config-if)# no shutdown

R2(config)# ip route 0.0.0.0 0.0.0.0 10.2.2.1
```

**ISP құрылғысында / На ISP / On ISP:**
```
ISP(config)# interface S0/0/0
ISP(config-if)# ip address 10.1.1.2 255.255.255.252
ISP(config-if)# no shutdown

ISP(config)# interface S0/0/1
ISP(config-if)# ip address 10.2.2.1 255.255.255.252
ISP(config-if)# no shutdown
```

**Тексеру / Проверка / Verify:** PC-A PC-B-ге VPN конфигурациясына дейін ping жасай алуы керек.  
**Проверка:** PC-A должен иметь возможность пинговать PC-B **до** настройки VPN.  
**Verify:** PC-A should be able to ping PC-B **before** VPN is configured.

---

## 6. 1-тапсырма: R1-де IPSec VPN конфигурациялау / Задача 1: Настройка IPSec VPN на R1 / Task 1: Configure IPSec VPN on R1

### Қадам 1: IKE Phase 1 саясатын жасау (ISAKMP Policy) / Шаг 1: Создание политики IKE Phase 1 (ISAKMP Policy) / Step 1: Create IKE Phase 1 Policy (ISAKMP Policy)

**KZ:** IKE Phase 1 қауіпсіз арна құру үшін параметрлерді анықтайды.  
**RU:** IKE Phase 1 определяет параметры для создания безопасного канала.  
**EN:** IKE Phase 1 defines parameters for building a secure channel.

```
R1(config)# crypto isakmp policy 10
R1(config-isakmp)# encryption aes 256
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 5
R1(config-isakmp)# exit
```

### Қадам 2: Алдын ала бөліскен кілтті конфигурациялау / Шаг 2: Настройка предварительно разделенного ключа / Step 2: Configure Pre-Shared Key

**KZ:** Алдын ала бөліскен кілт екі маршрутизаторда да бірдей болуы керек.  
**RU:** Предварительно разделенный ключ должен совпадать на обоих маршрутизаторах.  
**EN:** The pre-shared key must match on both routers.

```
R1(config)# crypto isakmp key vpnpa55 address 10.2.2.2
```

### Қадам 3: IPSec Transform Set жасау (IKE Phase 2) / Шаг 3: Создание IPSec Transform Set (IKE Phase 2) / Step 3: Create IPSec Transform Set (IKE Phase 2)

**KZ:** IKE Phase 2 нақты деректерді шифрлау параметрлерін анықтайды.  
**RU:** IKE Phase 2 определяет параметры шифрования самих данных.  
**EN:** IKE Phase 2 defines encryption parameters for the actual data.

```
R1(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
R1(config-crypto-trans)# exit
```

### Қадам 4: Қызықты трафик үшін кеңейтілген ACL жасау / Шаг 4: Создание расширенного ACL для интересующего трафика / Step 4: Create Extended ACL for Interesting Traffic

**KZ:** Бұл ACL қай трафикті шифрлау керектігін анықтайды (Office A LAN-нан Office B LAN-ға).  
**RU:** Этот ACL определяет, какой трафик должен шифроваться (из LAN Office A в LAN Office B).  
**EN:** This ACL defines which traffic should be encrypted (from Office A LAN to Office B LAN).

```
R1(config)# access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
```

### Қадам 5: Crypto Map жасау / Шаг 5: Создание Crypto Map / Step 5: Create Crypto Map

**KZ:** Crypto map ACL, transform set және peer ақпаратын біріктіреді.  
**RU:** Crypto map объединяет ACL, transform set и информацию о peer.  
**EN:** Crypto map combines ACL, transform set, and peer information.

```
R1(config)# crypto map VPN-MAP 10 ipsec-isakmp
R1(config-crypto-map)# set peer 10.2.2.2
R1(config-crypto-map)# set transform-set VPN-SET
R1(config-crypto-map)# match address 110
R1(config-crypto-map)# exit
```

### Қадам 6: Crypto Map-ты WAN интерфейсіне қолдану / Шаг 6: Применение Crypto Map к WAN-интерфейсу / Step 6: Apply Crypto Map to WAN Interface

```
R1(config)# interface S0/0/0
R1(config-if)# crypto map VPN-MAP
R1(config-if)# exit
```

---

## 7. 2-тапсырма: R2-де IPSec VPN конфигурациялау / Задача 2: Настройка IPSec VPN на R2 / Task 2: Configure IPSec VPN on R2

### Қадам 1: IKE Phase 1 саясатын жасау (R1-мен сәйкес болуы керек) / Шаг 1: Создание политики IKE Phase 1 (должна совпадать с R1) / Step 1: Create IKE Phase 1 Policy (Must Match R1)

```
R2(config)# crypto isakmp policy 10
R2(config-isakmp)# encryption aes 256
R2(config-isakmp)# authentication pre-share
R2(config-isakmp)# group 5
R2(config-isakmp)# exit
```

### Қадам 2: Алдын ала бөліскен кілтті конфигурациялау (R1-мен сәйкес болуы керек) / Шаг 2: Настройка предварительно разделенного ключа (должен совпадать с R1) / Step 2: Configure Pre-Shared Key (Must Match R1)

```
R2(config)# crypto isakmp key vpnpa55 address 10.1.1.1
```

### Қадам 3: IPSec Transform Set жасау (R1-мен сәйкес болуы керек) / Шаг 3: Создание IPSec Transform Set (должен совпадать с R1) / Step 3: Create IPSec Transform Set (Must Match R1)

```
R2(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
R2(config-crypto-trans)# exit
```

### Қадам 4: Қызықты трафик үшін кеңейтілген ACL жасау / Шаг 4: Создание расширенного ACL для интересующего трафика / Step 4: Create Extended ACL for Interesting Traffic

```
R2(config)# access-list 110 permit ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```

### Қадам 5: Crypto Map жасау / Шаг 5: Создание Crypto Map / Step 5: Create Crypto Map

```
R2(config)# crypto map VPN-MAP 10 ipsec-isakmp
R2(config-crypto-map)# set peer 10.1.1.1
R2(config-crypto-map)# set transform-set VPN-SET
R2(config-crypto-map)# match address 110
R2(config-crypto-map)# exit
```

### Қадам 6: Crypto Map-ты WAN интерфейсіне қолдану / Шаг 6: Применение Crypto Map к WAN-интерфейсу / Step 6: Apply Crypto Map to WAN Interface

```
R2(config)# interface S0/0/0
R2(config-if)# crypto map VPN-MAP
R2(config-if)# exit
```

---

## 8. 3-тапсырма: Тексеру / Задача 3: Проверка / Task 3: Verification

### Қадам 1: Қызықты трафик генерациялау / Шаг 1: Генерация интересующего трафика / Step 1: Generate Interesting Traffic

**KZ:** PC-A-дан PC-B-ге ping жасаңыз:  
**RU:** С PC-A пингуйте PC-B:  
**EN:** From PC-A, ping PC-B:

```
PC-A> ping 192.168.2.2
```

**KZ:** Бірінші ping туннель орнатылуына байланысты сәтсіз болуы мүмкін. Күтіп, қайта ping жасаңыз.  
**RU:** Первый ping может не пройти из-за установки туннеля. Подождите и пингуйте снова.  
**EN:** The first ping may fail due to tunnel establishment. Wait and ping again.

### Қадам 2: IPSec туннелінің күйін тексеру / Шаг 2: Проверка состояния туннеля IPSec / Step 2: Verify IPSec Tunnel Status

**R1 немесе R2 құрылғысында / На R1 или R2 / On R1 or R2:**

```
R1# show crypto isakmp sa
```

**Күтілетін шығыс / Ожидаемый вывод / Expected Output:**
```
IPv4 Crypto ISAKMP SA
dst             src             state          conn-id status
10.2.2.2        10.1.1.1        QM_IDLE           1001 ACTIVE
```

**KZ:** `QM_IDLE` күйі туннельдің белсенді екенін көрсетеді.  
**RU:** Состояние `QM_IDLE` указывает на активность туннеля.  
**EN:** State `QM_IDLE` indicates the tunnel is active.

```
R1# show crypto ipsec sa
```

**Күтілетін шығыс (іздеу керек) / Ожидаемый вывод (ищите) / Expected Output (look for):**
```
inbound esp sas:
  spi: 0x... (...)
    transform: esp-aes esp-sha-hmac
    ...
    pkts encaps: X, pkts encrypt: X
    ...
outbound esp sas:
  spi: 0x... (...)
    transform: esp-aes esp-sha-hmac
    ...
    pkts encaps: X, pkts encrypt: X
```

**KZ:** `pkts encrypt` және `pkts decrypt` санауыштарының өсіп жатқанын бақылаңыз.  
**RU:** Следите за увеличением счетчиков `pkts encrypt` и `pkts decrypt`.  
**EN:** Look for `pkts encrypt` and `pkts decrypt` counters increasing.

### Қадам 3: Ping арқылы тексеру / Шаг 3: Проверка с помощью ping / Step 3: Verify with Ping

```
PC-A> ping 192.168.2.2
```

**KZ:** Ping сәтті орындалуы керек, ал IPSec SA санауыштары өсуі керек.  
**RU:** Ping должен успешно пройти, а счетчики IPSec SA должны увеличиться.  
**EN:** The ping should succeed, and the IPSec SA counters should increase.

---

## 9. 4-тапсырма: Бақылау сұрақтары / Задача 4: Контрольные вопросы / Task 4: Review Questions

**KZ:** Зертханалық тәжірибеңізге сүйене отырып, келесі сұрақтарға жауап беріңіз:  
**RU:** Ответьте на следующие вопросы, основываясь на вашем опыте в лабораторной работе:  
**EN:** Answer the following questions based on your lab experience:

1. **`crypto isakmp policy` не үшін қажет?** / **Для чего нужна `crypto isakmp policy`?** / **What is the purpose of `crypto isakmp policy`?**
   - **KZ:** Ол IKE Phase 1 параметрлерін анықтайды — қауіпсіз арна құру үшін.
   - **RU:** Она определяет параметры IKE Phase 1 для создания безопасного канала.
   - **EN:** It defines IKE Phase 1 parameters for building a secure channel between peers.

2. **`crypto ipsec transform-set` не үшін қажет?** / **Для чего нужна `crypto ipsec transform-set`?** / **What is the purpose of `crypto ipsec transform-set`?**
   - **KZ:** Ол IKE Phase 2 параметрлерін анықтайды — нақты деректерді шифрлау үшін.
   - **RU:** Она определяет параметры IKE Phase 2 для шифрования самих данных.
   - **EN:** It defines IKE Phase 2 parameters for encrypting the actual data traffic.

3. **ACL (access-list 110) осы конфигурацияда не істейді?** / **Что делает ACL (access-list 110) в этой конфигурации?** / **What does the ACL (access-list 110) accomplish in this configuration?**
   - **KZ:** Ол «қызықты трафикті» анықтайды — VPN туннелі арқылы шифрланып жіберілетін трафик.
   - **RU:** Он определяет «интересующий трафик» — трафик, который должен шифроваться и отправляться через VPN-туннель.
   - **EN:** It defines "interesting traffic" — the traffic that should be encrypted and sent through the VPN tunnel.

4. **R1 мен R2-дегі алдын ала бөліскен кілттер сәйкес келмесе не болады?** / **Что произойдет, если предварительно разделенные ключи на R1 и R2 не совпадут?** / **What happens if the pre-shared keys do not match on R1 and R2?**
   - **KZ:** IPSec туннелі орнатылмайды. IKE Phase 1 сәтсіз аяқталады.
   - **RU:** Туннель IPSec не установится. IKE Phase 1 завершится неудачно.
   - **EN:** The IPSec tunnel will not establish. IKE Phase 1 will fail.

5. **IPSec туннелінің белсенді екенін қай команда көрсетеді?** / **Какая команда показывает, что туннель IPSec активен?** / **What command shows whether the IPSec tunnel is active?**
   - **KZ:** `show crypto isakmp sa` IKE Phase 1 күйін көрсетеді. `QM_IDLE` күйі белсенді екенін білдіреді.
   - **RU:** `show crypto isakmp sa` показывает состояние IKE Phase 1. Состояние `QM_IDLE` указывает на активность.
   - **EN:** `show crypto isakmp sa` shows the IKE Phase 1 status. State `QM_IDLE` indicates active.

6. **Анықтамалық материалға сәйкес, IPSec қарапайым түсінікте немен салыстырылады?** / **Согласно справочному материалу, с чем сравнивается IPSec в простом объяснении?** / **According to the reference material, what is IPSec compared to in simple terms?**
   - **KZ:** «Броньды конверт» — деректер шифрланып, тек алушы аша алады.
   - **RU:** «Бронированный конверт» — данные шифруются, и только получатель может их открыть.
   - **EN:** An "armored envelope" — data is encrypted and only the recipient can open it.

---

## 10. Қосымша тапсырма (міндетті емес) / Дополнительное задание (необязательно) / Challenge (Optional)

**KZ:** Ерте бітірсеңіз, келесіні орындап көріңіз:  
**RU:** Если вы закончили раньше, попробуйте следующее:  
**EN:** If you finish early, try the following:

1. **Шифрлау алгоритмін** екі маршрутизаторда да `aes 128` етіп өзгертіңіз және туннель әлі де жұмыс істейтінін тексеріңіз.  
   **Измените алгоритм шифрования** на `aes 128` на обоих маршрутизаторах и проверьте, что туннель все еще работает.  
   **Modify the encryption algorithm** to `aes 128` on both routers and verify the tunnel still works.

2. **Алдын ала бөліскен кілтті** тек R1-де өзгертіңіз және не болатынын бақылаңыз. `show crypto isakmp sa` көмегімен сәтсіздікті көріңіз.  
   **Измените предварительно разделенный ключ** только на R1 и понаблюдайте, что произойдет. Используйте `show crypto isakmp sa`, чтобы увидеть сбой.  
   **Change the pre-shared key** on only R1 and observe what happens. Use `show crypto isakmp sa` to see the failure.

3. **Office A-ға екінші subnet** қосыңыз (мысалы, 192.168.3.0/24) және ACL-ді оны қосу үшін өзгертіңіз.  
   **Добавьте вторую подсеть** в Office A (например, 192.168.3.0/24) и измените ACL, чтобы включить ее.  
   **Add a second subnet** to Office A (e.g., 192.168.3.0/24) and modify the ACL to include it.

---

## 11. Қорытынды / Итог / Summary

**KZ:** Осы зертханалық жұмыста сіз:
- ✅ Екі маршрутизаторда IKE Phase 1 (ISAKMP policy) конфигурацияладыңыз
- ✅ Екі маршрутизаторда IKE Phase 2 (IPSec transform set) конфигурацияладыңыз
- ✅ Сайттар арасындағы трафикті шифрлау үшін crypto map жасап, қолдандыңыз
- ✅ `show crypto isakmp sa` және `show crypto ipsec sa` көмегімен IPSec туннелін тексердіңіз

**RU:** В этой лабораторной работе вы успешно:
- ✅ Настроили IKE Phase 1 (ISAKMP policy) на обоих маршрутизаторах
- ✅ Настроили IKE Phase 2 (IPSec transform set) на обоих маршрутизаторах
- ✅ Создали и применили crypto map для шифрования трафика между сайтами
- ✅ Проверили туннель IPSec с помощью `show crypto isakmp sa` и `show crypto ipsec sa`

**EN:** In this lab, you successfully:
- ✅ Configured IKE Phase 1 (ISAKMP policy) on both routers
- ✅ Configured IKE Phase 2 (IPSec transform set) on both routers
- ✅ Created and applied crypto maps to encrypt traffic between sites
- ✅ Verified the IPSec tunnel using `show crypto isakmp sa` and `show crypto ipsec sa`

**Негізгі қорытынды / Ключевой вывод / Key Takeaway:**  
**KZ:** IPSec екі сайт арасындағы сенімсіз желі арқылы өтетін деректердің құпиялылығы мен тұтастығын қамтамасыз етеді. Туннель «қызықты трафик» (ACL-мен анықталған) анықталған кезде автоматты түрде орнатылады.  
**RU:** IPSec обеспечивает конфиденциальность и целостность данных, передаваемых между двумя сайтами через недоверенную сеть. Туннель устанавливается автоматически при обнаружении «интересующего трафика» (определенного ACL).  
**EN:** IPSec provides confidentiality and integrity for data traveling between two sites over an untrusted network. The tunnel is established automatically when "interesting traffic" (defined by the ACL) is detected.

---

## 12. Бақылау сұрақтарына жауаптар / Ответы на контрольные вопросы / Answer Key for Review Questions

| Сұрақ / Вопрос / Question | Жауап / Ответ / Answer |
|---|---|
| 1 | IKE Phase 1 параметрлерін анықтайды (шифрлау, аутентификация, DH тобы) / Определяет параметры IKE Phase 1 (шифрование, аутентификация, группа DH) / Defines IKE Phase 1 parameters (encryption, authentication, DH group) |
| 2 | IKE Phase 2 параметрлерін анықтайды (деректерді шифрлау және хэштеу) / Определяет параметры IKE Phase 2 (шифрование и хэширование данных) / Defines IKE Phase 2 parameters (encryption and hashing for data) |
| 3 | Шифрланатын трафикті анықтайды (192.168.1.0/24-тен 192.168.2.0/24-ке) / Определяет трафик для шифрования (из 192.168.1.0/24 в 192.168.2.0/24) / Identifies traffic to be encrypted (from 192.168.1.0/24 to 192.168.2.0/24) |
| 4 | IKE Phase 1 сәтсіз аяқталады; туннель орнатылмайды / IKE Phase 1 завершается неудачно; туннель не устанавливается / IKE Phase 1 fails; tunnel does not establish |
| 5 | `show crypto isakmp sa` — `QM_IDLE` күйін іздеңіз / `show crypto isakmp sa` — ищите состояние `QM_IDLE` / `show crypto isakmp sa` — look for `QM_IDLE` state |
| 6 | «Броньды конверт» — деректер шифрланып, тек алушы аша алады / «Бронированный конверт» — данные шифруются, и только получатель может их открыть / An "armored envelope" — data is encrypted and only recipient can open |

---

**Зертханалық жұмыстың соңы / Конец лабораторной работы / End of Lab**
