# Установка DotPlant2 на Windows 7 с нуля, под Open Server

Устанайвливаем Open Server, memcached, git:

+ [Open Server](http://open-server.ru/download/ "Download Open Server").
+ [Git](https://git-scm.com/download/win "Download Git for Windows")

Создаем MySQL базу данных и пользователя для нее.
В IDE MySQL, например, SQLyog, пишем запрос на создания БД `dotplant2` и выполняем:

```bash
CREATE DATABASE dotplant2;
GRANT ALL PRIVILEGES ON dotplant2.* To 'dotplant2'@'localhost' IDENTIFIED BY 'REPLACE_WITH_YOUR_PASSWORD';
```

Открываем Git bush console в папке проэкта `<pathto>`. Склонируйте репозиторий dotpant2:
`
bash
git clone https://github.com/DevGroup-ru/dotplant2.git
`

Обновляем зависимости, подтягиваем вендоров (консоль Open Server: Меню / Дополнительно / Консоль):
```
cd <pathto>/application
php ../composer.phar global require "fxp/composer-asset-plugin:1.0.0"
php ../composer.phar install --prefer-dist --optimize-autoloader
```

Правим конфигурационный файл Open Server для всех хостов, используя модуль nginx(for v.1.7), файл    
`<OpenServer>\userdata\config\Nginx-1.7_vhost.conf`, приводим к следующему виду:

```
#-----------------------------------------------#  
# Начало блока конфигурации хоста  
#-----------------------------------------------#      

server {        
	listen         %ip%:%httpport%;     
	listen         %ip%:%httpsport% ssl;        
	server_name    %host% %aliases%;        
	# if ($request_method !~* ^(GET|HEAD|POST)$ ){return 403;}      
	location ~ /\. {deny all;}      
	index index.php;        

    location / {
        root       "%hostdir%";
        #index      index.php index.html index.htm;
		try_files $uri $uri/ /index.php?$args;
    }
    ...
```

Добавляем папку `application\web` для доменного имени `<pathto>` в настройках домена Open Server.  
Не забудьте перезагрузить сервер. Теперь исходники dotplant2 готовы к установке.  

Как видим, до сих пор процесс подготовки мало чем отличаеться от \*unix ситем подобных.  
Для установки БД можно использовать консоль Open Server'a и браузер.

Установка через веб-интерфейс dotplant2:   
- В адресной строке - http://`<dotplant2_domain>`/installer.php. Проходим пункты установки. Доходим до шага миграции, ставим флажок "Миграция вручную"

Установка dotplant2, используя консоль Open Server
- Открываем Консоль - Open Server menu / Дополнительно / Консоль.  
	`
	cd <pathto>/application   
	<pathto>/application: install.sh
	`

Выполняем миграцию БД из созданых ранее конфигурационных файлов (`application\migrations\\*`), но прежде    
укажем путь к БД в конфигурации dotplant2. Создаем файл db-local.php (рекомендация, пользоваться -local суффиксом) в паке `application\config\`

```
<?php   
return [    
    'username' => 'root',   
    'password' => 'rootpassword',   
    'enableSchemaCache' => true,    
    'schemaCacheDuration' => 86400, 
    'schemaCache' => 'cache',   
    'dsn' => 'mysql:host=localhost;dbname=dotplantdbname',  
    'class' => 'yii\\db\\Connection',   
];  

```

сама миграция  
`
<pathto>\application>yii migrate
`       

Установка закончена. После установки, следует провести тонкую настройку - [настройка](web-application-configuration.md)