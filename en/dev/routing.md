# Principles of routing

Routing in DotPlant2 based on Yii2. Basic settings are stored in config `@app/config/web.php`
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

Learn more about the formation of `url` that can be found in the Yii2 documentation [Routing and URL Creation
](http://www.yiiframework.com/doc-2.0/guide-runtime-routing.html)

## The procedure for parsing Urls

Non-static URL matching options goes through the array `rules`  from top to bottom. Static rules are obvious to those who familiar with the principles of routing in Yii2. We should also mention the `robots.txt` - it is made to `/robots.txt` could be stored in the database, edit via admin panel and make it dynamics.

Dynamic rules - that introduce to the great value and interest. 
First class - `PageRule` responsible for handling and shaping URLs of content pages. Also handle simple Url matching (see `\app\modules\page\models\Page::getByUrlPath()`). 
Second - `ObjectRule` - is responsible for all cases and grants the special features:

1. First, there is a search for a suitable PrefilteredPage
2. then climb all the rules of `route` table, process and turns

If `ObjectRule` could not find a suitable route will, it is tried to execute a standard Yii2 handler" beautiful "URLs:

* `module/controller/action`
* `module/controller[/index]`
* `controller/action`
* `controller[/index]`

### Table `route`

Table structure is simple:

| id | route | url_template | `object_id` | name |
| --- | --- | --- | --- | --- |
| 1 | shop/product/list | json here | 2 | |

* `route` - which route will be processed if the URL matches a rule
* `url_template` - encode json array
* `object_id` - id of the object (see table `object`)
* `name` - is not used in the existing code base

#### `url_template`

Consider the example of default settings for processing of goods cards (route `shop/product/show`)

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

Classes for processing Url part inherits `\app\properties\url\UrlPart`. The rest of the property - it's public properties classes (property `parameters` - announced app `\app\properties\url\UrlPart`). Instances of these classes are created using `Yii::createObject`, who cares about filling properties.

Class - one part of Urls. They do not necessarily have to execute everything, but definitely at the end of treatment `url_template` in Urls should not be just as well stay rough parts - otherwise it is considered that this option is not approached.

#### Available Url part handlers

##### `StaticPart`

Static predetermined portion of Urls in the form of string. Typically used for "dividing" several categories of groups.

###### Settings

* `static_part` - static part which is sought in Urls (in the example above - `catalog`)

##### `CategoryPart`

It is not used directly, but it serves as a basis for  `FullCategoryPathPart` and `PartialCategoryPathPart`

###### Settings

* `category_group_id`
* `include_root_category` - должен ли урл включать в себя корневую категорию

##### `FullCategoryPathPart`

Each came slug, slug considered as category. URL parsed as long as it is not going to end or for the next slug is not appropriate category.

###### Settings

* `model_category_attribute` - название атрибута, содержащего в себя привязку к категориям. В качестве категории тут может выступать только `\app\modules\shop\models\Category`

##### `PartialCategoryPathPart`

Работает аналогично `FullCategoryPathPart` т.к. реализация метода `getNextPart` находится в общем родителе - `CategoryPart`. Различия между двумя этими вариантами, только в методе `appendPart`

###### Settings

Уникальные настройки отсутствуют.

##### `ObjectSlugPart`

Look for an object (object type is listed as `object_id` in line with the current rule) by the method of `findBySlug`. Seeking only one object and, accordingly, it uses only one slug.

###### Settings

Unique missing configuration.

## Creating links

### The Category

```
\yii\helpers\Url::to(
    [
        'shop/product/list',
        'category_group_id' => $category->category_group_id,
        'last_category_id' => $category->id,
    ]
);

```

where

* `category_group_id` — category group id,
* `last_category_id` — category id

### To the product

```
\yii\helpers\Url::to(
    [
        'shop/product/show',
        'model' => $product,
        'category_group_id' => $category_group_id,
    ]
);

```
where

* `model` — an object of class `Product`,
* `category_group_id` - categories group id


### Jump to page

```
\yii\helpers\Url::to(['page/page/show', 'id' => $child->id])
```
where 

* `id` - page id is required
