# Реализация своего типа свойств

Если Вы хотите создать свой тип свойств необходимо выполнить следующие действия:

#### Шаг 1. Создание структуры файлов

В директории с темой (рекомендуемая директория `application/web/theme`) создать следующую структуру:

```
properties/
	handlers/
		...
		custom/
			views/
				backend-edit.php // Представление свойства при редактировании в административной панели
				backend-render.php // Представление свойства в административной панели
				frontend-edit.php // Заполнение форм на внешней части сайта
				frontend-render.php // Представление свойства на внешней части сайта
			CustomProperty.php // Класс обработчик свойства
		...
```

#### Шаг 2. Класс обработчик

Простейший вид класса обработчика:

```php
namespace app\web\theme\properties\handlers\custom;

use app;
use app\properties\handlers\Handler;
use yii;

class CustomProperty extends Handler
{
}
```

#### Шаг 3. Представления свойства

Простейший вид представлений:

* **backend-edit.php**

```php
echo $form->field($model, $property_key.'[0]');
```

* **backend-render.php**

```php
use app\models\Property;
use kartik\helpers\Html;

if (count($values->values) == 0) {
    return;
}

$property = Property::findById($property_id);
echo Html::tag('dt', $property->name);

foreach($values->values as $val) {
    if (isset($val['value'])) {
        echo Html::tag('dd', $val['value']);
    }
}

```

* **frontend-edit.php**

```php
echo $form->field($model, $property_key);
```

* **frontend-render.php**

```php
use app\models\Property;
use kartik\helpers\Html;

if (count($values->values) == 0) {
    return;
}

$property = Property::findById($property_id);
$result = "";

foreach($values->values as $val) {
    if (isset($val['value'])) {
        if (!empty(trim($val['value']))) {
            $result.= Html::tag('dd', $val['value']);
        }
    }
}

$result = trim($result);

if (!empty($result)) {
    echo '<dl>' . Html::tag('dt', $property->name) . $result . "</dl>\n\n";
}
```

#### Шаг 4. Миграция

Создадим миграцию и применим ее:

* В консоли необходимо выполнить команду `yii migrate/create Custom_Property --migrationPath=@app/web/theme/migrations`. 
* Внесем изменения в созданную миграцию в метод up:
```php
$this->insert(\app\models\PropertyHandler::tableName(), [
    'name' => 'Custom',
    'frontend_render_view' => 'frontend-render',
    'frontend_edit_view' => 'frontend-edit',
    'backend_render_view' => 'backend-render',
    'backend_edit_view' => 'backend-edit',
    'handler_class_name' => 'app\web\theme\properties\handlers\custom\CustomProperty',
]);
```
* Применим миграцию `yii migrate --migrationPath=@app/web/theme/migrations`