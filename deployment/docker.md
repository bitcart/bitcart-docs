# Docker Deployment

{% hint style="info" %}
You may check out an easier method: [Configurator](configurator.md)
{% endhint %}

BitcartCC uses docker for deployment. This allows us to simplify the installation and make BitcartCC installable in almost any environment.

Currently BitcartCC runs on 2 architectures: amd64 (most PC and servers), arm64 (raspberry pi). Arm32 is not supported officially anymore.

Almost every docker deployment starts like that:

```bash
sudo su -
git clone https://github.com/bitcartcc/bitcart-docker
cd bitcart-docker
# export needed settings, for example
export BITCART_HOST=yourdomain.tld
./setup.sh
```

{% hint style="warning" %}
Note the minus sign after `su`. It is required. If you are deploying on mac os, you don't need to enter the first command. This is to load all settings
{% endhint %}

There are different environment variables available in order to customize the deployment

{% hint style="info" %}
In order for BitcartCC to work, if you use `BITCART_HOST`, you should create a DNS A record from your domain registar, pointing to your current server
{% endhint %}

Environment variables are set like so:

```bash
export VARIABLE_NAME=value
```

Here are the main ones:

* `BITCART_HOST` configures on which domain BitcartCC should run. It is required unless you use any of the methods from [local deployment](local.md). It works in [one domain mode](../guides/one-domain-mode.md). Your BitcartCC Store will be accessible at `BITCART_HOST`, admin panel at /admin and Merchants API at /api. For other ways of configuration (for example different servers), check the one domain guide
* `BITCART_CRYPTOS` configures which coins to enable. It is a list of coin symbols separated by commas. By default only btc is enabled. For example, to enable btc and eth, you would run `export BITCART_CRYPTOS=btc,eth`
* `BITCART_REVERSEPROXY` - configures the reverse proxy used. By default `nginx-https` is used (with automatic ssl certificates generation). It might be useful to disable it to access your services directly or you can set it to `nginx` to disable ssl
* `BITCART_ADDITIONAL_COMPONENTS` - you can add additional components to your deployment. For example, using `export BITCART_ADDITIONAL_COMPONENTS=tor` enables tor.

There are also quite a few settings related to configuring coins in BitcartCC. Each coin has the same set of settings you can configure:

* `COIN_NETWORK` changes on which network the coin runs. For example: `export BTC_NETWORK=testnet` would enable testnet in BTC coin with no other changes required!
* `COIN_LIGHTNING` enables lightning network for coins which support it (BTC-based). For BTC you would do: `export BTC_LIGHTNING=true`
* `COIN_DEBUG` enables debug mode for daemons to log more information. For example `export BCH_DEBUG=true`
* `COIN_SERVER` configures the daemon to use exact server you specify instead of connecting to many servers at once. For example `export BNB_SERVER=https://bsc-dataseed.binance.org` For btc-based coins, you can set up your own [ElectrumX](https://github.com/spesmilo/electrumx) or [Fulcrum](https://github.com/cculianu/Fulcrum) server with your own full node. For eth-based coins, server is your full node's RPC url.

For the complete list of configuration settings you can use (to e.g. open some ports, change networks used or anything else), check out full configuration description at [bitcart-docker github](https://github.com/bitcartcc/bitcart-docker/blob/master/README.md#configuration).
