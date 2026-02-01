# Инструкция: GitHub, Cloudflare, Telegram

Ниже краткий чек‑лист, чтобы развернуть шаблон бота и мини‑аппа HoReCa.

## 1) GitHub (код и история)
1. Создай новый репозиторий на GitHub.
2. В терминале в корне проекта выполни:

```bash
cd /Users/maksim/Desktop/Bot:Miniapp:Horeca
git init
git add .
git commit -m "Horeca demo template"
git branch -M main
git remote add origin <URL_РЕПОЗИТОРИЯ>
git push -u origin main
```

## 2) Cloudflare Pages (хостинг мини‑аппа)
Вариант без сборки — статический сайт.

1. Открой Cloudflare → Pages → Create project → Connect to Git.
2. Выбери репозиторий.
3. В настройках сборки:
   - Framework preset: None
   - Build command: пусто
   - Build output directory: `миниапп`
4. Деплой. Получишь публичный адрес страницы мини‑аппа.
5. Этот адрес укажи в `WEBAPP_URL` (см. шаг 4).

## 3) Cloudflare Workers (опционально для API брони)
Если хочешь отправку брони/заказа через API (не через sendData), то:
1. Создай Worker и публичный endpoint.
2. Добавь проверку `X-API-KEY`.
3. Укажи значения в `миниапп/index.html`:
   - `window.HORECA_API_URL`
   - `window.HORECA_API_KEY`

Если не нужен API — оставь пустыми (как сейчас), всё будет работать через Telegram sendData.

## 4) Telegram Bot и запуск
1. В `бот/config.env` укажи:

```
BOT_TOKEN="<ТВОЙ_ТОКЕН_ОТ_BOTFATHER>"
WEBAPP_URL="<URL_МИНИАППА_ИЗ_CLOUDFLARE_PAGES>"
# NOTIFY_CHAT_IDS="-1001234567890"
```

2. (Опционально) `NOTIFY_CHAT_IDS` — chat_id группы, куда приходят заказы. Можно несколько, через запятую.
3. Установи зависимости и запусти бота:

```bash
cd /Users/maksim/Desktop/Bot:Miniapp:Horeca/бот
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
./start.sh
```

## 5) Проверка
- Открой бота в Telegram → /start → кнопка «Меню / Бронь».
- Оформи тестовый заказ или бронь.
- Если `NOTIFY_CHAT_IDS` не задан, подтверждение придёт пользователю, но в группу ничего не уйдёт.
