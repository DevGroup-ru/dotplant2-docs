# Написание расширения для DotPlant2 на примере WYSIWYG-редактора

В данном документе мы опишем создание простого расширения - WYSIWYG-редактора для backend-части DotPlant2.

## Настройка окружения

Поскольку расширения в DotPlant2 представляют собой composer-пакеты, удобнее всего для разработки использовать [studio](https://packagist.org/packages/franzl/studio).

Установка studio:
```
composer global require franzl/studio
```

Теперь переходим в папку application нашего приложения и создаем composer-пакет:

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

Команда `studio create` создает composer-пакет в директории на два уровня выше application и прописывает этот пакет в специальном autoloader-е.

Также будет предложено указать название пакета и namespace по-умолчанию.

## composer.json

В данном случае наше расширение будет использовать уже готовую реализацию виджета CKEditor для yii2, поэтому необходимо указать нужный пакет в качестве зависимости и заодно добавить описания в composer.json:

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

## Модуль расширения

Расширения в DotPlant2 - это модули. Расширениям вовсе не обязательно предоставлять какие-то дополнительные контроллеры. Однако это необходимо для корректной автоматической установки.

studio создал для нас файл `src/Example.php`. Переименуем его в `Module` и определим наш модуль:

```

```

> Файл-класс модуля расширения DotPlant2 обязетельно должен называться Module, расширять `app\components\ExtensionModule` и переопределять статическую переменную `$moduleId`

## Миграции

Доступные в backend WYSIWYG-редакторы описываются в таблице `{{%wysiwyg}}`. Для добавления нового редактора достаточно просто добавить новую запись в эту таблицу и сбросить commonTag кеш модели.

Расширения DotPlant2 могут иметь собственные миграции. Как правило, они располагаются в папке `src/migrations/`.

Создадим пустую миграцию в нашем расширении(рабочая директория - application непосредственно самой CMS, путь до расширения указан выше):

```
[12:46:51][~/git-my/dotplant2/application]on(master *)$ ./yii migrate/create --migrationPath=../../dotplant-wysiwyg-ckeditor/src/migrations initial
Yii Migration Tool (based on Yii v2.0.6-dev)

Create new migration '../../dotplant-wysiwyg-ckeditor/src/migrations/m150720_094657_initial.php'? (yes|no) [no]:y
New migration created successfully.

```

Теперь у нас есть новая пустая миграция. Отредактируем файл, добавив туда добавление и удаление(при migrate/down) необходимой записи:

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

Структура записи wysiwyg:
- **name** - название редактора, отображается в списке выбора WYSIWYG в настройках
- **class_name** - название класса виджета, виджет должен расширять `yii\widgets\InputWidget`
- **params** - параметры виджета, упакованные в json
- **configuration_model** - название класса модели конфигурации виджета
- **configuration_view** - путь до представления конфигурации виджета

