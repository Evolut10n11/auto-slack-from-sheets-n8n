# 🔁 Автоматизация Slack-уведомлений из Google Sheets через n8n

Этот проект запускает [n8n](https://n8n.io) через Docker и выполняет автоматическую проверку Google Sheets: если найдены необработанные строки (`Processed == "no"`), отправляет уведомление в Slack и обновляет таблицу.

---

## ⚙️ Возможности

✅ Автоматический запуск по расписанию  
✅ Проверка Google Sheets на наличие новых заявок  
✅ Отправка сообщений в Slack через Webhook  
✅ Пометка строк как `Processed = yes`, чтобы избежать дублирования  
✅ Импортируемый `workflow.json`  
✅ Полностью автономен, разворачивается через Docker

---

## 🧩 Схема работы

```markdown
```mermaid
graph LR
    Schedule[Schedule Trigger] --> SheetsRead[Google Sheets (read)]
    SheetsRead --> Loop[Loop Over Items]
    Loop --> IF[If Processed == "no"]
    IF -- true --> Slack[HTTP Request (Slack)]
    Slack --> SheetsUpdate[Google Sheets (append/update)]
    IF -- false --> End[skip]
```

---

## 📄 Формат Google Sheets

Таблица должна содержать такие колонки:

| name  | email            | message             | Processed |
|-------|------------------|---------------------|-----------|
| Иван  | ivan@example.com | Привет, это тест!   | no        |

---

## 💬 Пример Slack-сообщения

```
📩 Новая заявка!
👤 Имя: Иван  
✉️ Email: ivan@example.com  
💬 Сообщение: Привет, это тест!
```

---

## 🚀 Быстрый старт

### 1. Клонируй репозиторий

```bash
git clone https://github.com/Evolut10n11/auto-slack-from-sheets-n8n.git
cd auto-slack-from-sheets-n8n
```

### 2. Запусти через Docker

```bash
docker run -d ^
  --name n8n ^
  -p 5678:5678 ^
  -v %cd%\n8n_data:/home/node/.n8n ^
  -e N8N_ENCRYPTION_KEY=<твой_ключ> ^
  n8nio/n8n:1.59.0
```

> ⚠️ Замените `<твой_ключ>` на ключ, использованный в прошлой сессии n8n, чтобы восстановить workflow.

---

### 3. Настрой workflow

1. Открой `http://localhost:5678`
2. Импортируй `workflow.json` из корня проекта
3. Вставь Slack Webhook URL в ноду `HTTP Request`
4. Убедись, что подключен Google Service Account к Google Sheets

---

## 📂 Структура проекта

```
n8n-slack-sheet-automation/
├── docker-compose.yml     # (опционально) Docker Compose файл
├── workflow.json          # Сам workflow (экспорт из n8n)
├── n8n_data/              # Хранение состояния n8n
└── README.md
```

---

## 📎 Полезные ссылки

- 📄 [workflow.json](./workflow.json) — JSON-файл готового workflow
- 📖 [Документация n8n](https://docs.n8n.io/)
- 🔗 [Slack Webhook](https://api.slack.com/messaging/webhooks)

---

## 👨‍💻 Автор

_Ivan Rodionov — github.com/Evolut10n11_

---

## 📝 Лицензия

[MIT License](LICENSE)

