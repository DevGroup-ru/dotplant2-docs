# Properties Control

## Properties Group

To manage groups of properties, go to the administrative part of the CMS in the left menu click on `Properties - Properties`.

#### Description of the fields

* **Name** - the name of the property group
* **Object ID** - the object to which the property group
* **Is Internal** - skips group widgets
* **Hidden Group Title** - hide group header widgets
* **Sort Order** - order of the group

#### Creation

1. Press `Add`
2. Fill out the field
3. Press the `Save`

#### Edit

1. Click on the pencil icon
2. Edit field
3. Press the `Save`

#### Removal

* Click on the basket icon
* Or select leaving groups in the left column and click `Delete Selected`


## Properties

To manage the properties, go to the administrative part of the CMS in the property group editing mode.

#### Description of the fields

- **Name** - name of the property;
- **Key** - unique ID of properties within a given group. It may consist of Latin letters and underscore;
- **Value Type** - the type of data. Used for data comparison and can be either a number or a string;
- **Property Handler IDe** - a way to display the data properties for editing. It can take the following values:

    - **Text** - single-line text input field;
    - **Select** - drop-down list. Used for the type of data storage in `static pages`;
    - **Checkbox** - check;
    - **Text area** - multiline text field;
    - **File** - download file file;
    - **Hidden** - hidden field;
    - **Redactor** - WYSIWYG editor html;

- **Has Static Values** - storage method in which the predetermined value. This type should be used if this property should be used in filters or to a specific list of values. For example, brands or colors;
- **Has Slugs In Values** - used CNC filters (only works with `static values`);
- **Is Eav** - the property values ​​are stored in a common table. This method is great for storing values ​​that are rarely (not) repeat and do not require filtering. For example, washing instructions;
- **Is Column Type Stored** - properties are stored as a column in the general table (if you want to change the properties of the database structure change key). This option is good if there is no need for filtration and the value should be stored for each item. For example, the weight of the goods or article;
- **Multiple** - the property can accept multiple values;
- **Required** - the property is required;
- **Interpret Field As** - used to check for spam ([more](spam-checker.md));
- **Captcha** - use the property as a captcha;
- **Sort Order** - order number of the properties;
- **Logic settings** - logic used in the filtering widget (for versions below 2.0.0-beta).

#### Creation

1. Press `Add`
2. Fill out the field
3. Press the `Save`

#### Edit

1. Click on the pencil icon
2. Edit field
3. Press the `Save`

#### Removal

* Click on the basket icon
* Or select leaving groups in the left column and click `Delete Selected`

## Example of creating properties for the goods


The first step is to create a group. To do this, go to `Properties/Properties` administrative section and click the `Add` button. In the form we will be asked the name of the group (eg, drills), the object (Product). Click to save and proceed to create properties.
                                                                               
Now you need to add properties. Let us proceed. `Brand`,  which we plan to use in filters and know all the values ​​of the first property. Therefore, press the `Add` button, in the `Properties` panel at the bottom of the page and fill in the form by the following data:
- Name: Brand
- Key: brand
- Property Handler ID: Select
- Has Static Values
- Has Slugs In Values

Click the button `Save`. Then at the bottom of the page appears `Static Value` panel, then click `Add Value`. You can add all the values​at once (by clicking on the button), or do it as filling properties of the goods (the goods directly to the edit page).

Now add one more property - `Dimensions`. We do not need to filter on the property and its value hardly repeated. Therefore, create a property `key-value`:
- Title: Dimensions
- Key: size
- Property type: Text
- Key-Value

Click the button `Save`. Property group created is now necessary to add it to the product.
