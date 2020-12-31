# Templates

Templates in BitcartCC are powered by the Jinja2 templating engine. It means that you have the full flexibility of the templating engine.

There are currently 3 different templates you able to customize in BitcartCC: `notification`, `product` and `shop`. Plus you are able to create custom-named templates for use with our future scripting language.

Check the example templates [here](../examples/templates.md)

For each of the objects, you are able to select a custom template.

The templates for an object are selected as per [template selection rules](../bitcartcc-basics/walkthrough.md#template-selection-rules)

![Store default templates](../.gitbook/assets/store_default_templates.png)

## Notification

Notification template is used when building the message to be sent via all configured [notification providers](../bitcartcc-basics/walkthrough.md#notification-providers) to the merchant notifying of successful order \(to start shipping, for example\).

The are two variables passed:

* `store`, containing the store this notification belongs to
* `invoice`, containing the invoice that has been paid

You can use those two variables to build whatever message that fits the best for you. 

The default template is the following:

```text
New order from {{ invoice.buyer_email }}
```

 An up-to-date version can always be found at this [link](https://github.com/bitcartcc/bitcart/blob/master/api/templates/notification.j2)

## Shop

Shop template is used when building the base message for a successful payment associated with a store. It may optionally include paid invoice products' templates.

The are two variables passed:

* `store` is the store the invoice is associated with
* `products` is a list of already rendered individual product's templates

You can use this template to provide some design for the order confirmation message sent to the buyer.

The default template is the following:

```text
Welcome to our shop!
Thank you so much for your purchase!
Your summary is below:
{% for product in products %}
{{product}}
{% endfor %}
If you've got any questions, email us at {{store.email}}.
Best wishes, your {{store.name}}.
```

An up-to-date version can always be found at this [link](https://github.com/bitcartcc/bitcart/blob/master/api/templates/shop.j2)

## Product

Product template is used when building the message for each individual product. Normally it is included as part of the main store message sent to the customer upon successful payment.

There are 3 variables passed:

* `store` is the store the product is associated with
* `product` is the current product being processed
* `quantity` is the quantity the customer has selected to buy

Note that, you can configure different templates for each product, or use the same template for all products.

The default template is the following:

```text
Thanks for buying {{product.name}} x {{quantity|int}}!
{% if product.download_url %}
Your download link: {{product.download_url}}
{% else %}
It'll ship shortly!
{% endif %}
```

An up-to-date version can always be found at this [link](https://github.com/bitcartcc/bitcart/blob/master/api/templates/product.j2)



