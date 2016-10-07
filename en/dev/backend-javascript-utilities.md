# JavaScript utility in Backend

## Dialogs

The library is connected backend [Bootbox.js](http://bootboxjs.com).

For more convenient work with the Ajax-dialogue is a jQuery plugin `DialogAction`(application/backend/assets/backend/js/DialogAction.js).  The plugin automatically connected to each BackendController page.

##### Use:

PHP (view):

``` php
<?=
Html::a(
    Icon::show('cogs') . Yii::t('app', 'Configure'),
    [
        '/DefaultTheme/backend-configuration/configure-json',
        'id' => $activeWidget->id,
        'returnUrl' => \app\backend\components\Helper::getReturnUrl(),
    ],
    [
        'class' => 'btn btn-success btn-xs configure-active-widget',
    ]
)
?>
```

Javascript (register in the presentation through the `$this->registerJs($js)`:

``` javascript
$(".configure-active-widget").click(function(){
    var that = $(this),
        url = that.attr('href');

    that.dialogAction(
        url,
        {
            title: 'JSON'
        }
    );

    return false;
});
```

## POST-links

Sometimes it is necessary to make reference, in which will be a POST request to the correct URL (attribute of the `href`) instead of directly clicking on the link.
To do this, add an attribute to the link `data-action="post"`.

For example:

``` php
<?=
Html::a(
    Icon::show('cloud-download') . ' ' . Yii::t('app', 'Install extension'),
    ['/core/backend-extensions/install-extension', 'name' => $package->getName()],
    [
        'class' => 'btn btn-primary',
        'data-action' => 'post',
    ]
);
?>
```

This prevents CSRF attack, and when you send a POST request - application successfully get CSRF token.

## Buttons save and return

TBD