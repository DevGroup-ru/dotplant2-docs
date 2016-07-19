# Implementation of type properties

If you want to create your own type of properties, do the following:

#### Step 1: Create the file structure

In the directory with the theme (recommended directory is `application/web/theme`) to create the following structure:

```
properties/
	handlers/
		...
		custom/
			views/
				backend-edit.php // Presentation of the properties while editing in the administration panel
				backend-render.php // Presentation of the properties in the administrative panel
				frontend-edit.php // Filling out forms on the outside of the site
				frontend-render.php // Presentation of the properties on the outer part of the site
			CustomProperty.php // handler class properties
		...
```

#### Step 2. Class handler

The simplest form handler class:

```php
namespace app\web\theme\properties\handlers\custom;

use app;
use app\properties\handlers\Handler;
use yii;

class CustomProperty extends Handler
{
}
```

#### Step 3. View Properties
     
The simplest form of representations:

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

#### Step 4: Migration
     
Create migration and it is applicable:
     
* In the console, you must run `yii migrate/create Custom_Property --migrationPath=@app/web/theme/migrations`. 
* Make changes in migration created in up method:
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
* Apply migration `yii migrate --migrationPath=@app/web/theme/migrations`