#Пример создания собственного виджета
Подразумевается что уже создана новая тема, как описано [здесь](theme-skelet-creation.md)

В папке `application/web/theme/module/widgets` создайте папку для нового виджета `NewWidget`
В ней создайте файл `Widget.php` с содержимым
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

Рядом создайте папку `views`, а в ней файл `new-widget.php` с содержимым
```php
<div>
    new-widget-content
</div>
```

## Добавление нового виджета в интерфейсе.
Зайдите по пути `DefaultTheme/backend-configuration/index`, вкладка `Все виджеты`
Нажмите `Добавить`
Заполните поля:
- `Название`: NewWidget
- `Виджет`: app\web\theme\module\widgets\NewWidget\Widget

В разделе  `Applicable parts` выбирете в каких Частях темы будет доступен виджет.
Для примера выберите `Left Sidebar`
Нажмите `Сохранить`.

Дальше необходимо вставить виджет в одну из Вариаций темы.
Для примера вставим на `Main page`.
Переходим в DefaultTheme/backend-configuration/index вкладку Вариации темы
В строке Main page выбирите синюю кнопку Показать активные виджеты
В разделе Left sidebar будет доступен созданный виджет NewWidget
Выбирете его

После этого на главной странице в `Left Sidebar` должно отобразиться `new-widget-content`
