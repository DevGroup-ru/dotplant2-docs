# Тегированный кэш


При работе с CMS DotPlant2 у разработчика есть возможность уменьшить нагрузка на сервер, за счет использования кэша. В то-же время, правильная настройка кэша позволяет предоставлять пользователю сайта получать всегда актуальную информацию. Кэш  DotPlant2 основан на [кэше от Yii2](https://github.com/yiisoft/yii2/blob/master/docs/guide-ru/caching-data.md)

### Настройка кэша

1. Редактировать фаил `@confifg/web-local` Прописать в `components` настройки кэша, к примеру для `FileCache` Настройки будут такими

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



### Создание кэша

Для создания кэша необходимо
1. Иметь уникальное имя кэша
2. Данные


Так же желательно, указать от каких данных зависит заданный кэш. Пример создания кэша.
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

В данном случае, в качестве  $cacheKey, выступает уникальное текстовое выражение `$cacheKey = static::tableName() . ":$id";`

В качестве данных у нас используются переменная  $model

Кэш сохраняется на  86400 с

Далее, указывается что кэш зависит от AR класса, и его связей.



### Получение кэша

Далее будет приведен пример получения кэша по по ключу.

```
$cacheKey = static::tableName() . ":$id";
Yii::$app->cache->get($cacheKey);
```