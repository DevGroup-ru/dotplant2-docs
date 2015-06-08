# JavaScript утилиты в Backend

## Диалоговые окна

В backend подключена библиотека [Bootbox.js](http://bootboxjs.com).

Для более-удобной работы с Ajax-диалогами имеется jQuery плагин `DialogAction`(application/backend/assets/backend/js/DialogAction.js). Плагин подключается автоматически для каждой страницы BackendController.

##### Использование:

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

Javascript (регистрируем в представлении через `$this->registerJs($js)`:

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

## POST-ссылки

Иногда необходимо сделать ссылку, при которой будет осуществляться POST запрос на нужный URL(из атрибута `href`) вместо непосредственного перехода по ссылке.
Для этого добавьте к ссылке атрибут `data-action="post"`.

Например:

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

Таким образом предотвращается CSRF атака, а при отправке  POST запроса - приложение успешно получит CSRF токен.

## Кнопки сохранения и возврата

TBD