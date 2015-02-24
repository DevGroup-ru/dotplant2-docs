Фильтрация по свойствам
==========================


В CMS DotPlant2 существует seo-ореентированный функционал фильтрации товаров по свойствам.  Для этого необходимо настроить свойства и вставить в представление код  виджета фильтра.

### Настройка свойств

1.  Создание группы свойств на странице свойства — свойства (/backend/properties/index)
	* Название — произвольное текстовое значение
	* Объект — Product
2. Создание свойств в определенной группе
    * Зайти на страницу конкретной группы свойств и нажать на кнопку «добавить»
    	- **Название** — произвольное текстовое значение
    	- **Ключ** — произвольное текстовое значение содержащие знаки 0-9a-z_
    	- **Тип значения** — выбрать в зависимости от типа данных в свойствах ( текстовое или числовое )
    	- **Тип свойства** — выбрать пункт select
    	- Выбрать пункт **Статичные значения**
    	- Выбрать пункт **Хранить урл-ы в значениях**
    	- Сохранить
    * Добавить Статичные значения
    	- **Название** -  произвольное текстовое значение
    	- **Значение** -  произвольное текстовое значение
    	- **Урл** — произвольное текстовое значение содержащие знаки 0-9a-z_
    	- Сохранить
3. Редактировать вручную (через phpmyadmin\adminer и.т.д.) запись таблицы **route**, где поле **route = product/list** . Это необходимо для правильного формирования url в фильтре. Поле для редактирования **url_template** представляет из себя json- массив.
	- Редактировать поле  **url_template** , вставить строку со следующим кодом `"class":"app\\properties\\url\\PropertyPart","property_id":"ID"},`  где ID — id  нужного свойства, который будет участвовать в фильтрации.

    #####Примерный вид поля **url_template**  после редактирования

    `[{"class":"app\\properties\\url\\StaticPart","static_part":"catalog","parameters":{"category_group_id":1}},{"class":"app\\properties\\url\\PropertyPart","property_id":1},{"class":"app\\properties\\url\\PartialCategoryPathPart","category_group_id":1},{"class":"app\\properties\\url\\PropertyPart","property_id":"8"},{"class":"app\\properties\\url\\PropertyPart","property_id":"10"},{"class":"app\\properties\\url\\PropertyPart","property_id":"75"},{"class":"app\\properties\\url\\PropertyPart","property_id":"97"},{"class":"app\\properties\\url\\PropertyPart","property_id":"5"},{"class":"app\\properties\\url\\PropertyPart","property_id":"14"},{"class":"app\\properties\\url\\PropertyPart","property_id":"98"}]`


### Настройка виджета

В представление `product/list` Вставить код виджета:

```
echo \app\widgets\filter\FilterWidget::widget(
    [
        'objectId' => $object->id,
        'currentSelections' => [
            'properties' => $values_by_property_id,
            'last_category_id' => $selected_category_id,
        ],
        'categoryGroupId' => $category_group_id,
        'title' => null,

    ]);
```

- **objectId** — id объекта, (В станадартных настройках равен 3)
- **properties** - текущие выбранные свойства фильтрации
- **last_category_id'** - id  текущей категории
- **categoryGroupId** - id текущей группы категории
- ** title** - Заголовок виджета фильтрации

Так же можно переопределить, представление виджета,  через переменную $viewFile
(по умолчанию `filterWidget`)