# Store POS

Your store POS is a ready-to-use store, which you can use in online or physical stores.

It is enabled by default in the full docker deployment.

It is enabled in the `frontend` or `all` installation groups (see `BITCART_INSTALL` setting at [Docker deployment](../deployment/docker.md#configuration))

The store POS displays the information for the store configured by server admin. It can display one store at once. It is controlled from [server management settings](server-management-settings.md#id-of-the-store-to-enable-on-pos)

The price slider will automatically adjust to the maximum price of the products in the store. The currency for all products in the store is determined by the [default currency](../bitcart-basics/walkthrough.md#stores) of selected store

Categories are built from all of the selected product's categories, plus the `all` category to show all products

The Only Sale setting shows only products that have an active [discount](../bitcart-basics/walkthrough.md#discounts) connected to them.

The Contact Us text shows the store's email. It is configured in the email settings of the store

At the header, store name is displayed.

Tor icon is added if [Tor support ](tor.md)is enabled.

Before checkout, customer will be able to enter their email, and optionally, a promocode.

![Store POS main page](<../.gitbook/assets/store\_pos (1).png>)

![Store POS product details](../.gitbook/assets/store\_pos\_product.png)

![Cart](../.gitbook/assets/store\_pos\_cart.png)

![Checkout details](../.gitbook/assets/store\_pos\_email.png)

![Checkout page](../.gitbook/assets/store\_pos\_checkout.png)
