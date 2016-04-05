# Настройки ЧПУ категорий
Настройки ЧПУ категорий хранятся в таблице `{{%route%}}`.

Приведем пример создания категории `/actcii` . Для демонстрации мы создадим миграцию в которой будет следующее:


1) Создаем группу категории
```
$newCategoryGroup = new CategoryGroup([
    'name' => 'Акции'
]);
$newCategoryGroup->save();
```

2) Создаем нужную категорию

```
$category = new Category();
...
$category->slug = 'actcii';
$category->parent_id = 0; // категория лежит в корне
$category->category_group_id = $newCategoryGroup->id; // Указываем id нужной группы категории
$category->save();
```

3) Добавляем в таблицу `{{%route%}}` записи с шаблонами формирования чпу категорий и продуктов.
- категорий
```
(new Query())->createCommand()->insert(
    '{{%route%}}',
    [
        'route' => 'shop/product/list',
        'object_id' => Object::getForClass(Product::class)->id,
        'url_template' => Json::encode([
            [
                'class' => 'app\\properties\\url\\StaticPart',
                'static_part' => $category->slug,
                'parameters' =>
                    [
                        'category_group_id' => $category->category_group_id,
                    ],
            ],
            [
                'class' => 'app\\properties\\url\\PartialCategoryPathPart',
                'category_group_id' => $category->category_group_id,
            ],
            [
                'class' => 'app\\properties\\url\\PropertyPart',
                'property_id' => 8,
            ],
        ])
    ]
)->execute();
```
- Продуктов
```
(new Query())->createCommand()->insert(
    '{{%route%}}',
    [
        'route' => 'shop/product/show',
        'object_id' => Object::getForClass(Product::class)->id,
        'url_template' => Json::encode([
            [
                'class' => 'app\\properties\\url\\StaticPart',
                'static_part' => $category->slug,
                'parameters' =>
                    [
                        'category_group_id' => $category->category_group_id,
                    ],
            ],
            [
                'class' => 'app\\properties\\url\\FullCategoryPathPart',
                'category_group_id' => $category->category_group_id,
            ],
            [
                'class' => 'app\\properties\\url\\ObjectSlugPart',
            ],
        ])
    ]
)->execute();

```