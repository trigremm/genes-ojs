# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Deployment of Open Journal Systems (OJS 3.5) for the scientific journal **Contig** at [ojs.genes.kz](https://ojs.genes.kz). The goal is a complete editorial workflow: manuscript submission, peer review, and publication.

This repository contains configuration files, customizations, and documentation. The OJS source code is NOT stored here — it is installed separately from the official tar.gz release.

## Repository Structure

```
genes-ojs/
├── CLAUDE.md
├── README.md
├── config/                             # Server config files (copy to OJS root)
│   ├── config.inc.php.example          #   OJS config template (placeholders: __DB_USERNAME__ etc.)
│   ├── .htaccess                       #   Rewrite rules, security, caching for LiteSpeed
│   └── .user.ini                       #   PHP settings for LiteSpeed FastCGI
├── custom/                             # Customizations (copy over OJS installation)
│   ├── plugins/themes/                 #   Custom themes
│   ├── plugins/generic/                #   Custom plugins
│   └── locale/                         #   Additional translations
└── docs/
    ├── INSTALL.md                      #   Step-by-step installation guide (Russian)
    └── KNOWN_ISSUES.md                 #   Limitations and workarounds (Russian)
```

### Deployment workflow
1. Install OJS from official `ojs-3.5.0-3.tar.gz` into subdomain document root
2. Copy `config/` files into OJS root, fill in credentials
3. Copy `custom/` contents over OJS (mirrors OJS directory structure)

## Hosting Environment

**Provider:** hoster.kz (cloud server acloud-4.hoster.kz)
**Control Panel:** Plesk
**Web Server:** LiteSpeed (LSPHP FastCGI)
**Access:** FTP + Plesk panel (no SSH)

### Server Paths
| Path | Purpose |
|------|---------|
| `/var/www/vhosts/genes.kz/ojs.genes.kz/httpdocs/` | Document root (OJS installed here) |
| `/var/www/vhosts/genes.kz/ojs_files/` | Private files — manuscripts, reviews (outside web root) |

### PHP 8.4
- Path: `/opt/alt/php84/`
- memory_limit: 512M, upload_max_filesize: 512M, max_execution_time: 180s

### Disabled PHP Functions
```
exec, passthru, shell_exec, system, proc_open, popen, opcache_get_status
```
OJS handles this via built-in `job_runner` and `task_runner`. PDF full-text indexing unavailable. Composer plugins installed manually via FTP.

### Database
MariaDB via Plesk. Collation: `utf8_general_ci` (avoid utf8mb4 — OJS 3.5 bug pkp-lib#11563).

## Key References

- OJS download: https://pkp.sfu.ca/software/ojs/download/
- OJS GitHub: https://github.com/pkp/ojs
- OJS Documentation: https://docs.pkp.sfu.ca/

## Git Workflow

- **main** — production branch
- **dev** — active development
- Documentation is in Russian
