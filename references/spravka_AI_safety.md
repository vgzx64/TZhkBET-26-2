# Сабақ жоспары / План урока / Lesson Plan

**КМ 1 «Ақпараттық коммуникациялық жүйелердің желілік құрылғыларын орнату процесін басқару»**

**КМ 1 «Управление процессом установки сетевых устройств информационно-коммуникационных систем»**

**КМ 1 “Management of Network Device Installation Processes in Information and Communication Systems”**

---

## 1. Сабақтың мақсаттары / Цели урока / Lesson Objectives

**ҚАЗАҚША:**

Осы сабақтың соңында студенттер:

1. **Түсіндіре алады** желілік инфрақұрылымды басқаруда қолданылатын AI қауіпсіздігінің негізгі қағидаттарын.
2. **Анықтай алады** желілік құрылғыларды орнату кезінде қорғауды қажет ететін дербес деректер (PII) мен корпоративтік деректер санаттарын.
3. **Қолдана алады** практикалық желілік сценарийлерде дербес деректерді қорғау және деректерді локализациялау бойынша Қазақстан заңнамасының талаптарын.
4. **Конфигурациялай алады** AI жүйелеріне деректердің таралуын болдырмау үшін желілік құрылғылардағы қарапайым қорғаныс шараларын (шифрлау, қатынауды бақылау, PII маскировкалау).
5. **Дұрыс әрекет ете алады** Қазақстан заңнамасына сәйкес деректер қауіпсіздігі оқиғаларына жауап беру кезінде.

**РУССКИЙ:**

К концу данного урока студенты смогут:

1. **Объяснять** основные принципы безопасности ИИ применительно к управлению сетевой инфраструктурой.
2. **Определять** категории персональных данных (PII) и корпоративных данных, требующих защиты при установке сетевых устройств.
3. **Применять** требования законодательства Казахстана о защите персональных данных и локализации данных в практических сетевых сценариях.
4. **Настраивать** базовые средства защиты (шифрование, контроль доступа, маскирование PII) на сетевых устройствах для предотвращения утечки данных в системы ИИ.
5. **Реагировать** надлежащим образом на инциденты информационной безопасности в соответствии с законодательством Казахстана.

**ENGLISH:**

By the end of this lesson, students will be able to:

1. **Explain** the core principles of AI safety as they apply to network infrastructure management.
2. **Identify** categories of Personally Identifiable Information (PII) and corporate data that require protection during network device installation.
3. **Apply** Kazakhstan‘s legal requirements for personal data protection and data localization in practical network scenarios.
4. **Configure** basic safeguards (encryption, access control, PII masking) on network devices to prevent data leakage to AI systems.
5. **Respond** appropriately to data security incidents in compliance with Kazakhstani law.

---

## 2. Негізгі заңнамалық база / Ключевая нормативная база / Key Legal Framework

### 2.1 «Дербес деректер және оларды қорғау туралы» ҚР Заңы (2013 жылғы 21 мамырдағы № 94-V, 30.12.2025 ж. өзгертулермен)

**ҚАЗАҚША:**

Қазақстандағы дербес деректерді өңдеуді реттейтін негізгі заң. Желілік мамандарға қатысты негізгі ережелер:

- **Дербес деректердің анықтамасы:** «дербес деректер субъектісі туралы бір немесе бірнеше дербес деректер идентификаторымен толықтырылған мәліметтер немесе мәліметтер жиынтығы». Бұл анықтама әдейі кең – сәйкестендіруге мүмкіндік беретін кез келген ақпарат (аты-жөні, ЖСН, мүліктік жағдайы, транзакция деректері) қорғауға жатады.
- **Деректерді локализациялау (12.2-бап):** Дербес деректер меншік иесі және/немесе оператор, сондай-ақ үшінші тұлғалар тарапынан **Қазақстан Республикасының аумағында орналасқан дерекқорда** сақталуы тиіс. Бұл PII өңдейтін бұлттық қызметтерді немесе AI құралдарын таңдау кезінде маңызды шектеу болып табылады.
- **Дербес деректер субъектісінің құқықтары:** Азаматтар деректерді жоюды немесе өңдеуді тоқтатуды талап етуге құқылы («цифрлық ұмытылу» құқығы), ал компаниялар сұраныс бойынша орындауға міндетті болады.

**РУССКИЙ:**

Основополагающий закон, регулирующий обработку персональных данных в Казахстане. Ключевые положения, актуальные для сетевых специалистов:

- **Определение персональных данных:** «сведения или совокупность сведений о субъекте персональных данных, дополненные одним или несколькими идентификаторами персональных данных». Это определение намеренно широкое — любая информация, позволяющая идентифицировать личность (ФИО, ИИН, имущественное положение, данные транзакций), подлежит защите.
- **Локализация данных (статья 12.2):** Персональные данные должны храниться собственником и/или оператором, а также третьими лицами в базе данных, **расположенной на территории Республики Казахстан**. Это критическое ограничение при выборе облачных сервисов или инструментов ИИ, обрабатывающих PII.
- **Права субъекта персональных данных:** Граждане вправе требовать удаления или прекращения обработки данных (право на «цифровое забвение»), а компании обязаны исполнить запрос.

**ENGLISH:**

The foundational law governing personal data processing in Kazakhstan. Key provisions relevant to network professionals:

- **Definition of Personal Data:** “information or a set of information about a personal data subject, supplemented by one or more personal data identifiers”. This definition is deliberately broad—any information allowing identification (name, IIN, property status, transaction details) falls under protection.
- **Data Localization (Article 12.2):** Personal data must be stored by the owner and/or operator, as well as third parties, in a database **located within the territory of the Republic of Kazakhstan**. This is a critical constraint when selecting cloud services or AI tools that process PII.
- **Data Subject Rights:** Individuals have the right to demand deletion or cessation of processing (the “right to digital forgetting”), and companies are obliged to comply upon request.

### 2.2 «Жасанды интеллект туралы» ҚР Заңы (2025 жылғы 17 қарашадағы № 230-VIII ҚРЗ)

**ҚАЗАҚША:**

Қазақстан Орталық Азияда бірінші болып арнайы AI заңын қабылдады, ол **2026 жылғы 18 қаңтарда** күшіне енді. Негізгі ережелер:

- **Жекелеген AI жүйелеріне тыйым салу:** Қазақстан аумағында белгілі бір мүмкіндіктері бар AI жүйелерін жасауға және пайдалануға тыйым салынады.
- **AI контентін таңбалау:** AI өндірген контентті машина оқи алатын форматта таңбалау талап етіледі.
- **Шетелдік бұлттық LLM-ге шектеулер:** Стандартты API арқылы шикі клиенттік деректерді шетелдік жария бұлттарға (ChatGPT, Claude және т.б.) тікелей жіберу локализация режиміне сәйкес заңды түрде мүмкін емес. Шетелдік AI провайдерлерінің серверлері Қазақстанның заңдық юрисдикциясынан тыс жатыр.

**РУССКИЙ:**

Казахстан стал первой страной в Центральной Азии, принявшей специальный закон об ИИ, который вступил в силу **18 января 2026 года**. Ключевые положения:

- **Запрет отдельных систем ИИ:** На территории Казахстана запрещается создание и эксплуатация систем ИИ с определёнными возможностями.
- **Маркировка ИИ-контента:** Требуется маркировка контента, созданного ИИ, в машиночитаемом формате.
- **Ограничения на зарубежные облачные LLM:** Прямая передача сырых клиентских данных в зарубежные публичные облака (ChatGPT, Claude и др.) через стандартные API юридически невозможна в рамках режима локализации. Серверы зарубежных провайдеров ИИ находятся вне правовой юрисдикции Казахстана.

**ENGLISH:**

Kazakhstan became the first Central Asian country to adopt a dedicated AI law, which entered into force on **18 January 2026**. Key points:

- **Prohibition on Certain AI Systems:** Bans the creation and operation of AI systems with specific capabilities on Kazakhstani territory.
- **AI Content Labeling:** Requires machine-readable labeling of AI-generated content.
- **Restrictions on Foreign Cloud LLMs:** Direct transmission of raw client data to foreign public clouds (ChatGPT, Claude, etc.) via standard APIs is legally impossible under the localization regime. Foreign AI providers’ servers lie outside Kazakhstan‘s legal jurisdiction.

### 2.3 Соңғы өзгертулер (2026 жылғы 24 маусымдағы № 326-VIII Заңы, 25.08.2026 ж. күшіне енді)

**ҚАЗАҚША:**

- **Контроллерлерді тәуекелге негізделген жіктеу:**
  - **Шағын:** ≤ 10 000 бірегей деректер субъектілері
  - **Орташа:** 10 000–500 000 деректер субъектілері
  - **Ірі:** ≥ 500 000 деректер субъектілері
- **Жаңа техникалық талаптар:** Деректердің тұтастығын бақылау құралдарын пайдалану; дербес деректерді қауіпсіз арналар арқылы немесе шифрлаумен жіберу; криптографиялық қорғаныс құралдарын пайдаланып сақтау; пайдаланушыны сәйкестендіру/аутентификациялау (100 000-нан астам жазбасы бар дерекқорлар үшін биометриялық аутентификация); ақпараттық қауіпсіздік құралдарын уақтылы жаңарту және дерекқор оқиғаларының журналдарын жүргізу.
- **Жаңа мемлекеттік тізілімдер:** Дербес деректер қауіпсіздігінің бұзылу тізілімі; Дербес деректерді жинайтын/өңдейтін тұлғалар тізілімі.

**РУССКИЙ:**

- **Классификация контроллеров на основе рисков:**
  - **Малые:** ≤ 10 000 уникальных субъектов данных
  - **Средние:** 10 000–500 000 субъектов данных
  - **Крупные:** ≥ 500 000 субъектов данных
- **Новые технические требования:** Использование средств контроля целостности данных; передача персональных данных по защищённым каналам или с шифрованием; хранение с использованием криптографических средств защиты; идентификация/аутентификация пользователей (биометрическая аутентификация для баз данных >100 000 записей); своевременное обновление средств информационной безопасности и ведение журналов событий баз данных.
- **Новые государственные реестры:** Реестр нарушений безопасности персональных данных; Реестр лиц, собирающих/обрабатывающих персональные данные.

**ENGLISH:**

- **Risk-Based Classification of Controllers:**
  - **Small:** ≤ 10,000 unique data subjects
  - **Medium:** 10,000–500,000 data subjects
  - **Large:** ≥ 500,000 data subjects
- **New Technical Requirements:** Use of data integrity control tools; transmission of personal data through secure channels or with encryption; storage using cryptographic protection measures; user identification/authentication (biometric authentication for databases >100,000 records); timely updating of information security tools and maintenance of database event logs.
- **New State Registers:** Personal Data Security Breach Register; Register of Persons Collecting/Processing Personal Data.

### 2.4 Жауапкершілік / Ответственность / Liability

**ҚАЗАҚША:** Дербес деректер заңнамасын бұзғаны үшін әкімшілік айыппұлдар **2 000 АЕК**-ке дейін жетуі мүмкін; киберқауіпсіздік талаптарын бұзу **1 000 АЕК**-ке дейінгі айыппұлдарға әкелуі мүмкін.

**РУССКИЙ:** Административные штрафы за нарушения законодательства о персональных данных могут достигать **2 000 МРП**; нарушения требований кибербезопасности могут повлечь штрафы до **1 000 МРП**.

**ENGLISH:** Administrative fines for violations of personal data legislation may reach **2,000 MCI**; violations of cybersecurity requirements may result in fines up to **1,000 MCI**.

---

## 3. Желілік инфрақұрылым үшін AI қауіпсіздігі тәуекелдері / Риски безопасности ИИ для сетевой инфраструктуры / AI Safety Risks for Network Infrastructure

**ҚАЗАҚША:**

Желілік құрылғыларды орнату және басқару кезінде AI құралдары (кіріктірілген және сыртқы) нақты тәуекелдерді тудырады:

| Тәуекел санаты | Сипаттамасы | Желілік контекстегі мысал |
|---|---|---|
| **Деректердің таралуы** | PII немесе корпоративтік конфигурация деректерінің сыртқы AI қызметтеріне жіберілуі | Клиент IP-лері мен хост атауларын қамтитын конфигурация файлдарын ChatGPT-ке қою |
| **Prompt injection** | Зиянды кірістер AI-дың құпия деректерді ашуына әкеледі | Компромисске ұшыраған құрылғы AI мониторинг жүйесін манипуляциялайтын жазбалар жібереді |
| **Модельді улау** | Шабуылдаушылар зиянды оқу деректерін енгізеді | Компромисске ұшыраған микробағдарлама AI негізіндегі шабуылдарды анықтау моделіне жалған деректер береді |
| **Жеткізу тізбегінің тәуекелі** | Сенімсіз көздерден AI компоненттері | Ашық емес деректерді өңдеу тәжірибесі бар үшінші тарап AI плагині |
| **Деректер резиденттігін бұзу** | PII Қазақстанның заңдық периметрінен тыс өңделеді | Шетелдегі серверлері бар бұлттық AI аналитикалық қызметі клиенттік идентификациялық деректерді өңдейді |

**РУССКИЙ:**

При установке и управлении сетевыми устройствами инструменты ИИ (встроенные и внешние) создают специфические риски:

| Категория риска | Описание | Пример в сетевом контексте |
|---|---|---|
| **Утечка данных** | PII или корпоративные данные конфигурации отправляются во внешние сервисы ИИ | Вставка конфигурационных файлов с клиентскими IP и именами хостов в ChatGPT |
| **Prompt injection** | Вредоносные входные данные заставляют ИИ раскрывать конфиденциальную информацию | Скомпрометированное устройство отправляет записи журнала, манипулирующие системой мониторинга на базе ИИ |
| **Отравление модели** | Злоумышленники внедряют вредоносные обучающие данные | Скомпрометированная прошивка передаёт ложные данные модели обнаружения вторжений на базе ИИ |
| **Риск цепочки поставок** | Компоненты ИИ из ненадёжных источников | Сторонний плагин ИИ с нераскрытыми практиками обработки данных |
| **Нарушение резидентности данных** | PII обрабатываются вне правового периметра Казахстана | Облачный аналитический сервис ИИ с серверами за рубежом обрабатывает идентификационные данные клиентов |

**ENGLISH:**

When installing and managing network devices, AI tools (both embedded and external) introduce specific risks:

| Risk Category | Description | Example in Network Context |
|---|---|---|
| **Data Leakage** | PII or corporate configuration data sent to external AI services | Using ChatGPT to troubleshoot a network issue by pasting configuration files containing client IPs and hostnames |
| **Prompt Injection** | Malicious inputs cause AI to reveal sensitive data | A compromised device sends crafted log entries that manipulate an AI-based monitoring system |
| **Model Poisoning** | Attackers inject malicious training data | Compromised firmware in a network device feeds false data to an AI-based intrusion detection model |
| **Supply Chain Risk** | AI components from untrusted sources | Third-party AI plugin with undisclosed data handling practices |
| **Data Residency Violation** | PII processed outside Kazakhstan’s legal perimeter | Cloud-based AI analytics service with servers abroad processing client identification data |

**Негізгі қағидат / Ключевой принцип / Key Principle:** Your AI training infrastructure shouldn‘t trust requests from corporate networks by default. Implement least-privilege access for service accounts running AI workloads, and segment AI systems processing different PII categories into separate trust zones.

---

## 4. Желілік құрылғыларды орнату кезіндегі практикалық қорғаныс шаралары / Практические меры защиты при установке сетевых устройств / Practical Safeguards for Network Device Installation

### 4.1 PII маскировкалау және токенизация / Маскирование и токенизация PII / PII Masking and Tokenization

**ҚАЗАҚША:** Кез келген деректер (журналдар, конфигурация файлдары, құрылғы идентификаторлары) AI жүйесіне талдау үшін жіберілмес бұрын: идентификаторларды маскировкалау (нақты ЖСН, аты-жөні, телефон нөмірлері, IP мекенжайларын детерминдік жалғандарға ауыстыру); PII тазалау қорғанысын орналастыру (сұраныстар желіден шықпас бұрын сезімтал деректерді анықтайтын және редакциялайтын шлюз); клиенттік токенизацияны енгізу (деректерді жергілікті жедел жадыда өңдеу, сыртқы AI қызметтеріне тек токенизацияланған мәндерді жіберу).

**РУССКИЙ:** Прежде чем любые данные (журналы, конфигурационные файлы, идентификаторы устройств) будут отправлены в систему ИИ для анализа: маскирование идентификаторов (замена реальных ИИН, ФИО, номеров телефонов и IP-адресов на детерминированные подделки); развёртывание защиты для очистки PII (шлюз, который обнаруживает и редактирует конфиденциальные данные до того, как запросы покинут вашу сеть); внедрение клиентской токенизации (обработка данных локально в оперативной памяти, отправка только токенизированных значений во внешние сервисы ИИ).

**ENGLISH:** Before any data (logs, configuration files, device identifiers) is sent to an AI system for analysis: mask identifiers (replace real IINs, full names, phone numbers, and IP addresses with deterministic fakes); deploy a PII-scrubbing guardrail (a gateway that detects and redacts sensitive data before requests leave your network); implement client-side tokenization (process data locally in RAM, sending only tokenized values to external AI services).

### 4.2 Шифрлау және қауіпсіз арналар / Шифрование и защищённые каналы / Encryption and Secure Channels

**ҚАЗАҚША:** Жаңартылған Қағидаларға сәйкес (12.07.2026 ж. күшіне енді): барлық дербес деректерді **қауіпсіз байланыс арналары арқылы немесе шифрлаумен** жіберу; дербес деректерді **криптографиялық қорғаныс құралдарын** пайдаланып сақтау; шектеулі қолжетімділіктегі қызметтік ақпаратты сақтау үшін криптографиялық қорғанысты пайдалану.

**РУССКИЙ:** Согласно обновлённым Правилам (вступили в силу 12.07.2026): передача всех персональных данных через **защищённые каналы связи или с шифрованием**; хранение персональных данных с использованием **криптографических средств защиты**; использование криптографической защиты для хранения служебной информации ограниченного доступа.

**ENGLISH:** Per the updated Rules (effective 12 July 2026): transmit all personal data through **secure communication channels or with encryption**; store personal data using **cryptographic protection measures**; use cryptographic protection for stored restricted-access service information.

### 4.3 Желілік құрылғылардағы қатынауды бақылау / Контроль доступа на сетевых устройствах / Access Control on Network Devices

**ҚАЗАҚША:** Көп факторлы аутентификация (MFA) – PII өңдейтін немесе маршруттайтын барлық әкімшілік қатынау үшін MFA орналастыру; ең аз артықшылық қағидаты – AI жүктемелерін орындайтын қызметтік тіркелгілер минималды рұқсаттарға ие болуы тиіс; сенім аймақтарын сегменттеу – әртүрлі PII санаттарын өңдейтін AI жүйелерін оқшауланған желілік сегменттерге бөлу; физикалық портты бақылау – рұқсатсыз абоненттік құрылғыларды, модемдерді және алынбалы тасымалдағыштарды желіге қосуды болдырмау.

**РУССКИЙ:** Многофакторная аутентификация (MFA) — развернуть MFA для всего административного доступа к сетевым устройствам, особенно тем, которые обрабатывают или маршрутизируют PII; принцип наименьших привилегий — сервисные учётные записи, выполняющие рабочие нагрузки ИИ, должны иметь минимально необходимые разрешения; сегментация зон доверия — разделение систем ИИ, обрабатывающих разные категории PII, на изолированные сетевые сегменты; контроль физических портов — запрет подключения несанкционированных абонентских устройств, модемов и съёмных носителей к сети.

**ENGLISH:** Multi-factor authentication (MFA) — deploy MFA for all administrative access to network devices, especially those processing or routing PII; least-privilege access — service accounts running AI workloads should have only the minimum permissions necessary; segment trust zones — separate AI systems processing different PII categories into isolated network segments; physical port control — disallow connection of unauthorized subscriber devices, modems, and removable media to the network.

### 4.4 AI шлюзін орналастыру / Развёртывание шлюза ИИ / AI Gateway Deployment

**ҚАЗАҚША:** Желілік деректерді AI арқылы өңдеу қажет болған кез келген сценарийде: ішкі периметр мен сыртқы AI провайдерлері арасындағы шекарада **Sovereign Data Gateway** орналастыру. Бұл кез келген сұраныс Қазақстандық периметрден шықпас бұрын үш нақты уақыттағы операцияны орындайды: (1) тексеру, (2) PII редакциялау, (3) саясатты орындау. Шлюз «зияткерлік оқшаулағыш буфер» ретінде қызмет етеді, шикі клиенттік деректердің шетелдік серверлерге жетуін болдырмайды.

**РУССКИЙ:** В любом сценарии, где сетевые данные должны обрабатываться ИИ: развернуть **Sovereign Data Gateway** на границе между внутренним периметром и внешними провайдерами ИИ. Он выполняет три операции в реальном времени до того, как любой запрос покинет казахстанский периметр: (1) инспекция, (2) редактирование PII, (3) применение политики. Шлюз функционирует как «интеллектуальный изолирующий буфер», гарантируя, что сырые клиентские данные не достигнут зарубежных серверов.

**ENGLISH:** For any scenario where network data must be processed by AI: deploy a **Sovereign Data Gateway** at the boundary between your internal perimeter and external AI providers. This performs three real-time operations before any request leaves the Kazakhstani perimeter: (1) inspection, (2) PII redaction, (3) policy enforcement. The gateway functions as an “intelligent insulating buffer,” ensuring no raw client data reaches foreign servers.

### 4.5 Үздіксіз мониторинг / Непрерывный мониторинг / Continuous Monitoring

**ҚАЗАҚША:** Барлық операциялық өмірлік цикл бойына мониторинг жүргізу, тек орналастыру кезінде ғана емес. NIST Cybersecurity Framework 2.0 сәйкес үздіксіз мониторинг (DE.CM) және жағымсыз оқиғаларды талдауды (DE.AE) енгізу. Шектеулі қолжетімділіктегі дербес деректерді қамтитын операцияларды тіркеу үшін **дерекқорды басқару жүйесінің оқиға журналдарын** жүргізу.

**РУССКИЙ:** Мониторинг на протяжении всего операционного жизненного цикла, а не только во время развёртывания. Внедрение непрерывного мониторинга (DE.CM) и анализа неблагоприятных событий (DE.AE) согласно NIST Cybersecurity Framework 2.0. Ведение **журналов событий системы управления базами данных** для регистрации операций с персональными данными ограниченного доступа.

**ENGLISH:** Monitor throughout the **entire operational lifecycle**, not just during deployment. Implement continuous monitoring (DE.CM) and adverse event analysis (DE.AE) per NIST Cybersecurity Framework 2.0. Maintain **database management system event logs** to record operations involving restricted-access personal data.

---

## 5. Оқиғаларға жауап беру: заңнамалық талаптар / Реагирование на инциденты: законодательные требования / Incident Response: Legal Requirements

**ҚАЗАҚША:**

Деректер қауіпсіздігі оқиғасы орын алған кезде (мысалы, компромисске ұшыраған желілік құрылғы арқылы PII-ге рұқсатсыз қатынау):

1. Оқиға фактісін **дереу тіркеу**.
2. Әрі қарай рұқсатсыз қатынауды болдырмау үшін **қатынауды бұғаттау**.
3. Мемлекеттік органдарды **бір жұмыс күні ішінде хабардар ету**.
4. Ішкі тергеу жүргізу және нәтижелерді құжаттау.
5. Қажет болған жағдайда **Дербес деректер қауіпсіздігінің бұзылу тізіліміне** есеп беру.

**РУССКИЙ:**

При возникновении инцидента информационной безопасности (например, несанкционированный доступ к PII через скомпрометированное сетевое устройство):

1. Немедленно **зафиксировать факт** инцидента.
2. **Заблокировать доступ** для предотвращения дальнейшего несанкционированного доступа.
3. **Уведомить государственные органы в течение одного рабочего дня**.
4. Провести внутреннее расследование и задокументировать результаты.
5. При необходимости **отчитаться в Реестре нарушений безопасности персональных данных**.

**ENGLISH:**

When a data security incident occurs (e.g., unauthorized access to PII via a compromised network device):

1. **Record the fact** of the incident immediately.
2. **Block access** to prevent further unauthorized access.
3. **Notify state authorities within one business day**.
4. Conduct an internal investigation and document findings.
5. **Report to the Personal Data Security Breach Register** if applicable.

---

## 6. Сыныптағы іс-шаралар / Аудиторные мероприятия / Classroom Activities

### 6.1 PII анықтау жаттығуы / Упражнение по идентификации PII / PII Identification Exercise (15 мин)

**ҚАЗАҚША:** Студенттерге қамтитын желілік құрылғының конфигурация файлының үлгісі беріледі: жеке адамдардың есімдерін қамтитын клиент хост атаулары; нақты пәтерлерге/кеңселерге салыстырылған IP мекенжайлары; транзакция идентификаторлары бар журнал жазбалары. **Тапсырма:** Барлық PII элементтерін анықтау және AI диагностикалық құралымен бөліспес бұрын әрқайсысы үшін маскировкалау стратегияларын ұсыну.

**РУССКИЙ:** Студентам предоставляется образец конфигурационного файла сетевого устройства, содержащий: имена хостов клиентов с личными именами; IP-адреса, привязанные к конкретным квартирам/офисам; записи журнала с идентификаторами транзакций. **Задание:** Определить все элементы PII и предложить стратегии маскирования для каждого перед передачей в инструмент диагностики ИИ.

**ENGLISH:** Provide students with a sample network device configuration file containing: client hostnames that include personal names; IP addresses mapped to specific apartments/offices; log entries with transaction IDs. **Task:** Identify all PII elements and propose masking strategies for each before sharing with an AI diagnostic tool.

### 6.2 Кейс-стади — Шетелдік LLM тұзағы / Кейс-стади — Ловушка зарубежной LLM / Case Study — The Foreign LLM Trap (20 мин)

**ҚАЗАҚША:**

**Сценарий:** Қазақстандық банктің желілік инженері клиент ЖСН-дері мен транзакция уақыт белгілерін қамтитын желілік трафик журналдарын талдау үшін ChatGPT пайдаланғысы келеді. Журналдар Алматыдағы серверде сақталады.

**Талқылау сұрақтары:**
1. Осы журналдарды ChatGPT-ке жіберу арқылы қандай заңдар бұзылады?
2. Қазақстанның деректерді локализациялау талаптарына сәйкес келетін қандай балама тәсіл бар?
3. Sovereign Data Gateway бұл мәселені қалай шешеді?

**РУССКИЙ:**

**Сценарий:** Сетевой инженер казахстанского банка хочет использовать ChatGPT для анализа журналов сетевого трафика, содержащих ИИН клиентов и временные метки транзакций. Журналы хранятся на сервере в Алматы.

**Вопросы для обсуждения:**
1. Какие законы нарушаются при отправке этих журналов в ChatGPT?
2. Какой альтернативный подход соответствовал бы требованиям Казахстана о локализации данных?
3. Как Sovereign Data Gateway решил бы эту проблему?

**ENGLISH:**

**Scenario:** A network engineer at a Kazakhstani bank wants to use ChatGPT to analyze network traffic logs that contain customer IINs and transaction timestamps. The logs are stored on a server in Almaty.

**Discussion questions:**
1. What laws are violated by sending these logs to ChatGPT?
2. What alternative approach would comply with Kazakhstan’s data localization requirements?
3. How would a Sovereign Data Gateway solve this problem?

### 6.3 PII тазалау қорғанысын конфигурациялау / Настройка защиты для очистки PII / Configuring a PII-Scrubbing Guardrail (25 мин)

**ҚАЗАҚША:** Модельденген желілік ортаны (немесе қағаз жаттығуын) пайдаланып: ЖСН (12 таңбалы формат) және телефон нөмірлері үшін қарапайым regex негізіндегі PII анықтау ережесін конфигурациялау; оны үлгі журнал деректеріне қарсы тексеру; ережені құрылғының қауіпсіздік саясатында құжаттау.

**РУССКИЙ:** Используя смоделированную сетевую среду (или бумажное упражнение): настроить базовое правило обнаружения PII на основе регулярных выражений для ИИН (12-значный формат) и номеров телефонов; протестировать его на образце данных журнала; задокументировать правило в политике безопасности устройства.

**ENGLISH:** Using a simulated network environment (or paper exercise): configure a basic regex-based PII detection rule for IINs (12-digit format) and phone numbers; test it against sample log data; document the rule in the device‘s security policy.

### 6.4 Оқиғаларға жауап беру имитациясы / Имитация реагирования на инцидент / Incident Response Simulation (20 мин)

**ҚАЗАҚША:**

**Сценарий:** AI негізіндегі аномалияларды анықтау функциясы бар желілік мониторинг құрылғысы компромисске ұшырады. Шабуылдаушылар клиент атаулары, ЖСН-дері және IP мекенжайларын қамтитын 15 000 жазбаны алды.

**Тапсырма:** Уәкілетті органға хабарлама жобасын дайындау: оқиғаның сипаты; әсер еткен деректер санаттары; деректер субъектілерінің саны; қабылданған түзету шаралары; уақыт шеңбері (хабарлама бір жұмыс күні ішінде берілуі тиіс).

**РУССКИЙ:**

**Сценарий:** Сетевое устройство мониторинга с функцией обнаружения аномалий на базе ИИ было скомпрометировано. Злоумышленники извлекли 15 000 записей, содержащих имена клиентов, ИИН и IP-адреса.

**Задание:** Составить проект уведомления в уполномоченный орган, включая: характер инцидента; категории затронутых данных; количество субъектов данных; принятые меры по устранению; сроки (уведомление должно быть подано в течение одного рабочего дня).

**ENGLISH:**

**Scenario:** A network monitoring device with an AI-based anomaly detection feature has been compromised. Attackers extracted 15,000 records containing client names, IINs, and IP addresses.

**Task:** Draft the notification to the competent authority, including: nature of the incident; data categories affected; number of data subjects; remediation measures taken; timeline (notification must be within one business day).

---

## 7. Желілік мамандар үшін негізгі қорытындылар / Ключевые выводы для сетевых специалистов / Key Takeaways for Network Professionals

**ҚАЗАҚША:**

1. **Деректерді локализациялау — келісімге келмейтін талап.** PII Қазақстанда физикалық түрде орналасқан серверлерде қалуы тиіс. Шетелдік AI қызметтері шикі PII-ді өңдей алмайды.
2. **AI деректерді қорғау заңынан босатылмайды.** AI туралы заң (2025) Дербес деректер туралы заңды алмастырмайды, толықтырады. Екеуі де бір уақытта қолданылады.
3. **Жібермес бұрын маскировкалаңыз.** AI өңдеу үшін желілік периметрден шығатын кез келген деректер токенизациялануы, маскировкалануы немесе шифрлануы тиіс.
4. **Контроллер санатын біліңіз.** Шағын, орташа немесе ірі жіктеу сіздің сәйкестік міндеттемелеріңізді анықтайды.
5. **Оқиға туралы хабарлау уақытпен шектелген.** Органдарды хабардар ету үшін бір жұмыс күні — заңды максимум.
6. **Барлығын құжаттаңыз.** Оқиға журналдары, өңдеу мақсаттары және қауіпсіздік шаралары жүргізілуі және аудит үшін қолжетімді болуы тиіс.

**РУССКИЙ:**

1. **Локализация данных — неоспоримое требование.** PII должны оставаться на серверах, физически расположенных в Казахстане. Зарубежные сервисы ИИ не могут обрабатывать сырые PII.
2. **ИИ не освобождается от закона о защите данных.** Закон об ИИ (2025) дополняет, а не заменяет Закон о персональных данных. Оба применяются одновременно.
3. **Маскируйте перед отправкой.** Любые данные, покидающие ваш сетевой периметр для обработки ИИ, должны быть токенизированы, замаскированы или зашифрованы.
4. **Знайте свою категорию контроллера.** Классификация — малый, средний или крупный — определяет ваши обязательства по соблюдению.
5. **Уведомление об инциденте ограничено по времени.** Один рабочий день для уведомления органов — это законный максимум.
6. **Документируйте всё.** Журналы событий, цели обработки и меры безопасности должны вестись и быть доступными для аудита.

**ENGLISH:**

1. **Data localization is non-negotiable.** PII must remain on servers physically located in Kazakhstan. Foreign AI services cannot process raw PII.
2. **AI is not exempt from data protection law.** The AI Law (2025) complements, not replaces, the Personal Data Law. Both apply simultaneously.
3. **Mask before you send.** Any data leaving your network perimeter for AI processing must be tokenized, masked, or encrypted.
4. **Know your controller category.** Small, medium, or large classification determines your compliance obligations.
5. **Incident notification is time-bound.** One business day to notify authorities is the legal maximum.
6. **Document everything.** Event logs, processing purposes, and security measures must be maintained and available for audit.

---

## 8. Бағалау сұрақтары / Вопросы для оценки / Assessment Questions

**ҚАЗАҚША:**

1. Қазақстанда дербес деректер заңнамасын бұзғаны үшін ең жоғары айыппұл қандай?
2. Дербес деректерді шетелдік AI қызметіне қандай жағдайларда беруге болады?
3. Жаңартылған Қағидаларда шектеулі қолжетімділіктегі дербес деректерді қорғау үшін талап етілетін үш техникалық қорғаныс шарасын атаңыз.
4. Желілік құрылғы 8 000 пайдаланушының биометриялық аутентификация деректерін өңдейді. Бұл қандай контроллер санатына жатады?
5. Sovereign Data Gateway сұраныс Қазақстандық периметрден шықпас бұрын орындайтын үш операцияны сипаттаңыз.

**РУССКИЙ:**

1. Каков максимальный штраф за нарушения законодательства о персональных данных в Казахстане?
2. При каких условиях персональные данные могут быть переданы в зарубежный сервис ИИ?
3. Перечислите три технические меры защиты, требуемые обновлёнными Правилами для защиты персональных данных ограниченного доступа.
4. Сетевое устройство обрабатывает данные биометрической аутентификации 8 000 пользователей. К какой категории контроллера это относится?
5. Опишите три операции, выполняемые Sovereign Data Gateway до того, как запрос покинет казахстанский периметр.

**ENGLISH:**

1. What is the maximum fine for violations of personal data legislation in Kazakhstan?
2. Under what conditions can personal data be transferred to a foreign AI service?
3. List three technical safeguards required by the updated Rules for protecting restricted-access personal data.
4. A network device processes biometric authentication data for 8,000 users. What controller category does this fall under?
5. Describe the three operations performed by a Sovereign Data Gateway before a request leaves the Kazakhstani perimeter.

---

## 9. Ұсынылатын әдебиеттер / Рекомендуемая литература / Recommended Reading

**ҚАЗАҚША:**

- «Дербес деректер және оларды қорғау туралы» Қазақстан Республикасының Заңы № 94-V (21.05.2013, 30.12.2025 ж. өзгертулермен)
- «Жасанды интеллект туралы» Қазақстан Республикасының Заңы № 230-VIII ҚРЗ (17.11.2025)
- № 326-VIII Заңы «Цифрландыру, дербес деректерді қорғау... жөніндегі кейбір заңнамалық актілерге өзгертулер мен толықтырулар енгізу туралы» (24.06.2026)
- Дербес деректерді қорғау жөніндегі іс-шараларды іске асыру қағидалары (AI және цифрлық даму министрінің бұйрығы, 12.07.2026 ж. күшіне енді)

**РУССКИЙ:**

- Закон Республики Казахстан «О персональных данных и их защите» № 94-V (21.05.2013, с изм. 30.12.2025)
- Закон Республики Казахстан «Об искусственном интеллекте» № 230-VIII ЗРК (17.11.2025)
- Закон № 326-VIII «О внесении изменений и дополнений в некоторые законодательные акты по вопросам цифровизации, защиты персональных данных...» (24.06.2026)
- Правила реализации мер по защите персональных данных (Приказ Министра ИИ и цифрового развития, в силе с 12.07.2026)

**ENGLISH:**

- Law of the Republic of Kazakhstan “On Personal Data and Their Protection” No. 94-V (21.05.2013, as amended 30.12.2025)
- Law of the Republic of Kazakhstan “On Artificial Intelligence” No. 230-VIII ЗРК (17.11.2025)
- Law No. 326-VIII “On Amendments and Additions to Certain Legislative Acts on Digitalization, Personal Data Protection...” (24.06.2026)
- Rules for the Implementation of Personal Data Protection Measures (Order of the Minister of AI and Digital Development, effective 12.07.2026)
