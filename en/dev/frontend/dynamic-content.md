# Dynamic Content

## Checking that is applied from the dynamic page content

```php
Yii::$app->response->isDynamicTitle(); // Whether rewritten title
Yii::$app->response->isDynamicMetaDescription(); // Whether rewrite meta description
Yii::$app->response->isDynamicContentBlock('h1'); // Whether rewritten h1
```
