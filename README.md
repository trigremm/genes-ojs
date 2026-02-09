# genes-ojs

Конфигурация и кастомизация Open Journal Systems (OJS) для научного журнала **Contig** на субдомене [ojs.genes.kz](https://ojs.genes.kz).

## Цель

Развернуть и настроить OJS для полного редакционного цикла журнала Contig: подача рукописей, рецензирование, публикация.

## Технологический стек

- **OJS** 3.5.0-3 (Open Journal Systems)
- **PHP** 8.4 (LiteSpeed FastCGI)
- **MariaDB** (collation: utf8_general_ci)
- **Хостинг:** hoster.kz, облачный сервер (Plesk)
- **Веб-сервер:** LiteSpeed
- **Субдомен:** ojs.genes.kz

## Структура репозитория

Репозиторий содержит конфигурации, кастомизации и документацию. Исходный код OJS устанавливается отдельно из официального tar.gz.

```
config/                             — конфиги для сервера
├── config.inc.php.example          — шаблон конфигурации OJS
├── .htaccess                       — правила перенаправления и безопасности
└── .user.ini                       — настройки PHP для LiteSpeed

custom/                             — кастомизации (копируются поверх OJS)
├── plugins/themes/                 — кастомные темы
├── plugins/generic/                — кастомные плагины
└── locale/                         — дополнительные переводы

docs/                               — документация
├── INSTALL.md                      — пошаговая инструкция установки
└── KNOWN_ISSUES.md                 — известные ограничения
```

## Быстрый старт

См. [docs/INSTALL.md](docs/INSTALL.md) — подробная инструкция по установке OJS на ojs.genes.kz.

## Деплой кастомизаций

Содержимое `custom/` зеркалит структуру OJS. Для деплоя — скопировать файлы из `custom/` в корень установленного OJS поверх существующих файлов.

## Ссылки

- Журнал Contig: https://genes.kz/Contig/index.html
- OJS (Public Knowledge Project): https://pkp.sfu.ca/software/ojs/
- OJS на GitHub: https://github.com/pkp/ojs
- OJS документация: https://docs.pkp.sfu.ca/
- OJS 3.5 — что нового: https://pkp.sfu.ca/2025/07/16/ojs-3-5-enhanced-features/
