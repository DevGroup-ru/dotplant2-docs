# Dynamic Content

> Dynamic Content - includes information from the text, title, and seo-danymi. Used to add [blocks](http://www.yiiframework.com/doc-2.0/guide-structure-views.html#using-blocks) and seo-data on a page with the results of the filtered data.

### Fields

1. Object - the object to which it is added dininamichesky content
2. Route - [route](http://www.yiiframework.com/doc-2.0/guide-structure-controllers.html#routes) display, to which is added dininamichesky content
3. Name - identifier of dynamic content
4. Title - The title of the page with dynamic content
5. H1 - highlighted an inscription, usually located on top of content
6. Meta-description - description pages with dynamic content, typically used in the search snippet
7. Content Block Name - the block ID
8. Content - text block

### Creation:

1. You must be in the admin panel menu item Dynamic content (/backend/dynamic-content/index)
2. Press the Add button
3. Fill out the fields and click on the Save button

### Edit:

1. You must be in the admin panel menu item Dynamic content (/backend/dynamic-content/index)
2. Click on the icon pencil
3. Fill out the fields and click on the Save button

### Removal:

1. You must be in the admin panel menu item Dynamic content (/backend/dynamic-content/index)
2. Click on the basket icon

### Conclusion:

The easiest way to bring dynamic content to a page:

```php
if (isset($this->blocks['ID'])) {
    echo Html::tag('div', $this->blocks['ID']);
}
```

where ID - the name of the content block.