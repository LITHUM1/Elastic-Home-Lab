[![en](https://img.shields.io/badge/lang-en-blue.svg)](https://github.com/LITHUM1/Wazuh-Home-Lab/blob/main/README.md)


# Лаборатория WAZUH

### Обзор
Этот гид поможет вам настроить домашнюю лабораторию для использования WAZUH Stack, мощного решения для управления информацией и событиями безопасности (SIEM). Мы будем использовать веб-портал WAZUH вместе с виртуальной машиной (VM) Kali Linux для создания полностью функциональной лабораторной среды.

### Необходимые инструменты:
1. **VMware** - для виртуализации
2. **WAZUH** - для SIEM
3. **Kali Linux VM** - для пентестинга и исследований в области безопасности

---

### Настройка среды: Развертывание виртуальной машины Wazuh Server

Начнем с развертывания Wazuh Server локально на VMware. Для получения подробных инструкций обратитесь к официальной [документации Wazuh](https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html).

1. **Скачайте OVA образ Wazuh Server**:
   ![Download](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Download-Wazuh-server-image.png)

2. **Запуск Wazuh Server на VMware**:
   После загрузки запустите сервер на VMware и подождите несколько минут, пока он инициализируется.

3. **Вход в систему и проверка развертывания**:
   Как только сервер будет готов, войдите в систему, используя учетные данные по умолчанию. Если в терминале отображается следующий экран, значит развертывание прошло успешно:
   ![Terminal](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/up-and-running.png)

4. **Доступ к веб-порталу Wazuh**:
   Найдите IP сервера, выполнив команду `ip a`, и введите его в браузере. Используйте учетные данные по умолчанию `admin:admin`, чтобы получить доступ к панели управления Wazuh.
   ![Portal](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Wazuh-portal.png)

---

### Развертывание агентов Wazuh

Далее мы развернем агентов Wazuh на двух виртуальных машинах: одна с Windows и одна с Kali Linux. Эти агенты будут собирать телеметрию и обеспечивать видимость хостов.

1. **Установка агентов Wazuh**:
   ![Windows Agent](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Win-Agent.png)
   ![Kali Agent](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Kali-agent.png)

2. **Проверка развертывания агентов**:
   Теперь оба агента должны быть активны и передавать данные на сервер Wazuh.
   ![Wazuh Agents](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Wazuh-agents.png)

---

### Стресс-тестирование агентов

После развертывания агентов проверим их способность обнаруживать вредоносные активности.

#### Ручное тестирование

1. **Тестирование с использованием Metasploit**:
   - На виртуальной машине с Windows установите Metasploit Framework.
   ![Windows Install](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/installing-metasploit-windows.png)

   - На виртуальной машине с Kali выполните сканирование портов с помощью Metasploit.
   ![Metasploit](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/metasploit-kali.png)

   - Запустите сканирование портов:
   ![Port Scan](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/port-scanner.png)

   Хотя изначально не были сгенерированы высокоопасные предупреждения, попробуем что-то более шумное.

2. **Создание нового пользователя в Windows**:
   - Выполните следующие команды, чтобы создать нового пользователя и добавить его в группу Администраторов:
     ```shell
     net user <username> <password> /add
     net localgroup Administrator <username> /add
     ```
   ![Windows Commands](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/adding-user.png)

   - Это должно сгенерировать предупреждение, которое можно увидеть в модуле SIEM:
   ![Risk Alert](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/report-Wazuh.png)

#### Автоматическое тестирование

Теперь давайте автоматизируем тестирование с использованием скрипта Endpoint Detection and Response (EDR).

1. **Запуск скрипта EDR Testing**:
   - Скачайте и распакуйте скрипт, затем выполните `runtests.bat`.
   ![Script Running](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/cal-script.png)

   - Скрипт будет симулировать техники MITRE ATT&CK. Защитник Windows может сгенерировать предупреждения, а на панели MITRE ATT&CK в Wazuh будет отображаться активность:
   ![MITRE ATT&CK Dashboard](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/wazuh-mitre-attack.png)

2. **Анализ результатов**:
   - Перейдите в редактор запросов SIEM, нажав на "Persistence" в разделе "Top tactics". Вы можете добавить фильтры для уточнения результатов.
   ![SIEM Query](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/event-t1078.png)

   - Расширив результаты, вы увидите подробные журналы событий, такие как создание новых служб.
   ![Event Log](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/script.png)

---

На этом завершается настройка простой домашней лаборатории с использованием Wazuh. Эта среда позволит вам изучать мониторинг безопасности, реагирование на инциденты и возможности SIEM в контролируемой, практической обстановке.
