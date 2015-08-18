# DotPlant2 installing

### Minimal system requirements:

- PHP 5.5.9
- server with *nix operation system (tested on Ubuntu 14.10)
- MySQL 5.5+
- we strongly recommend to use a Memcached or a APC.

### Required PHP modules:

- gd
- json
- pdo, pdo-mysql
- memcached (for memcache cache only)
- curl
- intl

A full `CMS DotPlant2` installation process has been written [here](web-application-configuration.md).

## Downloading and installing

Clone a git repository:

```bash
git clone git@github.com:DevGroup-ru/dotplant2.git
```

Set your application web root directory to

```php
%PATH_TO_DOTPLANT2%/application/web/
```

Go to `application` directory and execute next commands:

```bash
php ../composer.phar global require "fxp/composer-asset-plugin:1.0.0"
php ../composer.phar install --prefer-dist --optimize-autoloader
```

Open in your browser `YOUR_HOST_NAME/installer.php` and follow instructions.

## Installing example for Ubuntu 14.10

An installation example is located [here](setup-example.md)

## Demo data installing

How to install a [demo data](install-demo-data.md)