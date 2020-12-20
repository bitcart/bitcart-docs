# Walkthrough

In this article, we will walk you through the BitcartCC admin panel user interface and show you how to navigate through different options.

When creating an account, if you're the first user registered on the instance, you'll granted superuser rights\(full control over the server\). On third-party hosts it usually not so, but if you're hosting your own instance you'll of course become the full owner of it.

After you created the account on the BitcartCC instance hosted by yourself or a third-party, you'll see a lot of information cards.

![Overview of your created wallets, stores, invoices and more!](../.gitbook/assets/admin_cards.png)

Note about the night mode of the panel. By default, admin panel uses day mode, but if in your local time it is from 8 pm to 6 am, night mode will be enabled automatically 😉 You can configure it by clicking the moon icon in the top right corner of the page.

![Day mode](../.gitbook/assets/day_mode.png)

Each information card contains summary about something in your account: wallets, stores, products, invoices, etc.

By clicking Details button in any of the cards you will be able to see full information about specific unit.

Note, wallets balance is displayed not in BTC, but in abstract currency. It is a sum of all your balances in different currencies.

If you're a superuser, you can click on profile icon in the top right corner of the page to visit server settings page.

![Server management button](../.gitbook/assets/superuser_button.png)

From server settings page, you can control the users of your server, and many more. For more information, check Server Settings FAQ

Now, to the other common settings.

Each page basically contains one common thing - a datatable.

It is a feature-rich datatable, supporting searching, ordering, create/edit/delete actions, and some additional actions depending on what page you're on. For example, on stores page, you can configure email settings for a store by clicking email icon in actions column.

![Actions example](../.gitbook/assets/email_icon.png)

## Wallets

The core of BitcartCC is creating a wallet. In BitcartCC, one wallet represents one currency.

Default currency is btc, you can change that. When creating wallet, just type in the currency code\(btc,ltc etc.\) in currency field. Wallets may have a name and their xpub.

The nice feature of BitcartCC is that it does not require your private keys, but it supports many different formats.

You can enter \(x/y/z\)pub - public key, \(x/y/z\)prv - private key, or even electrum seed - providing easiest migration possible from many wallets, especially electrum. If you don't have an xpub yet, we recommend you create a wallet somewhere. [Electrum](https://electrum.org) wallet provides the best integration with BitcartCC, check Architecture page for more information.

When creating a wallet, it will get synced very fast\(depends on the size of the wallet, but shoudn't take too long\), and BitcartCC will fetch it's balance and display it.

![Wallets page](../.gitbook/assets/wallets.png)

![Create wallet pop-up](../.gitbook/assets/create_wallet.png)

By clicking arrow near any of the rows, you can view some additional details, like wallet xpub.

![Wallet details](../.gitbook/assets/wallet_details.png)

## Stores

You can create unlimited amount of stores in BitcartCC.

Each store can contain any amount of products and associated invoices. Store is a base entrypoint for anything related to checkout.

Store may have multiple wallets connected. That way, connecting different wallets with different base currencies, you can achieve multicurrency checkout.

Selecting multiple wallets of the same currency is **NOT** recommended. On invoice creation, BitcartCC will pick the first wallet of that currency, returned by database\(they might be returned in any order\).

![Stores page](../.gitbook/assets/store_page.png)

![Create store pop-up](../.gitbook/assets/create_store.png)

If you want to send customers invoices on successful checkout, you should configure email server.

To do that, click on email icon in actions column, and enter SMTP server details.

Store email is the email used to send messages from, and the display email in BitcartCC POS store.

Email host, port, login and password are credentials for your SMTP server. Email host shoudn't include any http:// or https:// parts. If your SMTP server requires TLS, turn on SSL/TLS switch.

When done\(you should click save button first\), click on Test ping button to see if your setup is working.

Note for gmail SMTP servers, you should enable access by turning on [less secure apps](https://myaccount.google.com/lesssecureapps)\(it is still secure, gmail apps aren't the requirement\). You might also need to [allow access](https://accounts.google.com/DisplayUnlockCaptcha) on a new account.

![Email server settings](../.gitbook/assets/email_settings.png)

## Discounts

It is optional page for your initial setup, you may skip it for now.

In many cases you might want to add some discounts to your store. New year discounts, other holidays? Limited time promocode discounts? Discount when paying in your preferred currency? Anything is possible with BitcartCC.

Just provide a percent\(integer\) for your discount, and discount apply conditions:

* promocode\(optional, when not provided discount is always applied when other conditions succeed\)
* end date
* currencies\(comma separated list of currency to apply to, or empty to apply to all currencies\)

You can link discounts to products in the products page.

When one invoice at creation time has matched multiple discounts, BitcartCC will pick the best discount\(by percent\).

![Discounts page](../.gitbook/assets/discounts_page.png)

![Create discount pop-up](../.gitbook/assets/create_discount.png)

## Products

Products are your base selling unit. Create your products, link them to your stores, add product details - and they will get displayed in your store POS!

BitcartCC supports many different information for creating products:

* Amount, displayed in store POS in USD
* quantity\(how many products of the same kind available\)
* product category\(used for filtering in store POS to classify your products\)
* Discounts applied to the product\(see previous section\)
* Product status\(like in stock, not available, up to you\)
* Download url\(for digital content, will be sent to customer in email\)
* Store, from which to take wallets and other information
* Date of creation\(auto-filled\)
* Product image\(supports cropping, rotating and many more!\)
* Product description

![Products page](../.gitbook/assets/products_page.png)

![Image preview](../.gitbook/assets/image_preview.png)

![IDs of connected discounts](../.gitbook/assets/discounts_list.png)

![Click on any ID to copy it](../.gitbook/assets/copy_snackbar.png)

![Create product pop-up](../.gitbook/assets/create_product.png)

![Create a perfect image for your product \(:](../.gitbook/assets/edit_image.png)

![Store POS reflecting entered products information](../.gitbook/assets/store_pos.png)

## Invoices

This page can be used to monitor your paid invoices, create new invoices and send them to friends, or to pay an invoice.

Supported information:

* Amount is amount in currency unit\(BTC, LTC, etc.\) which will be used to generate invoice
* Store, from which to take wallets and other information
* Connected products\(for store POS, optional\)
* Promocode, if customer entered it during checkout process\(auto-filled\)
* Notification URL where to send IPN notifications on invoice status change\(more below\)
* Redirect URL, customer will be redirected to it after successful checkout.
* Buyer email, if customer entered it during checkout process\(auto-filled\)
* Order ID, used by external integrations like woocommerce, track your orders by searching for order id\(auto-filled\)
* Discount - ID of the discount applied during invoice creation\(auto-filled\)
* Invoice status, more below
* Date of creation\(auto-filled\)
* Payment methods - not editable fields, displaying checkout information

If your invoice contains connected products, you'll be able to know which products were bought by the customer. The name of the first connected product will be used on checkout, or words "QR code" otherwise.

Amount will be the same for every payment method, so if you enter 5 as amount, it'll create payment methods for example for 5 BTC, 5 LTC and 5 GZRO. Take that in mind when creating a new invoice. This might be changed in future.

### Notification URL

If you fill in notification URL, BitcartCC instance will send IPN notifications to that URL via a POST request.

It will send the following json data:

`{"id": invoice_id, "status": new_status}`

When invoice status changes\(Pending-&gt;complete, Pending-&gt;expired, etc.\), notification will be sent.

It's up to you how to process that IPN notification. You should also verify that data sent is correct, as theoretically, anyone can send that POST request if they know your IPN handler URL. So, check that sent status is the same as the one got from get invoice request.

Invoices statuses can be one of the following:

* Pending\(in progress\)
* complete\(invoice paid\)
* invalid\(unexpected error\)
* expired
* In progress\(lightning network status\)
* Failed\(lightning network status\)



After invoice creation, you'll be able to view checkout information by clicking show button in payment methods column. It will display a so-called "invoice preview", it is not a fully functional checkout, but just an information dialog to display payment methods\(it ignores invoice status\).

By clicking open checkout you'll be redirected to full checkout, respecting invoice statuses and other things. Invoice url can be safely shared with others and used for checkout right from your admin panel.

![Invoices page](../.gitbook/assets/invoices_page.png)

![Create invoice pop-up](../.gitbook/assets/create_invoice_popup.png)

![Preview payment methods in admin panel](../.gitbook/assets/checkout_preview.png)

![Invoice expired](../.gitbook/assets/checkout_expired.png)

![Invoice paid](../.gitbook/assets/checkout_complete.png)

![Full checkout page](../.gitbook/assets/checkout_full.png)

## Notification providers

On this page you can configure your notification providers, to later connect them to your stores.

Supported information:

* Notification provider name for display
* Provider to use, you can choose of many available ones
* Provider options, differing from provider to provider

![Notification providers page](../.gitbook/assets/notifications.png)

![Create notification provider pop-up](../.gitbook/assets/create_notification.png)

![Choice of various providers](../.gitbook/assets/notification_provider_selection.png)

Each notification provider has different settings. Refer to their documentation about how to get certain settings. After that, select needed provider, fill in the settings and save changes.

Then you can reuse notification providers by connecting them to needed stores!

When notification provider is connected, on each successful order it will be run to deliver a notification to you.

![Connect notification provider to store](../.gitbook/assets/connect_notification_provider.png)

![Sample notification via telegram provider](../.gitbook/assets/notification.png)

## Templates

On templates page you can override global server templates, or create custom ones.

Available fields:

* Name of template, you can select from built-in ones or type in a new one
* Template text

All templates in BitcartCC are rendered via [Jinja2](https://jinja.palletsprojects.com/en/2.11.x).

Read about it's syntax in their [template designer documentation](https://jinja.palletsprojects.com/en/2.11.x/templates).

![Templates page](../.gitbook/assets/templates.png)

![Create template pop-up](../.gitbook/assets/create_template.png)

![Default templates list](../.gitbook/assets/default_templates.png)

### Template selection rules

When a template is being requested to render \(for example, when sending notification via notification providers, or composing email message\), it is selected in the following order:

1. If this product or store has template connected, it will be used
2. If it has no template connected, default global store or product template will be used \(named store or product\), if exists
3. If none of templates above are customized, [default templates](https://github.com/bitcartcc/bitcart/tree/master/api/templates) are used 

### Changing object's templates

On some pages, for example, stores or products pages, you will be able to edit templates per each item \(per each product, per each store, etc.\)  
If so, on such pages you will see the following icon:

![Edit templates icon](../.gitbook/assets/edit_templates.png)

By clicking on it, you will be able to override default templates for this item. Such templates are always used the first if they are set.

![Edit default templates pop-up](../.gitbook/assets/edit_templates_popup.png)

Note that in the example image above it is not neccesary to connect default templates for each store, as the template we created is named notification, therefore overriding default ones for each store.

