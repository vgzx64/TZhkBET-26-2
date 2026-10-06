# VPN технологиялары / VPN технологии / VPN Technologies

## 1. Жалпы түсінік / Общее понимание / General Overview

**VPN (Virtual Private Network)** — бұл қоғамдық интернет арқылы жеке, қауіпсіз байланыс құру технологиясы. VPN туннель деп аталатын «көрінбейтін жол» жасайды: деректер шифрланып, сыртқы адамдар оқи алмайды.

**VPN (Virtual Private Network)** — это технология создания частного, безопасного соединения через публичный интернет. VPN создаёт «невидимый путь», называемый туннелем: данные шифруются, и посторонние не могут их прочитать.

**VPN (Virtual Private Network)** is a technology for creating a private, secure connection over the public internet. VPN creates an "invisible path" called a tunnel: data is encrypted, and outsiders cannot read it.

---

## 2. Технологияларды салыстыру / Сравнение технологий / Technology Comparison

### 2.1. Негізгі айырмашылықтар кестесі / Таблица основных различий / Main Differences Table

| Технология | Не істейді? / Что делает? / What it does | Қауіпсіздік / Безопасность / Security | Қолданылуы / Применение / Use case |
|---|---|---|---|
| **IPSec** | Екі аралықты шифрлайды / Шифрует между двумя точками / Encrypts between two points | ✅ Бар (шифрлау) / Есть (шифрование) / Yes (encryption) | Site-to-site VPN |
| **SSL VPN** | Қолданушыны серверге қосады / Подключает пользователя к серверу / Connects user to server | ✅ Бар (TLS арқылы) / Есть (через TLS) / Yes (via TLS) | Қашықтан жұмыс / Удалённая работа / Remote work |
| **GRE** | Туннель жасайды, бірақ шифрламайды / Создаёт туннель, но не шифрует / Creates tunnel, but no encryption | ❌ Жоқ (тек тасымалдау) / Нет (только транспорт) / No (transport only) | Multicast, routing өткізу / Пропуск multicast, routing / Pass multicast, routing |
| **DMVPN** | GRE + IPSec бірге / GRE + IPSec вместе / GRE + IPSec together | ✅ Бар (IPSec) / Есть (IPSec) / Yes (IPSec) | Көп филиалдар / Много филиалов / Many branches |

### 2.2. IPSec — «Шифрлау стандарты» / «Стандарт шифрования» / «The Encryption Standard»

IPSec — бұл IP деңгейінде жұмыс істейтін хаттамалар жиынтығы. Ол пакетті толық шифрлап, жаңа IP тақырыпшасына орайды (tunnel mode).

IPSec — это набор протоколов, работающих на IP-уровне. Он полностью шифрует пакет и оборачивает его в новый IP-заголовок (tunnel mode).

IPSec is a suite of protocols operating at the IP layer. It fully encrypts the packet and wraps it in a new IP header (tunnel mode).

**Қарапайым түсінік:** IPSec — бұл «броньды конверт». Хатты (деректерді) конвертке салып, шифрлап жібересіз. Алушы ғана аша алады.

**Простое объяснение:** IPSec — это «бронированный конверт». Вы кладёте письмо (данные) в конверт, шифруете и отправляете. Только получатель может открыть.

**Simple explanation:** IPSec is an "armored envelope." You put a letter (data) in the envelope, encrypt it, and send it. Only the recipient can open it.

**Packet Tracer мысалы / Пример в Packet Tracer / Packet Tracer Example:**

```
R1(config)# crypto isakmp policy 10
R1(config-isakmp)# encryption aes 256
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 5
R1(config)# crypto isakmp key vpnpa55 address 10.2.2.2

R1(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
R1(config)# crypto map VPN-MAP 10 ipsec-isakmp
R1(config-crypto-map)# set peer 10.2.2.2
R1(config-crypto-map)# set transform-set VPN-SET
R1(config-crypto-map)# match address 110
R1(config)# interface s0/0/0
R1(config-if)# crypto map VPN-MAP
```



**Есте сақтау керек:** `crypto isakmp policy` — IKE Phase 1 (қауіпсіз арна құру). `crypto ipsec transform-set` — IKE Phase 2 (нақты деректерді шифрлау).

**Напоминание:** `crypto isakmp policy` — IKE Phase 1 (создание безопасного канала). `crypto ipsec transform-set` — IKE Phase 2 (шифрование самих данных).

**Reminder:** `crypto isakmp policy` — IKE Phase 1 (building a secure channel). `crypto ipsec transform-set` — IKE Phase 2 (encrypting the actual data).

### 2.3. SSL VPN — «Браузер арқылы VPN» / «VPN через браузер» / «VPN via Browser»

SSL VPN TCP 443 портын пайдаланады. Бұл — HTTPS порты. Сондықтан көптеген файрволдар оны өткізеді. Қолданушыға арнайы бағдарлама орнатудың қажеті жоқ — браузер жеткілікті.

SSL VPN использует порт TCP 443. Это порт HTTPS. Поэтому многие файрволы его пропускают. Пользователю не нужно устанавливать специальную программу — достаточно браузера.

SSL VPN uses TCP port 443. This is the HTTPS port. Therefore, many firewalls allow it through. The user does not need to install special software — a browser is enough.

**Қарапайым түсінік:** SSL VPN — бұл «онлайн-дүкеннің сайты». Кіресіз, логин/құпиясөз енгізесіз — ішке кіресіз. Қосымша бағдарлама жоқ.

**Простое объяснение:** SSL VPN — это «сайт интернет-магазина». Заходите, вводите логин/пароль — попадаете внутрь. Никаких дополнительных программ.

**Simple explanation:** SSL VPN is like an "online store website." You go in, enter your login/password — you get inside. No additional programs.

**Packet Tracer ескертуі / Примечание по Packet Tracer / Packet Tracer Note:**

Packet Tracer-де SSL VPN тек ASA 5505 файрволында қолжетімді. Қарапайым маршрутизаторларда (2811, 2620) SSL VPN істемейді.

В Packet Tracer SSL VPN доступен только на файрволе ASA 5505. На обычных маршрутизаторах (2811, 2620) SSL VPN не работает.

In Packet Tracer, SSL VPN is only available on the ASA 5505 firewall. On regular routers (2811, 2620), SSL VPN does not work.

### 2.4. GRE — «Туннель, бірақ қауіпсіздіксіз» / «Туннель, но без безопасности» / «Tunnel, but No Security»

GRE — қарапайым туннель. Ол пакеттерді орайды, бірақ **шифрламайды**. GRE-дің басты артықшылығы: ол **multicast** және **routing** хаттамаларын өткізе алады.

GRE — простой туннель. Он инкапсулирует пакеты, но **не шифрует**. Главное преимущество GRE: он может пропускать **multicast** и **routing** протоколы.

GRE is a simple tunnel. It encapsulates packets but does **not encrypt**. The main advantage of GRE: it can pass **multicast** and **routing** protocols.

**📌 Есте сақтау керек: Multicast дегеніміз не?**

**Multicast** — бір дереккөзден бір уақытта бірнеше алушыға деректер жіберу. Мысалы, бір бейнеконференцияны 100 адам көреді — деректер 100 рет емес, 1 рет жіберіледі. Multicast адрестер 224.0.0.0 – 239.255.255.255 диапазонында.

**Multicast** — отправка данных от одного источника одновременно нескольким получателям. Например, одну видеоконференцию смотрят 100 человек — данные отправляются не 100 раз, а 1 раз. Multicast-адреса находятся в диапазоне 224.0.0.0 – 239.255.255.255.

**Multicast** — sending data from one source to multiple recipients simultaneously. For example, 100 people watch one video conference — data is sent not 100 times, but 1 time. Multicast addresses are in the range 224.0.0.0 – 239.255.255.255.

**Packet Tracer мысалы / Пример в Packet Tracer / Packet Tracer Example:**

```
RA(config)# interface Tunnel0
RA(config-if)# ip address 10.10.10.1 255.255.255.252
RA(config-if)# tunnel source Serial0/0/0
RA(config-if)# tunnel destination 209.165.122.2
RA(config-if)# tunnel mode gre ip
```



**Маңызды:** GRE өздігінен қауіпсіз емес. Оны IPSec-пен бірге қолдану керек: `tunnel protection ipsec profile`.

**Важно:** GRE сам по себе не безопасен. Его нужно использовать вместе с IPSec: `tunnel protection ipsec profile`.

**Important:** GRE by itself is not secure. It must be used together with IPSec: `tunnel protection ipsec profile`.

### 2.5. DMVPN — «Көп филиалға арналған VPN» / «VPN для многих филиалов» / «VPN for Many Branches»

DMVPN — бұл GRE + IPSec + NHRP біріктірілген жүйе. NHRP (Next Hop Resolution Protocol) — туннельдерді динамикалық түрде автоматты құруға мүмкіндік беретін хаттама.

DMVPN — это объединение GRE + IPSec + NHRP. NHRP (Next Hop Resolution Protocol) — протокол, позволяющий динамически автоматически создавать туннели.

DMVPN is a combination of GRE + IPSec + NHRP. NHRP (Next Hop Resolution Protocol) is a protocol that allows dynamic automatic tunnel creation.

**Қарапайым түсінік:** DMVPN — бұл «хаб және спица» (hub-and-spoke) жүйесі. Барлық филиалдар орталыққа (hub) қосылады. Бір филиал екінші филиалға тікелей қосылғысы келсе, хаб оларға «таныстырып», тікелей туннель ашуға көмектеседі. Бұл — full-mesh IPSec-ке қарағанда әлдеқайда оңай.

**Простое объяснение:** DMVPN — это система «хаб и спица» (hub-and-spoke). Все филиалы подключаются к центру (hub). Если один филиал хочет подключиться к другому напрямую, хаб их «знакомит» и помогает открыть прямой туннель. Это гораздо проще, чем full-mesh IPSec.

**Simple explanation:** DMVPN is a "hub-and-spoke" system. All branches connect to the center (hub). If one branch wants to connect directly to another, the hub "introduces" them and helps open a direct tunnel. This is much easier than full-mesh IPSec.

**⚠️ Packet Tracer ескертуі / Примечание по Packet Tracer / Packet Tracer Note:**

DMVPN **Packet Tracer-де қолданылмайды**. Ол GNS3 немесе EVE-ng эмуляторларында ғана істейді.

DMVPN **не поддерживается в Packet Tracer**. Он работает только в эмуляторах GNS3 или EVE-ng.

DMVPN is **not supported in Packet Tracer**. It only works in GNS3 or EVE-ng emulators.

**Debian мысалы (StrongSwan):**

Debian-да IPSec-ті StrongSwan арқылы конфигурациялауға болады. `/etc/ipsec.conf` файлында қосылым параметрлері жазылады.

В Debian IPSec можно настроить через StrongSwan. В файле `/etc/ipsec.conf` записываются параметры соединения.

In Debian, IPSec can be configured via StrongSwan. Connection parameters are written in the `/etc/ipsec.conf` file.

---

## 3. Қорытынды салыстыру / Итоговое сравнение / Final Comparison

| Сұрақ / Вопрос / Question | IPSec | SSL VPN | GRE | DMVPN |
|---|---|---|---|---|
| Шифрлайды ма? / Шифрует? / Encrypts? | ✅ | ✅ | ❌ | ✅ |
| Multicast өткізе ме? / Пропускает multicast? / Passes multicast? | ❌ | ❌ | ✅ | ✅ |
| Packet Tracer-де істей ме? / Работает в Packet Tracer? / Works in PT? | ✅ | ⚠️ (тек ASA) | ✅ | ❌ |
| Қай кезде қолданылады? / Когда используется? / When used? | Екі кеңсе / Два офиса / Two offices | Үйден жұмыс / Работа из дома / Remote work | Routing/multicast қажет / Нужен routing/multicast / Need routing/multicast | Көп филиал / Много филиалов / Many branches |

---

## 4. Бақылау сұрақтары / Контрольные вопросы / Review Questions

1. **IPSec пен GRE арасындағы басты айырмашылық неде?** / **В чём главное различие между IPSec и GRE?** / **What is the main difference between IPSec and GRE?**

2. **Multicast дегеніміз не? Ол қандай VPN технологиясында қолданылады?** / **Что такое multicast? В какой VPN-технологии он используется?** / **What is multicast? In which VPN technology is it used?**

3. **SSL VPN қай портты пайдаланады? Неге бұл маңызды?** / **Какой порт использует SSL VPN? Почему это важно?** / **Which port does SSL VPN use? Why is it important?**

4. **DMVPN-де NHRP қандай рөл атқарады?** / **Какую роль играет NHRP в DMVPN?** / **What role does NHRP play in DMVPN?**

5. **Packet Tracer-де қай VPN технологияларын конфигурациялауға болады?** / **Какие VPN-технологии можно конфигурировать в Packet Tracer?** / **Which VPN technologies can be configured in Packet Tracer?**

6. **GRE туннелін қалай қауіпсіз етуге болады?** / **Как сделать GRE-туннель безопасным?** / **How can a GRE tunnel be made secure?**

7. **Debian-да IPSec конфигурациялау үшін қандай бағдарлама қолданылады?** / **Какая программа используется для настройки IPSec в Debian?** / **Which program is used to configure IPSec in Debian?**

8. **Қай жағдайда SSL VPN-ді IPSec-ке қарағанда таңдау керек?** / **В каком случае следует выбрать SSL VPN вместо IPSec?** / **In which case should SSL VPN be chosen over IPSec?**
