# BoxBilling/FOSSBilling

Integrate BitcartCC into your self-hosted BoxBilling/FOSSBilling instance easily!

{% embed url="https://www.youtube.com/watch?v=t8YFOhTngQo" %}

### Integration Requirements

This version requires the following:

* BoxBilling/FOSSBilling instance
* Running BitcartCC instance: [deployment guide](https://docs.bitcartcc.com/deployment)

### Installing the Plugin

1. From your BoxBilling/FOSSBilling panel, go to configuration > payment gateways -> New payment gateway
2. Upload BitcartCC.php to the directory suggested by your deployment. [Download it](https://raw.githubusercontent.com/bitcartcc/bitcart-boxbilling/master/BitcartCC.php) from this repository
3. Enable it by clicking on the button near BitcartCC, fill in all settings and save.

### Plugin Configuration

After you have enabled the BitcartCC plugin, the configuration steps are:

1. Enter your admin panel URL (for example, https://admin.bitcartcc.com) without slashes. If deployed via configurator, you should use https://bitcart.yourdomain.com/admin
2. Enter your merchants API URL (for example, https://api.bitcartcc.com) without slashes. If deployed via configurator, you should use https://bitcart.yourdomain.com/api
3. Enter your store ID (click on id field in BitcartCC's admin panel to copy id)

Enjoy!
