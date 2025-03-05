# WooCommerce

To install the woocommerce plugin for Bitcart, please follow the steps below

{% embed url="https://youtu.be/04tWVvePBdI" %}

## Install the Bitcart Woocommerce plugin

### Via Wordpress

1. WordPress > Plugins > Add New.
2. In Search, type "Bitcart for WooCommerce"
3. Install and activate.

![Install the plugin via wordpress plugin repository](../.gitbook/assets/woocommerce_install_wordpress.png)

### From Github

Download the latest plugin [release](https://github.com/bitcart/bitcart-woocommerce/archive/master.zip), upload it in the .zip format to your wordpress instance and active it.

## Deploy Bitcart

Refer to our [deployment guide](../deployment/)

## Configure the plugin

Go to your store dashboard. WooCommerce > Settings > Payments. Click Bitcart.

1. Enter your Bitcart URL (URL of the Merchants API, like https://api.bitcart.ai)
2. Enter your Bitcart Admin Panel URL (for example, https://admin.bitcart.ai)
3. Change the store id used if needed
4. Save changes

## Important note
Bitcart will only be visible on the WooCommerce checkout page if you are using the Classic Checkout. This is no longer the default option, so Bitcart will not appear as a payment method unless you activate it. For instructions on how to enable the Classic Checkout, please visit: [WooCommerce.com - Reverting to the classic Cart and Checkout](https://woocommerce.com/document/woocommerce-store-editing/customizing-cart-and-checkout/#reverting-to-the-classic-cart-and-checkout)
## Test the checkout

If you have successfully configured your plugin, you will be able to checkout via cryptocurrency.

![](../.gitbook/assets/woocommerce_before_checkout.png)

That's it!
