# Домашнє завдання №6. Bash-скрипти

**Обраний варіант:** Варіант A — Скрипт бекапу логів

### 1. Код скрипта (`backup.sh`)

```bash
#!/bin/bash

# Зберігаємо шляхи з аргументів у змінні для зручності
LOG_DIR="$1"
BACKUP_DIR="$2"
LOCK_FILE="/tmp/backup.lock"

# 1. Перевірка аргументів
# Перевіряємо, чи передано рівно 2 аргументи, і чи обидва є існуючими каталогами
if [ "$#" -ne 2 ] || [ ! -d "$LOG_DIR" ] || [ ! -d "$BACKUP_DIR" ]; then
    echo "Usage: ./backup.sh <log_dir> <backup_dir>"
    exit 1
fi

# 2. Захист від паралельного запуску
# Якщо lock-файл вже існує, значить скрипт ще працює
if [ -f "$LOCK_FILE" ]; then
    echo "Backup already running"
    exit 1
fi

# Створюємо lock-файл
touch "$LOCK_FILE"

# Best practice: гарантуємо видалення lock-файлу при будь-якому виході зі скрипта
trap "rm -f $LOCK_FILE" EXIT

# 3. Створення архіву логів
# Формуємо ім'я файлу з поточною датою та часом
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M")
ARCHIVE_NAME="logs_backup_${TIMESTAMP}.tar.gz"
ARCHIVE_PATH="${BACKUP_DIR}/${ARCHIVE_NAME}"

# Архівуємо вміст каталогу логів
# -c (create), -z (gzip), -f (file), -C (змінити каталог перед архівацією)
tar -czf "$ARCHIVE_PATH" -C "$LOG_DIR" .

# 4. Перевірка результату
# $? зберігає код завершення останньої команди (0 - якщо успішно)
if [ $? -ne 0 ]; then
    echo "Backup failed"
    exit 2
else
    echo "Backup created: $ARCHIVE_PATH"
fi
```
