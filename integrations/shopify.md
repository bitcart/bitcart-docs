# Shopify

The following document guides you through **setting up BitcartCC with** [**Shopify**](https://www.shopify.com)**.**

### Prerequisites: <a href="#prerequisites" id="prerequisites"></a>

* Shopify account
* BitcartCC - [self-hosted](broken-reference) or run by a [third-party host](../deployment/thirdpartyhosting.md) v0.6.7.0 or later.
* [Created BitcartCC store](../getting-started/createstore.md) with [wallet set up](../getting-started/createwallet/)

{% embed url="https://youtu.be/c09Av7SgaQM" %}

### Setting up BitcartCC with Shopify

1. In Shopify, go to Apps > and at the bottom of the page click on the `Develop apps for your store`.
2. If prompted, click on `Allow custom app development`
3. `Create an app` and name it
4. On the app page, in `Overview` tab, click on the `Configure Admin API scopes`
5. In the filter admin access scopes type in `Orders`
6. In `Orders` enable `read_orders` and `write_orders` and then click `Save`
7. Click on the `Install App` in the top right corner and when pop-up window appears click `Install`
8. Reveal `Admin API access token` and `copy` it.
9. In your BitcartCC, go to Stores page and click on shopify integration icon.
10. Enter your shop name
11. In secret key field paste the `Admin API access token`
12. In the api key field paste the `API key` from Shopify.
13. In Shopify's `Store Settings > Checkout > Order status page > Additional Scripts` paste the script provided by BitcartCC in Shopify Integration dialog when you click the copy script button (including the opening and closing tag `</script>`.
14. In Shopify's `Store Settings > Payments > Manual payment methods` add `manual payment method` then click `create custom payment method`
15. In `Custom payment method name` fill in `BitcartCC`, optionally you can fill in other fields, but it's not required. Please see the message below about naming of a custom payment method
16. Hit `Activate` and you've set up Shopify and BitcartCC successfully.

{% hint style="info" %}
Custom Payment method name **must** contain at least one of the following words: `bitcoin`, `bitcartcc`, `bitcart` or `btc` to work.
{% endhint %}

