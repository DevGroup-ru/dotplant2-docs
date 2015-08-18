# Web application configuration

`DotPlant2` setting all main settings during installation.

This is a common `Yii2` configuration.

You can tune `DotPlant2` as any other `Yii2` application.

Configuration files are locating in `application/config` directory.

Config directory structure:

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

Default properties will be overridden in files (sorted by priority):

```conf
common.php
web.php
common-local.php
web-local.php
```

Local configuration files always overwrite a base settings.

For your settings we strongly recommend to use configuration files with suffix `*-local.php` only.


## DataBase connection

If you need to change MySQL DB connection parameters you should do it in `application/config/db-local.php` file:

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

## Backend settings

Since 2.0.0-beta version DotPlant2 has ability to configure some modules via backend.

For tuning go to `Settings/Config` administrative partition or to url `YOUR_HOST/config/backend/index`.

Data will have been saved to `web-configurables.php`, `common-configurables.php`, `console-configurables.php` files.

This settings may be overloaded by `*-local.php` configuration files only.

**Important** For correct working you should tuning [background tasks](user/background-tasks.md).
