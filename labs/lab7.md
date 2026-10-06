# Backup Strategies and Tools Lab: rsync, Windows Server Backup, and Script-Based Backup

# Резервтік көшіру стратегиялары мен құралдары: rsync, Windows Server Backup және скрипттік резервтік көшіру

# Лабораторная работа по стратегиям и инструментам резервного копирования: rsync, Windows Server Backup и скриптовое резервное копирование


## 1. Learning Objectives / Оқу мақсаттары / Цели обучения

### English

After completing this lab, students will be able to:

- **Analyze backup strategies and use Windows Server Backup, rsync, and script-based backup tools.**

- Explain and compare backup strategies: full, incremental, differential, and synthetic full.

- Configure and use **rsync** for Linux-to-Linux backups.

- Install and configure **Windows Server Backup** on Windows Server.

- Write **script-based backups** using Bash and PowerShell.

- Schedule automatic backups using `cron` (Linux) and Task Scheduler (Windows).

- Verify backups and perform a test restore.

### Қазақша

Осы зертханалық жұмысты орындағаннан кейін студенттер:

- **Резервтік көшіру стратегияларын талдайды және Windows Server Backup, rsync, script-based backup құралдарын қолданады.**

- Резервтік көшіру стратегияларын түсіндіріп, салыстыра алады: толық, инкременттік, дифференциалдық және синтетикалық толық.

- Linux-тен Linux-ке резервтік көшіру үшін **rsync**-ты конфигурациялайды және қолданады.

- Windows Server-де **Windows Server Backup** орнатады және конфигурациялайды.

- Bash және PowerShell көмегімен **скрипттік резервтік көшіруді** жазады.

- `cron` (Linux) және Task Scheduler (Windows) көмегімен автоматты резервтік көшіруді жоспарлайды.

- Резервтік көшірмелерді тексереді және сынақтық қалпына келтіруді орындайды.

### Русский

После выполнения этой лабораторной работы студенты смогут:

- **Анализировать стратегии резервного копирования и использовать Windows Server Backup, rsync, script-based backup.**

- Объяснять и сравнивать стратегии резервного копирования: полное, инкрементное, дифференциальное и синтетическое полное.

- Настраивать и использовать **rsync** для резервного копирования Linux-to-Linux.

- Устанавливать и настраивать **Windows Server Backup** на Windows Server.

- Писать **скриптовые резервные копии** с помощью Bash и PowerShell.

- Планировать автоматическое резервное копирование с помощью `cron` (Linux) и Task Scheduler (Windows).

- Проверять резервные копии и выполнять тестовое восстановление.


## 2. Lab Topology / Зертханалық топология / Топология лабораторной работы

```
+----------------+          +----------------+          +----------------+
|  Linux Source  |          |  Linux Backup  |          | Windows Server |
|  (Ubuntu 22.04)|  rsync   |  (Ubuntu 22.04)|          |  (2019/2022)   |
|  192.168.1.10  | -------> |  192.168.1.20  | <------- |  192.168.1.30  |
|  /data         |          |  /backup       |  SMB     |  C:\Shares     |
+----------------+          +----------------+          +----------------+
        |                           |                           |
        |                           |                           |
        +---------------------------+---------------------------+
                    Backup Network 192.168.1.0/24
```

### English

- **Linux Source:** Contains data to back up (`/data`).

- **Linux Backup:** Stores rsync backups (`/backup`).

- **Windows Server:** Hosts file shares (`C:\Shares`) and will use Windows Server Backup to back up to a network share on Linux Backup.

### Қазақша

- **Linux Source:** Резервтік көшіруге арналған деректерді қамтиды (`/data`).

- **Linux Backup:** rsync резервтік көшірмелерін сақтайды (`/backup`).

- **Windows Server:** Файлдық ресурстарды орналастырады (`C:\Shares`) және Windows Server Backup көмегімен Linux Backup-тағы желілік ресурсқа резервтік көшіруді орындайды.

### Русский

- **Linux Source:** Содержит данные для резервного копирования (`/data`).

- **Linux Backup:** Хранит резервные копии rsync (`/backup`).

- **Windows Server:** Размещает файловые ресурсы (`C:\Shares`) и использует Windows Server Backup для резервного копирования на сетевой ресурс на Linux Backup.


## 3. Part A: Backup Strategies Overview / A бөлімі: Резервтік көшіру стратегияларына шолу / Часть A: Обзор стратегий резервного копирования

| Strategy / Стратегия / Стратегия | Description / Сипаттамасы / Описание | Pros / Артықшылықтары / Плюсы | Cons / Кемшіліктері / Минусы |
| - | - | - | - |
| **Full** / **Толық** / **Полное** | Copies all selected data every time. / Әр жолы барлық таңдалған деректерді көшіреді. / Копирует все выбранные данные каждый раз. | Simple restore, self-contained. / Қарапайым қалпына келтіру. / Простое восстановление. | Slow, high storage. / Баяу, көп орын қажет. / Медленно, много места. |
| **Incremental** / **Инкременттік** / **Инкрементное** | Copies only changes since last backup (any type). / Соңғы көшірмеден кейінгі өзгерістерді ғана көшіреді. / Копирует только изменения с момента последней копии. | Fast, low storage. / Жылдам, аз орын. / Быстро, мало места. | Restore requires last full + all incrementals. / Қалпына келтіру толық + барлық инкременттерді қажет етеді. / Восстановление требует полной + всех инкрементов. |
| **Differential** / **Дифференциалдық** / **Дифференциальное** | Copies changes since last full backup. / Соңғы толық көшірмеден кейінгі өзгерістерді көшіреді. / Копирует изменения с момента последней полной копии. | Faster restore than incremental. / Инкременттен жылдам қалпына келтіру. / Быстрее восстановление, чем инкрементное. | Grows until next full. / Келесі толық көшірмеге дейін өседі. / Растет до следующего полного. |
| **Synthetic Full** / **Синтетикалық толық** / **Синтетическое полное** | Creates a full backup from previous full + incrementals. / Алдыңғы толық + инкременттерден толық көшірме жасайды. / Создает полную копию из предыдущей полной + инкрементов. | Fast backup, easy restore. / Жылдам көшіру, оңай қалпына келтіру. / Быстрое копирование, простое восстановление. | Requires backup software support. / Бағдарламалық қолдауды қажет етеді. / Требует поддержки ПО. |


**3-2-1 Rule / 3-2-1 ережесі / Правило 3-2-1:**  
Keep at least 3 copies of data, on 2 different media, with 1 offsite.  
Деректердің кемінде 3 көшірмесін, 2 түрлі тасымалдаушыда, 1-і сыртта сақтаңыз.  
Храните минимум 3 копии данных, на 2 разных носителях, 1 — за пределами площадки.


## 4. Part B: rsync for Linux Backups / B бөлімі: Linux резервтік көшіру үшін rsync / Часть B: rsync для резервного копирования Linux

### 4.1 Install rsync / rsync орнату / Установка rsync

On both Linux VMs / Екі Linux VM-де / На обеих Linux VM:

```
sudo apt update
sudo apt install rsync -y
```

### 4.2 Prepare Source Data / Бастапқы деректерді дайындау / Подготовка исходных данных

On **Linux Source** / **Linux Source**-та / На **Linux Source**:

```
sudo mkdir -p /data
sudo chown $USER:$USER /data
echo "Important file 1" > /data/file1.txt
echo "Important file 2" > /data/file2.txt
mkdir -p /data/docs
echo "Documentation" > /data/docs/readme.md
```

### 4.3 Create Backup Directory / Резервтік көшіру каталогын жасау / Создание каталога резервных копий

On **Linux Backup** / **Linux Backup**-та / На **Linux Backup**:

```
sudo mkdir -p /backup
sudo chown $USER:$USER /backup
```

### 4.4 Configure SSH Key Authentication / SSH кілтін аутентификациялауды конфигурациялау / Настройка аутентификации по SSH-ключу

On **Linux Source** / **Linux Source**-та / На **Linux Source**:

```
ssh-keygen -t rsa -b 4096
ssh-copy-id user@192.168.1.20
```

### 4.5 Perform Initial Full Backup with rsync / rsync көмегімен бастапқы толық көшірме жасау / Выполнение начальной полной копии с rsync

On **Linux Source** / **Linux Source**-та / На **Linux Source**:

```
rsync -avz --delete /data/ user@192.168.1.20:/backup/data/
```

- `-a`: archive mode (preserves permissions, timestamps, etc.) / архивтік режим / режим архива

- `-v`: verbose / егжей-тегжейлі / подробный вывод

- `-z`: compress during transfer / тасымалдау кезінде қысу / сжатие при передаче

- `--delete`: delete files on destination that no longer exist on source / көзде жоқ файлдарды тағайындалған жерде жою / удаление файлов на назначении, которых нет в источнике

### 4.6 Incremental Backups with rsync / rsync көмегімен инкременттік көшірмелер / Инкрементные копии с rsync

Use `--link-dest` to create hard-linked incremental backups / `--link-dest` қолданып, қатты сілтемелі инкременттік көшірмелер жасау / Используйте `--link-dest` для создания инкрементных копий с жесткими ссылками:

```
rsync -avz --delete --link-dest=/backup/data_$(date -d "yesterday" +%Y-%m-%d) \
  /data/ user@192.168.1.20:/backup/data_$(date +%Y-%m-%d)/
```

This creates a new directory for today’s backup, hard-linking unchanged files to yesterday’s backup to save space.  
Бұл бүгінгі көшірме үшін жаңа каталог жасайды, өзгермеген файлдарды кешегі көшірмеге қатты сілтеме жасайды.  
Это создает новый каталог для сегодняшней копии, жестко связывая неизмененные файлы со вчерашней копией.

### 4.7 Automate with cron / cron көмегімен автоматтандыру / Автоматизация с cron

On **Linux Source** / **Linux Source**-та / На **Linux Source**:

```
crontab -e
```

Add / Қосу / Добавить:

```
0 2 * * * rsync -avz --delete --link-dest=/backup/data_$(date -d "yesterday" +\%Y-\%m-\%d) /data/ user@192.168.1.20:/backup/data_$(date +\%Y-\%m-\%d)/ >> /var/log/rsync_backup.log 2>&1
```

This runs daily at 2:00 AM.  
Бұл күн сайын сағат 02:00-де іске қосылады.  
Это выполняется ежедневно в 02:00.


## 5. Part C: Windows Server Backup / C бөлімі: Windows Server Backup / Часть C: Windows Server Backup

### 5.1 Install Windows Server Backup / Windows Server Backup орнату / Установка Windows Server Backup

On **Windows Server** (192.168.1.30) / **Windows Server**-де (192.168.1.30) / На **Windows Server** (192.168.1.30):

1. Open **Server Manager** → **Add Roles and Features**. / **Server Manager** → **Add Roles and Features** ашыңыз. / Откройте **Server Manager** → **Add Roles and Features**.

2. Select **Windows Server Backup** under **Features**. / **Features** астында **Windows Server Backup** таңдаңыз. / Выберите **Windows Server Backup** в разделе **Features**.

3. Complete the wizard. / Шеберді аяқтаңыз. / Завершите мастер.

Or via PowerShell (as Administrator) / Немесе PowerShell арқылы (Әкімші ретінде) / Или через PowerShell (от имени администратора):

```
Install-WindowsFeature -Name Windows-Server-Backup
```

### 5.2 Create a File Share to Backup / Резервтік көшіруге файлдық ресурс жасау / Создание файлового ресурса для резервного копирования

Create a folder `C:\Shares` and add some files.  
`C:\Shares` бумасын жасап, бірнеше файл қосыңыз.  
Создайте папку `C:\Shares` и добавьте несколько файлов.

Share it as `\\WINSRV\Shares` with read/write permissions for the backup account.  
Оны `\\WINSRV\Shares` ретінде резервтік аккаунт үшін оқу/жазу рұқсаттарымен бөлісіңіз.  
Опубликуйте его как `\\WINSRV\Shares` с правами чтения/записи для учетной записи резервного копирования.

### 5.3 Configure Windows Server Backup to a Network Share / Windows Server Backup-ты желілік ресурсқа конфигурациялау / Настройка Windows Server Backup на сетевой ресурс

1. Open **Windows Server Backup** from Administrative Tools. / Әкімшілік құралдарынан **Windows Server Backup** ашыңыз. / Откройте **Windows Server Backup** из Административных инструментов.

2. Click **Backup Schedule** → **Backup Once** or **Schedule**. / **Backup Schedule** → **Backup Once** немесе **Schedule** басыңыз. / Нажмите **Backup Schedule** → **Backup Once** или **Schedule**.

3. Select **Custom** → **Add Items** → choose `C:\Shares`. / **Custom** → **Add Items** → `C:\Shares` таңдаңыз. / Выберите **Custom** → **Add Items** → выберите `C:\Shares`.

4. Choose **Remote shared folder**. / **Remote shared folder** таңдаңыз. / Выберите **Remote shared folder**.

5. Enter `\\192.168.1.20\backup\windows` (create this share on Linux Backup via Samba or NFS). / `\\192.168.1.20\backup\windows` енгізіңіз (Linux Backup-та Samba немесе NFS арқылы осы ресурсты жасаңыз). / Введите `\\192.168.1.20\backup\windows` (создайте этот ресурс на Linux Backup через Samba или NFS).

6. Set schedule (e.g., daily at 3:00 AM). / Кестені орнатыңыз (мысалы, күн сайын 03:00-де). / Установите расписание (например, ежедневно в 03:00).

7. Complete the wizard. / Шеберді аяқтаңыз. / Завершите мастер.

### 5.4 Verify Backup / Резервтік көшірмені тексеру / Проверка резервной копии

In Windows Server Backup, click **View Details** → **Last Backup** to see status.  
Windows Server Backup-та күйін көру үшін **View Details** → **Last Backup** басыңыз.  
В Windows Server Backup нажмите **View Details** → **Last Backup** для просмотра статуса.

Check the network share on Linux Backup / Linux Backup-тағы желілік ресурсты тексеріңіз / Проверьте сетевой ресурс на Linux Backup:

```
ls -l /backup/windows/
```


## 6. Part D: Script-Based Backup / D бөлімі: Скрипттік резервтік көшіру / Часть D: Скриптовое резервное копирование

### 6.1 Bash Script for rsync / rsync үшін Bash скрипті / Bash-скрипт для rsync

Create `/usr/local/bin/backup.sh` / `/usr/local/bin/backup.sh` жасаңыз / Создайте `/usr/local/bin/backup.sh`:

```
#!/bin/bash
# Simple rsync backup script / Қарапайым rsync резервтік көшіру скрипті / Простой скрипт резервного копирования rsync
SOURCE="/data/"
DEST="user@192.168.1.20:/backup/data_$(date +%Y-%m-%d)/"
LOG="/var/log/backup_script.log"

echo "Backup started at $(date)" >> "$LOG"
rsync -avz --delete "$SOURCE" "$DEST" >> "$LOG" 2>&1
echo "Backup finished at $(date)" >> "$LOG"
```

Make executable and schedule / Орындалатын етіп жасап, жоспарлаңыз / Сделайте исполняемым и запланируйте:

```
sudo chmod +x /usr/local/bin/backup.sh
sudo crontab -e
```

Add / Қосу / Добавить:

```
0 3 * * * /usr/local/bin/backup.sh
```

### 6.2 PowerShell Script for Windows Server Backup / Windows Server Backup үшін PowerShell скрипті / PowerShell-скрипт для Windows Server Backup

Create `C:\Scripts\Backup.ps1` / `C:\Scripts\Backup.ps1` жасаңыз / Создайте `C:\Scripts\Backup.ps1`:

```
# Windows Server Backup script / Windows Server Backup скрипті / Скрипт Windows Server Backup
$Policy = New-WBPolicy
$BackupLocation = New-WBBackupTarget -NetworkPath "\\192.168.1.20\backup\windows"
Add-WBBackupTarget -Policy $Policy -Target $BackupLocation
$Files = New-WBFileSpec -FileSpec "C:\Shares"
Add-WBFileSpec -Policy $Policy -FileSpec $Files
Set-WBSchedule -Policy $Policy -Schedule 03:00
Set-WBPolicy -Policy $Policy
Start-WBBackup -Policy $Policy
```

Run it manually first to test / Алдымен қолмен іске қосып тексеріңіз / Сначала запустите вручную для тестирования:

```
Set-ExecutionPolicy RemoteSigned -Force
.\Backup.ps1
```

Schedule with Task Scheduler / Task Scheduler көмегімен жоспарлаңыз / Запланируйте с помощью Task Scheduler:

```
$Action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\Scripts\Backup.ps1"
$Trigger = New-ScheduledTaskTrigger -Daily -At 3am
Register-ScheduledTask -TaskName "WindowsServerBackup" -Action $Action -Trigger $Trigger -RunLevel Highest
```


## 7. Verification and Analysis / Тексеру және талдау / Проверка и анализ

### 7.1 Verify rsync Backups / rsync резервтік көшірмелерін тексеру / Проверка резервных копий rsync

On **Linux Backup** / **Linux Backup**-та / На **Linux Backup**:

```
ls -l /backup/data_2025-01-01/
du -sh /backup/data_*/
```

Check hard links / Қатты сілтемелерді тексеру / Проверка жестких ссылок:

```
ls -li /backup/data_2025-01-01/file1.txt
ls -li /backup/data_2025-01-02/file1.txt
```

If inodes are the same, hard linking works.  
Егер inode-тар бірдей болса, қатты сілтемелеу жұмыс істейді.  
Если inode совпадают, жесткие ссылки работают.

### 7.2 Test Restore from rsync / rsync-тен қалпына келтіруді тексеру / Тест восстановления из rsync

On **Linux Source**, delete a file / **Linux Source**-та файлды жойыңыз / На **Linux Source** удалите файл:

```
rm /data/file1.txt
```

Restore from backup / Резервтік көшірмеден қалпына келтіріңіз / Восстановите из резервной копии:

```
rsync -avz user@192.168.1.20:/backup/data_2025-01-01/file1.txt /data/
```

### 7.3 Verify Windows Server Backup / Windows Server Backup тексеру / Проверка Windows Server Backup

On **Windows Server** / **Windows Server**-де / На **Windows Server**:

```
Get-WBSummary
Get-WBBackupSet
```

Restore a file / Файлды қалпына келтіру / Восстановление файла:

1. Open **Windows Server Backup** → **Recover**. / **Windows Server Backup** → **Recover** ашыңыз. / Откройте **Windows Server Backup** → **Recover**.

2. Select **This server** → choose date → select file → restore to original or alternative location. / **This server** → күнді таңдаңыз → файлды таңдаңыз → бастапқы немесе балама орынға қалпына келтіріңіз. / Выберите **This server** → выберите дату → выберите файл → восстановите в исходное или альтернативное место.

### 7.4 Analyze Backup Strategies / Резервтік көшіру стратегияларын талдау / Анализ стратегий резервного копирования

Compare the time and storage used by / Уақыт пен сақтау орнын салыстырыңыз / Сравните время и используемое хранилище для:

- Full backup (rsync without `--link-dest`) / Толық көшірме / Полная копия

- Incremental backup (rsync with `--link-dest`) / Инкременттік көшірме / Инкрементная копия

- Windows Server Backup (full + incremental) / Windows Server Backup (толық + инкременттік) / Windows Server Backup (полное + инкрементное)

Discuss which strategy fits different scenarios (e.g., daily backups of a web server vs. weekly backups of a file server).  
Қай стратегия әртүрлі сценарийлерге сәйкес келетінін талқылаңыз (мысалы, веб-сервердің күнделікті көшірмелері vs. файлдық сервердің апталық көшірмелері).  
Обсудите, какая стратегия подходит для разных сценариев (например, ежедневное резервное копирование веб-сервера vs. еженедельное резервное копирование файлового сервера).


## 8. Common Pitfalls / Жиі кездесетін қателер / Типичные ошибки

### English

- **rsync:** Forgetting trailing slash on source (`/data/` vs `/data`) changes behavior.

- **rsync:** Not using `--delete` leads to stale files on destination.

- **SSH:** Firewall blocking port 22; ensure SSH keys are correctly copied.

- **Windows Server Backup:** Network share permissions must allow the backup account write access.

- **PowerShell:** Execution policy may block scripts; use `Set-ExecutionPolicy`.

- **Scheduling:** Cron environment may lack PATH; use full paths in scripts.

### Қазақша

- **rsync:** Көздегі соңғы қиғаш сызықты ұмыту (`/data/` vs `/data`) мінез-құлықты өзгертеді.

- **rsync:** `--delete` қолданбау тағайындалған жерде ескірген файлдарға әкеледі.

- **SSH:** 22 портын брандмауэр блоктайды; SSH кілттерінің дұрыс көшірілгеніне көз жеткізіңіз.

- **Windows Server Backup:** Желілік ресурс рұқсаттары резервтік аккаунтқа жазу рұқсатын беруі керек.

- **PowerShell:** Орындау саясаты скрипттерді бұғаттауы мүмкін; `Set-ExecutionPolicy` қолданыңыз.

- **Жоспарлау:** Cron ортасында PATH жоқ болуы мүмкін; скрипттерде толық жолдарды қолданыңыз.

### Русский

- **rsync:** Забыли завершающий слэш в источнике (`/data/` vs `/data`) — меняется поведение.

- **rsync:** Без `--delete` на назначении остаются устаревшие файлы.

- **SSH:** Брандмауэр блокирует порт 22; убедитесь, что SSH-ключи скопированы правильно.

- **Windows Server Backup:** Права на сетевой ресурс должны давать учетной записи резервного копирования доступ на запись.

- **PowerShell:** Политика выполнения может блокировать скрипты; используйте `Set-ExecutionPolicy`.

- **Планирование:** В среде cron может отсутствовать PATH; используйте полные пути в скриптах.


## 9. Key Terms / Негізгі терминдер / Ключевые термины

| English | Қазақша | Русский |
| - | - | - |
| Full backup | Толық резервтік көшірме | Полная резервная копия |
| Incremental backup | Инкременттік резервтік көшірме | Инкрементная резервная копия |
| Differential backup | Дифференциалдық резервтік көшірме | Дифференциальная резервная копия |
| Synthetic full backup | Синтетикалық толық резервтік көшірме | Синтетическая полная резервная копия |
| rsync | rsync | rsync |
| Windows Server Backup | Windows Server Backup | Windows Server Backup |
| Script-based backup | Скрипттік резервтік көшіру | Скриптовое резервное копирование |
| 3-2-1 rule | 3-2-1 ережесі | Правило 3-2-1 |
| Hard link | Қатты сілтеме | Жесткая ссылка |
| Restore | Қалпына келтіру | Восстановление |



**End of Lab / Зертханалық жұмыстың соңы / Конец лабораторной работы**

