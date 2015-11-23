# Установка DotPlant2 на Ubuntu 14.10 с нуля

Установите LEMP, memcached, git:

```bash
sudo apt-get install  nginx php5-fpm php5-gd php5-json mysql-server php5-mysql php5-cli php5-memcached memcached php5-curl php5-intl git
```

Создайте MySQL базу данных и пользователя для нее.
Запустите командную оболочку MySQL:

```bash
mysql -uroot -p
```

И выполните команду:

```bash
CREATE DATABASE dotplant2;
GRANT ALL PRIVILEGES ON dotplant2.* To 'dotplant2'@'localhost' IDENTIFIED BY 'REPLACE_WITH_YOUR_PASSWORD';
```

Склонируйте гит репозиторий и обновите зависимости:

```bash
git clone https://github.com/DevGroup-ru/dotplant2.git
cd dotplant2/application
php ../composer.phar global require "fxp/composer-asset-plugin:~1.0"
php ../composer.phar install --prefer-dist --optimize-autoloader
```


Создайте конфигурационный файл для хоста `nginx` `/etc/nginx/sites-enabled/dotplant2-host`:

```conf
server {
    listen 80;

    # NOTE: Replace with your path here
    root /home/user/dotplant2/application/web;
    index index.php;

    # NOTE: Replace with your hostname
    server_name dotplant2.dev;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }

    location ~ /\.ht {
       deny all;
    }
}
```

Отредактируйте конфигурацию `PHP-fpm`: замените `cgi.fix_pathinfo = 1` на `cgi.fix_pathinfo = 0` или создайте файл `/etc/php5/fpm/pool.d/www_nginx.conf` со следующим содержанием:

```conf
[www]
php_admin_value[cgi.fix_pathinfo]=0
```

Не забудьте перезагрузить `nginx` и `php5-fpm`: 

```bash
sudo service nginx restart
sudo service php5-fpm restart
```

Установить базовые настройки CMS 
- Через консоль из папки application запустите `./installer`

Далее - [настройка](web-application-configuration.md)
