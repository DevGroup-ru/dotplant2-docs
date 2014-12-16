# Использование собственной темы

Для использования собственной темы и сайтоспецефичных особенностей выделена директория `application/web/theme/`, которая добавлена в `.gitignore` и не попадет в основной репозиторий. Для удобства резработки мы рекомендуем создать отдельный репозиторий с Вашей темой и сайтоспецефичными возможностями внутри указанной директории.

Например, можно использовать подобную структуру:

```
theme/
	module/ // модуль сайтоспецефичного функционала
		components/
		config/
			common.php
			console.php
			db.php
			web.php
		controllers/
			...
			DefaultController.php
			...
		themes/
			...
			mytheme/
				views/
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
			...
		views/
			...
			default
				...
				index.php
				...
		.htaccess // файл закрывающий доступ к директории модуля (содержит всего одну строку `Deny from All`)
		Module.php
	resources/ // web ресурсы (js скрипты, изображения, стили и прочее)
		css/
		img/
		js/
		less/
		product-images/ // каталог для загрузки изображения товаров (должен иметь разрешение на запись файлов)
		sass/
		index.html // пустой файл
	index.html // пустой файл
```

Для установки темы и сайтоспецефичного функционала необходимо в файле конфигураций задать путь к теме и модулю.


```
'defaultRoute' => 'site/default/index',
'components' => [
    'view' => [
        'theme' => [
            'pathMap' => [
                '@app/views' => '@webroot/theme/module/themes/mytheme',
            ],
            'baseUrl' => '@webroot/theme/module/themes/mytheme',
        ],
    ],
],
'modules' => [
    'site' => [
        'class' => 'app\web\theme\module\Module',
    ],
],
```

Подробнее о файлах конфигурации и правилах их загрузки можно прочитать [здесь](configuration_files).