# Content Blocks

Content blocks are used to avoid duplication of repeating units or elements in the content or site submissions.

> A similar functionality in MODX called chunks (chunks), and WordPress - Shortcodes (shortcodes).

## The definition of the unit

To determine the content block - create a corresponding entry in the "content blocks" Administration panel:

- **Name** - used for visual identification unit
- **Key** - the key that will be called in this block content
- **Value** - in fact something that will be replaced by the content block call the operator directly to the content
- **Preload** - the content block will be loaded on each request to the site. Turn on this option for the most frequently used blocks.

Content block may take additional parameters, such as:

```
This is a simple unit, which now displays formatted option
[[+paramName:format,format_attribute1,'format_attribute_n']].
And here we are just derive value as it is [[+anotherParam]].
```

Here:

- **paramName** - parameter name (this name will need to pass along block call). Plus sign before the parameter name is required.
- **format** - formatting function (optional)
- **format_attribute1** and **format_attribute_n** - an arbitrary number of parameters to be passed for formatting functions. Separator - comma. If the value enclosed by quote signs - it is treated as a string (string), without the quotes - as a floating (float) point.
- **[[+anotherParam]]** - name of the parameter. In this example, it displays "as is"

## The use of the content block

In the content inserting operator block call, which has the following format:

```
[[$contentBlockKey paramName='paramValue' anotherParam=3.14]]
```
Here:
- **contentBlockKey** - replace the content on your key unit. **Important!** dollar sign in front of the key is required.
- **paramName** - name of the parameter
- **paramValue** - value. If it is in single quotation marks - is treated as a string. Otherwise - as the number with point (float).

## Recursive content blocks
Content block is allowed to call another content block. This call will be carried out in exactly the same way as a challenge to the page content. Moreover, all the passed parameters and other content boxes will be processed, if they are contained in nested blocks of content.

**It is unacceptable to call a content block within themselves, as well as to call the parent content box inside the called child!**

## The use of the content in the template block
It is good practice to the imposition of the entire text information, including static blocks layout of patterns in content boxes. This will allow content managers much easier and clearer to edit this information.

To display the content blocks in a template, you must call a special helper method:

```php
<?= \app\modules\core\helpers\ContentBlockHelper::getChunk($key, $params = [], yii\base\Model $model = null) ?>
```

It takes as parameters three values:
- Key Chunk;
- Array of parameters;
- Model, in whose view-file, we call this method.

It is mandatory only the first option - a key chunk. In this case, the minimum method call will look like this:

```php
<?= \app\modules\core\helpers\ContentBlockHelper::getChunk('chunkKey') ?>
```

If the called Content block are placeholders that must be replaced with the actual values, you must pass the second parameter an associative array of the form:

```php
[
    'paramName' => 'string param value',
    'param2Name' => 3.14,
]
```

The third optional parameter is passed to the method model (Product, Category, Page), in whose view file called helper method. It will be used to cache the results of rendering chunk. Model transmit only makes sense if the chunk is processed with the parameters, and the parameter values depend on a particular page, otherwise this parameter can be omitted.

Complete method call is as follows:

```php
<?= \app\modules\core\helpers\ContentBlockHelper::getChunk(
    'chunkKey',
    ['paramName' => 'string param value','param2Name' => 3.14,],
    $model
) ?>
```
If the called chunk chunks called others, they will be rendered.

## Forms a Call Content

By analogy with the content blocks in the content, you can started call in the administrative part of the site form.
Minimum call looks like this:
```php
[[%3]]
```
where:
-% - mandatory prefix;
- 3 - created id in the admin form.

Call types:
```php
[[%3#form-id]]
```

Call the form by setting the id - form-id

Call types:
```php
[[%3#form-id;isModal]]
```
render a modal form with the ID form-id

## Getting object references

In content, the announcement (pages, categories, products, dynamic content) and in chunks, you can get links to pages, products and categories

It works like this: `[[~page#2]] [[~product#2]] [[~category#1]]`
Or this:

Chunk page `[[$test page=2]]`

Chunk code:

```
<a href="[[~page#[[+page]]]]">ссылка на страницу с параметром</a>
<a href="[[~category#3]]">Категория</a>
<a href="[[~product#20]]">продукт</a>
```

## Display item card in the content

By analogy with the forms in the content, you can withdraw the card(s) on the identifier of the goods, or on the article.
Print the product card in the following ways:

`[[*product sku='code']]` - prints card on the article. **code must be unique**.
`[[*product#id]] ` - prints card on the identifier.

In addition to a list of products, you can display a list of cards as well:
`[[*productList parameter list]]`
The list of available options:

- `categoryId='id' ` - selects a card from the category on its identifier
- `limit='number' ` - limited sample
- `property='key:value'` - selects a card from the set values​of properties, where `key` - key properties, `value` - its value.

From the admin also can configure the default view to display the card and the card list. To do this, go to Settings admin - click `Config` - `Shop` tab.
To select submissions in a particular case, pass into option `itemView='@app/path/to/item/view'` for single card and `listView='@app/path/to/list/view'` for card List.




