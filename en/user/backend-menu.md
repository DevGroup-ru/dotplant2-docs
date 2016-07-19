# Menu administrative part

> In DotPlant2 have the ability to edit the administrative side of the menu. The menu is multi-level and does not contain any restrictions on the nesting, which allows you to group similar functionality within a certain item, rather than creating a huge sheet, which does not fit on the screen. Edit menu may be required when adding modules or extensions to CMS. Normally this happens automatically, but you can change it themselves if necessary.

In the editing menu can be accessed by clicking on the link `Settings/Backend Menu` in this same menu, the administrative part of the site :) It consists of a tree of menu items and a list of items within selected. Select the action from the point of you can by right-clicking on the item in the tree, or the appropriate button in the table.

Form menu item has the following fields:

* `Name` - the name of the menu item (usually in English);
* `Route`- url-address of the page;
* `Icon` - the name of the icon from a set of `font-awesome` without the prefix `fa-`;
* `Translation Category` - category of internationalization for translating the names of the menu item with the English language;
* `Added by extension` - the name of the extension, which added the menu item;
* `Css Class` - additional css class for the menu item;
* `RBAC Role` - the name of user's role, which will be available to the menu item;
* `Sort order` - order number of the menu item.