# === Этап 1: Сборка и подготовка зависимостей (Build stage) ===
FROM python:3.10-slim AS builder

WORKDIR /build

# Установка Flask в изолированную директорию (используем /install вместо /root)
RUN pip install --no-cache-dir --prefix=/install flask

# === Этап 2: Минимальный финальный образ (Runtime stage) =====
FROM python:3.10-slim AS runner

# Создаем безопасного non-root пользователя и его домашнюю директорию
RUN useradd -u 10001 -m appuser

WORKDIR /app

# Копируем установленные библиотеки из первого этапа в общедоступный каталог
COPY --from=builder /install /usr/local

# Копируем исходный код приложения
COPY app.py .

# Меняем владельца файлов в рабочей директории на созданного пользователя
RUN chown -R appuser:appuser /app

# Переключаемся на non-root пользователя перед запуском приложения
USER 10001

EXPOSE 5000

CMD ["python", "app.py"]
