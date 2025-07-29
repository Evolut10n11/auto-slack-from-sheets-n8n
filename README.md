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
  n8nio/n8n:1.59.0
```

---

### 3. Настрой workflow

1. Настрой Google Service Account
2. Укажи ID таблицы и Webhook URL Slack
3. Импортируй workflow в n8n
4. Запусти вручную или по расписанию

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

