# Writing extensions for DotPlant2 the example of a WYSIWYG-editor

Herein, we describe the creation of a simple extension - WYSIWYG-editor of the backend-DotPlant2.

## Setting up the environment

Since the expansion DotPlant2 are composer-packs, it's best to use for the development of [studio](https://packagist.org/packages/franzl/studio).

Installing the studio:
```
composer global require franzl/studio
```

Now we go to the application folder of our application and create composer-package:

```
[12:37:57][~/git-my/dotplant2/application]on(master)$ studio create ../../dotplant-wysiwyg-ckeditor
Please name this package dotplant/wysiwyg-ckeditor
Please provide a default namespace (PSR-4) [Dotplant\Wysiwyg-ckeditor] DotPlant\CKEditor
Do you want to set up PhpUnit as a testing tool? [y|N]
Do you want to set up PhpSpec as a testing tool? [y|N]
Do you want to set up TravisCI as continuous integration tool? [y|N]
Package directory ../../dotplant-wysiwyg-ckeditor created.
Running composer install for new package...
Package successfully created.
Dumping autoloads...
Autoloads successfully generated.
```

Team `studio create` reates composer-pack in the directory two levels higher application and registers the package in a special autoloader-e.

There will also be asked to specify the package name and the default namespace.

## composer.json

In this case, our expansion will use a ready-made implementation CKEditor widget yii2, so you must specify the appropriate package as a dependency, and at the same time add descriptions composer.json:

``` json

{
    "name": "dotplant/wysiwyg-ckeditor",
    "description": "CKEditor WYSIWYG widget for DotPlant2 backend",
    "type": "dotplant2-extension",
    "minimum-stability": "dev",
    "require": {
        "mihaildev/yii2-ckeditor": "~1.0.0"
    },
    "autoload": {
        "psr-4": {
            "DotPlant\\CKEditor\\": "src/"
        }
    }
}

```

## Expansion Module

Extensions DotPlant2 - are modules. Extensions do not have to provide any additional controllers. However, it is necessary to correct the automatic installation.

Studio created the file `src/Example.php`. for us. Rename it to `Module` and define our module:

```

```

> File Class Expansion Module DotPlant2 necessarily be called Module, expand `app\components\ExtensionModule` и and override the static variable `$moduleId`

## Migration
   
Available in backend WYSIWYG-editors are described in the table `{{%wysiwyg}}`. To add a new editor is simple to add a new record to the table and reset commonTag cache model.
                                                                                
Extensions DotPlant2 may have their own migration. As a rule, they are located in `src/migrations/`.

Create an empty migration in our expansion (working directory - application directly by the CMS, the way to expand above):

```
[12:46:51][~/git-my/dotplant2/application]on(master *)$ ./yii migrate/create --migrationPath=../../dotplant-wysiwyg-ckeditor/src/migrations initial
Yii Migration Tool (based on Yii v2.0.6-dev)

Create new migration '../../dotplant-wysiwyg-ckeditor/src/migrations/m150720_094657_initial.php'? (yes|no) [no]:y
New migration created successfully.

```

Now we have a new blank migration. Edit the file to add to the addition and removal (if migrate/down) the desired item:

``` php
    public function up()
    {
        $this->insert('{{%wysiwyg}}', [
            'name' => 'CKEditor',
            'class_name' => 'mihaildev\ckeditor\CKEditor',
            'params' => yii\helpers\Json::encode([
                'editorOptions' => [
                    'preset' => 'full',
                ],
            ]),
            'configuration_model' => 'app\modules\core\models\WysiwygConfiguration\CKEditor',
            'configuration_view' => '@ckeditor/views/ckeditor-config.php',
        ]);

        $this->insert(
            '{{%configurable}}',
            [
                'module' => 'WysiwygCKEditor',
                'sort_order' => 99,
                'section_name' => 'CKEditor WYSIWYG',
                'display_in_config' => 0,
            ]
        );
    }

    public function down()
    {
        $this->delete('{{%wysiwyg}}', ['name' => 'CKEditor']);
        $this->delete('{{%configurable}}', ['module' => 'WysiwygCKEditor']);
    }
```

The structure of the recording wysiwyg:
- **name** - editor's name is displayed in WYSIWYG selection list in the settings
- **class_name** - the name of the widget class, the widget should extend `yii\widgets\InputWidget`
- **params** - the parameters of the widget, packed in json
- **configuration_model** - the name of the model class configuration widget
- **configuration_view** - the way to a widget configuration

