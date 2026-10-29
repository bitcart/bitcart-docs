# WHMCS

WHMCS integration provides an easy way to use Bitcart as a payment gateway in your whmcs instance

{% embed url="https://youtu.be/lUNSbC8NzgQ" %}

## Requirements

This plugin requires the following:

- WHMCS 7.x
- Running Bitcart instance: [deployment guide](../deployment/)

## Installation

1. Open [https://github.com/bitcart/whmcs-plugin/releases/latest](https://github.com/bitcart/whmcs-plugin/releases/latest) and download the zip archive with the plugin
2. Copy the archive on your server
3. Extract the archive to your whmcs root, so that in your whmcs root, in modules/gateways there is a file named `bitcartcheckout.php`
4. At whmcs panel, go to setup > payments > payment gateways
5. On the next screen, click on the **All Payment Gateways** tab and click on **Bitcart Checkout** to enable the plugin. The next step will be to configure it.

## Plugin Configuration

After you have enabled the Bitcart plugin, the configuration steps are:

1. In the admin panel URL field, enter your checkout URL (for example, [https://checkout.bitcart.ai](https://checkout.bitcart.ai)) without slashes. If your Bitcart doesn't run the new checkout, enter your admin panel URL (for example, [https://admin.bitcart.ai](https://admin.bitcart.ai)) instead
2. Enter your merchants API URl (for example, [https://api.bitcart.ai](https://api.bitcart.ai)) without slashes

This plugin also includes an IPN (Instant Payment Notification) endpoint that will update your WHMCS invoice status.

- Initially the WHMCS invoice will be in a **Unpaid** status when it is initially created.
- When the Bitcart invoice is complete, the payment is added to the WHMCS invoice, converted back to the invoice's currency if the gateway uses **Convert To For Processing**. Once fully paid, it will change to a **Paid** status, and your order will be safe to ship, allow access to downloadable products, etc.
