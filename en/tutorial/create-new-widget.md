# Example of creating a custom widget

It is understood that the already established a new theme, as described [here](theme-skelet-creation.md)

In the folder `application/web/theme/module/widgets` create a folder for the new widget `NewWidget`

In `Widget.php` create a file with the contents of

```php
<?php

namespace app\web\theme\module\widgets\NewWidget;

use app\extensions\DefaultTheme\components\BaseWidget;

class Widget extends BaseWidget
{
    public function widgetRun()
    {
        return $this->render('new-widget');
    }
}
```

Next, create a folder `views`, and fill in the file `new-widget.php`  with content

```php
<div>
    new-widget-content
</div>
```

## Adding a new widget interface

Go along the path of `DefaultTheme/backend-configuration/index`, tab `All Widgets`
Press `Add`
Fill in the fields:
- `name`: NewWidget
- `widget`: app\web\theme\module\widgets\NewWidget\Widget

In the `Applicable parts` choose the public part of the topic will be available widget.
For example, select `Left Sidebar`
Press `Save`.

Then you need to insert a widget in one of the variations of the theme.
For example, to insert the `Main page`.
Go to the `DefaultTheme/backend-configuration/index` tab `Theme Variations`
In line `Main Page` select a blue button `Show active widgets`
In the `Left sidebar` section will be available created widget `NewWidget`
Choose this

Then on the main page in the `Left Sidebar` should be displayed
```
new-widget-content
```
