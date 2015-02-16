Разметка базового шаблона магазина
===========================

### Шаг 1. Создание новой темы
Система DotPlant2 сделана таким образом, что бы было легко переопределять базувую верстку в своих проектах.

Вся верстка по умолчанию хранится в папке  **@app/views**

Для переопделения темы, необходимо переопределить **web-config** , который распологается по адресу **web/theme/module/config/web.php**

```php
return [
    'components' => [

        'view' => [
            'theme' => [
                'pathMap' => [
                    '@app/views' => '@app/web/theme/module/themes/themeName',
                    '@app/widgets' => '@app/web/theme/module/widgets',
                ],
                'baseUrl' => '@app/web/theme/module/themes/atlant',
            ],
        ],
    ],

];
```
В данном примере мы указали, что по умолчанию файлы представления будут искаться в папке **@app/web/theme/module/themes/themeName** , если они не будут найдены, то будут браться из **@app/views**

### Описание структуры папки представления
* cabinet - кабинет пользователя. 
    * change-password - изменение пароля
    * index - главная страница кабинета
    * order - отдельный заказ
    * orders - все заказы пользователя
    * orders-table - информация о заказе, в виде таблицы 
    * profile - смена email, имени, фамилии итд
* cart - страница корзины, она состоит из 3х шагов 
    * cart-items - блок продуктов в корзине
    * client-order-email-template - шаблон письма о заказе пользователю
    * index - главнася страницы корзины, 1-й шаг
    * order-email-template - шаблон владельцу магазина
    * order-items - блок продуктов в заказе, похож на cart-items
    * payment - техническая страница перенаправления на страницу платежного сервиса
    * payment-error - если возникнут проблемы с оплатой
    * payment-success - если платеж был произведен
    * payment-type -  страница оплаты, 3-й шаг
    * shipping-options - страница выбора опций, 2-й шаг
* default - типичные страницы сайта 
    * contact -  страница контакта
    * error - страница ошибки
    * index - главная страница сайта
    * login - страница входа на сайт
    * search - страница поиска
    * signup - страница регистрации
* layouts - страница основного шаблона сайта
    * blocks - основные блоки сайта
        * carousel - слайдер 
        * footer - футер сайта
        * header - шапка сайта
        * sidebar - левое меню
    * main - основой шаблон сайта
    * main-page - шаблон главной страницы
    * no-sidebar - шаблон без левого меню
    * print - шаблон для печати
* page - отображение статичных страниц (к примеру статьи, новости итд)
    * list - список страниц
    * search - поиск страниц
    * show - отображение отдельной страницы
* product - отображения, связанные с каталогом товаров
    * item - отображается в категории. Товар  в виде плитки, несколько элементов на строке
    * item-row -  отображается в категории. 1 товар на одну строку
    * list - страница каталога
    * search - страница поиска
    * show - отдельный товар
* product-compare - сравнение продуктов
    * compare - страница сравнения продуктов
    * print - страница для печати


> так же можно переопределять представления виджетов (если это указано в **web/theme/module/config/web.php** ) . 



### Шаг 2. Переопределение темы. Разметка базового шаблона.

В первую очередь стоит переопределить файлы из представлений layouts.

Фаил layouts/main.php - содержит основной шаблон сайта. В него могут быть включены части шаблонов, которые применяются в других шаблонах (например в шаблоне главной страницы). 
Подключение части шаблона происходит при помощи php-функции include, к примеру подключение шапки сайта
```php
<?php include('blocks/header.php') ?>
```

Далее будут описаны представления с обязательными частями:
1. Шапка сайта (layouts/blocks/header.php) . В данном блоке должны храниться следующие элементы
    * `<?php $this->beginPage(); ?>` - Начало блока Head. в это место будут всавлены определенные css и js - файлы
    * `<html lang="<?= Yii::$app->language ?>">` - будет назначен язык приложения (ru, en итд)
    * `<base href="http://<?=Config::getValue('core.serverName', Yii::$app->request->serverName)?>">` - базовый путь к началу приложения, при этом не будут работать якоря без указания полного пути
    * `<meta charset="<?= Yii::$app->charset ?>">` - кодировка, по умолчанию utf-8
    * `<title><?= Html::encode($this->title) ?></title>` - Title страницы
    * `<?= Html::csrfMetaTags() ?>` - технический метатег, применятся для борьбы против csrf-атак на сайт
    * `<?php $this->head(); ?>` - Конец блока Head. в это место будут вставлены определенные css и js - файлы
    * `<?php $this->beginBody(); ?>` - Начало блока body. в это место будут вставлены определенные css и js - файлы
2. Футер сайта (layouts/blocks/footer.php) . В данном блоке должны храниться следующие элементы
    * `<?php $this->endBody(); ?> `- Конец блока body. В это место будут вставлены определенные css и js - файлы
    * `<?php $this->endPage(); ?> `- Конец блока Page. В это место будут вставлены определенные css и js - файлы

### Шаг 3. Правильное отображение h1, title, meta, content

На страницах (page|product)/(list|show)  рекомендуется использовать `$this->blocks->h1` вместо `$model->h1` и т.п.! В противном случае попытка использовать prefiltered-pages потерпит неудачу - контент переписан не будет!!!
Сам prefiltered-pages делает страницу со статичным абсолютным URL(точное совпадение URI по slug), показывающую отфильтрованные по заданным в админке условиям с кастомными h1, title, meta, content и прочим. Полезно для создания разделов типа "Новые товары", когда нужно отобразить например товары со свойством Новый-Да

### Шаг 4. Взаимодействия с JavaScript
1. Кнопка добавления товара в корзину должна иметь два атрибута `data-action="add-to-cart"` и `data-id="22"` , где 22 - id продукта.
2. Отображение стоимости и количества товаров в корзине. Что бы вывести стоимость при загрузки страницы надо использовать виджет CartInfo 
```php
<?= \app\widgets\CartInfo::widget(); ?>
```
Если представление этого виджета, будет редактироваться. То обновления кооличества продуктов и стоимости корзины, тэги должны содержать следующие атрибуты `class="items-count"` - для обозначения количества товаров в корзине. `class="total-price"` - для обозначения общей цены корзины.
3. Увелечение колличества товара. У пользователя есть воможность изменять колличество позиций товара, что бы изменить колличества товара используется js - метод changeAmount ( @app/web/js/main.js )
```js
'changeAmount' : function(orderItemId, quantity, callback) {
    var data = {
        'id' : orderItemId,
        'quantity' : quantity
    };
    jQuery.ajax({
        'data' : data,
        'dataType' : 'json',
        'success' : function(data) {
            if (typeof(callback) === 'function') {
                callback(data);
            }
        },
        'type' : 'post',
        'url' : '/cart/change-quantity'
    });
}
```

    Пример вызова этого метода, можно увидеть в представлении начальной страницы корзины (@app/views/cart/index)
```js
  jQuery('input[data-type=quantity]').blur(function() {
    var $input = jQuery(this);
    var quantity = parseInt($input.val());
    if (isNaN(quantity) || quantity < 1) {
        quantity = 1;
    }
    Shop.changeAmount($input.data('id'), quantity, function(data) {
        if (data.success) {
            jQuery('#cart-table .total-price').text(data.totalPrice);
            jQuery('#cart-table .items-count').text(data.itemsCount);
            $input.parents('tr').eq(0).find('.item-price').text(data.itemPrice);
            $input.val(quantity);
        }
    });
});
jQuery('#cart-table [data-action="change-quantity"]').click(function() {
    var $this = jQuery(this);
    var $input = $this.parents('td').eq(0).find('input[data-type=quantity]');
    var quantity = parseInt($input.val());
    if (isNaN(quantity)) {
        quantity = 1;
    }
    if ($this.hasClass('plus')) {
        quantity++;
    } else {
        if(quantity > 1) {
            quantity--;
        }
    }
    Shop.changeAmount($input.data('id'), quantity, function(data) {
        if (data.success) {
            jQuery('#cart-table .total-price').text(data.totalPrice);
            jQuery('#cart-table .items-count').text(data.itemsCount);
            $input.parents('tr').eq(0).find('.item-price').text(data.itemPrice);
            $input.val(quantity);
        }
    });
    return false;
});
```

