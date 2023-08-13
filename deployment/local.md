# Local Deployment

If you want to try out Bitcart on your local machine, it is also possible. If you don't have a domain name, Bitcart provides a way to test in a local-only deployment.

The setup is almost the same as always (see [docker deployment](docker.md)), the catch is: the domain name must end with .local. Bitcart then modifies your host machines' /etc/hosts file to make it work.&#x20;

{% hint style="info" %}
This only works from the computer on which you install Bitcart directly. To access it from outside, you should either use Tor or your own domain name + static ip
{% endhint %}

Here and later we assume that you've cloned the bitcart-docker repository, entered that directory and entered root shell by using `sudo su -` (note that minus at the end, it's important!). If you're on mac os, use the scripts as your current user and don't enter root shell.

### Local setup (.local domains, only current PC)

```bash
export BITCART_HOST=bitcart.local
export BITCART_REVERSEPROXY="nginx"
./setup.sh
```

You will get the admin panel running at http://bitcart.local/admin, store at http://bitcart.local and api at http://bitcart.local/api. Good for testing locally/developing without all the hassles of [manual deployment](manual.md). Note that if you don't use [one domain mode](../guides/one-domain-mode.md), it will still work.

### Tor setup (everywhere, requires tor browser)

```bash
export BITCART_ADDITIONAL_COMPONENTS=tor
./setup.sh
```

It isn't even required to set `BITCART_HOST` if tor is enabled. You can get onion addresses generated from the `compose_tor_servicesdir` docker volume. For more details check our [tor guide](../guides/tor.md).&#x20;



It is even possible to combine both ways to be able to access both locally and from anywhere!
