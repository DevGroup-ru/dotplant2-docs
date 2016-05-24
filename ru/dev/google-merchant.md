# Google Merchant

Merchant Center позволяет продавцам публиковать данные о своих магазинах и товарах в Google Покупках и других сервисах Google.

С информацией о сервисе и его настройке можно ознакомится по адресу [https://support.google.com/merchants/](https://support.google.com/merchants/#topic=3404818)

DotPlant2 позволяет создать фид данных для Google Merchant.
Для этого нужно перейти в админке на `/shop/backend-google-feed/settings`, заполнить необходимые поля и нажать кнопку `Create Google feed`.
Файл с указанным именем появится в корневой директории Вашего сайта.

Событие `/application/modules/shop/components/GoogleMerchants/GoogleMerchants::MODIFICATION_DATA`.
В событие передается модель продукта.
DefaultHandler `/application/modules/shop/components/GoogleMerchants/DefaultHandler`


