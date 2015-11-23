# Установка DotPlant2

### Минимальные системные требования

- PHP 5.5.9
- сервер на базе *nix-подобной операционной системы (протестировано на Ubuntu 14.10)
- MySQL 5.5+
- Настоятельно рекомендуется использование Memcached или APC.

### Необходимые PHP модули:

- gd
- json
- pdo, pdo-mysql
- memcached (for memcache cache only)
- curl
- intl

Полный процесс установки `CMS DotPlant2` описан [здесь](web-application-configuration.md).

## Загрузка и установка

Склонируйте гит репозиторий:
```bash
git clone git@github.com:DevGroup-ru/dotplant2.git
```
В качестве корневой директории вебсервера установите
```php
%PATH_TO_DOTPLANT2%/application/web/
```
Переходим в директорию `application` и выполняем следующие команды:

```bash
php ../composer.phar global require "fxp/composer-asset-plugin:~1.0"
php ../composer.phar install --prefer-dist --optimize-autoloader
```

Запускаем консольный инсталятор
```bash
./installer
```

## Пример установки на Ubuntu 14.10

Пример установки разобран вот [здесь](setup-example.md)

## Установка демонстрационных данных

Инструкция по установке [демо-данных](install-demo-data.md)
