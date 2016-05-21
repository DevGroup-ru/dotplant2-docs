# Принципы роутинга

Роутинг в DotPlant2 основан на Yii2.  Основные настройки хранятся в конфиге `@app/config/web.php`
```
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

## Порядок разбора урла

Матчинг для нестатичных вариантов URL идёт по массиву `rules` сверху вниз. Статичные правила очевидны для тех, кто знаком с принципами роутинга в Yii2. Отдельно стоит упомянуть `robots.txt` - это сделано, чтобы `/robots.txt` можно было бы хранить в базе, редактировать через админку и отдавать динамикой.

Динамические правила - то, что предстовляет собой наибольшую ценность и интерес. Первый класс - `PageRule` отвечает за обработку и формирование урлов контентных страниц. Матчинг элементарный - по урлу (см `\app\modules\page\models\Page::getByUrlPath()`). Второй - `ObjectRule` - отвечает за все особои случаи и возможности:

1. сначала идёт поиск подходящей PrefilteredPage
2. затем забираются все правила из таблицы `route` и обрабытаваются по очереди

Если и `ObjectRule` не удалось найти подходящий роут, пробуется выполнится стандартный для  Yii2 обработчик "красивых" урлов:

* `module/controller/action`
* `module/controller[/index]`
* `controller/action`
* `controller[/index]`

### Таблица `route`

Структура таблицы элементарна:

| id | route | url_template | `object_id` | name |
| --- | --- | --- | --- | --- |
| 1 | shop/product/list | json here | 2 | |

* `route` - какой роут будет обработан в случае если URL соответствует правилу
* `url_templalte` - закодированны json массив
* `object_id` - id объекта (смотри таблицу `object`)
* `name` не используются в существующей кодовой базе

#### `url_template`

Рассмотрим на примере дефолтных настроек для обработки карточек товара (роут `shop/product/show`)

```
[
    {
        "class": "app\\properties\\url\\StaticPart",
        "static_part": "catalog",
        "parameters": {
            "category_group_id": 1
        }
    },
    {
        "class": "app\\properties\\url\\FullCategoryPathPart",
        "category_group_id": 1
    },
    {
        "class": "app\\properties\\url\\ObjectSlugPart"
    }
]
```

Классы для обработки частей урла - наследники `\app\properties\url\UrlPart`. Остальные свойства - это публичные свойства классов (свойство `parameters` - объявлено в `\app\properties\url\UrlPart`). Экземпляры этих классов создаются с помощью `Yii::createObject`, который и заботится о заполнении свойств.

1 класс - одна часть урла. Они не обязательно должны выполнится все, но обязательно в конце обработки `url_template` в урле не должно остатся необработанных частей - иначе считается, что этот вариант не подошёл.

#### Доступные обработчики частей урла

##### `StaticPart`

Статично заданная часть урла в виде строки. Обычно используется для "разруливания" нескольких групп категорий.

###### Настройки

* `static_part` - статичная часть, которая ищется в урле (в примере выше - `catalog`)

##### `CategoryPart`

Напрямую нигде не используется, но служит в качестве основы для `FullCategoryPathPart` и `PartialCategoryPathPart`

###### Настройки

* `category_group_id`
* `include_root_category` - должен ли урл включать в себя корневую категорию

##### `FullCategoryPathPart`

Каждый пришедший слаг, считает слагом категории. Разбирает урл до тех пор, пока он не кончится или для очередного слага не будет подходящей категории.

###### Настройки

* `model_category_attribute` - название атрибута, содержащего в себя привязку к категориям. В качестве категории тут может выступать только `\app\modules\shop\models\Category`

##### `PartialCategoryPathPart`

Работает аналогично `FullCategoryPathPart` т.к. реализация метода `getNextPart` находится в общем родителе - `CategoryPart`. Различия между двумя этими вариантами, только в методе `appendPart`

###### Настройки

Уникальные настройки отсутствуют.

##### `ObjectSlugPart`

Ищет объект (тип объекта указан в качестве `object_id` в строке с текущим правилом) с помощью метода `findBySlug`. Ищет только 1 объект и соответственно использует только 1 слаг.

###### Настройки

Уникальные настройки отсутсвуют.

## Создание ссылок

### К категории

```
\yii\helpers\Url::to(
    [
        'shop/product/list',
        'category_group_id' => $category->category_group_id,
        'last_category_id' => $category->id,
    ]
);

```

где

* `category_group_id` — группа категории,
* `last_category_id` — id категории

### К продукту

```
\yii\helpers\Url::to(
    [
        'shop/product/show',
        'model' => $product,
        'category_group_id' => $category_group_id,
    ]
);

```
где

* `model` — объект класса `Product`,
* `category_group_id` - id группы категорий


### К странице

```
\yii\helpers\Url::to(['page/page/show', 'id' => $child->id])
```
где `id` - это id нужной старницы
