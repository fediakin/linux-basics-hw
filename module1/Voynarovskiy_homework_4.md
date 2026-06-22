# Домашнє завдання №4. Пакети, сервіси та журнали

### Завдання 1. Менеджери пакетів

1. Оновлення списку пакетів у системі:
   `sudo apt update`

2. Встановлення утиліти tree:
   `sudo apt install tree -y`

3. Перевірка версії встановленого пакета:
   `tree --version`

4. Видалення пакета tree із системи:
   `sudo apt remove tree -y`

### Завдання 2. Керування сервісами через systemctl

_(Всі команди виконувалися на прикладі системного сервісу cron)_

1. Перевірка поточного статусу сервісу:
   `systemctl status cron`

2. Зупинка сервісу та перевірка його стану:
   `sudo systemctl stop cron`
   `systemctl status cron`

3. Запуск сервісу:
   `sudo systemctl start cron`

4. Додавання сервісу в автозавантаження системи:
   `sudo systemctl enable cron`

### Завдання 3. Робота з логами

1. Перехід в каталог з логами та вивід 10 останніх рядків файлу syslog:
   `cd /var/log`
   `tail -n 10 syslog`

2. Фільтрація логів через journalctl для відображення тільки помилок (рівень err):
   `journalctl -p err`

3. Пошук записів про роботу сервісу cron:
   `journalctl -u cron`

### Завдання 4. Створення власного сервісу

1. Створення bash-скрипту `logger.sh` у домашньому каталозі:

```bash
#!/bin/bash
while true; do
  date >> ~/date_log.txt
  sleep 1
done
```

Команда для надання прав на виконання скрипту:
`chmod +x ~/logger.sh`

2. Створення та редагування файлу конфігурації сервісу:
   `sudo nano /etc/systemd/system/myscript.service`

3. Вміст файлу конфігурації сервісу `myscript.service`:

```ini
[Unit]
Description=My Custom Date Logger Service

[Service]
ExecStart=/home/student/logger.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

4. Команди для перезапуску конфігурації systemd, запуску нового сервісу та перевірки логування:
   `sudo systemctl daemon-reload`
   `sudo systemctl start myscript.service`
   `systemctl status myscript.service`
   `cat ~/date_log.txt`
