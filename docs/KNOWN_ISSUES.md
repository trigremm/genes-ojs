# Известные ограничения и обходные пути

OJS 3.5 на hoster.kz (Plesk, LiteSpeed, PHP 8.4, MariaDB).
Субдомен: `ojs.genes.kz`

## Отключены shell-функции PHP

На сервере заблокированы: `exec`, `passthru`, `shell_exec`, `system`, `proc_open`, `popen`.

### Что не работает

| Функционал | Причина | Обходной путь |
|---|---|---|
| PDF full-text индексация | Требует `exec()` для вызова `pdftotext` | Поиск работает по заголовкам, аннотациям и метаданным. Полнотекстовый поиск по PDF недоступен. |
| CLI-утилиты OJS (`tools/*.php`) | Требуют запуска из командной строки | Использовать «Run a command» или «Run a PHP script» в планировщике Plesk |
| Composer-плагины | Установка через Composer требует `proc_open` | Скачать плагин вручную, загрузить через FTP в `plugins/generic/` (или соответствующую подпапку) |

### Что работает без ограничений

- Встроенный job runner (`job_runner = On`) — обрабатывает задачи в конце каждого веб-запроса
- Встроенный task runner (`task_runner = On`) — заменяет плагин Acron из OJS 3.4
- Установка плагинов из Plugin Gallery в OJS admin — работает по HTTP, не через CLI
- Все основные функции: подача рукописей, рецензирование, публикация

## LiteSpeed — особенности

### Кэширование .htaccess
LiteSpeed может кэшировать правила из `.htaccess`. Если после изменения правил они не вступают в силу — обратиться в поддержку hoster.kz для перезапуска LiteSpeed.

### PHP-настройки через .user.ini
LiteSpeed с FastCGI **игнорирует** директивы `php_value` / `php_flag` в `.htaccess`. Для изменения PHP-настроек использовать файл `.user.ini` в корне сайта или панель Plesk.

### Загрузка больших файлов
Если загрузка файлов (>50 MB) завершается ошибкой при корректных настройках PHP, причина может быть в лимите LiteSpeed. В `.htaccess` уже добавлены директивы `noabort` и `noconntimeout`. Если проблема сохраняется — обратиться к провайдеру.

## MariaDB — collation

В OJS 3.5 есть [баг](https://github.com/pkp/pkp-lib/issues/11563): таблицы создаются как `utf8mb3` даже когда в конфиге указано `utf8mb4`. Базу данных создавать с collation `utf8_general_ci`.

## OPcache

Функция `opcache_get_status` заблокирована. Сам OPcache работает нормально. Некоторые плагины мониторинга могут показывать предупреждения — это не влияет на работу OJS.

## Обновление OJS

Стандартная команда `php tools/upgrade.php upgrade` недоступна из PHP напрямую. Варианты:

1. В Plesk → **Планировщик задач** → создать задачу типа «Run a command»:
   ```
   /opt/alt/php84/usr/bin/php /var/www/vhosts/genes.kz/ojs.genes.kz/httpdocs/tools/upgrade.php upgrade
   ```
2. Если «Run a command» недоступен — обратиться в поддержку hoster.kz.

> Перед обновлением всегда делать резервную копию БД и файлов через Plesk.
