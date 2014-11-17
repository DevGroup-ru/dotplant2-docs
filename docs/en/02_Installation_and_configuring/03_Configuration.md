# Configuring DotPlant2

Most common configuration options are set by `install.php` script.

> Actually, all configs are the native Yii2 configs. 
> 
> So you can configure DotPlant2 CMS the same way as you [configure any Yii2 application](http://www.yiiframework.com/doc-2.0/guide-tutorial-advanced-app.html#configuration-and-environments)

Configs are stored in `application/config` directory with the following structure:

<pre>
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
</pre>

The override priorities of configs is the following(lower value - final, on the example of web app flow):

<pre>
common.php
web.php
common-local.php
web-local.php
</pre>

Local configs always override base config.

> We highly recommend you to make changes only in *-local.php configs!


## Database config

If you want to change database credentials edit db-local.php:

``` php
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


## Web application config

The main thing you need to change in web application config is the cache handler.

By default install script tries to use memcached if it is available. If it is unavailable - try to use APC cache. Otherwise - file cache.

You can read about yii2 caching in the official documentation - http://www.yiiframework.com/doc-2.0/guide-caching-data.html#cache-components