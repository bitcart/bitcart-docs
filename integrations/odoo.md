# Odoo

Integrate Bitcart into Odoo and bridge your company's management system with Bitcart as a way to process payments!

{% embed url="https://youtu.be/9vorewcOBgE" %}
Bitcart Odoo Plugin
{% endembed %}

### Integration Requirements <a href="#integration-requirements" id="integration-requirements"></a>

This version requires the following:

- Your Odoo instance
- Running Bitcart instance: [deployment guide](../deployment/)

### Installing the Plugin <a href="#installing-the-plugin" id="installing-the-plugin"></a>

1. Add `payment_bitcart` directory to your addons directory (You can take [the latest release](https://github.com/bitcart/bitcart-odoo/releases/latest), unzip it and get `payment_bitcart` from there)
2. From your Odoo apps page, activate "Bitcart Payments" app
3. Configure the plugin (see details below)

### Plugin Configuration <a href="#plugin-configuration" id="plugin-configuration"></a>

After you have enabled the Bitcart plugin, the configuration steps are:

1. Enter your admin panel URL (for example, [https://admin.bitcart.ai](https://admin.bitcart.ai/)) without slashes. If deployed via configurator, you should use [https://bitcart.yourdomain.com/admin](https://bitcart.yourdomain.com/admin)
2. Enter your merchants API URL (for example, [https://api.bitcart.ai](https://api.bitcart.ai/)) without slashes. If deployed via configurator, you should use [https://bitcart.yourdomain.com/api](https://bitcart.yourdomain.com/api)
3. Enter your store ID (click on id field in Bitcart's admin panel to copy id)

Enjoy!
