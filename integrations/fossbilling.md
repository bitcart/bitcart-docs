# FOSSBilling

Integrate Bitcart into your self-hosted FOSSBilling instance easily!

{% embed url="https://www.youtube.com/watch?v=t8YFOhTngQo" %}

### Integration Requirements

This version requires the following:

- FOSSBilling instance
- Running Bitcart instance: [deployment guide](https://docs.bitcart.ai/deployment)

### Installing the Plugin

1. From your FOSSBilling panel, go to configuration > payment gateways -> New payment gateway
2. Upload Bitcart.php to the directory suggested by your deployment. [Download it](https://raw.githubusercontent.com/bitcart/bitcart-boxbilling/master/Bitcart.php) from this repository
3. Enable it by clicking on the button near Bitcart, fill in all settings and save.

### Plugin Configuration

After you have enabled the Bitcart plugin, the configuration steps are:

1. Enter your admin panel URL (for example, https://admin.bitcart.ai) without slashes. If deployed via configurator, you should use https://bitcart.yourdomain.com/admin
2. Enter your merchants API URL (for example, https://api.bitcart.ai) without slashes. If deployed via configurator, you should use https://bitcart.yourdomain.com/api
3. Enter your store ID (click on id field in Bitcart's admin panel to copy id)

Enjoy!
