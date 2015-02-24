# Разметка шаблонов для поддержки мульивалютности

#### Для отображения цен в валюте моделью `Product` предоставляются следующие методы:

+ `public function convertedPrice($currency = null, $oldPrice = false)`

    Возвращает цену(float) в необходимой валюте, если `$currency === null`, то будет возвращена цена в основной валюте магазина.

    Если нужно вернуть поле "старая цена" - `$oldPrice` устанавливаем в true.

    ---

+ `public function formattedPrice($currency = null, $oldPrice = false, $schemaOrg = true)`

    Возвращает HTML-код отображения цены в необходимой валюте с форматированием согласно настройкам валюты(модель `Currency`).

    `$currency` и `$oldPrice` аналогично `convertedPrice`.

    По-умолчанию возвращает с учетом микроразметки Schema.org/Offers примерно в таком виде:

    ``` html

    <span itemtype="http://schema.org/Offer" itemprop="offers" itemscope>
        <meta itemprop="priceCurrency" content="RUB">
        <span itemprop="price" content="996">
            996,00 руб.
        </span>
    </span>

    ```

    Если нужно вывести без schemaOrg - выставляем параметр `$schemaOrg` в false.

    ---

+ `public function nativeCurrencyPrice($oldPrice = false, $schemaOrg = true)`

    Возвращает HTML-код отображения цены аналогично `formattedPrice`, но в валюте конкретного товара.

#### Также класс Currency предоставляет нам следуюище полезные функции:

- `public function format($price)`
    
    Возвращает отформатированную строку цены $price в нужной валюте.

    Пример использования(_вывод цены в формате валюты с ID=1_):

    ``` php
    $currency = \app\models\Currency::findById(1);
    echo $currency->format(1488.1);
    ```

- `public static function findById($id)`

    Возвращает экземпляр модели `Currency` для `id` с использованием кеша и Identity Map.

    Пример использования(_вывод цены продукта в его родной валюте_):

    ``` php
    echo \app\models\Currency::findById($model->currency_id)->format($model->price) ?>
    ```

## Список товаров

Реализация отображения цены на примере демонстрационной темы, где каждый товар списка отображается через представление `views/product/item.php` (представлен только нужная часть разметки):

``` html
    <h4 style="text-align:center">
        <a class="btn" href="#" data-action="add-to-cart" data-id="<?=$product->id?>"><?=Yii::t(
                'shop',
                'Add to'
            )?> <i class="fa fa-shopping-cart"></i></a>
        <button class="btn btn-primary">
            <?= $product->formattedPrice(null, false, false) ?> <!-- отображение цены в валюте -->
        </button>
    </h4>

```

В данном случае мы отображаем обычную цену(не старую) в главной валюте магазина без использования schema.org, поскольку товарное предложение в шаблоне не оформлено должным образом.

Для `item-row` разметка будет аналогичной.

## Карточка товара

Пример реализации для демонстрационной темы, представление `views/product/show.php`:

``` html
<form class="form-horizontal qtyFrm">
    <div class="control-group">
        <label class="control-label">
            <?php if ($model->price < $model->old_price): ?>
                <small class="text-muted">
                    <del>
                    <!-- отображение старой цены в валюте без вывода schema.org -->
                        <?= $model->formattedPrice(null, true, false) ?>
                    </del>
                </small>
                <br>
            <?php endif; ?>
            <!-- отображение обычной цены в основной валюте магазина с учетом schema.org -->
            <?= $model->formattedPrice() ?>
            
            <?php if ($model->currency_id !== \app\models\Currency::getMainCurrency()->id): ?>
            <small class="text-muted">
                <!-- отображение обычной цены в валюте товара, если она не основная -->
                <?= $model->nativeCurrencyPrice(false ,false) ?>
            </small>
            <?php endif; ?>

        </label>
        <div class="controls">
            <div class="pull-right" style="text-align: right;">
                <?=
                \kartik\helpers\Html::a(
                    Yii::t(
                        'shop',
                        'Add to compare'
                    ),
                    [
                        '/product-compare/add',
                        'id' => $model->id,
                        'backUrl' => Yii::$app->request->url,
                    ],
                    [
                        'class' => 'btn',
                    ]
                )
                ?>
                <br />
                <br />
                <button type="submit" class="btn btn-large btn-primary" data-action="add-to-cart" data-id="<?= $model->id ?>">
                    <?= Yii::t('shop', 'Add to') ?> <i class="fa fa-shopping-cart"></i>
                </button>
            </div>
        </div>
    </div>
</form>
```

В данном примере функция `formattedPrice` вызывается с параметрами по-умолчанию для основной цены. Используется вывод с поддержкой микрофората Schema.org, поскольку выше в шаблоне определена сущность http://schema.org/Product.