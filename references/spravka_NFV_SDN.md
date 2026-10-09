# NFV және SDN: студенттерге үш тілдегі справка  
**КМ 1 «Ақпараттық коммуникациялық жүйелердің желілік құрылғыларын орнату процесін басқару»**

---

## 1. Негізгі ұғымдар / Основные понятия / Basic concepts

| Қазақша | Русский | English |
|---|---|---|
| **SDN** – басқару жазықтығын деректер жазықтығынан бөлетін, орталықтандырылған бағдарламалық контроллер арқылы желіні бағдарламалайтын архитектура. | **SDN** – архитектура, отделяющая плоскость управления от плоскости данных и программирующая сеть через централизованный программный контроллер. | **SDN** – architecture that separates control plane from data plane and programs the network via a centralized software controller. |
| **NFV** – желілік функцияларды арнайы құрылғылардан бөліп, стандартты серверлерде VM/контейнер түрінде іске қосу. | **NFV** – отделение сетевых функций от специализированного оборудования и запуск их как ПО на стандартных серверах в VM/контейнерах. | **NFV** – decoupling network functions from proprietary hardware and running them as software on commodity servers in VMs/containers. |
| **VNF** – виртуалды желілік функция: vRouter, vFW, vNAT, vEPC. | **VNF** – виртуальная сетевая функция. | **VNF** – virtual network function. |
| **NFVI** – VNF іске қосылатын виртуалды инфрақұрылым: есептеу, сақтау, желі. | **NFVI** – инфраструктура NFV. | **NFVI** – NFV infrastructure. |
| **MANO** – NFV басқару және оркестрациялау: NFVO, VNFM, VIM. | **MANO** – управление и оркестрация NFV: NFVO, VNFM, VIM. | **MANO** – NFV management and orchestration: NFVO, VNFM, VIM. |
| **Контроллер** – желі күйін жинақтап, құрылғыларға ережелерді беретін бағдарламалық компонент. | **Контроллер** – программный компонент, собирающий состояние сети и задающий правила устройствам. | **Controller** – software component that maintains network state and programs rules on devices. |

---

## 2. SDN мен NFV салыстыру / Сравнение SDN и NFV / SDN vs NFV

| Критерий / Критерий / Criterion | SDN | NFV |
|---|---|---|
| Мақсаты / Цель / Purpose | Басқаруды деректерден бөлу, орталықтандырылған бағдарламалау / Разделение управления и данных, централизованное программирование / Separate control/data, centralized programmability | Желілік функцияларды аппараттан бөлу, виртуалдау / Отделение сетевых функций от оборудования, виртуализация / Decouple functions from hardware, virtualize |
| Не виртуалданады? / Что виртуализуется? / What is virtualized? | Басқару/тасымалдау абстракциясы / Абстракция управления/пересылки / Control/forwarding abstraction | Желілік функциялар / Сетевые функции / Network functions |
| Негізгі компоненттер / Ключевые компоненты / Key components | Контроллер, northbound/southbound API, OpenFlow, NETCONF | VNF, NFVI, MANO, NFVO, VNFM, VIM |
| Орналасуы / Где работает / Location | Басқару жазықтығы / Плоскость управления / Control plane | Қызметтер қабаты, VM/контейнер / Сервисный слой, VM/контейнер / Service layer, VMs/containers |
| Стандарттар / Стандарты / Standards | ONF, IETF, OpenFlow, P4 | ETSI NFV, TOSCA, YANG |
| Қолданылуы / Применение / Use cases | Дата-орталық, SD-WAN, кампус, WAN | vCPE, vEPC, vFW, 5G core, SD-WAN |
| Байланысы / Связь / Relation | NFV-сіз де жұмыс істей алады / Может работать без NFV / Can work without NFV | SDN-сіз де жұмыс істей алады / Может работать без SDN / Can work without SDN |
| Бірге / Вместе / Together | SDN контроллері VNF ретінде орналасуы мүмкін; NFVI SDN арқылы басқарылады / SDN-контроллер может быть VNF; NFVI управляется через SDN / SDN controller can be a VNF; NFVI managed by SDN |

---

## 3. Контроллерге негізделген басқару тәсілдері / Подходы к управлению на основе контроллера / Controller-based management approaches

| Тәсіл / Подход / Approach | Сипаттама / Описание / Description | Артықшылықтары / Преимущества / Advantages | Шектеулері / Ограничения / Limitations | Мысалдар / Примеры / Examples |
|---|---|---|---|---|
| Орталықтандырылған / Централизованный / Centralized | Бір контроллер бүкіл желіні басқарады. | Қарапайым, біртұтас саясат. | Ақау нүктесі, масштаб шектеулі. | Ryu, POX, Floodlight |
| Таратылған/кластерлік / Распределённый/кластерный / Distributed/clustered | Бірнеше контроллер күйді бөліседі. | Масштабталу, жоғары қолжетімділік. | Күйді үйлестіру күрделі. | ONOS, OpenDaylight |
| Иерархиялық / Иерархический / Hierarchical | Ата-аналық және бала контроллерлер. | Көп домен, жүктемені бөлу. | Күрделі басқару. | SDN WAN, multi-domain |
| Федеративті/гибридті / Федеративный/гибридный / Federated/hybrid | Тәуелсіз домендер east-west API арқылы байланысады. | Провайдераралық, икемділік. | Саясатты үйлестіру қиын. | Multi-provider, ONAP |
| Ниетке негізделген / На основе намерений / Intent-based | Контроллер «не істеу керек» деген ниетті конфигурацияға айналдырады. | Автоматтандыру, қарапайым басқару. | Дәлдік, assurance қажет. | Cisco DNA, ONOS Intent |
| Декларативті/DevOps / Декларативный/DevOps / Declarative/DevOps | NETCONF/YANG, Ansible, TOSCA арқылы конфигурацияны код ретінде басқару. | Нұсқалау, қайталану. | Құралдарды білу қажет. | Ansible, NETCONF/YANG |

---

## 4. Орнату процесін басқаруға әсері / Влияние на управление процессом установки / Impact on installation management

- **ZTP (Zero-Touch Provisioning):** құрылғы қосылғанда автоматты түрде DHCP/TFTP/HTTP арқылы бастапқы конфигурация алады. / Устройство при подключении автоматически получает начальную конфигурацию через DHCP/TFTP/HTTP. / Device automatically gets initial config via DHCP/TFTP/HTTP.
- **NETCONF/YANG:** контроллер құрылғы конфигурациясын модель бойынша тексереді және орнатады. / Контроллер проверяет и задаёт конфигурацию по модели. / Controller validates and sets config by model.
- **VNF lifecycle:** instantiate, scale, heal, terminate — MANO арқылы. / VNF lifecycle через MANO. / VNF lifecycle via MANO.
- **Автоматтандыру:** Ansible, Python, TOSCA, CI/CD. / Автоматизация: Ansible, Python, TOSCA, CI/CD. / Automation: Ansible, Python, TOSCA, CI/CD.
- **Мониторинг:** Telemetry, SNMP, gNMI, Prometheus. / Мониторинг: Telemetry, SNMP, gNMI, Prometheus. / Monitoring: Telemetry, SNMP, gNMI, Prometheus.
- **Қауіпсіздік:** RBAC, TLS, secure boot, аудит. / Безопасность: RBAC, TLS, secure boot, аудит. / Security: RBAC, TLS, secure boot, audit.

---

## 5. Глоссарий / Глоссарий / Glossary

| Қазақша | Русский | English |
|---|---|---|
| Басқару жазықтығы | Плоскость управления | Control plane |
| Деректер жазықтығы | Плоскость данных | Data plane |
| Солтүстік API | Northbound API | Northbound API |
| Оңтүстік API | Southbound API | Southbound API |
| Оркестрациялау | Оркестрация | Orchestration |
| Виртуалды желілік функция | Виртуальная сетевая функция | Virtual network function |
| NFV инфрақұрылымы | Инфраструктура NFV | NFV infrastructure |
| Басқару және оркестрация | Управление и оркестрация | Management and orchestration |
| NFV оркестраторы | Оркестратор NFV | NFV orchestrator |
| VNF менеджері | Менеджер VNF | VNF manager |
| Виртуалды инфрақұрылым менеджері | Менеджер виртуальной инфраструктуры | Virtual infrastructure manager |
| Нөлдік конфигурациямен орнату | Zero-touch provisioning | Zero-touch provisioning |
| Ниетке негізделген желілеу | Сеть на основе намерений | Intent-based networking |

---

## 6. Бақылау сұрақтары / Контрольные вопросы / Control questions

1. SDN мен NFV айырмашылығы неде? / В чём разница между SDN и NFV? / What is the difference between SDN and NFV?
2. SDN контроллерінің рөлі қандай? / Какова роль SDN-контроллера? / What is the role of an SDN controller?
3. Орталықтандырылған және таратылған контроллерлерді салыстырыңыз. / Сравните централизованный и распределённый контроллеры. / Compare centralized and distributed controllers.
4. ZTP орнату процесін қалай жеңілдетеді? / Как ZTP упрощает установку? / How does ZTP simplify installation?
5. NFV MANO компоненттерін атаңыз. / Назовите компоненты NFV MANO. / Name NFV MANO components.
6. SDN және NFV бірге қалай қолданылады? / Как SDN и NFV используются вместе? / How are SDN and NFV used together?
