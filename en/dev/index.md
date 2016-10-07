# Developer's Guide

> **Warning!** Developer's Guide is in the process of writing.
> 
> If you were faced with some difficulties - please contact the [Gitter-chat](https://gitter.im/DevGroup-ru/dotplant2) *(requires GitHub account)*

## Content

- [The organization of work and work space](workspace.md)

- CMS Architecture
    - [List of main URL routes](url-routes.md)
    - [Category CNC Settings](category-route.md)
    - [Principles for routing](routing.md)
    - [Tagged cache](taggable-cache.md)

- Layout Template and representations
    - Overview
    - [Using your own theme](custom-theme.md)
    - [JavaScript tools](javascript-utilities.md)
    - Widgets
    	- [Navigation](frontend/widgets/navigation.md)
    	- [Shape](frontend/widgets/form.md)
    	- [Reviews](frontend/widgets/review.md)
    	- [Cart](frontend/widgets/cart-info.md)
    	- [Search with autocompletion](frontend/widgets/autocomplete-search.md)
    	- [Images](frontend/widgets/object-image.md)
    - [Dynamic content](frontend/dynamic-content.md)
    - [Example of creating a custom widget](../tutorial/create-new-widget.md)
    - [Google Merchant](google-merchant.md)

- Backend-development
	- [Writing an arbitrary backend-controller](custom-backend-controller.md)
	- [Creating your own property type](custom-property.md)
	- [The implementation of any type of payment](custom-payment-type.md)
	- [The implementation of the course provider class currency](custom-rate-provider.md)
	- [JavaScript utility for Backend](backend-javascript-utilities.md)
	- Console Commands
		- `admin/create-theme` - created theme template `@app/web/theme-name`, where `theme-name` - the name of folder, which is value of the first parameter passed to the command (by default - `theme`).
	- [Writing expansion on the example of WYSIWYG](extension-wysiwyg.md)



