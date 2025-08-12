### tg_downloader — экспорт постов из публичных Telegram‑каналов за период

Короткий скрипт на Python (Telethon) для выгрузки сообщений из публичных каналов за заданный период. Поддерживает одиночный канал или список каналов из файла. На выходе — JSON‑файлы по каждому каналу в каталоге периода. В консоли — цветной лог и общий прогресс по списку каналов.

#### Требования
- Python 3.10+
- Установка зависимостей: `pip install telethon tqdm colorama`
- Получите `api_id` и `api_hash` на `https://my.telegram.org` → API Development Tools. Удобно экспортировать в переменные окружения:
  ```bash
export TG_API_ID=123456
export TG_API_HASH=your_api_hash
  ```

При первом запуске скрипт запросит номер телефона и, при включённой 2FA, пароль. Создастся локальный файл сессии `tg_downloader_session.session`.

#### Примеры запуска
Перейдите в каталог скрипта:
```bash
cd tools/telegram/tg_downloader
```

- Одиночный канал:
```bash
python tg_downloader.py @barybino1900 --date-from 2025-05-01 --date-to 2025-05-31 --out-dir ./exports
```

- Список каналов из файла (по одному в строке; поддерживаются `@name`, `name`, `https://t.me/name`; строки с `#` игнорируются):
```text
news_channels.txt
@barybino1900
https://t.me/rian_ru
kanobu_ru
# комментарий
```
Запуск:
```bash
python tg_downloader.py --channels-file news_channels.txt --date-from 2025-05-01 --date-to 2025-05-31 --out-dir ./exports
```

#### Вывод
- Создаётся каталог периода: `<out-dir>/<source>_posts_YYYY-MM-DD_YYYY-MM-DD`, где `<source>` — имя файла списка каналов без расширения или имя канала.
- Внутри — файлы: `posts_<channel>_<YYYY-MM-DD>_<YYYY-MM-DD>.json`, например:
  `exports/news_channels_posts_2025-05-01_2025-05-31/posts_barybino1900_2025-05-01_2025-05-31.json`
- Если за период у канала нет сообщений, файл не создаётся.

Примечание: соблюдайте ToS Telegram и ограничения по частоте запросов. Скрипт выгружает время в UTC.


