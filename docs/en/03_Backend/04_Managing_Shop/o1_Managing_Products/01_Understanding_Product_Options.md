#Managing products options
DotPlant CMS provides you ability to create and manage product’s options or variations.

This ability makes much more easier products relations control and allows you to do not copy products if their differences is only in color, memory volume etc.

Furthermore, you can create product equipment based on this capability.

Now you have two ways to create product options:
-   automatic generation;
-   manual creation;

Whatever which way you choose, all of them starts in site control panel.

In the `Control panel` main menu select the `Shop` item. Then - `Products`. Now find the product you want to add option to and click `Edit` button.

You can find options managing section in the right bottom part of `Product Edit` page. It is called `Generate Product Options`.

##Automatic generation.
Automatic generation works based on properties assigned to base product. See details [here](). 

First of all, from the dropdown list select necessary products options group. Next, you have to check properties which will take part in options generation. These are properties that will be different for each product variation. After that click `Generate` button. This reloads web page and you will see list of generated product options below `Generate Product Options` section.
This will create [Cartesian product](http://en.wikipedia.org/wiki/Cartesian_product) of chosen properties.

For example, for color property you choose Yellow, Red and Green, and for memory volume 16Gb and 32Gb.
This will create a set of next properties combinations Yellow 16Gb, Yellow 32Gb, Red 16Gb, Red 32Gb, Green 16Gb and Green 32Gb.

Each created product-option you can edit as a regular product, except you can't create its own options.

While generating all off the base product fields values will be copied into the appropriate option field. Images, content, announce properties values etc will be copied. Except `Title` and `URL` fields. In the `Title` will be added properties values. And `URL` will be updated to be unique according to the CMS rules.

If you won’t to copy all of the host product fields, you can create option manually.

##Manually creating.
Manually creation of the product option have only one difference from the regular product creation. You have to push `Add` button in the `Generate Product Options` section. Another points of option creation are similar to appropriate regular product creation.
