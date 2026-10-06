# Желіні анықтау және трафикті талдау құралдары  
Инструменты обнаружения сети и анализа трафика  
Network Discovery and Traffic Analysis Tools

**Не забываем термины:**  
**Unicast** — один отправитель, один получатель (как личное сообщение).  
**Broadcast** — один отправитель, все получатели в сети (как крик «Всем!»).  
**Multicast** — один отправитель, только заранее выбранная группа получателей (как рассылка в группе).  
**Протокол** — правила общения устройств.  
**Порт** — «дверь» в устройстве для определённой службы (например, 80 — веб, 22 — SSH).

---

## CDP (Cisco Discovery Protocol)

**Қазақша:** CDP — бұл Cisco құрылғыларының көршілерін «танып-білу» құралы. Ол екінші деңгейде (канальдық деңгей) жұмыс істейді, сондықтан IP-мекенжайы жоқ құрылғылар туралы да ақпарат бере алады. Мысалы, коммутатордың аты, моделі, интерфейсі. Packet Tracer-де CDP көршілерін көру үшін: коммутаторға кіріп, `show cdp neighbors` командасын жазыңыз. Бұл желінің картасын жасауға көмектеседі. CDP әдепкі бойынша қосылып тұрады.

**Русский:** CDP — это инструмент «знакомства» между устройствами Cisco. Он работает на канальном уровне, поэтому может рассказать о соседях даже без IP-адресов. Например, имя коммутатора, модель, интерфейс. В Packet Tracer: зайдите на коммутатор и введите `show cdp neighbors`, чтобы увидеть, кто подключён. Это помогает нарисовать карту сети. CDP включён по умолчанию.

**English:** CDP is a “getting to know you” tool for Cisco devices. It works at Layer 2, so it can share info about neighbors even without IP addresses — like switch names, models, and interfaces. In Packet Tracer: enter a switch and type `show cdp neighbors` to see who is connected. This helps map the network. CDP is on by default.

---

## NetFlow

**Қазақша:** NetFlow — трафикті «ағындар» түрінде жазатын құрал. Ол әрбір қосылым туралы қысқаша жазба жасайды: кімнен, кімге, қай портқа, қанша пакет/байт кетті. Толық пакеттерді сақтамайды, тек статистика. Packet Tracer-де NetFlow коллекторын қарапайым түрде көруге болады: мысалы, firewall-ды NetFlow экспорттаушы етіп, серверде жазбаларды бақылауға болады. Напоминание: **порт** — бұл қызметтің «есігі» (80 — веб, 443 — қауіпсіз веб).

**Русский:** NetFlow — записывает трафик в виде «потоков». Для каждого соединения он делает краткую запись: откуда, куда, на какой порт, сколько пакетов/байт. Полные пакеты не сохраняет, только статистику. В Packet Tracer можно посмотреть базовые записи NetFlow: например, настроить firewall как экспортёр, а на сервере смотреть коллектор. Напоминание: **порт** — это «дверь» для службы (80 — веб, 443 — безопасный веб).

**English:** NetFlow records traffic as “flows.” For each connection it keeps a short record: who, to whom, which port, how many packets/bytes. It does not store full packets, only statistics. In Packet Tracer you can view basic NetFlow records: e.g., set a firewall as exporter and watch the collector on a server. Reminder: a **port** is a “door” for a service (80 — web, 443 — secure web).

---

## Syslog

**Қазақша:** Syslog — журнал жүргізу жүйесі. Құрылғылар оқиғалар туралы хабарламаларды бір серверге жібереді, сонда барлық логтар бір жерде жиналады. Debian-да әдепкі rsyslog қолданылады, логтар `/var/log/syslog` файлында сақталады. Мысалы, SSH-ге кіру әрекеттерін көру үшін: `grep "Failed password" /var/log/auth.log`. Syslog әдетте UDP 514 портын пайдаланады. Напоминание: **UDP** — жылдам, бірақ жеткізілуіне кепілдік жоқ протокол.

**Русский:** Syslog — система ведения журналов. Устройства отправляют сообщения о событиях на один сервер, и все логи собираются в одном месте. В Debian по умолчанию используется rsyslog, логи хранятся в `/var/log/syslog`. Например, чтобы увидеть неудачные попытки SSH: `grep "Failed password" /var/log/auth.log`. Syslog обычно использует UDP-порт 514. Напоминание: **UDP** — быстрый, но без гарантии доставки.

**English:** Syslog is a logging system. Devices send event messages to one server, so all logs are in one place. On Debian, rsyslog is default, logs are in `/var/log/syslog`. For example, to see failed SSH logins: `grep "Failed password" /var/log/auth.log`. Syslog usually uses UDP port 514. Reminder: **UDP** is fast but does not guarantee delivery.

---

## SNMP (Simple Network Management Protocol)

**Қазақша:** SNMP — желі құрылғыларын қашықтан бақылау және басқару протоколы. Әр құрылғыда SNMP-агент жұмыс істейді, ол менеджерден сұраныстарды (GetRequest) қабылдап, жауап береді. Құрылғы өздігінен «Trap» хабарламасын жібере алады. Packet Tracer-де PC-де MIB Browser ашып, коммутатордың ақпаратын сұрауға болады. Напоминание: **MIB** — бұл құрылғы туралы ақпараттың дерекқоры, **OID** — сол дерекқордағы нақты бір нысанның мекенжайы.

**Русский:** SNMP — протокол удалённого мониторинга и управления сетевыми устройствами. На каждом устройстве работает SNMP-агент, который принимает запросы от менеджера (GetRequest) и отвечает. Устройство может само отправить «Trap»-сообщение. В Packet Tracer на PC можно открыть MIB Browser и запросить информацию у коммутатора. Напоминание: **MIB** — это база данных об устройстве, **OID** — адрес конкретного объекта в этой базе.

**English:** SNMP is a protocol for remote monitoring and management of network devices. Each device runs an SNMP agent that accepts requests from a manager (GetRequest) and replies. The device can also send a “Trap” notification on its own. In Packet Tracer, open MIB Browser on a PC and query a switch. Reminder: **MIB** is a database about the device, **OID** is the address of a specific object in that database.

---

## tcpdump

**Қазақша:** tcpdump — Linux-те пакеттерді нақты уақытта ұстайтын командалық жол құралы. Debian-да орнату: `sudo apt install tcpdump`. Мысалы, eth0 интерфейсіндегі трафикті көру: `sudo tcpdump -i eth0`. Тек 80-портты сүзу: `sudo tcpdump -nn port 80`. Нәтижені файлға сақтап, Wireshark-та ашуға болады: `-w capture.pcap`. Напоминание: **интерфейс** — құрылғының желіге қосылатын «терезесі» (мысалы, eth0, wlan0).

**Русский:** tcpdump — консольный инструмент для захвата пакетов в Linux в реальном времени. Установка в Debian: `sudo apt install tcpdump`. Например, смотреть трафик на интерфейсе eth0: `sudo tcpdump -i eth0`. Только порт 80: `sudo tcpdump -nn port 80`. Можно сохранить в файл и открыть в Wireshark: `-w capture.pcap`. Напоминание: **интерфейс** — это «окно» устройства в сеть (например, eth0, wlan0).

**English:** tcpdump is a command-line tool that captures packets in real time on Linux. Install on Debian: `sudo apt install tcpdump`. For example, watch traffic on eth0: `sudo tcpdump -i eth0`. Only port 80: `sudo tcpdump -nn port 80`. You can save to a file and open in Wireshark: `-w capture.pcap`. Reminder: an **interface** is a device’s “window” to the network (e.g., eth0, wlan0).

---

## Нәтижелерді интерпретациялау / Интерпретация результатов / Interpreting Results

**Қазақша:** CDP шығысында көршінің аты, интерфейсі, платформасы көрсетіледі. NetFlow жазбасында дереккөз және мақсат IP, порттар, пакет саны болады — қай қосылым көп трафик тудырып тұрғанын көруге болады. Syslog-та уақыт белгісі, құрылғы аты, оқиға сипаттамасы бар. SNMP-те OID мәнін сұрап, құрылғының сипаттамасын, жүктемесін білуге болады. tcpdump шығысында уақыт, IP, порт, протокол, жалаушалар (SYN, ACK) көрсетіледі. Мысалы, `Flags [S]` — қосылым бастау әрекеті.

**Русский:** В выводе CDP видны имя соседа, интерфейс, платформа. В записи NetFlow — IP-адреса источника и назначения, порты, количество пакетов — можно увидеть, какое соединение создаёт много трафика. В Syslog — время, имя устройства, описание события. В SNMP можно запросить значение OID и узнать описание устройства, нагрузку. В выводе tcpdump — время, IP, порт, протокол, флаги (SYN, ACK). Например, `Flags [S]` — попытка начать соединение.

**English:** CDP output shows neighbor name, interface, platform. NetFlow records show source/destination IP, ports, packet count — you can see which connection generates a lot of traffic. Syslog shows time, device name, event description. In SNMP you can query an OID value to learn device description or load. tcpdump output shows time, IP, port, protocol, flags (SYN, ACK). For example, `Flags [S]` means a connection attempt.

---

## Бақылау сұрақтары / Контрольные вопросы / Review Questions

**Қазақша:**
1. CDP қай деңгейде жұмыс істейді және оның басты артықшылығы неде?
2. NetFlow толық пакеттерді сақтай ма, әлде тек статистика ма?
3. Debian-да syslog логтары қай файлда сақталады?
4. SNMP-те MIB және OID дегеніміз не?
5. tcpdump көмегімен тек 22-портты қалай сүзуге болады?
6. Multicast пен broadcast айырмашылығы неде?

**Русский:**
1. На каком уровне работает CDP и в чём его главное преимущество?
2. NetFlow хранит полные пакеты или только статистику?
3. В каком файле хранятся логи syslog в Debian?
4. Что такое MIB и OID в SNMP?
5. Как с помощью tcpdump отфильтровать только порт 22?
6. В чём разница между multicast и broadcast?

**English:**
1. At which layer does CDP work and what is its main advantage?
2. Does NetFlow store full packets or only statistics?
3. In which file are syslog logs stored on Debian?
4. What are MIB and OID in SNMP?
5. How do you filter only port 22 using tcpdump?
6. What is the difference between multicast and broadcast?
