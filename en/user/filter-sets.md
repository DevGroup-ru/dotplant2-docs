# Sets the filters

> DotPlant2 allows you to specify your filter sets for specific categories, giving them the opportunity to inherit the child categories, restrict the number of choices, and more. This functionality can be found in the `Shop / Filter Sets` administrative section.

In the transition to this section, we see the category tree on the left and right sets of properties for the current category.

[![IMAGE_ALT_TEXT_HERE](http://st-1.dotplant.ru/docs-assets/filter-sets.png)](http://st-1.dotplant.ru/docs-assets/filter-sets.png)

Select a category by selecting the item `Open` in the context menu that appears when you click the right mouse button on the item category tree.

### Adding a filter

Open the desired category. You will see the `Add Property` and groups tables and their properties which have been added as filters. After clicking on the button `Add Property`. You must select the desired property from the pop-up menu. It is a two-level list of the form `Group Properties / Property`. After selecting the properties will be a table with the filter settings. Here you can check the box `Delegate subsidiary elementy` , `Range` or edit the available values `Show values`, `Multiple values` - to enable multiple filters for this property. By the way, here it is possible to correct the NC filter.

### Multiple filters
    
Multiple filters DotPlant2 operate in mode 2:
    
- Crossing - Displays products in which the chosen **All** properties
- Merge - displays the products for which you have set at least one of the properties of the filter multiple choice
  
  The required mode can be set in the admin settings in `Shop`.

> **Caution**
> Add a file `robots.txt` string `Disallow: ?properties` if you are going to use multiple filters.