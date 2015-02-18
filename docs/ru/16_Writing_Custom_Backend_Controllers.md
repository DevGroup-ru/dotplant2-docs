Написание произвольного бэкэнд контроллера
===========================

Для создания произвольного бэкэнд контроллера необходимо создать [вложенный модуль](http://www.yiiframework.com/doc-2.0/guide-structure-modules.html#nested-modules) Backend в пространстве имён `app\web\theme\module`. Если у Вас мало опыта работы с Yii2 рекомендуется использовать генератор кода [Gii](http://www.yiiframework.com/doc-2.0/guide-start-gii.html).

### Шаг 1. Создание модуля

Рекомендуемая структура модуля:
```
backend\
	controlles\
		...
		CustomController.php
		...
	views\
		...
		custom\
			...
			index.php
			...
		...
	Backend.php
```

### Шаг 2. Настройка модуля

Для того чтобы Yii2 увидел наш модуль необходимо внести следующие изменения:

* В файле `theme/module/Module.php` после строки `parent::init();` добавить следущие строки

```
$this->modules = [
    'backend' => [
        'class' => 'app\web\theme\module\backend\Backend',
        'layout' => '@app/backend/views/layouts/main',
    ],
];
```
* Файл `theme/module/backend/Backend.php` привести к следующему виду

```
<?php

namespace app\web\theme\module\backend;

use app\backend\BackendModule;

class Backend extends BackendModule
{
    public $defaultRoute = 'dashboard/index';
    public $administratePermission = 'administrate';

    public function init()
    {
        parent::init();        
    }
}
```

### Шаг 3. Создание контроллера и представления

Создаем `theme/module/backend/controllers/CustomController.php` со следующим содержимым

```
<?php

namespace app\web\theme\module\backend\controllers;

use Yii;
use yii\web\Controller;

class CustomController extends Controller
{
    public function actionIndex()
    {
        return $this->render('index', [
		'content' => 'CustomController index action',
        ]);
    }
}
```

Создаем `theme/module/backend/views/custom/index.php` со следующим содержимым

```
<?= $content ?>
```

Результат работы контроллера можно увидеть по адресу http://URL/site/backend/custom/

### Шаг 4. Настройка RBAC

Для того, чтобы увидеть эту страницу могли только те пользователи, у которых есть права на ее просмотр необходимо выполнить следующие шаги:
* Создать разрешение `custom manage` и назначить необходимым ролям это разрешение. Как это сделать, можно узнать [тут](Users_And_Roles)
* В контроллер добавить метод 

```
public function behaviors()
{
	return [
	    'access' => [
    		'class' => \yii\filters\AccessControl::className(),
    		'rules' => [
    		    [
    		        'allow' => true,
    		        'roles' => ['custom manage'],
    		    ],
    		],
	    ],
	];
}
```

### Шаг 5. Добавление ссылки в меню

Необходимо, чтобы к созданному контроллеру был доступ не только у тех кто знает прямой URL, по этому нужно создать пункт в меню административной панели. Для этого:
1. Необходимо прейти в левом меню в "Настройки / Меню админки"
2. Нажать добавить
3. Заполнить поля:
    * Название - custom
    * Маршрут - site/backend/custom/index
    * Роль RBAC - custom manage
4. Нажать сохранить
