# Руководство разработчика

> **Внимание!** Руководство разработчика находится в стадии написания.
> 
> Если вы сталкнулись с какими-нибудь трудностями - обращайтесь в [Gitter-чат](https://gitter.im/DevGroup-ru/dotplant2)*(требуется учетная запись GitHub)*

## Содержание

- [Организация работы и рабочего пространства](workspace.md)

- Архитектура CMS
    - [Список основных URL маршрутов](url-routes.md)
    - [Принципы роутинга](routing.md)
    - [Тегированный кэш](taggable-cache.md)

- Верстка шаблона и представлений
    - Общие сведения
    - [Использование собственной темы](custom-theme.md)
    - [JavaScript утилиты](javascript-utilities.md)
    - Виджеты
    	- [Навигация](frontend/widgets/navigation.md)
    	- [Формы](frontend/widgets/form.md)
    	- [Отзывы](frontend/widgets/review.md)
    	- [Корзина](frontend/widgets/cart-info.md)
    	- [Поиск с автодополнением](frontend/widgets/autocomplete-search.md)
    	- [Изображения](frontend/widgets/object-image.md)
    - [Динамический контент](frontend/dynamic-content.md)
    - [Пример создания собственного виджета](../tutorials/create-new-widget.md)

- Backend-разработка
	- [Написание произвольного backend-контроллера](custom-backend-controller.md)
	- [Создание собственного типа свойства](custom-property.md)
	- [Реализация произвольного типа оплаты](custom-payment-type.md)
	- [Реализация класса провайдера курса валют](custom-rate-provider.md)
	- [JavaScript утилиты Backend-а](backend-javascript-utilities.md)
	- Консольные команды
		- `admin/create-theme` - создание каркаса темы в `@app/web/theme-name`, где `theme-name` - название папки, которое является значение переданным первым параметром в команду (по-умолчанию - `theme`).
	- [Написание расширения на примере WYSIWYG](extension-wysiwyg.md)



