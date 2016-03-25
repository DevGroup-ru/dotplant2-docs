# Написание произвольного бэкэнд контроллера

> Если у Вас мало опыта работы с Yii2 рекомендуется использовать генератор кода [Gii](http://www.yiiframework.com/doc-2.0/guide-start-gii.html).

## Шаг 1. Создание контроллера и представления

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

Чтобы Yii2 увидел наш контроллер необходимо внести следующие изменения в конфигурационный файл `theme/module/config/web.php`:
```
'modules' => [
        'site' => [
            'class' => 'app\web\theme\module\ThemeModule',
            'controllerNamespace' => 'app\web\theme\module\backend\controllers',
        ],
]
```

Результат работы контроллера можно увидеть по адресу http://URL/site/custom/

## Шаг 2. Настройка RBAC

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

### Шаг 3. Добавление ссылки в меню

Необходимо, чтобы к созданному контроллеру был доступ не только у тех кто знает прямой URL, по этому нужно создать пункт в меню административной панели. Для этого:
1. Необходимо прейти в левом меню в "Настройки / Меню админки"
2. Нажать добавить
3. Заполнить поля:
    * Название - custom
    * Маршрут - site/backend/custom/index
    * Роль RBAC - custom manage
4. Нажать сохранить

## Шаг 4. Стандартные CRUD хелперы

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
    }
    
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

Пример представления со списком моделей и кнопками удаления:
```php
<?php
use kartik\dynagrid\DynaGrid;
use yii\helpers\Html;
use app\backend\components\ActionColumn;
/*
 * @var $dataProvider yii\data\ActiveDataProvider
 * @var $searchModel app\components\SearchModel
 * @var $this yii\web\View
 */

$this->title = Yii::t('app', 'Addons categories');
$this->params['breadcrumbs'][] = $this->title;

?>
<?=
DynaGrid::widget(
    [
        'options' => [
            'id' => 'addons-categories-grid',
        ],
        'theme' => 'panel-default',
        'gridOptions' => [
            'dataProvider' => $dataProvider,
            'filterModel' => $searchModel,
            'hover' => true,
            'panel' => [
                'heading' => Html::tag('h3', $this->title, ['class' => 'panel-title']),
                'after' => Html::a(
                    \kartik\icons\Icon::show('plus') . Yii::t('app', 'Add'),
                    ['edit-category'],
                    ['class' => 'btn btn-success']
                ) .  \app\backend\widgets\RemoveAllButton::widget([
                    'url' => 'remove-all-categories',
                    'gridSelector' => '.grid-view',
                    'htmlOptions' => [
                        'class' => 'btn btn-danger pull-right'
                    ],
                ]),
            ],
        ],
        'columns' => [
            [
                'class' => \app\backend\columns\CheckboxColumn::className(),
            ],
            'id',
            'name',
            [
                'class' => ActionColumn::className(),
                'buttons' => [
                    [
                        'url' => 'edit-category',
                        'icon' => 'pencil',
                        'class' => 'btn-default',
                        'label' => Yii::t('app', 'Edit'),

                    ],
                    [
                        'url' => 'view-category',
                        'icon' => 'list',
                        'class' => 'btn-primary',
                        'label' => Yii::t('app', 'View'),

                    ],
                    [
                        'url' => 'delete-category',
                        'icon' => 'trash-o',
                        'class' => 'btn-danger',
                        'label' => Yii::t('app', 'Delete'),
                    ],
                ]
            ],
        ],
    ]
);
?>
```

## Пример готового кода контроллера
```php
<?php

namespace app\modules\shop\backend;

use app\backend\components\BackendController;
use app\modules\shop\models\Addon;
use app\traits\LoadModel;
use app\backend\traits\BackendRedirect;
use app\backend\actions\MultipleDelete;
use app\backend\actions\DeleteOne;
use app\modules\shop\models\AddonCategory;
use Yii;
use yii\filters\AccessControl;
use yii\web\NotFoundHttpException;

/**
 * Backend controller for managing addons, their categories and bindings
 * @package app\modules\shop\backend\
 */
class AddonsController extends BackendController
{
    use BackendRedirect;
    use LoadModel;
    /**
     * @inheritdoc
     * @return array
     */
    public function behaviors()
    {
        return [
            'access' => [
                'class' => AccessControl::className(),
                'rules' => [
                    [
                        'allow' => true,
                        'roles' => ['product manage'],
                    ],
                ],
            ],
        ];
    }

    /**
     * @inheritdoc
     */
    public function actions()
    {
        return [
            'remove-all-categories' => [
                'class' => MultipleDelete::className(),
                'modelName' => AddonCategory::className(),
            ],
            'delete-category' => [
                'class' => DeleteOne::className(),
                'modelName' => AddonCategory::className(),
            ],
        ];
    }

    /**
     * Renders AddonCategory grid
     * @return string
     */
    public function actionIndex()
    {
        $searchModel = new AddonCategory();
        $params = Yii::$app->request->get();

        $dataProvider = $searchModel->search($params);

        return $this->render(
            'index',
            [
                'searchModel' => $searchModel,
                'dataProvider' => $dataProvider,
            ]
        );
    }

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
    }


}
```
