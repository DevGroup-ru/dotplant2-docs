# Writing an arbitrary backend controller

> If you have little experience with Yii2 recommended to use a code generator [Gii](http://www.yiiframework.com/doc-2.0/guide-start-gii.html).

## Step 1: Create a controller and view
   
Create `theme/module/backend/controllers/CustomController.php` with the following contents

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

Create `theme/module/backend/views/custom/index.php` with the following contents

```php
<?= $content ?>
```

To make Yii2 use that controller you must make the following changes to the configuration file `theme/module/config/web.php`:
```
'modules' => [
        'site' => [
            'class' => 'app\web\theme\module\ThemeModule',
            'controllerNamespace' => 'app\web\theme\module\backend\controllers',
        ],
]
```

The controller results can be seen at http://URL/site/custom/

## Step 2: Configure RBAC

In order to see this page, only users who have the right to view it, follow these steps:
* Create `custom manage` permission and assign the necessary roles permission. How to do this can be found [here] (Users_And_Roles)
* In the controller add method

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

### Step 3: Add links to the menu

It is necessary to create a controller to have access not only to those who know the URL direct, on the need to create an item in the menu of administrative panel. For this:
1. It is necessary to move to the left-hand menu in the "Settings / admin menu"
2. Press the add
3. Fill in the fields:
    * Title - custom
    * Route - site / backend / custom / index
    * Role RBAC - custom manage
4. Press to save

## Step 4. Standard CRUD helper

DotPlant2 provides a special helper class, accelerating the implementation of standard CRUD operations:
- BackendRedirect - trait used to correct redirect when editing/creating the item. Used in conjunction with `\app\backend\components\Helper::saveButtons`
- MultipleDelete & DeleteOne - Special Action for single and multiple delete records.

Adding necessary classes in scope:

```php
use app\backend\traits\BackendRedirect;
use app\backend\actions\MultipleDelete;
use app\backend\actions\DeleteOne;
```

Inside the class body we use our traits:
```php
use BackendRedirect;
use LoadModel;
```

## An example of using these two traits:
   
The controller creates a universal action, which will be used to add or edit an existing entry:

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

This action will use trait loadModel. In the absence of the parameter `id` - a new instance of the model`AddonCategory` will be created. If the parameter `id` set - will be found a model or aborted exception (`NotFoundException`).

It is also used after a successful save trait `redirectUser`, which redirects the user either to the previous page (category list), or to create a new model, or edit creation.

To be successful in the `redirectUser` representation instead of the normal submit buttons you must use a special helper:

```php
<?= \app\backend\components\Helper::saveButtons($model) ?>
```

This helper displays three buttons 4:
- **Back** - Go back without saving (on the list)
- **Save & Go next** - to maintain and create a new record. This button does not appear when you edit an existing entry, only when you create a new one.
- **Save & Go back** - save and back (on the list)
- **Save** - save and continue editing the current model

Example presentation with a list of models and the delete button:

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

## An example of a finished controller code

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
