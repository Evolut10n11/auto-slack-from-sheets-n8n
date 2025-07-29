# 🔁 Автоматическая обработка заявок из Google Sheets через Slack (n8n workflow)

Этот workflow на базе [n8n](https://n8n.io) автоматически читает новые строки из Google Sheets, отправляет уведомления в Slack и помечает обработанные заявки как `Processed = yes`.

## 📌 Функционал

✅ Проверяет таблицу Google Sheets по расписанию  
✅ Отправляет новые заявки в Slack через Webhook  
✅ Помечает обработанные строки, чтобы не дублировать отправку  
✅ Работает локально через Docker или на n8n Cloud

---

## ⚙️ Компоненты workflow

```mermaid
graph LR
    Schedule[Schedule Trigger] --> SheetsRead[Google Sheets (read)]
    SheetsRead --> Loop[Loop Over Items]
    Loop --> IF[If Processed = "no"]
    IF -- true --> Slack[HTTP Request (Slack)]
    Slack --> SheetsUpdate[Google Sheets (append/update)]
    IF -- false --> End[skip]
```

---

## 🗂 Структура Google Sheets

| Имя   | Email            | Сообщение          | Processed |
|--------|-------------------|---------------------|-------------|
| Иван  | ivan@example.com | Привет, это тест! | no          |

---

## 💬 Формат Slack-сообщения

```text
📩 Новая заявка!
👤 Имя: Иван
✉️ Email: ivan@example.com
💬 Сообщение: Привет, это тест!
```

---

## 🔧 Как запустить

1. **Настрой Docker n8n (если локально):**
   ```bash
   docker run -it --rm \
     -p 5678:5678 \
     -v ~/.n8n:/home/node/.n8n \
     -e TZ=Europe/Moscow \
     -e N8N_BASIC_AUTH_USER=admin \
     -e N8N_BASIC_AUTH_PASSWORD=1234 \
     n8nio/n8n
   ```

2. **Создай Google Service Account и подключи Google Sheets credentials в `n8n`.**

3. **Создай Slack Incoming Webhook и вставь URL в `HTTP Request`.**

4. **Импортируй workflow (`workflow.json`) и запусти.**

---

## 📁 Файл workflow

Положи JSON экспорт сюда (например, `workflow.json`) и добавь ссылку в README:

```
📄 [workflow.json](./workflow.json)
```

---

## ✍️ Автор

- 🔗 Telegram: [@neverrm1ndo](https://t.me/neverrm1ndo)
- 🧠 Архитектор: Элейн-Сама (локальный AI ассистент)

---

## 🏆 Лицензия

MIT — используй и автоматизируй на здоровье.
