# Installing DotPlant 2

## Minimal system requirements:

- PHP 5.5.9
- *nix-based server(well tested on Ubuntu 14.10)
- MySQL 5.5+
- Memcached server or APC for caching purposes is highly recommended

Needed PHP modules:
- gd
- mcrypt
- json
- pdo, pdo-mysql
- memcached(for memcache cache only)
- curl
- intl

Full installation process for Ubuntu 14.10 [read here](Full_stack_installation/Ubuntu_14_10.html).

## Downloading and installing

Clone git repository:

``` bash
git clone git@github.com:DevGroup-ru/dotplant2.git
```

Make your server's document root `%PATH_TO_DOTPLANT2%/application/web/`.

Run installation script from dotplant2 root folder:

``` bash
cd application && php install.php
```

Follow installation instructions.

**Note: ** You will be asked for admin user credentials. Admin user is the only one(for now) user, that can use backend located at http://YOUR_HOSTNAME/backend/


Continue to [configuring &rarr;](Configuration.html)