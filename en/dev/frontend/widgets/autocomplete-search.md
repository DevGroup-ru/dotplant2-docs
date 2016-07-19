# Autocomplete widget

Autocomplete widget is used to automatically prompt results in the form of site search. At the time set in the request for input, he loads the appropriate results and displays them in a list below the input field.
If some of them are suitable result, you can click on it and will switch to the appropriate page.

### Minimum call widget might look like this:

```php
<?php
$search = new \app\models\Search;
$search->load(Yii::$app->request->get());
$form = \kartik\widgets\ActiveForm::begin(
   [
       'action' => ['/search'],
       'method' => 'get',
       'id' => formid,
   ]
);

echo \app\widgets\AutoCompleteSearch::widget(
   [
       'model' => $search,
       'attribute' => 'q',
   ]
);
\kartik\widgets\ActiveForm::end();
?>
```

It is called within the form widget and generates an input field `input type=" text "`.
The input accepts the basic parameters:
- `model` => search model
- `attribute => q`, the name of the request parameter

Extra options:
- `clientOptions` - array of options from http://api.jqueryui.com/autocomplete/ 
- `listClass` - css class to the list of execution results in a tooltip
- `route` - route to auto complete the action, the default `'/default/auto-complete-search'`