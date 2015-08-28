# Написание произвольного бэкэнд контроллера

Для создания произвольного бэкэнд контроллера необходимо создать [вложенный модуль](http://www.yiiframework.com/doc-2.0/guide-structure-modules.html#nested-modules) Backend в пространстве имён `app\web\theme\module`. Если у Вас мало опыта работы с Yii2 рекомендуется использовать генератор кода [Gii](http://www.yiiframework.com/doc-2.0/guide-start-gii.html).

## Шаг 1. Создание модуля

Рекомендуемая структура модуля:
```php
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

## Шаг 2. Настройка модуля

Для того чтобы Yii2 увидел наш модуль необходимо внести следующие изменения:

* В файле `theme/module/Module.php` после строки `parent::init();` добавить следущие строки

```php
$this->modules = [
    'backend' => [
        'class' => 'app\web\theme\module\backend\Backend',
        'layout' => '@app/backend/views/layouts/main',
    ],
];
```
* Файл `theme/module/backend/Backend.php` привести к следующему виду

```php
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

## Шаг 3. Создание контроллера и представления

Создаем `theme/module/backend/controllers/CustomController.php` со следующим содержимым

```php
<?php

namespace app\web\theme\module\backend\controllers;

use Yii;
use app\backend\components\BackendController;

class CustomController extends BackendController
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

```php
<?= $content ?>
```

Результат работы контроллера можно увидеть по адресу http://URL/site/backend/custom/

## Шаг 4. Настройка RBAC

Для того, чтобы увидеть эту страницу могли только те пользователи, у которых есть права на ее просмотр необходимо выполнить следующие шаги:
* Создать разрешение `custom manage` и назначить необходимым ролям это разрешение. Как это сделать, можно узнать [тут](Users_And_Roles)
* В контроллер добавить метод 

```php
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

## Шаг 6. Стандартные CRUD хелперы

DotPlant2 предоставляет специальные хелперы, ускоряющие реализацию стандартных CRUD операций:
- BackendRedirect - трейт, используемый для правильной переадресации при редактировании/создании элемента. Используется совместно с `\app\backend\components\Helper::saveButtons`
- MultipleDelete & DeleteOne - специальные экшены для множественного и единичного удаления записей.

Добавялем необходимые классы в область видимости:

```php
use app\backend\traits\BackendRedirect;
use app\backend\actions\MultipleDelete;
use app\backend\actions\DeleteOne;
```

Внутри тела класса используем наши трейты:
```php
use BackendRedirect;
use LoadModel;
```

## Пример использования этих двух трейтов:

В контроллере создаем универсальный экшен, который будет использоваться для добавления и редактирования существующей записи:

```php
    /**
     * Add or edit existing AddonCategory model
     * @param null|string $id
     * @return string|\yii\web\Response
     * @throws NotFoundHttpException
     */
    public function actionEditCategory($id=null)
    {
        $model = $this->loadModel(AddonCategory::className(), $id, true);

        if ($model->load(Yii::$app->request->post()) && $model->validate()) {
                if ($model->save()) {
                    return $this->redirectUser($model->id, true, 'index', 'edit-category');
                }
        }
        return $this->render(
            'edit-category',
            [
                'model' => $model,
            ]
        );
    
```
В данном экшене используется трейт loadModel. При отсутствии параметра `id` - будет создан новый экземпляр модели `AddonCategory`. Если параметр `id` задан - будет найдена модель или выкинуто исключение(`NotFoundException`).

Также после удачного сохранения используется трейт `redirectUser`, который перенаправляет пользователя либо к предыдущей странице(список категорий), либо на создание новой модели, либо на редактирование созданой.

Для успешной работы `redirectUser` в представлении вместо обычных submit кнопок необходимо использовать специальный хелпер:

```php
<?= \app\backend\components\Helper::saveButtons($model) ?>
```
Этот хелпер выведет три 4 кнопки:
- **Back** - Вернуться назад без сохранения(на список)
- **Save & Go next** - сохранить и создать новую запись. Данная кнопка не появляется при редактировании уже существующей записи, только при создании новой.
- **Save & Go back** - сохранить и вернуться назад(на список)
- **Save** - сохранить и продолжить редактирование текущей модели

