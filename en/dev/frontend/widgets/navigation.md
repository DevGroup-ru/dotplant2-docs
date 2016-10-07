# The widget navigation.

Navigation widget allows you to display the menu on the site started in the administrative part of the application. To get started with the navigation widget, in the administrative part of the site go to the section Navigation. It already has the item category Main Menu. As in current time, all the other menu should be created within this category.
Creating a menu block like this, will be in same way. The only difference is that all the items are created within the parent, and the menu at the block there is no need to specify the field URL.

So, to create a new menu, click Add. Then fill the following fields in the forms:

Required field:
- Name - the name of the menu item as it will appear on the site. For example, Contacts.

Optional fields:
- Advanced CSS class - the class name for the design created menu item.
- Sort Order - an integer indicating the order created menu item.
- URL - relative or absolute URL of the page, for example `/contacts` or `http://domain.com/contacts`. it is not necessary to fill out the menu for the most field unit.
- Route - Route Manager for the URL.

The Add button lets you add a property, click Advanced Properties menu, such as data type attributes. To add a property you need to fill two fields:
- Key - property name, for example, `data-name`
- Value - the value of the properties, such as `slide-menu`.
The menu item will eventually receive the property type of data-name = "slide-menu"
Thereafter shape must be maintained.

Creating a menu is the same, only need to create them within the established menu block.

Once the menu in the administrative part is ready, you want to call the navigation widget in the right part of the theme template.

### Minimum widget call looks like this:

```php
<?= \app\widgets\navigation\NavigationWidget::widget(['rootId' => ID]) ?>
```

where ID - identifier of the menu block, which we created in the administrative part of the application.
This call will show a menu similar to the following website:

```html
<ul class="navigation-widget">
    <li ><a href="/item">Item</a></li>
    <li ><a href="/item2">Item2</a>
        <ul>
            <li><a href="/item2/subitem">Subitem</a></li>
        </ul>
    </li>
</ul>
```

You can expand a call widget, adding the own mapping file:

```php
<?= \app\widgets\navigation\NavigationWidget::widget([
    'rootId' => ID, 
    'viewFile' => 'path/to/file'
])?>
```

At the same time, in this file will be available an array of `$items`, the following structure:

```
Array (
    [0] => Array (
        [label] => Item
        [url] => /item
        [options] => Array (
            [class] =>
        )
        [items] => Array ( )
    )
)
```    

It can be processed and output as pipes.

Additional parameters can be passed as follows, in the options array:

```php
<?= \app\widgets\navigation\NavigationWidget::widget([
    'rootId' => 9,
    'options'=>[
        'name' => 'value'
    ]
]) ?>
```