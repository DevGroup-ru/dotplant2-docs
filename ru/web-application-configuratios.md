# Настройка DotPlant2

Основные настройки `DotPlant2` устанавливаеются во время работы установщика.

На самом деле, вся конфигурация - это обычная конфигурация для `Yii2` приложения.

Вы можете настраивать`DotPlant2` точно так же, как и любое `Yii2` приложение.

Конфигурационные файлы располагаются по пути: `application/config`.
Структура директории имеет следующий вид: 

```conf
config/
    common.php          # Common properties for both console and web app
    console.php         # Core options for console application
    console-local.php   # Installation-specific console options
    db.php              # Core database configuration
    db-local.php        # Installation-specific database configuration
    params.php          # Additional app parameters(core config)
    params-local.php    # Additional app parameters(installation-specific config)
    web.php             # Web-application base config
    web-local.php       # Installation-specific web-app config
```

Свойства по умолчанию переопределяются в файлах (свойство, описанное ниже, переопределяет верхнее свойство):

```conf
common.php
web.php
common-local.php
web-local.php
```
Локальные файлы конфигурации всегда переопределяют базовые настройки.

Настоятельно рекомендуем использовать для собственных настроек только файлы с суффиксом `*-local` !

## Настройка подключения к базе данных.

Если необходимо изменить учетные данные для подключения к `MySQL` серверу, сделайте это в файле `db-local.php`:

```php
<?php

return [
    // change localhost to your MySQL db hostname if it differs
    'dsn' => 'mysql:host=localhost;dbname=YOUR_DATABASE_NAME',
    'username' => 'YOUR_DATABASE_USERNAME',
    'password' => 'YOUR_DATABASE_PASSWORD',
    'charset' => 'utf8',
    'enableSchemaCache' => true,
    'schemaCacheDuration' => 3600,
    'schemaCache' => 'cache', // if you want to use another cache component for query results caching - enter it's id here
];
```

## Настройка из Backend

Начиная с версии 2.0.0-beta имеется возможность настройки некоторых модулей непосредственно из backend.

Для настройки перейдите в раздел Настройки-Конфигурация или по URL http://dotplant2.dev/config/backend/index

Данные настройки будут сохраняться в соответствующие файлы web-configurables.php, common-configurables.php, console-configurables.php

Эти настройки перекрываются только настройками из файлов *-local.php
