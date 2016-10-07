# Multi-currency

> **Multi-currency** - support for multiple currencies for goods in the shop.

the main currency should always be specified in the product (`Currency.is_main`),  in which the calculation of all orders will be carried out. It is on this currency will be billed and the payment is made.

[Implementation layout](Multicurrency_Markup) requires close attention, because you can not just display field `$product->price` or `$product->old_price`, if your store uses more than one currency.

Currently we implemented the ability to set the price for the products in the currency. Displaying goods prices specified in the template.

Changing the display of prices in the currency based on user selection has not been realized yet (follow https://github.com/DevGroup-ru/dotplant2/issues/173 ).

Currency management

To manage the currency, go to the administrative part of the site menu item Shop - Currency.

This section allows you to create / edit / delete currency exchange rates and providers. By default, the CMS added rubles, US dollars and euro as a currency, as well as Google Finance services and web service the Bank of Russia as a financial information provider.

> If you use the Bank of Russia as a provider, the main currency of the store **certainly**  must be Ruble.

To add a new currency, go to `Currency` page and press the `Add`. Fill in the fields:

* **Name** - the name of the currency
* **ISO-4217 code** - currency code in the format of [ISO-4217](https://ru.wikipedia.org/wiki/ISO_4217)
* **Is main currency** - whether the main currency
* **Convert nominal** - the nominal exchange rate conversion (default is 1)
* **Currency rate provider** - financial service provider in exchange rates
* **Convert rate** - currency exchange rate in relation to the basic currency (provided by your provider)
* **Additional rate** - surcharge to exchange rate (in percentage)
* **Additional nominal (coefficient)** - additional nominal exchange rate
* **Sort Order** - the order number Currency
* **Intl formatting with ICU** - price formatting in ICU format
* The unit of **currency formatting** used in the formatted output prices

and then click `Save`

To add a new provider go to `Currency rate provider` (/shop/backend-currency-rate-provider/index)  and click `Add`. Fill in the fields:

* **Name** - имя провайдера
* **Class name** - a class that implements an interface Swap\ProviderInterface
* **Params** - additional parameters of class in the JSON format

and then click `Save`

## The task to update the exchange rate schedule

To update the exchange rates on a schedule set [task](Background_Tasks) named `Currency update`. 