# Меры

> В DotPlant2 реализована поддержка товаров с различными единицами измерения. 

Для добавления, изменения или удаления мер необходимо перейти в раздел `Магазин/Меры` административной панели.

Форма редактирования состоит всего из трех полей:

* `название` - название меры;
* `символ` - символ обозначения;
* `номинал` - количество отпускаемого товара (используется при подсчете в корзине и не дает пользователю ввести количество товара, которое не равно `N * номинал`).

> **Важно!** Для удобства пользования желательно установить меру по умолчанию, которая будет устанавливаться при создании нового товара. Это можно сделать через `Настройки/Конфигурации` во вкладке `Магазин` в блоке `Корзина и заказы`.