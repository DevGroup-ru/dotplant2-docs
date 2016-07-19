# Shopping

Cart widget returns the current state of the shopping cart.

Minimum call widget is as follows:

```php
<?= \app\modules\shop\widgets\CartInfo() ?>
```

#### Call Options
- `viewFile` - path to the presentation file. String (optional)

Presentation file gets only one variable `$order` - current order (model `Order`) or `null`.

Out of the box widget data is dynamically updated by the addition of goods, the number of changes and other manipulation of the order. If you want to keep this possibility, it is necessary to leave the markup presentation elements with selectors:

- `#cart-info-widget .total-price` - the number of products in the basket
- `#cart-info-widget .items-count` - the total amount of the order