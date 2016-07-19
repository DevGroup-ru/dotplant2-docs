# Set DotPlant2 on Ubuntu 14.10 from scratch

Install LEMP, memcached, git:

```bash
sudo apt-get install  nginx php5-fpm php5-gd php5-json mysql-server php5-mysql php5-cli php5-memcached memcached php5-curl php5-intl git
```

Create a MySQL database and user for it.
Start the MySQL command shell:

```bash
mysql -uroot -p
```

And the following command:

```bash
CREATE DATABASE dotplant2;
GRANT ALL PRIVILEGES ON dotplant2.* To 'dotplant2'@'localhost' IDENTIFIED BY 'REPLACE_WITH_YOUR_PASSWORD';
```

Clone git repository and update dependencies::

```bash
git clone https://github.com/DevGroup-ru/dotplant2.git
cd dotplant2/application
php ../composer.phar global require "fxp/composer-asset-plugin:~1.0"
php ../composer.phar install --prefer-dist --optimize-autoloader
```


Create a configuration file for the host `nginx` `/etc/nginx/sites-enabled/dotplant2-host`:

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

Edit the configuration of `PHP-fpm`: replace `cgi.fix_pathinfo = 1` to `cgi.fix_pathinfo = 0` or create a file `/etc/php5/fpm/pool.d/www_nginx.conf` with the following content:

```conf
[www]
php_admin_value[cgi.fix_pathinfo]=0
```

Do not forget to reload `nginx` and `php5-fpm`:

```bash
sudo service nginx restart
sudo service php5-fpm restart
```

Set the basic settings CMS
- Through the console of the application folder, run `./installer`

Next - [Setting](web-application-configuration.md)
