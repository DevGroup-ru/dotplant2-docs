# Использование собственной темы

Для использования собственной темы и сайтоспецефичных особенностей выделена директория `application/web/theme/`, которая добавлена в `.gitignore` и не попадет в основной репозиторий. Для удобства резработки мы рекомендуем создать отдельный репозиторий с Вашей темой и сайтоспецефичными возможностями внутри указанной директории.

Например, можно использовать подобную структуру:

```
theme/
	module/ // модуль сайтоспецифичного функционала
		components/
		config/ // конфиг-файлы для конкретного сайта и сайтоспецифичных реализаций
			common.php
			console.php
			db.php
			web.php 
		controllers/
			...
			DefaultController.php
			...
		views/ // представления сайто-специфичных контроллеров
			...
			default
				...
				index.php
				...
		Module.php
	resources/ // web ресурсы (js скрипты, изображения, стили и прочее)
		css/
		img/
		js/
		less/
		product-images/ // каталог для загрузки изображения товаров (должен иметь разрешение на запись файлов)
		sliders/ // каталог для загрузки изображений слайдера (должен иметь разрешение на запись файлов)
		sass/
		index.html // пустой файл
	views/  // представления, переопределяющие базовые представления CMS
		...
		default/
			...
			index.php
			...
		layouts/
			...
			main.php
			...
		product/
			...
			list.php
			show.php
			...
		...
	index.html // пустой файл
```

Для установки темы и сайтоспецефичного функционала необходимо в файле конфигураций задать путь к теме и модулю. Это можно сделать в файле `theme/module/config/web.php`.


```
'defaultRoute' => 'site/default/index',
'components' => [
    'view' => [
        'theme' => [
            'pathMap' => [
                '@app/views' => '@webroot/theme/views',
            ],
            'baseUrl' => '@webroot/theme/views',
        ],
    ],
],
'modules' => [
    'site' => [
        'class' => 'app\web\theme\module\Module',
    ],
],
```

## Сайтоспецифичный функционал

Сайтоспецифичный функционал желательно реализовывать в рамках пространства имён `app\web\theme\module`.

Пример файла `Module.php`:

``` php

<?php

namespace app\web\theme\module;

use Yii;

class Module extends \yii\base\Module
{

}

```