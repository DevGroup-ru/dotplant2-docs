# Admin panel(backend) overview

To access backend you should login to your site with admin credentials.

Admin user is created during install process.
Default login action location: http://YOUR_HOSTNAME/login

After logging in you can proceed to backend : http://YOUR_HOSTNAME/backend

> **Note:** Backend uses commercial bootstrap 3 theme - SmartAdmin. Therefore theme assets(js, css, images, etc.) are loaded in backend from third-party static domains(st-[1-4].dotplant.ru). 
> 
> Please consider [buying SmartAdmin license](https://wrapbootstrap.com/theme/smartadmin-responsive-webapp-WB0573SK0?ref=dotplant_ru_cms) for your project if you want to modify original files or relocate them to another domain.
> 
> Until you don't change this assets location you don't need to buy separate license.

# Backend navigation

Backend navigation(left sidebar) is stored in database in tree hierarchy format.

The structure of table:

- `id`, `parent_id` - hierarchy keys.
- `sort_order` - integer that will be used for sorting purposes.
- `name` - the label for menu item.
- `translation_category` - i18n category for translating menu item label to other languages. See [yii2 internationalization manual](http://www.yiiframework.com/doc-2.0/guide-tutorial-i18n.html) for more details. If it is not set - items label will not be translated. By default 'app' category is used.
- `route` - route to backend action, empty for top-level menu items. For example 'backend/page/index' will map to `Url::to(['/backend/page/index'])`. You can also use absolute URL here starting with `/` or `http://`.
- `icon` - font awesome icon without 'fa' prefix to be used by this menu item.
- `css_class` - advanced css class attribute which will be applied to menu item tag.
- `rbac_check` - _optional_ name of RBAC role to check if this menu item and it's children should be visible to user(ie. user has access to this role).
- `added_by_ext` - what extension has added this menu item(to be used when marketplace will be done).

You can edit menu manually through special backend controller: `/backend/backend-menu/index` located under 'Backend menu' in 'Settings' menu.
