# Linux: Процессы, Systemd и Диски (Практика)

## 1. Процессы и сигналы
* `ps aux` — все процессы. `a` (all users), `u` (user format), `x` (no tty).
* `htop` — интерактивный мониторинг. Лучше `top`.
* **Состояния процессов:**
  * `R` (Running) — выполняется.
  * `S` (Sleeping) — ждет события (норма).
  * `D` (Uninterruptible Sleep) — ждет I/O. Нельзя убить. Признак проблем с диском.
  * `Z` (Zombie) — завершился, но родитель не считал код возврата. Не ест CPU, но занимает PID.
* **Сигналы:**
  * `SIGTERM (15)` — вежливое завершение (сохраняет данные). Использовать первым.
  * `SIGKILL (9)` — принудительное убийство. Использовать, если 15 не помог.
* `kill <PID>`, `pkill <name>`, `killall <name>`.

## 2. Systemd (Управление сервисами)
* `systemctl daemon-reload` — перечитать конфиги (после создания юнита).
* `systemctl start/stop/restart/status <service>` — управление.
* `systemctl enable/disable <service>` — автозагрузка.
* `journalctl -u <service> -f` — логи сервиса в реальном времени.
* **Структура юнита:**
  * `[Unit]`: Description, After (зависимости).
  * `[Service]`: Type (simple), User (НЕ root!), ExecStart, Restart=always (Best Practice).
  * `[Install]`: WantedBy=multi-user.target.

## 3. Диски и ФС
* `df -h` — свободное место на дисках.
* `du -sh *` — размер папок.
* `lsblk` — структура дисков и разделов.
* **Инцидент:** Если `df` 100%, а `du` не находит — файл удален, но держится процессом (lsof | grep deleted).

## 4. Частые вопросы на собеседовании:
* Что такое Load Average? (Средняя нагрузка за 1/5/15 мин. Если больше количества ядер — система перегружена).
* В чем разница между `SIGTERM` и `SIGKILL`? (Вежливость vs Принуждение).
* Как найти, какой процесс слушает порт 80? (`ss -tulpn | grep :80` или `netstat -tulpn | grep :80`).
