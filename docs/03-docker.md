# Docker: База для собеседования (Junior/Middle)

## 1. Docker vs VM
* **VM:** Своё ядро, эмуляция железа, тяжело (GB), медленно (минуты).
* **Container:** Общее ядро хоста, изоляция через Namespaces + Cgroups. Легко (MB), быстро (секунды).
* **Image:** Неизменяемый шаблон (класс).
* **Container:** Запущенный экземпляр (объект).

## 2. Dockerfile Best Practices (Продакшен)
1. **Фиксируй версию:** `python:3.12-slim`, а не `python:latest`.
2. **Multi-stage build:** Сборка и runtime в разных образах. Уменьшает размер в 10-100 раз.
3. **Не запускай от root:** `RUN useradd -m appuser` + `USER appuser`.
4. **Кэширование слоев:** Сначала `COPY requirements.txt`, `RUN pip install`, потом `COPY . .`.
5. **.dockerignore:** Исключай `.git`, `node_modules`, `.env`.
6. **HEALTHCHECK:** Проверка живости для Kubernetes.
7. **EXPOSE:** Только документация. Публикация через `-p`.
8. **CMD в exec-форме:** `CMD ["python", "app.py"]`, чтобы сигналы доходили.

## 3. Основные команды
* `docker build -t name:tag .` — собрать образ.
* `docker run -d -p 8080:8080 --name app image` — запустить контейнер.
* `docker ps` — запущенные. `docker ps -a` — все.
* `docker logs <container>` — логи.
* `docker exec -it <container> bash` — зайти внутрь.
* `docker stop/rm <container>` — остановить/удалить.
* `docker images` / `docker rmi` — образы.
* `docker system prune -a` — очистка (ОСТОРОЖНО!).

## 4. Docker Compose
* `docker compose up -d` — запустить всё.
* `docker compose down` — остановить и удалить.
* `depends_on` — порядок запуска (не гарантирует готовность).
* `volumes` — персистентные данные.
* `networks` — изоляция, DNS по имени сервиса.

## 5. Registry
* **Docker Hub** — публичный.
* **AWS ECR / GCP GCR / GitHub Container Registry** — приватные.
* `docker push` / `docker pull`.
* **Best Practice:** Тегировать образы версией (`v1.0.0`), НЕ использовать `latest` в продакшене. Git SHA как тег — золотой стандарт.

## 6. Частые вопросы на собеседовании:
* **Чем отличается COPY от ADD?** (ADD умеет распаковывать архивы и качать URL, COPY — только копирует. Используй COPY).
* **Что такое слои?** (Каждая инструкция в Dockerfile создает слой. Слои кэшируются и переиспользуются).
* **Как уменьшить размер образа?** (Multi-stage, alpine/slim, .dockerignore, удаление кэша apt/pip).
* **Что такое Namespaces и Cgroups?** (Namespaces — изоляция: PID, NET, MNT, USER. Cgroups — лимиты: CPU, RAM).
