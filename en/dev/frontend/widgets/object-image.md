# Image Widget

> DotPlant2 CMS allows you to bind to the image of an entity (product, page, etc.) directly through the admin panel and to display images on the frontend uses the widget `app\modules\image\widgets\ObjectImageWidget`.

Widget can take the following parameters:

* `model` - model of the object for which you want to display the image (required)
* `viewFile` - widget mapping file
* `limit` -  limit the number of displayed images
* `offset` - displacement position
* `thumbnailOnDemand` - false or true, or not to display a thumbnail
* `thumbnailWidth` и `thumbnailHeight` - width and height of thumbnails respectively
* `useWatermark` - false or true, display watermark or not

Minimum call widget is as follows:

```php
<?=
app\modules\image\widgets\ObjectImageWidget::widget(
	[
		'model' => $model,
	]
)
?>
```