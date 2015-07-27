# Динамический контент

## Проверка, что из динамического контента применено на странице

```php
Yii::$app->response->isDynamicTitle(); // был ли переписан title
Yii::$app->response->isDynamicMetaDescription(); // был ли переписан meta description
Yii::$app->response->isDynamicContentBlock('h1'); // был ли переписан h1
```
