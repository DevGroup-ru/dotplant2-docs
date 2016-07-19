# Tagged cache


When working with CMS DotPlant2 developer has the ability to reduce the load on the server, through the use of the cache. At the same time, the correct cache configuration allows the user to provide a site always get the latest information. DotPlant2 cache is based on [Yii2 cache] (https://github.com/yiisoft/yii2/blob/master/docs/guide-ru/caching-data.md)

### Configuring cache

1. Edit `@config/web-local` file registered in `components` cache settings, for example `FileCache` settings are such

```
'components' =>
    [
       ...
        'cache' =>
            [
                'class' => 'yii\caching\FileCache',
            ],
    	...
        ],
```



### Creating a cache

To create a cache is necessary
1. Have a unique cache name
2. Data


Also, it is desirable to specify what data from the specified cache depends. Example of creating a cache.
````
$cacheKey = static::tableName() . ":$id";

Yii::$app->cache->set(
    $cacheKey,
    $model,
    86400,
    new TagDependency([
        'tags' => [
            \devgroup\TagDependencyHelper\ActiveRecordHelper::getCommonTag(static::className())
        ]
    ])
);
````

In this case, as the $cacheKey, stands a unique expression of the text `$cacheKey = static::tableName() . ":$id";`

As the data we used $model variable

The cache is stored for 86400 seconds

Next, it indicates that the cache depends on the AR class, and its connections.



### Getting cache

Next, an example will be receiving on the cache key.

```
$cacheKey = static::tableName() . ":$id";
Yii::$app->cache->get($cacheKey);
```