# Use your own theme

To use your own theme and site specific features, take a look on `application/web/theme/` directory, which is added to the `.gitignore` and not fall into the main repository. For convenience design, we recommend that you create a separate repository to your topic and site specific functionality within the specified directory.

To create the basic structure, you can use the console command `./yii admin/create-theme`

For example, such a structure can be used:

```
theme/
	module/ // site specific functional module
		components/
		config/ // config files for a particular site
			common.php
			console.php
			db.php
			web.php 
		controllers/
			...
			DefaultController.php
			...
		views/ // presenting site-specific controllers
			...
			default
				...
				index.php
				...
		Module.php
	resources/ // web resources (js scripts, images, styles, etc.)
		css/
		img/
		js/
		less/
		product-images/ // directory to upload product images (must have permission to write files)
		sliders/ // download directory slider images (must have permission to write files)
		sass/
		index.html // empty file
	views/  // representation override basic understanding CMS
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
	index.html // empty file
```

To install the theme and site specific functional configurations, it is necessary to specify the file path to the theme and module. This can be done in the file `theme/module/config/web.php`.


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

Also, be sure to check with a special [guidance on working with resources and AssetBundle](../tutorial/asset-bundle.md).

## Site specific functionality

Site specific functionality is desirable to implement as part of the namespace  `app\web\theme\module`.

Example file `Module.php`:

``` php

<?php

namespace app\web\theme\module;

use Yii;

class Module extends \yii\base\Module
{

}

```