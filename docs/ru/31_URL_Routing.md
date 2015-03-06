Принципы роутинга
=====================

Роутинг в системе DotPlant2 основан на Yii2.  Основные настройки хранятся в конфиге `@app/config/web.php`
````
'urlManager' => [
    'enablePrettyUrl' => true,
    'showScriptName' => false,
    'rules' => [
        'login/<service:google_oauth|facebook|etc>' => 'default/login',
        'login' => 'default/login',
        'logout' => 'default/logout',
        'signup' => 'default/signup',
        'cart/payment-result/<id:.+>' => 'cart/payment-result',
        'search' => 'default/search',
        'robots.txt' => 'seo/manage/get-robots',
        [
            'class' => 'app\components\PageRule',
        ],
        [
            'class' => 'app\components\ObjectRule',
        ],
    ],
],

```
Подробнее об формировании `url` можно прочитать в документации Yii2 [Routing and URL Creation
](http://www.yiiframework.com/doc-2.0/guide-runtime-routing.html)

## Создание ссылок
### К категории
```
\yii\helpers\Url::to(
    [
        'product/list',
        'category_group_id' => $category->category_group_id,
        'last_category_id' => $category->id,
    ]
);

```

где  `category_group_id` — группа категории,
`last_category_id` — id категории

### К продукту
```
\yii\helpers\Url::to(
    [
        'product/show',
        'model' => $product,
        'category_group_id' => $category_group_id,
    ]
);

```
где model — объект класса `Product`,
category_group_id -


### К странице
```
\yii\helpers\Url::to(['page/show', 'id'=>$child->id])
```
где `id` - это id нужной старницы