# Установка DotPlant2 на Windows 7 с нуля, под Open Server

Все ссылочки внизу под катом.
Установите Openserver, memcached, git:

+ [Openserver](http://open-server.ru/download/ "Download Open Server").
+ [Git](https://git-scm.com/download/win "Download Git for Windows")

Создайте MySQL базу данных и пользователя для нее.
В IDE MySQL, Например, Sqlyog пишем запрос на создания БД `dotplant2` и выполняем:

```bash
CREATE DATABASE dotplant2;
GRANT ALL PRIVILEGES ON dotplant2.* To 'dotplant2'@'localhost' IDENTIFIED BY 'REPLACE_WITH_YOUR_PASSWORD';
```

Открываем Git bush console в папке проэкта. Склонируйте гит репозиторий и обновите зависимости (Вот сдесь лучше пользоваться консолью Опена: Меню/Дополнительно/Консоль):

```bash
git clone https://github.com/DevGroup-ru/dotplant2.git
cd <pathto>/application
php ../composer.phar global require "fxp/composer-asset-plugin:1.0.0"
php ../composer.phar install --prefer-dist --optimize-autoloader
```

Правим конфигурационный файл Опена для всех хостов, используя модуль nginx(for v.1.7), файлик    
`<OpenServer>\userdata\config\Nginx-1.7_vhost.conf`, приводим к следующему виду:

```
\#-----------------------------------------------#  
\# Начало блока конфигурации хоста  
\#-----------------------------------------------#      

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

Добавляем папку `application\web` для доменного имени `<pathto>` в настройках домена Опена.  
Не забудьте перезагрузить сервер.   

Как видим, до сих пор установка мало чем отличаеться от юникс*ситем плясок.  
Да, вот для установки базы будем пользоваться консолью Опена и окном браузера и протоколом http )

Устанавливаем либо через Броузер   
- Через веб-интерфейс http://`<pathto>`/installer.php и доходим до шага миграции, пока стоп

Либо сразу через консольку и все сразу ), чего и рекомендую  
- Через консоль из папки application запустите install.sh

Далее нужно провести миграцию конфигов в БД, прежде укажем в конфиге путь к БД   
Создаем файлик db-local.php (по рекомендации пользоваться -локал) в паке `application\config\`

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

далее сама миграция  
`
disk:\OpenServer\domains\<pathto>\application>yii migrate
`       

Далее - [настройка](web-application-configuration.md)
