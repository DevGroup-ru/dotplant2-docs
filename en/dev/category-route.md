# CNC Settings categories
CNC Settings categories are stored in the table `{{%route%}}`.

Here is an example of creating the category of `/actcii` . To demonstrate, we'll create a migration which will be the following:


1. Create a category group
```
$newCategoryGroup = new CategoryGroup([
    'name' => 'Акции'
]);
$newCategoryGroup->save();
```

2. Create the desired category

```
$category = new Category();
$category->slug = 'actcii';
$category->parent_id = 0; // Category at the root
$category->category_group_id = $newCategoryGroup->id; // Specify the desired group category id
$category->save();
```

3. Add `{{%route%}}` table for recording CNC categories and products template formation.
- Categories
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
- Products
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