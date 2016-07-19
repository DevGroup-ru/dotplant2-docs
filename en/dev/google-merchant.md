# Google Merchant

Merchant Center allows merchants to publish their stores and products on Google Product Search and other Google services.

With information about the service and its configuration can be found at [https://support.google.com/merchants/](https://support.google.com/merchants/#topic=3404818)

DotPlant2 allows you to create a data feed to Google Merchant.
To do this, go to the admin panel to `/shop/backend-google-feed/settings`, fill out the required fields and click `Create Google feed`.
The file with the specified name will appear in the root directory of your site.

Event `/application/modules/shop/components/GoogleMerchants/GoogleMerchants::MODIFICATION_DATA`.
In the event the product is transferred to the model.
DefaultHandler `/application/modules/shop/components/GoogleMerchants/DefaultHandler`
